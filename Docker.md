---
title: Docker-Task-14
updated: 2023-11-04 15:21:27Z
created: 2023-06-28 02:58:42Z

---

# Docker - Task-14

# Modernizacija aplikacija koristeci AWS i Docker

![1](./1.png)

* * *
## Uvod

* * *

### Šta je kontejner

- Kontejner je standardna jedinica softvera koja pakira kod i sve njegove ovisnosti kako bi se aplikacije mogle brzo i pouzdano izvoditi iz jednog računalnog okruženja u drugo. 
- Slika kontejnera je lagani, samostalni, izvršni paket softvera koji uključuje sve što je potrebno za pokretanje aplikacije: kod, vrijeme izvođenja, sistemske alate, sistemske biblioteke i postavke.
- To omogućuje razvojnim programerima da budu produktivniji jer umjesto da se moraju brinuti o raznim ovisnostima koje vaši kolege možda koriste za pokretanje svog koda, oni mogu jednostavno spakirati sav svoj kod I njegove ovisnosti u kontejner, tako da imate sve što vam je potrebno da ostanete produktivni.

### Koja je razlika između virtualne mašine (VM) i Dockera

- **Kontejneri virtaliziraju Operativni sustav (OS) umjesto hardware-a.**
- Kontejneri su mnogo više portabilni i efikasni.

### Virtualne mašine


- Virtualni strojevi (VM) su apstrakcija fizičkog hardvera koji pretvara jedan poslužitelj u više poslužitelja. Hipervizor omogućuje rad više VM-ova na jednom stroju. Svaki VM uključuje punu kopiju operativnog sustava, aplikacije, potrebne binarne datoteke i biblioteke - koje zauzimaju desetke GB. VM također mogu biti spori za pokretanje. Donji dijagram daje vizualni prikaz kako bi izgledalo pokretanje virtualnog stroja

![2](./2.png)

### Kontejneri

- Kontejneri su apstrakcija na sloju aplikacije koja zajedno pakira kod i zavisnosti. Više kontejnera može raditi na istom stroju i dijeliti jezgro OS-a s drugim kontejnerima, od kojih svaki radi kao izolirani procesi u korisničkom prostoru. Kontejneri zauzimaju manje prostora od VM-a (slike kontejnera su obično veličine desetine MB), mogu da obrađuju više aplikacija i zahtevaju manje VM-ova i operativnih sistema.

![3](./3.png)

* * *
# Docker Engine

* * *

- Docker Engine omogućava kontejnerskim aplikacijama da dosljedno rade bilo gdje na bilo kojoj infrastrukturi, rješavajući probleme ovisnosti za programere i operativne timove i eliminirajući "radi na mom laptopu!" problem. 
- Docker Engine obuhvata kompletan skup funkcija i alata koji pomažu korisnicima da budu što efikasniji kada rade sa kontejnerima. 
- Tri ključne karakteristike i mogućnosti koje pokreće Docker Engine mogu se sažeti na sljedeći način:

1. Pokreće ga containerd koji je standardno radno vrijeme kontejnera s naglaskom na jednostavnosti, robusnosti i prenosivosti

2. Integracija sa Buildkit-om koji je Dockers najčešće korištena karakteristika Docker Engine-a. Docker Buildkit se prvenstveno koristi za pravljenje slika iz Dockerfile-a 

3. Jednostavnost i pristupačnost Docker CLI-a 


![4](./4.png)


- Ispod haube, Docker Engine je klijent-server aplikacija sa sljedećim glavnim komponentama:

1. Server koji je tip dugotrajnog programa koji se naziva deamon proces (dockerd komanda).

2. REST API koji specificira interfejse koje programi mogu koristiti da razgovaraju sa deamonom i daju mu uputstva šta da radi.

3. Klijent interfejsa komandne linije (CLI) (docker komanda).

- CLI koristi Docker REST API za kontrolu ili interakciju s Docker demonom putem skriptiranja ili direktnih CLI komandi. Mnoge druge Docker aplikacije koriste osnovni API i CLI.

- Daemon kreira i upravlja Docker objektima, kao što su slike, kontejneri, mreže i volumeni.

* * *

# DOCKER SLIKE

* * *
- Kontejner slika je predložak je samo za čitanje s uputama za izradu spremnika. 
- Slika sadrži kôd koji će se izvoditi, uključujući sve definicije za sve biblioteke i ovisnosti koje vaš kôd treba izvoditi. 
- Često se slika temelji na drugoj slici uz neke dodatne prilagodbe. 
- Važno je napomenuti da ove slike nisu samo jedan monolitni blok, već se sastoje od mnogo slojeva. 


