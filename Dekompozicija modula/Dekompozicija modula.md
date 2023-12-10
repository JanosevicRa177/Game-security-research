# Online Video Igra(Trke Automobila)

## Uvod

Posmatrani sistem predstavlja online multiplayer video igru za trke automobila. Sistem se sastoji od nekoliko glavnih komponenti: Klijentska aplikacija, Load Balancer, Game Manager, Baza Podataka, Distributer poruka i Kubernetes Cluster. U nastavku sledi detaljan opis najvažnijih komponenti.

## Arhitektura sistema

Sistem je implementiran preko monolitne arhitekture. Na nivou aplikacije, postoji jedna deljena baza podataka. Kako bi bilo omogućeno skaliranje aplikacije, ispred Game Manager-a je postavljen Load Balancer, koji služi za rutiranje saobraćaja do neke od instanci Game Manager-a. Game manager vrši orkestraciju podataka o igračima i igrama, te preko Message Brokera komunicira sa Kubernetes Clusterom. Kubernetes Cluster kreira instance Game Node-ova po potrebi.[1]

![Slika dekompozicije modula](https://github.com/JanosevicRa177/Game-security-research/blob/main/Dekompozicija%20modula/Dekompozicija%20modula.png)

## Autentifikacija

Autentifikacija sistema se obavlja preko eksternog Firebase sistema za autentifikaciju. Firebase kreira token za svaku korisničku sesiju, a zatim se token koristi za autorizaciju zahteva.[4]

## Klijentska Aplikacija

Klijentska aplikacija predstavlja esencijalno ono što korisnik vidi na svom ekranu. Aplikacija je implementirana koristeći Unity Game Engine i Golang. Golang je upotrebljen kako bi se poboljšale performanse, iako unity nudi već gotovo rešenje za serverski deo. Unity u ovom slučaju služi za prikaz korisničkog interfejsa(fizika, efekti...). [2]

## Game Manager

Game manager predstavlja jezgro čitavog sistema. Njegova uloga je da se brine o sesijama igre, zatim da povezuje igrače preko matchmaking servisa, kao i da omogući klijentskoj aplikaciji pristup svim navedenim podacima. Još jedna bitna uloga Game Manager-a je da obaveštava Kubernetes Cluster preko Kubernetes API-ja o tome kada je sesija počela ili završila. Kako bi se poboljšale performanse servera, svaka instanca Game Manager-a ima pristup Redis bazi podataka. Redis, u ovom slučaju, ima ulogu da kešira podatke i da služi kao red čekanja klijenata za pristup sesijama igre. Game Manager čuva podatke o sesijama u deljenoj PostgreSQL bazi podataka. Pored svih navedenih funkcionalnosti, Game Manager sadrži implementaciju WebSocket protokola, koji mu omogućava slanje notifikacija klijentu u realnom vremenu.[3]

## Load Balancer

Kao što je navedeno ranije u sekciji "Arhitektura sistema", ispred instanci Game Manager-a, postavljen je Load Balancer. Konkretno za ovaj slučaj, korišćen je Nginx[5]. Uloga Load Balancera je da omogući podjednaku distribuciju zahteva ka svim instancama aplikacije.[6]

## Kubernetes Cluster

Uloga Kubernetes Cluster-a je da instancira dedicated servere za svaku sesiju igre. Pristup kubernetesovom servisu se vrši preko Kubernetes API-ja.[1]
Svaki od kreiranih servera sadrži privatni RSA ključ. Da bi igrač pristupio serveru, potrebno je da preuzme javni ključ tog servera, a pravo na to dobija ukoliko je autentifikovan od strane Game Managera.[3]

## Message Broker

Komunikacija između Game Node-ova i Game Managera obavlja se preko Message Brokera. Razmena poruka se obavlja u vidu događaja na koje se obe strane pretplaćuju. Svaki put kada Game Manager primi ili pošalje događaj, on ga zapisuje u bazu podataka u vidu loga.[7]

# Reference

1. https://theredrad.medium.com/designing-a-distributed-system-for-an-online-multiplayer-game-architecture-part-3-f9483ebbe5ac
2. https://theredrad.medium.com/designing-a-distributed-system-for-an-online-multiplayer-game-game-client-part-6-4af9692cf59b
3. https://theredrad.medium.com/designing-a-distributed-system-for-an-online-multiplayer-game-game-server-part-5-1-17ef2c7471ac
4. https://theredrad.medium.com/designing-a-distributed-system-for-an-online-multiplayer-game-game-manager-part-4-e3ffa4febc02
5. http://nginx.org/en/docs/http/load_balancing.html
6. https://aws.amazon.com/what-is/load-balancing/

7. https://theredrad.medium.com/designing-a-distributed-system-for-an-online-multiplayer-game-game-server-game-server-part-9ed8fdb5063b

Uopšteni video o dekompoziciji modula za video igre bez previše detalja, da bi se korisnik stekao širu sliku o domenu arhitekture online video igara:
https://www.youtube.com/watch?v=77vYKsXC4IE

Detaljan članak o modelovanju online video igara, sadrži detaljanu dekompoziciju modula i prikaz arhitekture sistema za online video igre:
https://theredrad.medium.com/designing-a-distributed-system-for-an-online-multiplayer-game-basics-part-1-17c149245bd2
