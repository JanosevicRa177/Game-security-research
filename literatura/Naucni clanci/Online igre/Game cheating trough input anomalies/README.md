# Istraživanje anomalija input-a korisnika[^1] za prevenciju varanja u video igrama

## Pronalazak botova u video igrama

### Online game bot detection based on party-play log analysis

  Članak prokiva pronalaženje botova u video igrama po sistemu analize logova na serveru, igra koja se analizira u članku je AION MMORPG[^10] igra od kompanije NCsoft. Svaki input korisnika se analizira i pamti u odredjenim logovima na serveru, Predloženi načini detekcije botova su bili i detekcije sa klijentske strane, detekcije na mreži i detekcije na serverskoj strani

  Na klijentskoj strani uglavnom nastaje problem što se dosta takvog software-a za detekciju botova sukobljava sa antivirus software-om[^3] kojem je cilj da zaštiti krajnjeg korisnika. Time može izazvati neprijatnosti krajnjem korisniku.

  Detekcija na mreži koja se realizuje kroz monitoring mreže može biti pogodna za analizu detekcije botova ali uvodi dodatan delay[^4] koji može biti poprilično nezgodan ako su u pitanju competitive igre[^5].

  Zbog ovoga je najveći fokus na detekciji na samom serveru, koja se i obradjuje u članku. Server za svaki input korisnika beleži logove. Ti logovi imaju razne kategorije u vidu kom tipu interakcije input pripada. Da li korisnik odnosno mogući bot ubija čudovišta, priča sa drugim igračima, trade-uje [^6] se, skuplja resurse, završava misije i slično.

  Dati članak se fokusirao na analizu logova korisnika koji su bili u party play-u [^7] time su limitirali količinu logova za analizu na samo taj aspekat zajedničke igre, Nakon analize utvrdili su da vreme koje korisnici provedu u party play-u dirketno ima korelaciju sa time koje su šanse da je korisnik bot. Ukoliko su korisnici proveli 12 ili 24 sata u party play-u šanse da je u pitanju bot su daleko veće. Isto tako analizom vrste logova primetilo se da botovi ostavljaju daleko veću količinu logova za levelovanje [^8] likova, prikupljanje resursa i trade-ovanje, dok su logovi običnih korisnika ravnomerno rasporedjeni. Time se zaključilo da je glavni cilj botova da leveluju naloge i prikupljaju In-game novac [^9]. 
  
<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Game%20cheating%20trough%20input%20anomalies/Slike/Online%20game%20bot%20detection%20based%20on%20party-play%20log%20analysis/Broj%20igrača%20po%20vremenu%20u%20party%20play-u.png" />
</p>

<p align="center">
  Broj igrača po vremenu u party play-u
</p>
  
  Isto tako primećuje se da po većoj dužini trajanja party play-a broj igrača se svodi na dva u većoj količini. Razlog toga je što botovi uglavnom idu u paru, jedan stražari dok drugi prikuplja resurse.

  Ovakav sistem za detekciju botova daje preciznost od 96% tačnosti.

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Game%20cheating%20trough%20input%20anomalies/Slike/Online%20game%20bot%20detection%20based%20on%20party-play%20log%20analysis/Raspodela%20logova%20po%20vremenu%20u%20party%20play-u.png" />
</p>

<p align="center">
  Raspodela logova po vremenu u party play-u
</p>

Problem koji nastaje u ovakvnom načinu detekcije je što se logovi moraju već napraviti, nije moguća detekcija u realnom vremenu, ali sa druge strane može biti znatno jednostavniji od drugog pristupa koji će biti analiziran.

