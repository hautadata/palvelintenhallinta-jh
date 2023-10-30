# H1 – Viisikko

Tein tehtävää osittain yhdessä Thomas Helmisen kanssa koululla torstaina ja perjantaina 26. & 27.10.2023. Viimeistelin tehtävän itse maanantaina 30.10.2023. Asensin Debian 12-käyttöjärjestelmän virtuaalikoneelle Oracle VM Virtualboxissa omalla HP Pavilion Gaming Laptop 15-kannettavallani. Käytin asennuksessa auttamiseen Teron sivua _“Install Debian on Virtualbox - Updated 2023”_. 

## x) Lue ja tiivistä

Alla tiivistettyinä pääasiat Teron artikkeleista.

### Create a Web Page Using Github

- Aloittelijaystävällinen opas web-sivujen luomiseen GitHubissa. 

- Repositoryn ja tiedoston luominen, MUISTA Readme-file. 

- MarkDownilla kirjoittaminen, otsikointi # = H1, ## = H2 jne. 

- “Paras päivä julkaista on eilen, toiseksi paras päivä on tänään” (Karvinen, 2023)
  

### Run Salt Command Locally

- Salt komentojen ajo paikallisesti = näet tuloksen heti.

- Tärkeimmät tilafunktiot ovat pkg, file, service, user & cmd.

- Selkeät esimerkit yllämainituista, ja etenkin idempotentista.

- Linuxissa kaikki asetukset ovat vain tekstitiedostoja. (Karvinen, 2021)

## a) Saltin asennus

Asensin Saltin terminalissa _"Infra as Code 2023"_-sivulta löytyvillä Debian 12-ohjeilla.

Aluksi ajoin komennon ``$ sudo mkdir /etc/apt/keyrings`` Seuraavaksi kokeilin komentoa ``$ sudo curl -fsSL -o /etc/apt/keyrings/salt-archive-keyring-2023.gpg https://repo.saltproject.io/salt/py3/debian/11/amd64/SALT-PROJECT-GPG-PUBKEY-2023.gpg`` (Karvinen, 2023), mutta curl-komento ei toiminut itselläni, koska en ollut asentanut curlia eikä se ollut vakiona.

Asensin curlin komennolla ``$ sudo apt-get install curl``, jonka jälkeen ajoin saman komennon ja pääsin eteenpäin.

Sen jälkeen komento ``$ echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring-2023.gpg arch=amd64] https://repo.saltproject.io/salt/py3/debian/11/amd64/latest bullseye main" | sudo tee /etc/apt/sources.list.d/salt.list``

Viimeisenä perinteinen ``$ sudo apt-get update``, ja itse Salt-minionin asennus komennolla ``$ sudo apt-get install salt-minion``

## b) Viisi tärkeintä tilafunktiota

Kaikissa allaolevissa funktioissa käytettynä apuna Teron materiaaleja sivulta _"Run Salt Command Locally"_. 

### pkg (package)

pkg-tilafunktiolla hallitaan ohjelmistopaketteja Saltilla. Sillä voi mm. asentaa, poistaa, muokata ja suojata paketteja. (Salt Project, s.a.) Käytin funktiota asentamaan apache2:n omalle palvelimelleni.

****Alla:**** Apache2-asennus komennolla ``$ sudo salt-call --local -l info state.single pkg.installed apache2`` . Syntaksi toimii tässä, ja jokaisessa muussa funktiossa niin, että funktiolla tarkistetaan jokin arvo, tässä tapauksessa se, että onko apache2 asennettuna. Koska sitä ei oltu asennettu, komennon ajettuani se asentuu. (Kohdassa Succeeded: 1 (Changed=1) näkyy että komento on muuttanut jotain.) Jos taas apache2 olisi ollut jo asennettuna, komennolla ei olisi tapahtunut muutoksia. 

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/c78be0fb-e856-4861-b958-c39610476851)

---

### file
file-tilafunktiolla voi mm. luoda, tarkistaa ja muokata tiedostoja. Käytin funktiota luomaan itselleni uuden tiedoston haluamaani hakemistoon, jonka jälkeen muokkasin vielä tiedoston sisältöä lisäämällä sinne haluamaani tekstiä. 

****Alla:**** Oman tiedoston muokkausta. Komennolla ``$ sudo salt-call --local -l info state.single file.managed /home/joonas/morojoonas contents="Tämä…"`` lisäsin sisältöä tiedostooni morojoonas, joka sijaitsee kotihakemistossani /home/joonas. Tarkistin vielä muutoksen komennolla ``$ cat morojoonas`` , ja hyvin näytti toimivan!

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/741930c8-8e9d-4a02-be7c-0479defa35c2)

---

### service

service-tilafunktiossa voidaan hallita daemon-palveluita. Niitä voidaan mm- käynnistää ja sammuttaa funktiolla helposti. pkg-kohdassa tuli jo sooloiltua ja ladattua apache2, joten sivulla näkyvä ensimmäinen service.running funktio ei muuttanut mitään, sillä palvelu oli jo käynnissä. Käytin seuraavaa funktiota sammuttamaan palvelun.

