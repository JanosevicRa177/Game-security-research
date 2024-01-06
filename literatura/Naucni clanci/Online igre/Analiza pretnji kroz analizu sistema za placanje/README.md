# Analiza sistema za placanje

### Uvod

Ovaj odeljak će se fokusirati na analizu potencijalnih ranjivosti sistema za plaćanje u online video igrama. Sistemi za plaćanje su veoma osetljiv deo bilo koje aplikacije,
pa samim tim podležu različitim vrstama napada. Motiv napadača u sistemima video igara moze biti različit, a većinski je orijentisan na stvaranje prednosti u odnosu na drugog igrača ili namerno pogoršavanje korisničkog iskustva kod drugih igrača. 
Moguće pretnje koje vrebaju sisteme za plaćanje u video igrama su:
- Nedozvoljen pristup resursima igre
    - Maliciozni korisnici mogu da iskoriste neke od mehanizama napada kako bi neovlašćeno pristupili resursima video igre koji im nisu dozvoljeni.
    Jedan od primera toga bi bilo zavaravanje sistema da se produži subskripcija na neki od asseta[^1] igre.
- Ometanje drugih korisnika video igara u korišćenju sistema za plaćanje
    - U ovom slučaju, napadač za cilj ima da ograniči ili u potpunosti odseče sistem za plaćanje od sistema za igru, kako bi sprečio korisnika da izvrši transakciju(plati
    subskripciju, kupi asset). Ovime napadač može da stekne prednost u odnosu na drugog igrača ili da našteti samom sistemu video igre, narušavajući njegovu funkcionalnost.

<p align="center">
    <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Analiza%20pretnji%20kroz%20analizu%20sistema%20za%20placanje/Slike/ModelPretnji.png"/>
</p>
<p align="center">
    Dijagram pretnji
</p>

### Napadi

### Webhook replay [1]
Webhook replay predstavlja jedan od načina napada na takozvane "webhook"-ove [^2]. Ovaj napad koristi ranjivost sistema koji nije idempotentan ili nema način provere ponavljanja istog zahteva. Pošto je webhook u suštini prost HTTP zahtev, podložan je presretanju u mreži. Upravo ova ranjivost webhooka može biti maliciozno iskorišćena. U slučaju sistema za plaćanje, webhook-ovi služe za dopremanje informacija o uspešnosti transakcija u sistemu ili ažuriranje informacija vezanih za subskripcije.
Primer jednog webhooka bi bio HTTP zahtev koji dojavljuje sistemu igre da je kupljena neka vrsta asseta. Ukoliko je asset potrošan, te se može kupiti na količinu, maliciozni korisnik može vrlo lako da po potrebi pošalje ponovljenu webhook dojavu o kupljenom assetu. Ovo bi značilo da ukoliko ne postoji zaštita, napadač može neograničeno da povećava kupljenu količinu, sa samo jednom stvarnom transakcijom. Naravno webhook replay napad nije ograničen samo na video igre, nego se može primeniti na bilo koji sistem koji koristi webhook-ove. 

## Koraci napada
1. Napadač pronalazi metu i odredjuje cilj napada.

2. Napadač pristupa prvoj fazi napada, gde mora na neki način da dodje do tela http zahteva. Ukoliko se napadač nalazi u istoj mreži kao i web server, onda ovaj korak postaje trivijalan zbog raznih alata za presretanje saobraćaja. Ukoliko se ne nalazi u istoj mreži, tada može da pokuša da pronadje ranjivost u sistemu koja bi mu omogućila pristup logovima(potencijalno se loguje telo webhooka) ili neki drugi način kojim bi iznudio pristup.

3. Nakon što napadač dobije u posed telo zahteva, napadač čeka pravi trenutak da ga upotrebi. Pravi trenutak zavisi od tipa napada koji se izvodi. Primer ovoga bi bio napad na subskripcije, gde nema smisla da se jedna te ista subskripcija produžava više puta prije nego što istekne. Dobar napad bi bio da se pošalje identičan webhook na isteku subskripcije, čime bi se ona dalje produžila.
  

## Mitigacije
- Prva u nizu odbrana od ovakve vrste napada jeste verifikacija potpisa platforme sa koje dobijamo webhook. Ovakva vrsta odbrane predstavlja preduslov za sve ostale vrste odbrane. Razlog tome je što se verifikacijom potpisa vrši sužavanje zahteva koji mogu dospeti na server i onemogućava da se šalju zahtevi koji nisu sa ciljane platforme.
Najčešće mesto gde se prosledjuje potpis, jesu header-i zahteva. Ova mitigacija ne može sama da brani sistem od replay napada, jer jos uvek ostaje problem presretanja zahteva.

```
export class StripeEventInterceptor implements NestInterceptor {
  constructor(
    @Inject(BILLING_SERVICE) private readonly billingService: IBillingService,
  ) {}

  intercept(
    context: ExecutionContext,
    next: CallHandler,
  ): Observable<any> | Promise<Observable<any>> {
    const request = context
      .switchToHttp()
      .getRequest<RawBodyRequest<Request>>();
    const response = context.switchToHttp().getResponse<Response>();
    try {
      this.billingService.validateEventSignature(
        request.headers['stripe-signature'] as string,
        request.rawBody,
      );
      return next.handle();
    } catch (e: any) {
      response
        .json({
          message: 'Signature invalid',
          status: 400,
        })
        .status(400);
    }
  }
}
```
<p align="center">Primer validatora potpisa stripe webhook-a</p>

