# Analiza "game cheating" mehanizama u kompetitivnim video igrama

## Uvod
Od nastanka kompetitivnih video igara, maliciozni igrači su nastojali da pronadju bilo kakvu osetljivost sistema kako bi ostvarili prednost u odnosu na ostale igrače. Ovaj trend je posebno postao popularan kod novijih igara koje su doživljavale nagli porast igrača.
U ovom članku pokušaćemo da analiziramo najpoznatije metode cheat-ovanja, kao i mitigacije kojima se autori igara služe.

Cheat-ovanje poprima različite forme i to u vidu skripti, botova ili nepodržanog načina igre koji se eksploatiše zavisno od funkcionalnosti koje poseduje igra. Zajedničko za svaku od ovih formi jeste da se na neki način ostvari prednost i to je veliki problem kako za ostale igrače, tako i za autore igara. Osim što su ostali igrači nadjačani, ne njihovom voljom, igrači gube lojalnost prema igri te samim tim direktno štete autorima igre. Sve ovo je uzrokovalo da se kompanije koje razvijaju video igre ozbiljno posvete odbrani od svih poznatih vidova cheat-ovanja te tako kreiraju svoje anti-cheat sisteme. Naravno, anti-cheat sistemi se razlikuju od kompanije/video igre i ne postoji univerzalno rešenje za svaku vrstu igre[1]. 

Najpoznatiji anti-cheat sistemi u modernim video igrama jesu **VAC (Valve)**[5], **Easy Anti-Cheat (Epic Games)**[8] i **Vanguard (Riot Games)**[9].

## Arhitektura cheat-ovanja
Kako bismo bolje razumeli na koji način funkcionišu cheat-ovi i na kojem arhitekturalnom nivou sistema se odvijaju, analiziraćemo sistem privilegija koji se odvija u kernelu(CPU).

<p align="center">
    <img src="https://github.com/JanosevicRa177/Game-security-research/blob/game-cheating-study/literatura/Naucni%20clanci/Online%20igre/Analiza%20game%20cheating%20mehanizama%20u%20kompetitivnim%20video%20igrama/Slike/SlojeviSistemaIgara.png"/>
</p>
<p align="center">
   Slika 1. CPU slojevi privilegija
</p>

Slojevi privilegija(slika 1) nam govore o zaštiti pristupa koje sistem dozvoljava korisniku. Što sloj više teži ka unutrašnjosti kružnice, to ima više privilegija. Vanjski slojevi ne mogu da pristupe unutrašnjim, dok unutrašnji mogu da pristupe vanjskim. 
Sloj 0 je sloj koji sadrži sve procese u sistemu i onaj koji kontroliše sloj 0 može direktno da manipuliše čitavom mašinom. Sloj kojem korisnici mogu da pristupaju jeste sloj 3(još se naziva i aplikacioni sloj). Ukoliko bi u ovom sloju došlo do manipulacije, video igre bi mogle da detektuju cheat, te ne bi bilo toliko problema sa takvim vidom napada. Ipak, baš iz ovog razloga, maliciozni igrači su napravili cheat-ove koji koriste sloj 0, kojem aplikacije ne mogu da pristupe zbog sloja privilegije, te su onda ovakvi cheat-ovi nemogući za detekciju od strane aplikacija. Da bi se kompanije izborile sa ovakvim problemom, one kreiraju svoj anti-cheat softver koji takodje radi na sloju 0. Neki od anti-cheat sistema koji rade na sloju 0 jesu **Vanguard (Riot Games)** i **Easy Anti-Cheat (Epic Games)**. Primer anti-cheat sistema koji radi na sloju 3(aplikacionom sloju) jeste **VAC (Valve)**. Anti-cheatovi u sloju 3 nisu beskorisni, ali kao što smo naveli već, ne mogu da detektuju cheat-ove iz sloja 0, već rade više kao antivirusni softver.[1]