****Alla:**** Komennolla ``$ sudo salt-call --local -l info state.single service.dead apache2 enable=False`` sammutin palvelun käytöstä. 

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/ae5132b9-a74a-44f1-a573-8977a0508bb3)

---

### user
user-tilafunktiolla voidaan hallita käyttäjiä, mm. luomalla ja poistamalla niitä. Käytin Teron ohjeessa näkyviä funktioita, joilla loin ja poistin tekemäni käyttäjän nimeltä vierailija.

****Alla:**** Poistin käyttäjäni vierailijan kommenolla ``$ sudo salt-call --local -l info state.single user.absent vierailija``. Raukalle oli vain hetkeä aikaisemmin annettu mahdollisuus vaikuttaa palvelimellani komennolla ``$ sudo salt-call --local -l info state.single user.present vierailija``.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/4d27c18f-7ac8-4100-a3b0-560257a8baf5)

---

### cmd
cmd-tilafunktiolla ajetaan komentoja minioineilla. 

****Alla:**** Ajoin komennon ``$ sudo salt-call --local -l info state.single cmd.run 'touch /home/joonas/moro' creates="/home/joonas/moro"`` joka ensin tarkistaa onko kyseistä tiedostoa olemassa. Koska sitä ei ole, se tunnistaa tämän ja luo kyseisen tiedoston oikeaan hakemistoon toteuttamalla tilafunktion. Kun ajoin komennon uudelleen, mitään ei enää muuttunut. Tässä siis malliesimerkki idempotentista.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/1d5673e6-620c-4624-bcca-2fe514b82eb4)

## c) Idempotentti

_“Yleinen määritelmä on, että tilan tulisi olla idempotentti. Riippumatta siitä kuinka monta kertaa sen suorittaa, sen pitäisi tuoda järjestelmä aina takaisin samaan tilaan.”_ (Klundert, 2020)

Idempotentti toimii siis niin, että järjestelmä tarkistaa tilan, ja tekee muutoksia vain jos tilan “ehdot” eivät vielä täyty, eli esimerkkinä tässä idempotentti tarkistaa, onko tiedostoa vielä luotu. Jos ei, se luo sellaisen. Käytin inspiraationa jo “viisi tärkeintä”-kohdassa ajettua “touch, creates…”-syntaksia, mutta muutin sitä niin että tilafunktio tekee kopion omasta tiedostostani, ja tarkistaa onko sellainen olemassa.

****Alla:**** Ensimmäisessä kuvassa ajan ensimmäistä kertaa komennon. ``$ sudo salt-call --local -l info state.single cmd.run 'cp /home/joonas/moro /home/joonas/morokopio' creates="/home/joonas/morokopio"``
Yhteenvedosta (summary) näkee että tiedosto on tällä komennolla luotu (changed = 1).

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/c17f5a94-a01f-4d7a-9e32-2e9a3dc54bf6)

Jos taas ajan saman komennon uudestaan, yhteenvedossa näkyy että mitään ei ole muuttunut. Hieman ylempänä comment-kohta kertoo että tiedosto on jo olemassa:

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/a65a0f89-12a3-44d4-9e6e-b29f5501f31b)

## d) Tietoa koneesta

Hain tietoa koneestani _"Infra as Code 2023-sivulta löytyvillä ohjeilla"_. Käytin komentoa ``$ sudo salt-call --local grains.items``, jolla sain pitkän listan koneen spekseistä.

Mielenkiintoisena silmääni pisti heti biosreleasedate, joka näytti vuoden 2006 päivämäärää. Virtuaalikoneen BIOS-versio on siis ilmeisesti siltä vuodelta. Muita mielenkiintoisia kohtia oli tarkka kernel-versio, ja sen julkaisupäivämäärä, sekä nykyinen salt-versio (Joka on 3006.3).

**Alla:** biosreleasedate

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/f40cf4df-5875-48a5-8da4-460fec67847a)

**Alla:** kernelversion

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/9d0cf855-8319-4381-8807-9efe706e624a)

## Lähteet)

Karvinen, T. 2023. Infra as code. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/. Luettu: 26.10.2023.

Karvinen, T. 2023. Install Debian on Virtualbox - Updated 2023. Luettavissa: https://terokarvinen.com/2021/install-debian-on-virtualbox/. Luettu: 26.10.2023.

Karvinen, T. 2021. Run Salt Command Locally. Luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/. Luettu: 26.10.2023.

Klundert, S. 20.3.2020. SaltStack overview. Luettavissa: https://saidvandeklundert.net/2020-03-20-saltstack-overview/. Luettu: 30.10.2023.

Salt Project. SALT.STATES.PKG. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/all/salt.states.pkg.html. Luettu: 27.10.2023.

---
Kiitos,

Joonas H





 

