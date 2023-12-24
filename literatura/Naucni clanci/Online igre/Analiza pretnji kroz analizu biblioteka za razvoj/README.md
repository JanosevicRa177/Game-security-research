### Analiza biblioteka za razvoj

## Uvod
Ovaj odeljak će se fokusirati na ranjivosti sistema prilikom upotrebe biblioteka za razvoj u online video igrama. Vecina danasnjih tehnologija za razvoj u svom jezgru ne sadrži sve potrebne alate kako bi obezbedila potpunu nezavisnost od eksternih biblioteka. Zbog toga developeri često posežu za bibliotekama koje im omogućavaju brži i lakši razvoj, a samim tim smanjuju trošak proizvoda. Ipak, ovakav pristup često nosi svoje posledice ukoliko se ne izvodi na pravilan način.
S obzirom da je većina biblioteka održavana od strane pojedinačnih developera ili čak nije održavana u dosta slučajeva, javljaju se ranjivosti koje se vrlo lako mogu eksploatisati u realnim sistemima.
Pretnje koje se mogu dogoditi u ovakvim sistemima su razne, ali ćemo se fokusirati na specifičnu pretnju i to neovlašten pristup sistemu servera. Motiv napadača je najčešće lična upotreba resursa koje je prethodno osvojio(skidanje torent sadržaja, server može postati učesnik ddos napada) ili narušavanje sposobnosti samog sistema. 

<p align="center">
    <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Analiza%20pretnji%20kroz%20analizu%biblioteka%20za%20razvoj/Slike/ModelPretnji.png"/>
</p>
<p align="center">
    Dijagram pretnji
</p>

## Napadi

### Eksploatacija ranjivosti biblioteke radi izvršavanja komandi na sistemu servera
Neretko se u poznatim bibliotekama dogadjaju propusti koji mogu dovesti do katastrofalnih posledica. Nažalost ovakvi propusti se u većini slučajeva pronadju kada već bude kasno.
Jedan od takvih primera jeste propust koji je u sebi imala biblioteka log4j [^1]. Veoma poznata biblioteka za logovanje, u sebi je imala propust koji je omogućavao napadaču da u potpunosti preuzme pristup računaru. Ovaj propust nije bio samo aktuelan za web aplikacije, već i za video igre koje su koristile java platformu. Jedna od njih je i super popularna igra Minecraft, koja ima na milione igrača. Napad na ovu biblioteku se bazirao na modul koji ona koristi "Apache Log4Shell" [^2] koji je omogućavao izvršavanje komandi direktno u terminalu sistema. [1]

### Mitigacije 
- Regularno ažuriranje verzija biblioteka jeste jedna od prevencija da se dogode stvari poput prethodno navedenih. Ukoliko dodje do nekih vrsta ranjivosti, one koje su pronadjene se najčešće brzo patch-uju i dolaze u novoj verziji biblioteke. [2]
- Automatsko skeniranje zavisnosti može dosta pomoći prilikom otkrivanja ranjivosti sistema. Naime, sistemi koji rade automatsko skeniranje zavisnosti uzimaju trenutne verzije biblioteka iz sistema(i njihove zavisnosti), te pokušavaju da pronadju da li postoji neka ranjivost koja se može pronaći u otvorenoj bazi ranjivosti.[2]
- Sandboxing [^3] može biti jedan od načina da se sigurnosni propust otkrije ranije. Zbog svoje prirode zatvorenosti u okruženje, omogućava da se testira softver bez efekta na mašine na kojima se izvodi [3]
- Code review predstavlja možda zapostavljen ali jako bitan način da se uoče nepravilnosti ili potencijalni propusti, najčešće dolazi kao dodatni sloj zaštite.[2]

## Reference
[1] https://www.dynatrace.com/news/blog/what-is-log4shell/

[2] https://newrelic.com/blog/how-to-relic/mitigate-open-source-library-security-risks

[3] https://www.linkedin.com/pulse/how-keep-check-security-vulnerabilities-your-libraries-aditya-kumar/


[^1]: Log4j - Biblioteka za logovanje za java platformu
[^2]: Apache Log4Shell
[^3]: Sandboxing - Metoda testiranja softvera gde se aplikacija izoluje od operativnog sistema u svoje okruženje. - Više o sandboxingu: https://www.checkpoint.com/cyber-hub/threat-prevention/what-is-sandboxing/ 