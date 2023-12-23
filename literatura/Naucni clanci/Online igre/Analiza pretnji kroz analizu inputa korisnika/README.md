# Analiza input-a korisnika [^1]

## Uvod

  Ovaj odeljak će se fokusirati na koji način napadač može da naruši integritet naše igre analizom i probijanjem konekcije izmedju klijenta i servera tokom igre. Ovaj deo sistema je izuzetno teško kvalitetno zaštititi jer je potrebno napraviti kompomise zaštite i responzivnosti našeg sistema. Naši korisnici zahtevaju brz odgovor u zadatom roku a zaštite vrlo često narušavaju responzivnost, pa se zbog toga osim analize pretnji potrebno je analizirati da li će mitigacija neke od pretnji više narušiti integritet igre zbog pada u performansama nego što će zaštititi korisnika.

  Fokusirani diagram sa dekompozicije modula se može videti ovde:

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Analiza%20pretnji%20kroz%20analizu%20inputa%20korisnika/Slike/fokus%20napada1.png" />
</p>

<p align="center">
  Fokus sa dekompozicije modula
</p>

  Lepši prikaz samog fokusa napada može se videti na drugoj slici gde su uklonjeni nebitni podaci i dodata površina za napad kao i resursi koji napadaču mogu biti primamljivi za napad. Na ovom diagramu vidi se da podaci koje moramo zaštiti jesu game state[^2] da bi znali koje je stvarno stanje igre, takodje u game state-u se mogu naći i podaci o korisnicima koji nikako ne smeju dospeti u napadačeve ruke da ne bi napadač mogao da napadne direktno korisnika umesto sam server. Isto tako isti korisnički podaci se mogu naći u prenosu pa se moraju i tu zaštititi. Takodje ti podaci u prenosu mogu biti i izvor napada pa se moraju validirati i analizirati.

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Analiza%20pretnji%20kroz%20analizu%20inputa%20korisnika/Slike/fokus%20napada2.png" />
</p>

<p align="center">
  Diagram polja za napad i zumirani prikaz na deo za analizu input-a korisnika
</p>

## Dekompozicija pretnji

  Ovakav deo sistema u video igrama izuzetno je bitno analizirati jer predstavlja veći deo interakcije sa korisnikom. Korisniku se za to vreme mora omogućiti da se oseća prijatno i sigurno dok je u igri. Načini na koje napadač može eksploatisati ovakav sistem su razni, da li da prikupi neke resurse u igri, stekne prednost u odnosu na druge korisnike u vidu nedozvoljenog znanja ili u vidu pripomaganja u competitive igrama[^3]. Na sledećem diagramu biće prikazan opširan pogled na pretnje i napade ove površine za napad (površina za napad je u ovom slučaju sama razmena podataka izmedju servera i klijenta)

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Analiza%20pretnji%20kroz%20analizu%20inputa%20korisnika/Slike/modelovanje%20pretnji%20game%20inputa.png" />
</p>

<p align="center">
  Model pretnji i napada sa svojim mitigacijama
</p>

Nakon diagrama osvrnućemo se na svaku pretnju u sistemu, detaljno opisati napade koji su vezani za datu pretnju i opisati na koji način se sistem može zaštititi od datih pretnji.

### Kradja korisničkih podataka u igri

  Poznat napad "Man in the middle" koji predstavlja prisluškivanje poruka koje pristižu na server od korisnika sistema, moguća je i izmena datih podataka ali nije tema pretnje ovakvog sistema, zato što napadač nema neku dobit u izmeni podataka, ono što mu je bitnije jeste da dopre do podataka o korisniku koji mu omogućavaju razne druge napade. Svakim input-om korisnika, šalju se podaci od ID-u korisnika kao i odakle je podataka došao (IP adresa korisnika). Ukoliko napadač prisluškuje mrežu može doći do ovih podataka i zloupotrebiti ih.

  Sličan napad gde napadač može doći do identičnih podataka o korisnicima jeste ukoliko uspe da dospe do keširanih podataka sa game servera. Tada suštinski dobija iste podatke o korisnicima a uz to dobija celokupnu sliku state-a igre.
  
  Napadač ovim podacima može izbacivati korisnika uporno sa game servera čime će mu onemogućiti da učestvuje u igri i realizovati **Denial of service** napad ali sa strane klijenta. Isto tako ako poseduje njegovu adresu može preplaviti klijentski deo igre nepotrebnim saobraćajem i uvesti kašnjenje izmedju klijenta i servera i time na skroz drugačiji način realizovati **Denial of service** napad. Isto tako može slati inpute umesto korisnika i time spoof-ovati korisnika. Dalje mitigacije će ovaj problem dalje proširiti.

  Da bismo korisnika zaštitili od ovakvih napada, postoje par mitigacija koje će zaštititi sistem od daljeg razaranja sistema. Ukoliko napadač već poseduje Id korisnika ili njegovu adresu, sistem je već probijen. Tako da se mora realizovati jaka zaštita na nivou podataka koji pristižu na server i koji se čuvaju na serveru.

  Način na koji možemo korisnika zaštititi jeste da uvedemo enkripciju u sistem, na nivou podataka u prenosu kao i enkripciju nad podacima koji se odnose na korisnike na serveru. Razlog zašto ne enkriptujemo ceo state sistema i time sprečimo da napadač pristupi celokumnom state-u igre na serveru jeste što bismo drastično narušili odziv servera u tom slučaju. Cilj ovog dela analiziranja jeste da zaštitimo krajnjeg korisnika a ne integritet stanja video igre.

  Enkripcija sa serverske strane če imati svoj ključ za enkripciju dok će svaki od korisnika imati svoj ključ za razmenu podataka izmedju servera i klijenta. Ukoliko je igra koja funkcioniše po principu partija koje traju od 10 minuta do sat vremena, za svaku sesiju partije će se korisniku generisati ključ za enkripciju, dok ukoliko je igra koja ne funkcioniše po principu partija već dugih sesija igranja generisaće se novi ključ posle odredjenog vremena. Time je korisnik u potpunosti zaštićen od kradje svojih podataka.

