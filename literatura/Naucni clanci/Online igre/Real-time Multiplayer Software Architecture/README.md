## Opis članka

  Članak obradjuje tematiku osnovne arhitekture video igara gde opisuje prednosti i mane pristupa multiplayer igrama [1]. Takodje dublje zalazi u sam prenos korisnčkih input-a [2] i kako zapravo izgleda komunikacija tokom igre izmedju klijenta odnosno korisnika i game servera [3].

## Arhitekture video igara

Pristupi arhitekturi su peer-to-peer arhitektura i client-server arhitektura, svaki od pristupa ima svoje prednosti i mane. Koje će se spomenuti u detaljnom opisu svake od arhitektura.

### Peer-to-peer arhitektura

  Ne postoji server, svaki klijent komunicira sa svakim klijentom i šalje pakete informacija. Klijent ima ulogu i klijenta i servera i samim tim se otežava sinhronizacija podataka izmedju klijenata. Može biti poprilično nezgodan pristup ukoliko ima puno igrača jer je ovaj model teško skalabilan, ali ima svoju primenu kod multiplayer igara sa manje igrača, jer nema jedan point of failure [4]. Isto tako ova arhitektura se teško štiti od napada malicioznih korisnika [6] jer ne postoji telo koje će validirati input-e korisnika.

### Client-server arhitektura

  Podrazumeva da postoje više klijenata i jedan server, češći pristup pravljenja multiplayer igara sa velikom količinom korisnika. Server čuva pravi state igre [5] dok klijenti čuvaju kopije i ažuriraju ih po potrebi. Problem je što igra ima jedan point of failure odnosno ako server padne, svim igračima će igra biti prekinuta. Sa ovakvim pristupom dosta napada malicioznih korisnika se automatski eliminiše jer je glavni state igre na samom serveru. A korisnici mogu samo svoje stanje da ažuriraju slanjem paketa informacija koji sadrže samo njihove izmene, a neretko te izmene su u vidu signala sa pomeraj ili pucanje iz puške, time se smanjuje količina informacija koje korisnik šalje na server i smanjuje se šansa da nekom svojim input-om naruši autoritet igre.

U daljem tekstu više je opisana client-server arhitektura jer je danas zastupljenija i sigurnija.

## Razlika u state-u klijenta i servera

  Ranije dok još nisu bile zastupljene online video igre, state igre se čuvao na samom računaru odnosno konzoli, i ažurirao se na odredjeni vremenski period. Ranije je to bilo 30 puta u sekundi a kasnije sa razvojem jačih konzola i računara ažuriranje state-a igre je skočilo na preko 100 puta u sekundi. Serveri sobzirom da moraju da obradjuju mnogo klijenata ne mogu da ažuriraju svoje stanje toliko puta u sekundi, gde je do skoro bio primer da u igri Counter Strike: Global offensive, igrači su ažurirali stanje igre preko 100 puta u sekundi dok su serveri ažurirali samo 60 puta u sekundi. Pa je dolazilo do neslaganja onoga što korisnik vidi i što se dešava na serveru. Naziv za ažuriranje state-a video igre na serveru se još poznato zove tick rate [7]. Poznato je da igre koje imaju veliku količinu igrača moraju raditi na nižem tick rate-u da ne bi došlo do opterećenja sistema, ali isto tako competitive igre [8] moraju raditi na većem tick rate-u zvog brzog odziva korisnika. Zato što klijent mnogo brže ažurira state igre, pri dobavljanju state-a igre sa servera izmedju iteracija se radi interpolacija state-a igre zbog fluidnosti igre.

## Načini komunikacije u video igrama

  Sa razvitkom online igara pojavio se problem latency-a, što predstavlja vreme za koje će korisnički input doći do servera, server obraditi korisnički input i vratiti mu povratnu informaciju u visu novog state-a igre. Ovaj proces se naziva Round trip time odnosno skraćeno RTT [9]. Da bi se poboljšao odziv, server i klijent neretko dele isti ili sličan kod za ažuriranje stanja determinističkih inputa korisnika.

## Načini komunikacije - protokoli

  Da bi se prenosili input-i korisnika mora se odabrati protokol preko koga će input-i putovati od klijenta ka serveru i obrnuto. Protokoli koji mogu biti pogodni za prenos podataka us UDP i TCP. TCP garantuje prenos podataka ali povećava vreme odziva, može biti pogodan ukoliko je neophodno da input sigurno stigne do servera, pogodan za Turn based igre [10]. UDP ima manje vreme odziva ali ne garantuje da će input stići do servera. 

## Načini rešavanja gubljenja podataka tokom UDP protokola

### Slanje više poslednjih paketa input-a

  Sistem po kome se uz najnoviji input, šalje i nekoliko prethodnih u slučaju da neki paket nije stigao, Na osnovu toga server će znati koje pakete input-a treba da primeni na state igre. Problem nastaje što se redudantno šalju input-i koji su verovatno već primenjeni.

### Keširanje par paketa input-a

  Klijent kešira par paketa input-a kod sebe i šalje najnoviji paket input-a zajedno sa informacijom da nije dobio odziv za odredjene pakete. Server mu zatim odgovara ukoliko neki paket fali ili mu javlja da je paket uspešno obradjen, nakon toga klijent šalje pakete koji fale na serverskoj strani. Mana je što se ne poštuje vreme primene input-a a neki inputi se moraju obraditi sekvencijalno.
  

## Rečnik pojmova

[1] Game server - Server na kom se obavlja sinhronizacija svih klijenata koji su u igri i potencijalno obradjuje dodatna logika.
[2] Korisnički input - delovanje korisnika u sistemu, unos komandi
[3] Multiplayer igra - Igra u kojoj učestvuje više od jednog igrača
[4] Point of failure - Mesto u kom ako program pukne ceo sistem prestaje da radi
[5] State igre - stanje u kom se igra nalazi, pozicije igrača, stvari koje oni poseduju i slično
[6] Maliciozni korisnik - Korisnik koji želi da naruši autoritet igre zbog ličnog dobitka
[7] Tick rate - Vreme ažuriranja state-a game servera
[8] Competitive igre - Takmičarske igre, igre koje zahtevaju veliku umešnost, reflekse i znanje o samoj igri
[9] Round trip time(RTT) - jedna iteracija komunikacije klijenta i servera
[10] Turn based igre - Igre u kojima korisnici sekvencijalno reaguju na igru a ne u isto vreme
