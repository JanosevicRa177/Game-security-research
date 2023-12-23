#Analiza input-a korisnika [^1]

## Uvod

Ovaj odeljak će se fokusirati na koji način napadač može da naruši integritet naše igre analizom i probijanjem konekcije izmedju klijenta i servera tokom igre. 

Fokusirani diagram sa dekompozicije modula se može videti ovde:

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Analiza%20pretnji%20kroz%20analizu%20inputa%20korisnika/Slike/fokus%20napada1.png" />
</p>

<p align="center">
  Fokus sa dekompozicije modula
</p>

Lepši prikaz samog fokusa napada može se videti na drugoj slici gde su uklonjeni nebitni podaci i dodata površina za napad kao i resursi koji napadaču mogu biti primamljivi za napad.

<p align="center">
  <img src="https://github.com/JanosevicRa177/Game-security-research/blob/main/literatura/Naucni%20clanci/Online%20igre/Analiza%20pretnji%20kroz%20analizu%20inputa%20korisnika/Slike/fokus%20napada2.png" />
</p>

<p align="center">
  Diagram polja za napad i zumirani prikaz na deo za analizu input-a korisnika
</p>

## Dekompozicija pretnji,

Ovakav deo sistema u video igrama izuzetno je bitno analizirati jer predstavlja veći deo interakcije sa korisnikom. Korisniku se za to vreme mora omogućiti da se oseća prijatno dok je u igri i sigurno. Načini na koje napadač može eksploatisati ovakav sistem su razni. Da li da prikupi neke resurse u igri, stekne prednost u odnosu na druge korisnike u vidu nedozvoljenog znanja ili u vidu pripomaganja u competitive igrama[^2]. Na sledećem diagramu biće prikazan opširan pogled na pretnje i napade ove površine za napad (površina za napad je u ovom slučaju sama razmena podataka izmedju servera i klijenta)


[^1]: Korisnički input - delovanje korisnika u sistemu, unos komandi

[^2]: Competitive igre - Takmičarske igre, igre koje zahtevaju veliku umešnost, reflekse i znanje o samoj igri
