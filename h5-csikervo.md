# H5 - CSI Kerava

Tein tehtävää itsenäisesti kotonani sunnuntaina 26.11. Tein tehtävät HP pavilion kannettavallani, jossa on Intel i5-9300H prosessori, 16gt RAM, 512gt SSD ja Windows 10 home-käyttöjärjestelmä.

## x) Lue ja tiivistä

### Apache User Homepages Automatically – Salt Package-File-Service Example

```
-Vasta sen jälkeen kun osaa tehdä asiat käsin, voi lähteä automatisoimaan. (Karvinen, 2018)
-Komento $ find -printf "%T+ %p\n" purettuna:
%T+ kertoo muokkausajan.
%p kertoo tiedostonimen ja - polun.
\n printtaa vastaukset erillisille riveille.
-Salt tiloissa käytettävät komennot oltava idempotentteja!
```

## a) CSI Kerava

Tässä tehtävässä lähdin kokeilemaan ylemmässäkin kohdassa hieman purkamaani "find"-komentoa. Päätin tehdä tämän ihan yksittäisellä virtuaalikoneella, joten avasin Virtualboxin, josta valitsin aiemmissakin tehtävissä käyttämäni tutun ja turvallisin Debian 12-virtuaalikoneen. Kirjauduin koneelle, avasin terminalin ja siirryin Teron ohjeen mukaisesti /etc-kansioon suorittamaan komentoa. Siirtymiseen helppoakin helpompi `$ cd /etc`. 

Kansiossa ajan Teron ohjeessakin esitellyn komennon `$ find -printf "%T+ %p\n"|sort` . Selitin jo sulkeissa olevat parametrit ylemmässä tehtävässä, mutta lopuista sen verran että find etsii tiedostoja ja hakemistoja, -printf formatoi ja printtaa dataa, sekä sort järjestää printattavat rivit. (tässä tapauksessa näyttää alhaalla uusimmat muutokset)

Sitten tuo itse printti mikä komennolla tuli. Printattavia rivejä tuli todella paljon, mutta aikaleimoista huomataan että kirjoitushetkellä tapahtuvia muutoksia on vain 5 riviä. Ei mitään hajua mitä nuo ovat ja mitä tekevät, mutta googlettamalla saan tiedon että CUPS tarkoittaa Common Unix Print Serveriä, ja se on pääasiallinen printtausjärjestelmä useimmissa Linux distroissa (Ubuntu, s.a.)

Hyvin suurella varmuudella voin siis olettaa että myös Debian käyttää oletuksena tätä printtausjärjestelmää, jos se kerran heti käynnistyy ja en sitä ole itse koskaan erikseen asentanut :D

`Alla: Komennon ajaminen /etc-kansiossa.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/a152fd4a-5219-450c-8863-f252d0aca586)

---

Seuravaaksi ajan samaisen komennon myös kotihakemistossani. Siirryn sinne komennolla `$ cd/home/joonas` ja ajan komennon uudelleen. Printattavaa on taas erittäin paljon, ja silmään pistää Mozilla Firefoxin nimissä tapahtuvat lukuisat muutokset, mm. .cache-kansiossa. Firefox on itselläni päällä, sillä luen sieltä ohjeita. Ei siis ihme jos sen välimuisti on jatkuvasti käytössä. 

`Alla: Komennon ajaminen omassa kotihakemistossani.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/0e907b73-2796-44a5-b44e-28d0e04b288f)

---

## b) Gui2fs

Tässä tehtävässä lähdin tekemään muutoksia jollain graafisen käyttöliittymän sovelluksella, ja sen jälkeen katsomaan löydänkö tekemäni muutokset tiedostotasolla. Yritin alkuun muokata Weather-sovelluksessa näkyviä kaupunkeja, ja toivovani että kaupungit ilmestyisivät konfiguraatiotiedostoihin mutta lukuisten muokkausten ja ` $ find -printf "%T+ %p\n"|sort` -komentojen jälkeen eksyin /usr/bin/gnome-weather tiedostoon joka oli linkki toiseen hakemistoon osoitteessa ../share/org.gnome.Weather./org.gnome.Weather , josta taas löytyi erilaisia src.- ja data.gresource-tiedostoja, joita tutkiessa ne olivat täynnä JavaScript stringejä, jolloin luovutin ja valitsin jonkun helpomman sovelluksen.

Päädyin Contacts-sovellukseen, jonne pystyin itse lisätä omia yhteystietojani. Päätin lisätä sinne tämän kurssin mainion opettajan eli itse Teron! Avasin sovelluksen, ja valitsin plus-nappulasta "create new contact".

`Alla: Uuden yhteystiedon luominen.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/600a0bb7-1309-4613-b4b3-5e03fda65299)

---

Tämän jälkeen ajan jo aiemmin käytetyn komennon ` $ find -printf "%T+ %p\n"|sort` jotta näen viimeisimmät muutokset. Näen alimpana addressbookin sisältävän polun ja olen varma että muutokseni on tapahtunut siellä. 

`Alla: Komennon ajo, ja viimeisimmät muutokset.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/4f55f669-cb56-444a-b5b7-bae514e82288)

---

Siirryn siinä osoitettuun hakemistoon komennolla ` $ cd ./joonas/.local/share/evolution/addressbook/system` . Siellä ajan uudestaan komennon ` $ find -printf "%T+ %p\n"|sort` jolla toivon näkeväni että missä muutos on tapahtunut. Huomaan, että muutosta on tapahtunut contacts.db nimisessä tiedostossa, ja sieltä löydänkin varmasti tekemäni muutokset.

