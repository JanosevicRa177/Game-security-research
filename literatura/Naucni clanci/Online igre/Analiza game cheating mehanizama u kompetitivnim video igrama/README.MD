# Analiza "game cheating" mehanizama u kompetitivnim video igrama

## Uvod
Od nastanka kompetitivnih video igara, maliciozni igrači su nastojali da pronadju bilo kakvu osetljivost sistema kako bi ostvarili prednost u odnosu na ostale igrače. Ovaj trend je posebno postao popularan kod novijih igara koje su doživljavale nagli porast igrača.
U ovom članku pokušaćemo da analiziramo najpoznatije metode cheat-ovanja, kao i mitigacije kojima se autori igara služe.

Cheat-ovanje poprima različite forme i to u vidu skripti, botova ili nepodržanog načina igre koji se eksploatiše zavisno od funkcionalnosti koje poseduje igra. Zajedničko za svaku od ovih formi jeste da se na neki način ostvari prednost i to je veliki problem kako za ostale igrače, tako i za autore igara. Osim što su ostali igrači nadjačani, ne njihovom voljom, igrači gube lojalnost prema igri te samim tim direktno štete autorima i njihovim prihodima. Sve ovo je uzrokovalo da se kompanije koje razvijaju video igre ozbiljno posvete odbrani od svih poznatih vidova cheat-ovanja te tako kreiraju svoje anti-cheat sisteme. Naravno anti-cheat sistemi se razlikuju od kompanije/video igre i ne postoji univerzalno rešenje za svaku vrstu igre. Najpoznatiji anti-cheat sistemi, koje ćemo detaljno opisati u daljem tekstu, jesu **VAC (Valve)**, **Easy Anti-Cheat (Epic Games)** i **Vanguard (Riot Games)**.

## Arhitektura cheat-ovanja
Kako bismo bolje razumeli na koji način funkcionišu cheat-ovi i na kojem arhitekturalnom nivou sistema se odvijaju, analiziraćemo sistem privilegija koji se odvija u kernelu(procesoru).

<p align="center">
    <img src="https://github.com/JanosevicRa177/Game-security-research/blob/game-cheating-study/literatura/Naucni%20clanci/Online%20igre/Analiza%20game%20cheating%20mehanizama%20u%20kompetitivnim%20video%20igrama/Slike/SlojeviSistemaIgara.png"/>
</p>
<p align="center">
   Slika 1. CPU slojevi privilegija
</p>

Slojevi privilegija(slika 1) nam govore o zaštiti pristupa koje sistem dozvoljava korisniku. Što sloj više teži ka unutrašnjosti kružnice, to ima više privilegija. Vanjski slojevi ne mogu da pristupe unutrašnjim, dok unutrašnji mogu da pristupe vanjskim. 
Sloj 0 je sloj koji sadrži sve procese u sistemu i onaj koji kontroliše sloj 0 može direktno da manipuliše čitavom mašinom. Sloj kojem korisnici mogu da pristupaju jeste sloj 3(još se naziva i aplikacioni sloj). Ukoliko bi u ovom sloju došlo do manipulacije, video igre bi mogle da detektuju cheat, te ne bi bilo toliko problema sa takvim vidom napada. Ipak, baš iz ovog razloga, maliciozni igrači su napravili cheat-ove koji koriste sloj 0, kojem aplikacije ne mogu da pristupe zbog sloja privilegije, te su onda ovakvi cheat-ovi nemogući za detekciju od strane aplikacija. Da bi se kompanije izborile sa ovakvim problemom, one kreiraju svoj anti-cheat softver koji takodje radi na sloju 0. Neki od anti-cheat sistema koji rade na sloju 0 jesu **Vanguard (Riot Games)** i **Easy Anti-Cheat (Epic Games)**. Primer anti-cheat sistema koji radi na sloju 3(aplikacionom sloju) jeste **VAC (Valve)**. Anti-cheatovi u sloju 3 nisu beskorisni, ali kao što smo naveli već, ne mogu da detektuju cheat-ove iz sloja 0, već rade više kao antivirusni softver.

### Problemi sa slojem 0
Kreiranje anti-cheat softvera u sloju 0 na prvu ruku izgleda kao rešenje problema. To jeste slučaj ako se sistem posmatra iz ugla kompanije. Ako posmatramo sistema iz ugla korisnika, tu nailazimo na veliki broj problema koji nosi ovakav softver. Kao što je već pomenuto, sloj 0 može da manipuliše čitavim sistemom, što znači da ako postoji softver na sloju 0 on takodje može da manipuliše sistemom. 
Sve navedeno upućuje na to da anti-cheat sistemi mogu stvoriti razne vrste ranjivosti u sistemu i to:
1. Otvaraju vektor napada na sistem koji napadači mogu iskoristiti
2. Anti-cheat sistemi u sloju 0 mogu da slučajno posmatraju legitiman softver kao cheat
3. Korisnički podaci sistema su u potpunosti izloženi developerima anti-cheat softvera koji mogu maliciozno da ih iskoriste.


## Pretnje, Napadi, Mitigacije
U narednom tekstu ćemo analizirati najpoznatije vrste napada na video igre, kao i mitigacije koje se mogu primeniti kao način zaštite
<p align="center">
    <img src="https://github.com/JanosevicRa177/Game-security-research/blob/game-cheating-study/literatura/Naucni%20clanci/Online%20igre/Analiza%20game%20cheating%20mehanizama%20u%20kompetitivnim%20video%20igrama/Slike/StabloNapada.png"/>
</p>
<p align="center">
   Slika 2. Stablo napada
</p>



