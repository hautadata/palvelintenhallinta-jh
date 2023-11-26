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



## Lähteet

Karvinen, T. 3.4.2018. Apache User Homepages Automatically – Salt Package-File-Service Example. Luettavissa: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/. Luettu: 26.11.2023.

Ubuntu, s.a. CUPS - Print Server. Luettavissa: https://ubuntu.com/server/docs/service-cups. Luettu: 26.11.2023.