`Alla: find-komennon ajo uudestaan yhteystietojen hakemistossa.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/e6d735f2-11c4-4b82-be8b-11bceccbeb7c)

---

Seuraavaksi ajan komennon `$ cat concacts.db` , ja huomaankin heti alimpana että Teron tietoja on päivittynyt kyseiseen .db, eli database tiedostoon. Jes!

`Alla: Aiemmin lisäämäni Teron tiedot`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/1c92a823-cad6-4dc5-88a3-e8461a6a601b)

## c) Komennus

Lähdin tässä tehtävässä luomaan Salt-tilaa, joka ajaa järjestelmässä uuden komennon. Ajattelin tehdä simppelin komennon, joka tarkastaa onko nyt viikonloppu ja ilmoittaa jos on, ja jos ei ole. Olin vielä Debian 12-virtuaalikoneessa, jossa haluan ensin testata shell scriptiä ja nähdä saanko sitä toimimaan, ennen kuin siirryn Vagrantiin ja Saltiin. Luon uuden tiedoston komennolla `$ sudo nano onkovklp.sh` , ja syötän sinne seuraavan koodinpätkän, jonka löysin verkosta StackOverflow foorumilta käyttäjältä paxdiablo:

```
#!/bin/bash

if [[ $(date +%u) -gt 5 ]]; then echo "Se on viikonloppu!"

else echo "Ei oo viälä..." ;fi
```

(StackOverflow, 2010)

Scripti toimii niin, että date +%u antaa viikonpäivät numeropätkässä lukuina 1-7. -gt 5 puolestaan tarkoittaa "greater than 5" eli "suurempi kuin 5", ja tässä tilanteessa sillä haetaan lukuja 6 ja 7, eli lauantai ja sunnuntai. Jos scriptiä ajaessa on lauantai tai sunnuntai, se ilmoittaa echolla että on viikonloppu. Jos taas ei ole (else), se ilmoittaa että ei ole vielä.

`Alla: onkovklp.sh tiedoston muokkaus Debianissa:`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/e4b8132a-367d-4e66-a73e-ad15096703ef)

---

Annan tiedostolle ajo-oikeuden komennolla `$chmod +x onkovklp.sh` , ja ajan sen testatakseni toimivuutta.

`Alla: scriptin ajaminen onnistuneesti:`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/65b3b5fb-e227-445a-8031-e29f98581b7e)

---

Sitten vaan samaa mutta automatisoituna, eli hypätään Vagrantin ja Saltin puolelle. Avaan Windows-koneellani komentokehotteen, siirryn vagrant-hakemistooni ja ajan koodin `$ vagrant up` , jolla pistän koneet käyntiin ja hyppään herran selkään komennolla `$ vagrant ssh tmaster`.

Olen ottanut kuvan Teron tunnilla näyttämästä "morning"-esimerkistä ja muistan jotenkin miten homma toimi. 

Menen ensin /usr/local/bin-hakemistoon komennolla `$ cd /usr/local/bin`. Luon uuden scriptitiedoston komennolla `$nano onkovklp.sh` , johon syötän saman scriptipätkän kuin ylempänäkin. 



Tämän jälkeen yritän ajaa scriptin komennolla `$ onkovklp.sh` . Vastauksena tyly permission denied, unohtui meinaas muokata oikeudet. Korjataan tilanne komennolla `$ sudo chmod 755 onkovklp.sh` . Kokeilen uudestaan, ja nyt toimii!

Sitten siirryn /srv/salt-hakemistoon komennolla `$ cd /srv/salt` . Teen siellä uuden hakemiston komennolla `$ mkdir onkovklp` . Siirryn sinne, ja teen vielä samannimisen tiedoston komennolla `$ sudo nano onkovklp` . Lisään sinne jälleen samaisen scriptipätkän kuin ylempänäkin, ja tallennan sen ctrl + o ja ctrl + x. 

Tarvitaan vielä init.sls-tiedosto, joka kertoo orjalle scriptin sijainnin ja tarvittavat oikeudet. Luon sellaisen komennolla `$ sudoedit init.sls` , ja lisään sinne alla olevan koodinpätkän.

`Alla: Init.sls-tiedoston muokkaus:`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/914331b3-0c79-4cf0-b1af-5ec62b504637)

---

Tämän jälkeen testauksen pariin, ajan komennon `$ sudo salt 't001' state.apply onkovklp` , joka luo t001-orjalle /usr/local/bin-hakemistoon samaisen scriptitiedoston. Varmistetaan vielä komennolla `$ sudo salt 't001' cmd.run 'ls /usr/local/bin'` että tiedosto on varmasti siellä.

Näyttää olevan, joten lopuksi ajetaan se komennolla `$ sudo salt 't001' cmd.run 'onkovklp'` . Vastauksena saan tiedon, että on viikonloppu. Eli homma toimii!

`Alla: Ylläajetut komennot.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/97af6006-85b7-4f41-af2a-085fe7dfcf38)

---



## Lähteet

Karvinen, T. 3.4.2018. Apache User Homepages Automatically – Salt Package-File-Service Example. Luettavissa: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/. Luettu: 26.11.2023.

StackOverflow, käyttäjältä paxdiablo. 16.8.2010. How to check if today is a weekend in bash? Luettavissa: https://stackoverflow.com/questions/3490032/how-to-check-if-today-is-a-weekend-in-bash. Luettu: 26.11.2023.

Ubuntu, s.a. CUPS - Print Server. Luettavissa: https://ubuntu.com/server/docs/service-cups. Luettu: 26.11.2023.