## Dockerfile i docker slika

- Sada kada razumijemo opće koncepte o tome što je slika spremnika, zaronimo u to kako se to odnosi na Docker slike koje se također mogu nazivati Dockerfile.
-  Baš kao i slika kontejnera koju smo spomenuli gore, Dockerfiles i Docker slike također pružaju prijenosno okruženje za izvršavanje aplikacija, pakiraju aplikacije i ovisnosti u jedan, nepromjenjivi artefakt, daju korisnicima mogućnost pokretanja različitih verzija aplikacija istovremeno i daju brže cikluse razvoja i implementacije.

```
FROM ubuntu:15.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

Ova Docker datoteka sadrži četiri naredbe, od kojih svaka stvara sloj. Izjava FROM počinje stvaranjem sloja iz ubuntu:15.04 slike. Naredba COPY dodaje neke datoteke iz trenutnog direktorija vašeg Docker klijenta. Naredba RUN gradi vašu aplikaciju pomoću naredbe make. Konačno, posljednji sloj određuje koju naredbu treba pokrenuti unutar spremnika.

Svaki sloj samo je skup razlika u odnosu na sloj prije njega. Slojevi se slažu jedan na drugi. Kada stvorite novi kontejner, dodajete novi sloj za pisanje na vrh donjih slojeva. Ovaj sloj se često naziva "sloj kontejnera". Sve promjene napravljene na kontejneru koji radi, kao što je pisanje novih datoteka, modificiranje postojećih datoteka i brisanje datoteka, zapisuju se u ovaj tanki sloj kontejnera za pisanje. Donji dijagram prikazuje spremnik temeljen na Ubuntu 15.04 slici.

![5](./5.png)


## Sažetak

Docker datoteke su izvor istine kada se radi o izgradnji i pokretanju Docker slika. Dockerfile je artefakt koji vi stvorite, a koji zauzvrat proizvodi Docker sliku kada je izgradite. Dockerfile uključuje upute o tome kako točno izraditi tu sliku i što ta slika treba sadržavati. Sjajan način razmišljanja o Dockerfileu je da se ne razlikuje od praćenja uputa za kuhanje vašeg omiljenog obroka. Docker datoteke uključuju korak po korak upute o tome kako treba izgraditi sliku vašeg spremnika. Primjeri mogu biti izvlačenje određene verzije Node.js ili mogu uključivati upute za instaliranje programa kao što je jq tako da kada pokrenete kontejner imate sve što vam je potrebno i uklanja potrebu za ručnim instaliranjem svih alata ili ovisnosti koje biste trebali pokrenuti spremnik ispravno.

Nakon što izgradite svoju Docker sliku prema uputama navedenim u Docker datoteci, sljedeći korak je gurnuti tu sliku u spremište slika kao što je Docker Hub  i tada imate izbor pokretanja tog kontejnera lokalno da se vidi da radi ili implementacije te slike na Amazon ECS 

U ovom odjeljku opisali smo što je Dockerfile i kako se koristi za stvaranje slika kontejnera. 

* * *
# DOCKER HUB
* * *

## Kontejner Registriji

### Šta je Docker Hub?

- Docker Hub najveće je svjetsko spremište slika kontejnera s nizom izvora sadržaja uključujući programere zajednice kontejnera, projekte otvorenog koda i neovisne dobavljače softvera (ISV) koji izgrađuju i distribuiraju svoj kod u kontejnerima.               
- Korisnici dobivaju pristup besplatnim javnim spremištima za pohranjivanje i dijeljenje slika ili mogu odabrati plan pretplate za privatne repozitorije.

### Zašto biste trebali koristiti Docker Hub za svoje organizacijske potrebe?

- Docker Hub izvrstan je izbor za dijeljenje slika kontejnera u vašoj organizaciji. 
- Docker Hub pruža jedinstveno iskustvo za pronalaženje, pohranjivanje i dijeljenje slika kontejnera.                                                 
- Milijuni pojedinačnih korisnika i više od sto tisuća organizacija koriste Docker Hub za svoje potrebe sadržaja spremnika.  
- Docker Hub osmišljen je kako bi pomogao spojiti sve značajke koje su se pokazale uspješnima u organizacijama svih veličina i razlog je zašto koristimo Docker Hub za ovu radionicu. Evo nekih od prednosti koje korisnici imaju korištenjem Docker Huba:

1. Kada koristite repozitorije unutar Docker Hub-a, možete pogledati nedavno dodane oznake i automatizirane nadogradnje na svojoj stranici repozitorija i jednostavno filtrirati slike kao što je prikazano u nastavku:

![6](./6.png)