### Problemi sa slojem 0
Kreiranje anti-cheat softvera u sloju 0 na prvu ruku izgleda kao rešenje problema. To jeste slučaj ako se sistem posmatra iz ugla kompanije. Ako posmatramo sistem iz ugla korisnika, tu nailazimo na veliki broj problema koji nosi ovakav softver. Kao što je već pomenuto, sloj 0 može da manipuliše čitavim sistemom, što znači da ako postoji softver na sloju 0 on takodje može da manipuliše sistemom. 
Sve navedeno upućuje na to da anti-cheat sistemi mogu stvoriti razne vrste ranjivosti u sistemu i to:
1. Otvaraju vektor napada na sistem koji napadači mogu iskoristiti
2. Anti-cheat sistemi u sloju 0 mogu da neposredno posmatraju legitiman softver kao cheat
3. Korisnički podaci sistema su u potpunosti izloženi developerima anti-cheat softvera koji mogu maliciozno da ih iskoriste.[1]


## Pretnje, Napadi, Mitigacije
U narednom tekstu ćemo analizirati najpoznatije vrste napada na video igre, kao i mitigacije koje se mogu primeniti kao način zaštite
<p align="center">
    <img src="https://github.com/JanosevicRa177/Game-security-research/blob/game-cheating-study/literatura/Naucni%20clanci/Online%20igre/Analiza%20game%20cheating%20mehanizama%20u%20kompetitivnim%20video%20igrama/Slike/StabloNapada.png"/>
</p>
<p align="center">
   Slika 2. Stablo napada
</p>

Na slici 2. možemo videti najpoznatije vrste napada na kompetitivne video igre. Iako smo se u prethodnom tekstu fokusirali na napade od kojih se branimo anti-cheat softverom, u narednom tekstu ćemo analizirati jos načina napada koji su mogući u kompetitivnim igrama.[2]


### Napadi [2]
Postoji mnoštvo napada koji se dogadjaju u kompetitivnim igrama, te će u ovom članku biti obradjena 3 napada i to:
1. Aim Bots
2. Lag Switch
3. Look Ahead


### Aim Bots Napad
Aim botovi su postali jako popularni u "pucačinama" zbog ekstremne prednosti koju daju igračima, kao i komplikovane detekcije. 
Njihova uloga jeste da pomognu malicioznom igraču u pucačkim igrama, tako što automatizuju nišan igrača i samim tim omogućavaju da maliciozni igrač ostvari prednost nad drugim igračima. Možda i najpoznatija kompetitivna video igra u kojoj se aim botovi upotrebljavaju, jeste Counter Strike (Valve). Većina nas je igrala ovu popularnu pucačinu makar jednom u životu, a oni koji su igrali kompetitivni mod, znaju koliko je frustrirajuće igrati protiv igrača koji koriste aim botove. Postoji više vrsta aim botova, kao što su:
- Pixel Scanning Aim Bots
- Direct Aim Bots
- Triggerbots

Pixel Scanning Aim Bots, kao što im samo ime kaže, rade na principu analize pixela trenutnog frejma, radeći ekstrapolaciju vizuelnih informacija i tako detektujući protivnika. Direct Aim Bots rade na principu analize podataka koji pristižu sa servera video igre, te onda na osnovu pozicije protivnika mogu da premeste nišan direktno na njega. Triggerbots su nešto drugačiji od prethodno navedenih aim botova po tome što ne rade nužno pomeranje nišana direktno na protivnika, već poboljšavaju reflekse igrača tako što detektuju da li je maliciozni igrač prevukao nišan preko protivnika, a zatim automatski okidaju akciju bez da maliciozni igrač učini išta.[4]

### Aim Bots Mitigacije

- Prva mitigacija jeste anti-cheat software. Kao primer za ovakvu vrstu mitigacije uzećemo VAC(Valve Anti Cheat) koji je razvila kompanije valve i čija je jedna od namena baš detekcija aim botova. VAC analizira fajlove igre, dynamic library links, kao i da li je došlo do promene kod executable fajlova. Ovime VAC postiže ne samo detekciju aim botova, već i detekciju svih vrsta manipulacija sadržajem igre. Ipak VAC ne mora u svim slučajevima da otkrije aim botove, te stoga nije kompletno rešenje za dati problem.[5]