### Nekontrolisano slanje input-a

  Napad koji je poznat kao **Denial of Service** napad koji smo do sad već spomenuli, u ovom slučaju napad je na sam server. Gde je cilj da se server preplavi podacima i time smanji odziv servera ili u potpunosti prekine konekcija na server. Način na koji se ovaj napad realizuje jeste da se šalje veći deo podataka nego što server može da primi.

  Mitigacija ovakvog napada se realizuje delom kroz samu arhitekturu sistema. Serveri većinski primaju podatke od korisnika po odredjenom tick rate-u[^4] koji predstavlja koliko korisničkih podataka server može da obradi u sekundi [1]. Tako da server kroz samu arhitekturu rešava ovakav problem jer prihvata samo jedan korisnički input u odredjenom vremenskom intervalu, a ostale ignoriše od datog korisnika. Još jedan način koji nije toliko kvalitetan jeste da se detektuju sa kojih adresa dolazi previše paketa i da se IP adrese tih korisnika blacklist-uju, ili ako je u pitanju neki interni server za igre možemo da korisnimo whitelist sistem da dopustimo pristup samo korisnicima kojima mi želimo. Blacklist i whitelist[^5] sistemi se više koriste kada korisnici imaju svoje servere, ako je malo veći server blacklist a ako je tipa server samo za mali skup korisnika(tipa grupa prijatelja) koristiće se whitelist mehanizam.

  Isto tako ako se desi navala podataka od raznih korisnika moguća je privremena zaštita kroz instanciranje više game servera i ubacivanje load balancing sloja da ravnomerno šalje korisnike na njih. Ovakav sistem je obradjen kroz samu dekompoziciju modula sistema.

  Napad koji se odnosi na state-igre predstavlja omogućavanje napadaču da utiče na state igre na način koji nije predvidjen. Ukoliko korisniku omogućimo da šalje veliku količinu podataka na server, velike su šanse da će na taj način naći kako da eksploatiše sistem. Isto tako odredjene grupe arhitektura su sigurnije od drugih zato što poseduju telo koje će posedovati stvaran state igre. Primer su razlike izmedju Peer-to-Peer arhitektura i Client-Server arhitektura, Client-Server arthitektura je drastično lakša sa osigurati jer poseduje Validaciono telo i vidu servera, dok kod Peer-to-Peer arhitekture svaki korisnik šalje svim korisnicima svoje inpute i svaki korisnik ima svoj state.[1]
  
  Što se više podataka šalje na server, to je potrebno više podataka validirati. Da bi korisniku smanjili mogućnosti da naruši state igre uglavnom se šalju samo signali da korisnik nešto želi da odradi, ili eventualno jako mali skup podataka koji je lako validirati[1].

