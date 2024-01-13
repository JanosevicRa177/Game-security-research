# Analiza DRM [^1] alata i njihov uticaj na video igre

## Uvod 

  Ovaj odeljak projekta fokusiraće se na analizu single player [^2], offline[^3] video igara. Arhitektura ovakvih igara na višem nivou nije kompleksna jer ne postoji server koji će voditi računa o odredjenim aspektima igre, već se sva interakcija sa korisnikom dešava na klijentskoj strani. Korisnik na svom računaru skida igru i može je igrati nezavisno od interneta. Samim tim što se sve potrebno za igru nalazi na korisničkom računaru, manipulacija igrom je izuzetno laka, jer napadamo nešto što se nalazi kod nas. U Standardnijim web primerima napadači napadaju naš sistem preko interneta, i ne poseduju veliku količinu informacija o samom sistemu, u ovom slučaju sve potrebne informacije se nalaze upravo na napadačevom računaru a da mi toga nismo ni svesni.

  U daljem tekstu biće više naziva za napadače, poput Repacker i Cracker[^4], nadalje napadača ćemo oslovljavati sa cracker. Cracker je osoba koja zaobilazi DRM alate drugim alatima koji služe za monitoring software-a. 
  
## Software koji cracker-i koriste

  Cracker-i ne uzimaju i nasumično pokušavaju da čitaju kod igre, prvo to je nemoguće jer kod igre nije običan kod na koji smo svi navikli već se svodi na mašinski kod koji je čitljiv računaru ali ne i ljudima. Zbog toga software koji koriste cracker-i se svodi na statičke i dinamičke napade. Glavni alati koje cracker-i koriste su disassembler-i i debugger-i. [2]
  
Disassembler-i su alati koji rade suprotno od assembler-a, umesto da asemblerski kod prevode na mašinski, oni mašinski kod prevode u asembler. Cracker-i ovaj alat često koriste za statičko rasparčavanje i analizu video igara.

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Offline%20igre/Slike/IDA%20pro%20reverse%20engineering.png" />
</p>

<p align="center">
  Primer jednog dissasembler-a u radu, u ovom slučaju IDA pro dissasembler
</p>

Debuggeri sa druge strane, kao i u programiranju, služe za ispitivanje rada koda, oni pri pokretanju igre analiziraju kod koji se pokreće i prikazuju ga redom po izvršavanju u adekvatnom asemblerskom kodu.

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Offline%20igre/Slike/x64dbg%20reverse%20engineering.png" />
</p>

<p align="center">
  Primer jednog debugger-a koji cracker-i koriste za analizu, u ovom slučaju x64dbg
</p>