- Druga mitigacija koja je veoma bitna jeste "Player Supervision" ili u prevodu nadgledanje od strane igrača. Igrači koji su stekli poverenje u igri i koji su ostvarili zavidne rezultate u kompetitivnim igrama, često dobijaju pristup softveru koji služi za nadgledanje kompetitivnih igara. Jedan od primera ovakvog softvera jeste Overwatch[6] kompanije valve, u kojem visoko rangirani igrači ocenjuju sumnjive situacije i da li je došlo do upotrebe cheat-ova, a zatim sistem glasanja odlučuje da li će igrač biti banovan. Primer upotrebe Overwatch-a možete pogledati na [linku](https://www.youtube.com/watch?v=a_n-_jIK9GY). 


### Lag Switch Napad
Ideja lag switch napada jeste da se upotrebi kašnjenje strima podataka ka serveru u svrhu zbunjivanja protivnika. 
Često u kompetitivnim igrama nesvesno dolazi do izvodjenja ovakve vrste napada. Razlog tome jeste što je lag switch uzrokovan lošom internet konekcijom, čime se strim podataka usporava do te mere da protivniku izgleda kao da se protivnik teleportuje. Ovakav napad može biti izveden tako što se isključi, a nakon toga brzo uključi ethernet kabel, dok se u isto vreme lokalna igra nastavi redovno. Ovo uzrokuje da lokalna instanca igre baferuje pokrete igrača i čeka da se povrati konekcija. Kada se konekcija povrati, svi podaci iz bafera se šalju velikom brzinom, te protivniku deluje kao da se igrač teleportovao. Ovakav napad se smatra hardverskim ali je isto tako moguće izvesti ga softverski. Dovoljno je imati program u mreži koji će po potrebi smanjiti protok mreže i time uzrokovati istu pojavu kao da smo hardverski isključili ethernet kabel. Primer ovakve vrste napada koji se manifestuje kroz teleportovanje igrača možete pogledati na [linku](https://www.reddit.com/r/EscapefromTarkov/comments/obg894/example_of_a_lagswitch/).

### Lag Switch Mitigacije

- Prva mitigacija jeste banovanje igrača koji nemaju dobru internet konekciju. Ovakav metod zaštite se može smatrati ograničavajući zbog toga što igrači sa lošijom internet konekcijom koji ne eksploatišu ovaj napad i dalje neće moći igrati igru. Ipak, ograničenje ne mora biti toliko ekstremno, već se može odrediti granica koja je dovoljno visoka da prepozna lag, ali isto tako dovoljno niska da većina igrača može da igra igru slobodno. Sve ovo se može postići upotrebom firewall-a ili ruter pravila koji mogu definisati granicu protoka za igrače. Naravno, ovakav vid zaštite i dalje nije ne predstavlja kompletnu zaštitu, iz razloga što je jako teško odrediti granicu. Zbog toga, u većini slučajeva će napadač moći steći prednost ali minimalno. [3]

- Još jedna zanimljiva mitigacija jeste korišćenje third party softvera koji omogućava hardversku detekciju problema sa internetom. Primer ovoga bi bila detekcija pada napona prilikom isključenja ethernet kabela, nakon čega bi korisnik kao i kod prve mitigacije bio banovan. [3]

### Look Ahead napad
Look Ahead napad se eksploatiše u peer to peer igrama tako što maliciozni igrač namerno odlaže svoje akcije u cilju da vidi šta će protivnik da uradi a zatim adekvatno odgovori akcijom. Maliciozni igrač se pretvara kao da ima problema sa konekcijom, te modifikuje pakete koje šalje, menjajući njihovu vremensku odrednicu. Ovime maliciozni igrač postiže da server detektuje paket kao da je poslat ranije, čime napadač stiče jasnu prednost nad protivnicima. [7]

<p align="center">
    <img src="https://github.com/JanosevicRa177/Game-security-research/blob/game-cheating-study/literatura/Naucni%20clanci/Online%20igre/Analiza%20game%20cheating%20mehanizama%20u%20kompetitivnim%20video%20igrama/Slike/LookAheadNapad.png"/>
</p>
<p align="center">
   Slika 3. Prikaz Look Ahead napada uz pomoć vremenskih odrednica
</p>


### Look Ahead mitigacije

- Glavna mitigacija vezana za Look Ahead napad jeste Lockstep protokol. Pošto svaka akcija, u igrama koje koriste peer to peer arhitekturu mora biti iskomunicirana sa svim čvorovima u igri, stvara se potencijal za upotrebu Look Ahead napada. Lockstep protokol rešava ovu situaciju tako što forsira igrače da se obavežu na odredjenu akciju pre nego što je otkriju. Svaka akcija se definiše uz pomoć hash funkcije i to je ujedno i identifikator da li akcija odgovara obavezi. Igra napreduje brzinom najsporijeg igrača zbog sinhronizacije, što ovaj protokol čini savršenim za "Turn Based" igre. Kako bi se prilagodio za realne igre, kreiran je asinhroni lockstep koji omogućava igračima da biraju akcije nezavisno od protivnika, ali ukoliko treba da dodje do interakcije sa protivnicima, igrači ulaze u takozvani lockstep režim.[7]


<p align="center">
    <img src="https://github.com/JanosevicRa177/Game-security-research/blob/game-cheating-study/literatura/Naucni%20clanci/Online%20igre/Analiza%20game%20cheating%20mehanizama%20u%20kompetitivnim%20video%20igrama/Slike/LookAheadPseudoCode.png"/>
</p>
<p align="center">
   Slika 4. Pseudo kod lockstep protokola
</p>

# Reference
[1] [https://www.tripwire.com/state-of-security/cybersecurity-in-competitive-online-gaming-cheating-mitigation-and-vulnerabilities](https://www.tripwire.com/state-of-security/cybersecurity-in-competitive-online-gaming-cheating-mitigation-and-vulnerabilities)

[2] [https://en.wikipedia.org/wiki/Cheating_in_online_games](https://en.wikipedia.org/wiki/Cheating_in_online_games)

[3] [https://security.stackexchange.com/questions/31303/preventing-artificial-latency-or-lag-hacking-in-multiplayer-games](https://security.stackexchange.com/questions/31303/preventing-artificial-latency-or-lag-hacking-in-multiplayer-games)

[4] [https://chatbotsjournal.com/what-are-aimbots-and-how-do-they-work-4936794e5453](https://chatbotsjournal.com/what-are-aimbots-and-how-do-they-work-4936794e5453)

[5] [https://help.steampowered.com/en/faqs/view/571A-97DA-70E9-FF74](https://help.steampowered.com/en/faqs/view/571A-97DA-70E9-FF74)

[6] [https://www.gosugamers.net/overwatch/streams/53612-bsdlp](https://www.gosugamers.net/overwatch/streams/53612-bsdlp)

[7] [https://github.com/JanosevicRa177/Game-security-research/blob/game-cheating-study/literatura/Naucni%20clanci/Online%20igre/Analiza%20game%20cheating%20mehanizama%20u%20kompetitivnim%20video%20igrama/Literatura/ActiveCheatingPrevention.pdf](https://github.com/JanosevicRa177/Game-security-research/blob/game-cheating-study/literatura/Naucni%20clanci/Online%20igre/Analiza%20game%20cheating%20mehanizama%20u%20kompetitivnim%20video%20igrama/Literatura/ActiveCheatingPrevention.pdf)
[8] [https://www.easy.ac/en-us/](https://www.easy.ac/en-us/)
[9] [https://support-valorant.riotgames.com/hc/en-us/articles/360046160933-What-is-Vanguard-](https://support-valorant.riotgames.com/hc/en-us/articles/360046160933-What-is-Vanguard-)