### Napredne anomalije input-a

  Pod naprednim anomalijama inputa svrstavaju se paketi informacija koji se šalju na server koji su sintaksno i po tehničnoj semantici ispravni, ali kada ljudsko biće pogleda date podatke primećuje da date inpute ne može proizvesti čovek. Ovo su najčešći napadi na video igre, koje napadaču mogu doneti neku vrednost jako brzo, ili u vidu resursa ili u vidu poena veštine. Većina ovakvih napada je lako realizovati zato što postoje grupe ljudi koji prodaju software koji omogućava ovakve napade.

  Postoje dva glavna načina napada naprednih anomalija, jedan jeste korišćenje botova da sakupljaju resurse. Time se može narušiti In-game ekonomija i obesmisliti korisničko prikupljanje resursa. Isto tako botovi mogu level-ovati korisničke likove i time ubrzati proces napretka u igri. Botovi neretko mogu neke strateške igre igrati i bolje od ljudi pa može biti jako frustrirajuće igrati protiv nekog protiv koga će korisnik sigurno izgubiti partiju a time i svoje vreme. 
  
  Drugi način napada jeste korišćenje nedozvoljenog software-a u competitive igrama kao što su Counter Strike 2, League of Legends, DOTA 2 i slične igre koje zahtevaju izuzetno umeće i reflekse korisnika. Napadač će sa uključenim software-om pristupiti igri i uz pomoć njega imati veliku prednost jer neće morati da reaguje na odredjene elemente igre jer će na njih reagovati sam nedozvoljeni software. 

  Postoje razni načini mitigacija ovakvih napada i razvijali su se više godina unazad, i dan danas ne postoji potpuna mitigacija ovakvog napada jer napadai evoluiraju iz dana u dan. Načini koji su bili standardni ranije za mitigaciju ovakvih napada jeste logovanje inputa korisnika u odredjene fajlove a zatim analiza datih podataka[2]. Time je omogućeno efektivno pronalaženje botova u video igrama, a isto tako i pronalaženje korisnika koji koriste nedozvoljeni software u competitive video igrama. Problem nastaje što ovakav način detekcije i mitigacije zahteva da prodje neko vreme od napada. I da se nakon toga analiziraju logovi i pronadju napadači. Ovo može biti jako stresno za korisnika koji igra protiv napadača, a u slučaju botova, botovi mogu već prodati i predati skupljene resurse trećem licu i time negirati štetu napadaču.

  Pristup koji je se u skorije vreme intenzivno razvija jeste detekcija anomalija putem neuronskih mreža gde se za oba napada može razviti sistem koji će u realnom vremenu analizirati inpute i sa velikom preciznošću pronalaziti napadače i vrlo brzo mitigirati štetu, ili u najgorem slučaju obavestiti korisnika koji potencijalno na primer igra protiv bota. Detaljan opis neuronskih mreža i pristupa koji se koriste za neuronske mreže može se naći u dokumentu **Varanje u video igrama kroz anomalije input-a** [2]. Još jedan od razloga za korišćenje neuronskih mreža za detekciju jeste što su napadači za svoje svrhe botovanja krenuli da koriste neuronske mreže i time smanje šansu da botovi budu detektovani.

  Neki od članaka su kao lošije performanse neuronskih mreža u odredjenim situacijama okrivili mali dataset kojim je obučena neuronska mreža, ili je dataset bio veštački u smislu da su inputi korisnika bili iz profesionalnih mečeva a ne mečeva običnih korisnika, i botovi su bili napravljeni da igraju protiv drugih botova a ne botovi koji su zapravo napadači sistema. Način da se ova dva sistema (logovanje i neuronske mreže) ukombinuju jeste da se prikupe velike količine logova na serverima, izfiltriraju se logovi i ti logovi se iskoriste za dalje obučavanje neuronskih mreža čime će se performanse povećati.

## Reference

[1] [Analiza arhitekture sistema, protokola i njihovih pretnji](https://github.com/JanosevicRa177/Game-security-research/tree/main/literatura/Naucni%20clanci/Online%20igre/Analiza%20pretnji%20kroz%20analizu%20inputa%20korisnika/Real-time%20Multiplayer%20Software%20Architecture%20and%20protocol%20threat%20mitigation)

[2] [Varanje u video igrama kroz anomalije input-a](https://github.com/JanosevicRa177/Game-security-research/tree/main/literatura/Naucni%20clanci/Online%20igre/Analiza%20pretnji%20kroz%20analizu%20inputa%20korisnika/Game%20cheating%20trough%20input%20anomalies)

[^1]: Korisnički input - delovanje korisnika u sistemu, unos komandi

[^2]: State igre - stanje u kom se igra nalazi, pozicije igrača, stvari koje oni poseduju i slično

[^3]: Competitive igre - Takmičarske igre, igre koje zahtevaju veliku umešnost, reflekse i znanje o samoj igri

[^4]: Tick rate - Vreme ažuriranja state-a game servera

[^5]: Whitelist/Blacklist - Sistemi u kojima se pamte IP adrese korisnika, Whitelist je lista svih korisnika koji mogu da pristupe sistemu, blacklist je lista korisnika kojima je pristup zabranjen