Razlika izmedju ova dva pristupa je što statičkom analizom moramo ispitati celokupan kod igre da bi došli do odredjene implementacije, dok dinamičkom analizom možemo ciljano tražiti zaštitu ali ne i videti celokupan kod igre. Ukoliko neko želi detaljan primer kako izgleda upadati i probijati zaštitu video igre može pogledati video jednog od poznatih cracker-a koji je probio najjači DRM u području video igara. Video se nalazi [ovde](https://www.youtube.com/watch?v=FnTNc-i2u-0&t=1375s).

## Diagram polja napada i napadači

  Na slici ispod može se videti uopštena skica kako bi sistem trebao da izgleda kod offline igara, posedujemo samu igru kao glavni resurs napada, dok su napadači crackeri, repackeri pa i sami korisnici. Crackeri pokušavaju da upadnu u samu igru, i time naštete kompaniji koja stoji iza igre, Repacker-i kao naredni korak u probijanju igre, teže ka tome da crack-ovane igre prepakuju i izbace odredjene delove, poput podrške za više jezika da bi fajlovi zauzimali manje prostora i na kraju sami korisnici koji biraju da skinu crack-ovanu igru umesto da istu igru kupe i podrže ljude koji stoje iza date igre.

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Offline%20igre/Slike/Diagram%20polja%20napada.png" />
</p>

<p align="center">
  Diagram polja napada
</p>

  Zaštititi offline video igre nije ni malo lak posao, jer resurs koji štitimo se nalazi baš kod napadača. Zaštititi video igru od cracker-a je kao štititi zamak sa limitiranom odbranom dok napadači imaju neograničene trupe, zbog toga je na Diagramu polja napada iznad kružno prikazana zaštita igre, jer napadi mogu doći sa svih strana.

## Istorijat

  Probijanje video igara nije novost, postoji još od devedesetih godina i vremenom su se razvijali i načini zaštite u vidu DRM alata a uporedo su se razvijali i načini za probijanje zaštita. Rat izmedju DRM developera i Cracker-a se i dan danas nastavlja dok cracker-i gube kako vreme teče. Razlog je što kompanije sve više ulažu u sigurnost svojih igara, dok su cracker-i pojedinci ili manji timovi koji u većini slučajeva ne zaradjuju od probijanja video igara. Od ranih dana Crack-ovanja video igara ti napadači su većinski bili obični korisnici, ili studenti koji imaju slobodnog vremena da čačkaju po kodu igre, u to vreme nije postojala adekvatna zaštita video igara, ovo doba je još poznato kao zlatno doba piraterije[^5]. Kako je vreme teklo, počele su da pojavljuju odredjene kompanije koje bi iznajmljivale svoje načine zaštite video igara kompanijama da bi ih zaštitili. Tada napadači prestaju da budu obični korisnici i studenti sa malo više volje i vremena i postaju timovi eksperata koji rade ozbiljan broj sati da bi probili zaštitu. Poznati timovi kroz istoriju piraterije su SKIDROW, RELOATED, CPY, CODEX. Dosta ovih timova se vremenom raspalo, delom jer je crack-ovanje postalo zahtevnije a ne donosi dovoljno novca, delom zato što kada budu otkriveni budu pozvani da rade za firme koje služe za zaštitu od crack-ovanja. [1]

  Najnovija zaštita koja je za malo zapečatila vrata crack-ovanju igara jeste Denuvo DRM sistem za zaštitu autorskih prava, koji se trenutno smatra za najjači sistem, pored njega postoji VMProtect koji je isto popularan. Dugo je Denuvo vladao kao neprobijajuća odbrana sve dok se par mladih cracker-a nisu pojavili, prvi je na scenu došao crack-er pod imenom Voksi, koji je probio Denuvo sistem i ignorisao sva pravila koja su se poštovala u Cracking grupama, krenuo je da snima video materijale kako probija sisteme i otkrio svoju ličnost što je stogo zabranjeno u Piratskim vodama. Njegovo ignorisanje pravila ga je na žalost dovelo do pravnih problema pa je morao da se povuče sa scene. Nakon njega se pojavio ženski cracker koji nije otkrivao svoj identitet, ali je vodila reddit stranicu gde je često bila u komunikaciji sa igračima video igara i ostalim cracker-ima i repacker-ima, problem je nastao jer je jako često bila u sukobu sa repacker-ima pa je bila i krivično gonjena.[1]

  Sve više se čini da će crack-ovanje video igara postati prošlost ali se onda na par godina pojave pojedinci koji nas ubede u suprotno i pojedinci koji uspeju da sruše DRM alate koji se naplaćuju za velike svote novca. Problem nastaje što većinu njih ili otkupe DRM kompanije da rade za njih ili povuče slava, jer su cracker-i poprilično cenjeni u grupama igrača video igara. Zato se donekle sami igrači mogu svrstavati u napadače video igara jer podstiču iskusne napadače da napadaju video igre. [1]

  Priča o istorijatu dugogodišnje borbe izmedju kompanija za izradu video igara i cracker-a je izuzetno lepa priča koja se može pročitati [ovde](https://karnavpopat.medium.com/the-scene-the-pirates-and-the-empress-2132713836ca).

## Dekompozicija pretnji

  Pretnje koje mogu nauditi samom sistemu su se razvijale uporedo sa razvijanjem DRM sistema, i koncentrišu se na odredjene načine odbrane od probijanja video igara. Resurs koji pokušavamo da zaštitimo je sama igra, a taj isti resurs tehnički već pripada našem napadaču, problem je kako omogućiti običnom korisniku da igra igru, a da onemogućimo ikakvu interakciju sa igrom ili kodom igre van predvidjenog. Odredjene vrste mitigacija povlače za sobom drugačije vrste napada, dok mitigacije koje rešavaju probleme na skoro savršen način dolaze u problem sa performansama igre gde mogu da naude samom iskustvu igranja video igre.

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Offline%20igre/Slike/DRM%20stablo%20napada.png" />
</p>

<p align="center">
  Stablo napada
</p>

### Napadi

  Glavni napad koji je pokrenuo lavinu probijanja video igara, predstavlja analizu koda video igara i nalaženje rupa u video igrama ili nalaženje zaštita od replikacije. Glavni razlog zašto su napadači počeli sa ovim napadom jeste jer je početno bila otežana replikacija video igara, prvobitno je replikacija zahtevala da korisnik poseduje disk, dok nakon prestanka korišćenja diskova i razvitka interneta ova mitigacija je zastarela, još osim toga disk se sam po sebi mogao replicirati tako da je bila privremena ali ne i dovoljno dobra. [3]
  
  Drugi razlog je što su igre nakon instalacije zahtevale ključ za pristup igri, koji ako napadač ne poseduje ne može pristupiti igri, pa je zbog toga bio primoran da pronadje algoritam koji validira da li je uneseni ključ validan. Nakon što je pronašao algoritam mogao je analizom napravi deo koda koji će generisati ključeve i time zaobići traženje ključa za pristup igri, još osim da poseduje jedan ključ može generisati neograničenu količinu ključeva za pristup igri. [3]

  Treći razlog je da bi se zaobišao Always online mitigacija koja zahteva od korisnika da non stop ima konekciju sa internetom. Napad na ovu odbranu je Server emulation gde će napadač simulirati konekciju sa internetom preko svog računara da bi prevario igru da misli da je povezana na server. [3]

### Mitigacije [4]

  Postoje razne mitigacije za zaštitu od crack-ovanja, uglavnom idu i u kombinaciji ali proizvodjači video igara moraju voditi računa da ne preopterete igru zaštitom jer DRM alati mogu drastično uticati na performanse igre. Iz tog razloga dosta kompanija nakon što DRM alati budu probijeni od stane cracker-a, patchevima ugase DRM alate da bi povećali performanse igre. [5] Primeri Uticaja na performanse se mogu videti ovde:

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Offline%20igre/Slike/Uticaj%20DRM%20alata%20na%20performanse%20igara.png" />
</p>

<p align="center">
  Uticaj DRM alata na performanse igre
</p>

Mitigacije koje zaustavljaju generatore ključeva jesu online key verifikacije, jer korisnik ne može pristupiti igri dok ne unese validan ključ a u ovom slučaju napadači ne mogu pristupiti kodu za proveru ključa jer se on nalazi na udaljenom serveru. Ovaj vid mitigacije uz limit aktivacija istim ključem predstavlja solidan vid zaštite video igara. Pristup odbrane ključevima se većinski danas ne koristi zato što može više problema naneti krajnjem korisniku nego rešiti problema, jer ne samo da korisnik mora imati internet već igricu može aktivirati samo odredjeni broj puta. Primer sa online key verifikacijom je pogodan za manje igre koje ne koštaju veliku svotu novca pa samim tim ni ne zahtevaju veliku zaštitu jer ukoliko igra ne košta puno krajnji korisnik više rizikuje da nanese sebi štetu malicioznim software-om koji se potencijalno nalazi u crack-ovanoj igri nego što će dobiti.

 Glavna mitigacija koja sprečava napadače da analiziraju kod igre jeste enkripcija koda igre tokom skidanja igre sa interneta i pokretanje igre u virtuelnim okruženjima kojima alati za probijanje ne mogu da pristupe. Primer DRM alata koji koristi virtuelna okruženja je VMProtect.

 Mitigacija koja je proizvela drugu vrstu napada je **Always online** koja forsira korisnika da dok igra igru bude konstantno povezan na internet a zaobilazi se emulacijom servera. U kombinaciji sa enkripcijom koda može predstavljati ozbiljan vid zaštite, koji takodje može izazvati ozbiljne frustracije kod korisnika jer od njega zahteva da ima internet konekciju za nešto što zapravo ne bi trebalo da zahteva internet konekciju.

 Korak dalje od **Always online** je mitigacija **Online kod igre** koja ne samo da zahteva da korisnik ima konekciju na internet već odredjene aktivnosti u igrici uopšte nisu moguće bez internet konekcije, jer se kod za izvršavanje odredjenih akcija nalazi na internetu i samim tim je potrebna konekcija na server da bi igranje igre bilo moguće. Primer ovakve igre je Assassins Creed 2 koja je zahtevala dobru internet konekciju u ranijim danima interneta, pa su korisnici imali izuzetno loša iskustva u vidu performansi ove igre.

 Najsvežija mitigacija koja je zapravo indirektna mitigacija, i možda smer u kome će se igre dalje razvijati jeste **Cloud gaming**. Poprilično nov način igranja igara koji se oslanja na princip subskripcija, jedna subsripcija je iznajmljivanje računara, dok je druga iznajmljivanje igara. U ovom slučaju slično kao i sa web tehnologijama, ako sistem dobro zaštitimo igre se praktično ponašaju kao klasišan client server arhitektura. Subskripcije mogu zahtevati drastično manje novca ukoliko korisnik želi samo povremeno da igra odredjene video igre i nije aktivan igrač, dodatno uz to ne želi da sastavlja računar koji može koštati dosta novca. Još uvek nedovoljno razvijen sistem ali validan način omogućavanja jeftinijeg igranja igara, i kasnije zaštite igara ako postane dovoljno zastupljen jer odredjene igre mogu biti limitirane da se igraju na odredjenim mašinama koje su iznajmljene i limitirane samo na igranje igara. Poprilično kontraverzna mitigacija koja u zavisnosti od cene može zauvek zaustaviti crack-ovanje, ali u istom korisnik ima samo subrkipciju i ne poseduje niti igru niti računar.

## Reference

[1] [The Scene, The Pirates, and The Empress](https://karnavpopat.medium.com/the-scene-the-pirates-and-the-empress-2132713836ca)

[2] [Software for cracking software. Selecting tools for reverse engineering](https://hackmag.com/security/software-for-cracking-software/)

[3] [Examples of game cracking attacks](https://hackmag.com/security/software-for-cracking-software/)

[4] [Digital rights management (DRM) examples](https://www.pcgamingwiki.com/wiki/Digital_rights_management_(DRM))

[5] [Video Game DRM Analysis and Paradigm Solution](https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Offline%20igre/Literatura/Video%20Game%20DRM%20Analysis%20and%20Paradigm%20Solution.pdf)


[^1]: DRM - sktraćenica za Digital Rights Management sisteme, služe da osiguraju digitalne proizvode od neovlašćenog repliciranja poput igara, pesama, filmova i slično

[^2]: Single player igre - Igre koje podržavaju samo jednog igrača

[^3]: Offline igre - Igre sa čiji efektivan rad nije potrebna konekcija sa internetom

[^4]: Cracker - Napadač koji svojim tehničkim umećem zaobilazi DRM alate da bi uspeo da replicira video igru, suštinski slomi igru da bi je oformio bez barikada

[^5]: Piraterija - Neovlašćeno repliciranje ili distribucija softvera zaštićenog autorskim pravom