### Efficient Deep Learning Bot Detection in Games Using Time Windows and Long Short-Term Memory (LSTM)

  Članak koji pokriva detekciju botova putem neuronske mreže. Sobzirom na razvoj veštačke inteligencije u poslednjih par godina, domen veštačke inteligencije došao je i do botova za video igre, gde se stariji sistemi za detekciju botova mogu pomučiti da otkriju da li je korisnik bot. Zbog toga se u članku opisuje razvitak neuronske mreže za detekciju botova u strateškoj igri StarCraft: Brood War.

  Princip koji su koristili je LSTM(Long Short-Term Memory) neuronske mreže. Za razliku od Rekurzivnih neuronskih mreža LSTM mreže traže manje resursa i brže su, samim tim i pogodnije za obradu velike količine igrača. Neuronska mreža ovakvog tipa pokazala se pogodna za detekciju botova, jer najbolje rezultate pokazuje za kratko vreme analize, testirano je na vremenskim intervalima od 15, 30,45 i 60 sekundi, a najbolje se pokazala za 15 i 30 sekundi pa je izbor pao na detekciju od 15 sekundi jer je detekcija na 30 sekundi neznatno bolja 15 sekundi, ali je potrebno duplo kraće vreme za detekciju. 

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Game%20cheating%20trough%20input%20anomalies/Slike/Efficient%20Deep%20Learning%20Bot%20Detection%20in%20Games(LSTM)/Primer%20obučavanja%20po%20vremenskim%20intervalima.png" />
</p>

<p align="center">
  Primer obučavanja po vremenskim intervalima
</p>

  Neuronska mreža ovog tipa imala je četiri ulaza koji su skalirani na vrednosti izmedju 0 i 1, Ulazi su bili broj frame-a[^11], drugi parametar jeste broj selektovanih trupa, a treći i četvrti parametar su X i Z koordinate miša. Izlaz je bio jedan i predstavljao je detekciju da li je korisnik bot. Izmedju su se nalazila dva sloja LTSM neuronske mreže sa 64 čvora po svakom sloju.

  <p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Game%20cheating%20trough%20input%20anomalies/Slike/Efficient%20Deep%20Learning%20Bot%20Detection%20in%20Games(LSTM)/Primer%20neuronske%20mreže%20iz%20članka.png" />
</p>

<p align="center">
  Primer neuronske mreže iz članka
</p>
  
  Da bi se sistem ubrzao još više neuronska mreža je obradjivala igrače po principu da pronadje potencijalne botove i pošalje ih na dalju analizu da bi bila u stanju da pokrije što veću količinu igrača. Ovakav sistem pokazao je tačnost od preko 98%. Mane ovakvog sistema jeste što da bi se tačnost povećala potrebna je veća količina podataka za obučavanje neuronske mreže, i činjenica da pri obučavanju neuronske mreže koripćeni su podaci o partijama koje su igrali samo profesionalni igrači jer je takvim podacima mnogo lakše naći pristup. Isto tako botovi koji su bili analizirani bili su botovi koji su napravljeni da se takmiče protiv drugih botova a ne botovi koji trebaju da simuliraju običnog korisnika. Rezultati bi pri detekciji botova koji simuliraju ljude verovatno bili lošiji.

  Ako ukombinujemo prethodna dva članka najbolje unapredjenje neuronske mreže jeste da se detaljno obradi logovanje inputa korisnika i time napravi nova neuronska mreža sa podacima tih logova.

## Detekcija cheat-ovanja[^2] u video igrama



## Rečnik pojmova

[^1]: Korisnički input - delovanje korisnika u sistemu, unos komandi

[^2]: Cheating - Varanje u video igrama korišćenjem nedozvoljenig sredstava

[^3]: Antivirus - Tip software-a za zaštitu krajnjih korisnika, onemogućava napadaču da krajnjenm korisniku onesposobi računar, izvuče lične podatke i štiti korisnika na internetu.

[^4]: Delay - Vreme čekanja na odgovor servera

[^5]: Competitive igre - Takmičarske igre, igre koje zahtevaju veliku umešnost, reflekse i znanje o samoj igri

[^6]: Trading - Gest razmene In-game novca i itema izmedju igrača

[^7]: Party play - Oformljavanje grupe igrača koji zajedno igraju video igru zarad zajedničke dobiti

[^8]: Levelovanje - Unapredjenje virtuelnog lika korisnika radi sticanja moći

[^9]: In-game novac - virtuelni novac u igri

[^10]: MMORPG - Massible multiplayer online role playing game, igra koju igra masivan broj igrača i koji često mogu medjusobno interagovati u velikim grupama

[^11] Frame - sve akcije koje su primenjene u jednoj iteraciji prikaza na ekranu