- Pored odbrane verifikacijom potpisa, moguće je suziti zahteve i uz pomoć ip whitelist-a[^4], koji suštinski radi isti posao kao verifikacija potpisa, onemogućavajući slanje zahteva sa platforme koja nije ciljana. I verifikacija i ip whitelist zavise od sistema za plaćanje i toga šta on nudi kao vrstu zaštite.

- U kombinaciji sa verifikacijom potpisa, najčešće se koristi timestamp. Timestamp omogućava da se odredi kada je webhook kreiran, čime možemo odrediti dozvoljeni fiksni vremenski period koji sme proći nakon slanja. Stripe najčešće koristi period od 5 minuta, te se timestamp nadodaje na postojeći potpis prije hešovanja potpisa.

```
{
  "event": "payment_success",
  "data": {
    "amount": 100.00,
    "currency": "USD",
    "transaction_id": "abc123",
    "timestamp": "2024-01-06T12:30:45Z",
    "unique_id": "a1b2c3d4e5"
  }
}
```
<p align="center">Primer webhook payload-a koji koristi timestamp kao mehanizam zaštite</p>

- Idempotentnost [^3] predstavlja još jedno od rešenja za ovakav problem. Koncept idempotentnosti govori o tome da se jedan zahtev može obraditi samo jedanput. Ukoliko je idempotentnost endpointa korektno implementirana, timestamp-ovi nisu potrebni kao dodatna zaštita. 

### DDOS napad na endpointe za kupovinu resursa u igri [2]
DDOS(Distributed Denial of Service) je jedan od najpoznatijih napada na sisteme svih vrsta. Cilj ovakvog napada jeste da se zaustavi u potpunosti ili smanji koliko je moguće, saobraćaj prema odredjenom servisu. Online video igre nisu izuzetak od ovakvih napada, te napadači za cilj mogu imati razne stvari. Jedan od ciljeva bi mogao da bude sprečavanje regularnih korisnika da kupuju assete u igri. Razlog za sprečavanje bi bilo ostvarivanje prednosti nad protivnicima u competitive[^6] igrama. Ovakva vrsta napada se može izvesti na više načina a najčešća je upotreba distribuiranog napada sa više servera koji svi u isto vreme pokušavaju da preplave sistem sa zahtevima. 

### Mitigacije 
- Firewall predstavlja jednu od osnovih vrsta zaštite od DOS napada. Firewall može biti postavljen i na softverskom i na hardverskom nivou, te je njegova uloga da filtrira saobraćaj u zavisnosti od toga kako je podešen.[4]
- Ip whitelisting/blacklisting [^4] kao i u prethodnom primeru može da posluži za zabranu saobraćaja sa nepoželjnih ip adresa. Kao i firewall uloga mu je da vrsi filtraciju i smanji mogućnost napada na sistem.[3]
- Jedna od najbitnijih odbrana sistema od DOS napada jesu load balanceri. Load balanceri služe za izjednačavanje saobraćaja izmedju deployanih instanci sistema, a samim tim sprečavaju opterećavanje pojedinačnih instanci sistema. Ovime se postiže da su svi delovi sistema podjednako opterećeni i da se padovi instanci radi saobraćaja ne dešavaju tako često.[3]
- Veoma bitna stvar za odbranu od DOS napada jeste analiza pristiglih napada i identifikacija malicioznih zahteva. U današnje vreme primat u ovakvim analizama preuzimaju neuronske mreže. Najčešći odabir u današnjim sistemima jeste da se koriste eksterni servisi koji su specijalizovani baš za analizu pristiglih zahteva.[3]


## Reference
[1] https://hookdeck.com/webhooks/guides/webhook-security-vulnerabilities-guide

[2] https://www.fortinet.com/resources/cyberglossary/dos-vs-ddos

[3] https://aws.amazon.com/shield/ddos-attack-protection/

[4] https://ddos-guard.net/en/blog/what-is-a-firewall-and-how-it-works

[^1]: Asset - Bilo koji resurs u video igrama(specijalni item-i, moći, bilo šta što može da se kupi i iskoristi u igri)

[^2]: Webhook - Http zahtev koji šalje platforma sa kojom je sistem integrisan kako bi dojavila promenu stanja resursa

[^3]: Idempotentnost - svojstvo operacije gde višestruko primenjivanje proizvodi isti rezultat kao i jednokratna primena

[^4]: Ip whitelisting - Sve ip adrese koje se nalaze na listi imaju dozvolu pristupa

[^5]: Ip blacklisting - Sve ip adrese koje se nalaze na listi nemaju dozvolu pristupa

[^6]: Competitive igre - Takmičarske igre, igre koje zahtevaju veliku umešnost, reflekse i znanje o samoj igri