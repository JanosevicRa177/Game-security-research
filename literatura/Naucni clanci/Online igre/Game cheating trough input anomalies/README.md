# Istraživanje anomalija input-a korisnika[1] za prevenciju varanja u video igrama

## Pronalazak botova u video igrama

### Online game bot detection based on party-play log analysis

  Članak prokiva pronalaženje botova u video igrama po sistemu analize logova na serveru. Svaki input korisnika se analizira i pamti u odredjenim logovima na serveru, Predloženi načini detekcije botova su bili i detekcije sa klijentske strane, detekcije na mreži i detekcije na serverskoj strani

  Na klijentskoj strani uglavnom nastaje problem što se dosta takvog software-a za detekciju botova sukobljava sa antivirus software-om [3] kojem je cilj da zaštiti krajnjeg korisnika. Time može izazvati neprijatnosti krajnjem korisniku.

  Detekcija na mreži koja se realizuje kroz monitoring mreže može biti pogodna za analizu detekcije botova ali uvodi dodatan delay [4] koji može biti poprilično nezgodan ako su u pitanju competitive igre [5].

  Zbog ovoga je najveći fokus na detekciji na samom serveru, koja se i obradjuje u članku. Server za svaki input korisnika beleži logove. Ti logovi imaju razne kategorije u vidu kom tipu interakcije input pripada. Da li korisnik odnosno mogući bot ubija čudovišta, priča sa drugim igračima, trade-uje [6] se, skuplja resurse, završava misije i slično.

  Dati članak se fokusirao na analizu logova korisnika koji su bili u party play-u [7] time su limitirali količinu logova za analizu na samo taj aspekat zajedničke igre, Nakon analize utvrdili su da vreme koje korisnici provedu u party play-u dirketno ima korelaciju sa time koje su šanse da je korisnik bot. Ukoliko su korisnici proveli 12 ili 24 sata u party play-u šanse da je u pitanju bot su daleko veće. Isto tako analizom vrste logova primetilo se da botovi ostavljaju daleko veću količinu logova za levelovanje [8] likova, prikupljanje resursa i trade-ovanje, dok su logovi običnih korisnika ravnomerno rasporedjeni. Time se zaključilo da je glavni cilj botova da leveluju naloge i prikupljaju In-game novac [9]. Isto tako primećuje se da po većoj dužini trajanja party play-a broj igrača se svodi na dva u većoj količini. Razlog toga je što botovi uglavnom idu u paru, jedan stražari dok drugi prikuplja resurse.

  Ovakav sistem za detekciju botova daje preciznost od 96% tačnosti.

### Efficient Deep Learning Bot Detection in Games Using Time Windows and Long Short-Term Memory (LSTM)


## Detekcija cheat-ovanja u video igrama



## Rečnik pojmova

[1] Korisnički input - delovanje korisnika u sistemu, unos komandi

[2] Cheating - Varanje u video igrama korišćenjem nedozvoljenig sredstava

[3] Antivirus - Tip software-a za zaštitu krajnjih korisnika, onemogućava napadaču da krajnjenm korisniku onesposobi računar, izvuče lične podatke i štiti korisnika na internetu.

[4] Delay - Vreme čekanja na odgovor servera

[5] Competitive igre - Takmičarske igre, igre koje zahtevaju veliku umešnost, reflekse i znanje o samoj igri

[6] Trading - Gest razmene In-game novca i itema izmedju igrača

[7] Party play - Oformljavanje grupe igrača koji zajedno igraju video igru zarad zajedničke dobiti

[8] Levelovanje - Unapredjenje virtuelnog lika korisnika radi sticanja moći

[9] In-game novac - virtuelni novac u igri
