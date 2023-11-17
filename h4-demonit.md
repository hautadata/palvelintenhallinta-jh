# H4 - Demonit 

Tein tehtävää itsenäisesti reissussa perjantaina 17.11. Tein tehtävät HP pavilion kannettavallani, jossa on Intel i5-9300H prosessori, 16gt RAM, 512gt SSD ja Windows 10 home-käyttöjärjestelmä.

## x) Lue ja tiivistä.

### Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves

```
-Init.sls on YAML-syntaksinen tiedosto, joka toteuttaa "Infra koodina"-komentojen ajoa.
-Sisennys, 2 välilyöntiä ja EI tabia!
-Top.sls-tiedosto määrittää mitä tiloja mikäkin orja suorittaa.
-State.apply-tilassa ei tarvitse enää erikseen nimetä mitään moduuleja. 
```

### Salt contributors: Salt overview

```
-Vaihees
```

## a) Hello SLS!

Lähdin tekemään tehtävää kirjautumalla H2 - karjaa tehtävässä luodulle herra-orja-arkkitehtuurille, ja itse herralle tietenkin. Avasin Windowsilla komentokehotteeni, siirryin oikeaan hakemistoon komennolla `$ mkdir vagrant_joonas` , jossa pistin Vagrantin pyörimään komennolla `$ vagrant up`. Tämän jälkeen otin yhteyden herrakoneelle komennolla `$ vagrant ssh tmaster` . 

Siirryin aiemmassa tehtävässä tehtyyn /srv/salt/hello-hakemistoon komennolla `$ cd /srv/salt/hello/` , jossa lähdin muokkaamaan jo valmiiksi löytynyttä init.sls-tiedostoa komennolla `$ sudo nano init.sls` . Siellä oli vielä h2-tehtävän infraa koodina-YAML:ia joten kumitin ne ja lisäsin oman koodinpätkäni seuraavanlaisesti:

```
heimaailma:
  cmd.run:
    - name: echo "Hei maailma!"
```

`Alla: Kuvassa hei maailma tiedoston muokkaus.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/a539158e-5d60-4144-9896-5d4adb6bf087)

---

Tallensin tiedoston komennoilla ctrl + o ja ctrl + x, jonka jälkeen lähdin testaamaan tilan toimintaa! Ajoin komennon `$ sudo salt '*' state.apply hello` , ja luulen onnistuneeni tässä! Sain molemmilta orjilta palautteen onnistuneesta cmd.run ajosta, jossa stdout printtasi "Hei maailma! ja succeeded-kohdassa oli "changed= 1".

`Alla: Onnistunut komennon ajo herrakoneelta, jota molemmat orjat tottelivat!`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/7d8b1304-2e4f-4a10-b0b9-150e026fda37)

---

## b) Top.

Tässä tehtävässä lähdin tekemään tiedostoa top.sls, joka ajaa tilat automaattisesti. Tämä tehtävä oli tuttu jo h2:sta, joten osasin tehdä tämän helposti. Aloin muokkaamaan top.sls-tiedostoa komennolla `$ sudo nano /srv/salt/top.sls` , joka avasi tiedoston. Varmistin että siellä on oikea koodinpätkä, eli haluamani:

```
base:
  '*':
    - hello
```

`Alla: Tiedoston muokkausta.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/23df4a1a-44e1-495f-9ec4-fd42f620e67a)

---

Tämän jälkeen suljin editorin jälleen komennoilla ctrl + o ja ctrl + x, ja lähdin ajamaan tilaa! Tein tämän komennolla `$ sudo salt '*' state.apply` , jossa meidän ei nyt siis ylläolevan top.sls-tiedoston mukaisesti tarvitse kutsua erikseen minkään nimistä moduulia.

`Alla: Tilan ajaminen onnistuneesti pelkällä state.apply-tilalla.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/5c23b885-39fd-4f99-89ed-2f4c6ad38827)

--- 

Mielenkiintoisena tässä huomasin sen, että succeeded-kohdassa näkyy että olemme tehneet taas muutoksen (changed= 1), vaikka olemme ajaneet juuri aiemmin täysin saman tilan. Uskoisin tämän johtuvan siitä, että itse echo tulee joka kerralla omana komentona ja ei tallennu käytännössä mihinkään josta salt voisi sen tarkistaa, ja se haluaa kuitenkin ilmoittaa muutoksen tapahtuneena siinä, mitä tila on saanut aikaan. 

## c) Apache.

Lähdin asentamaan apache2, korvaamaan testisivun ja varmistamaan että demoni pyörii. Ja tietenkin käsin alkuun! Kirjauduin ulos herralta komennolla `exit` , ja otin yhteyden t001 orjaan komennolla `$ vagrant ssh t001` . Olin sitten sisällä orjassa numero 1. Lähdin alkuun katsomaan mikä tilanne apache2 kanssa komennolla `$ sudo systemctl status apache2`. Oho, meillähän on se jo asennettuna aiemman tehtävän tiimoilta!

Poistetaan se siten komennolla `$ sudo apt-get remove apache2` . (Ask Ubuntu s.a.) Sitten asennetaan se takaisin komennolla `$ sudo apt-get install apache2` . Komento pyörii, sitten katsotan tilanne sen suhteen uudelleen komennolla `$ sudo systemctl status apache2`. Status näyttää aktiivista, joten asennus onnistunut.

Sitten siirrytään Apachen hakemistoon komennolla `$ cd /var/www/html` , ja poistetaan index.html komennolla `$ rm index.html` . Sitten tilalle uusi tiedosto komennolla `$ sudo nano korvaaja.txt` . Lisään sinne tekstiä, tallennan ja katson `$ ls` ja `$ cat korvaaja.txt` -komennoilla että homma pelittää. Ja pelittäähän se!

`Alla: Index.html poistaminen ja uuden tiedoston luonti sen tilalle, käsin.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/05ba3e5c-2cb5-4a6b-b446-7154e19c3615)

---

Sitten katsotaan se demonin käynnistys. No sitähän tässä tehtiin jo ylempänäkin, sillä systemctl status-komennolla sen näkee aika helposti eikä tarvitse lähteä kikkailemaan demonilistojen ja ID-lukujen kanssa. Vai olisiko se juuri sitä mitä Tero haluaa :D 

Noh, tehdään se helposti. Komennolla `$ sudo systemctl status apache2 | grep "Active"` saamme printattua statuksesta pelkän aktiivisuusstatuksen, joka tässä tapauksessa näyttää että demoni on aktiivinen.

`Alla: Ylläolevan komennon ajo ja lopputulos`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/abbfc45a-7372-445a-8ed3-0676b519b738)

---

Sitten se hauska osuus, eli automatisointi. Poistetaan vielä orjalla tuo apache uudelleen komennolla `$ sudo apt-get remove apache2` ja poistutaan koneelta exitillä. Kirjaudutaan takaisin herralle komennolla `$ ssh tmaster` ja aletaan muokkaamaan tilatiedostoa. Menen helpolla ja muokkaan jo valmista init.sls tiedostoa komennolla `$ sudo nano /srv/salt/hello/init.sls` . Teen 4 funktiota, joista ensimmäinen lataa apache2, toinen poistaa index.html, kolmas luo uuden .html-tiedoston tilalle, ja viimeinen tarkistaa demonin tilan. Tässä naurettavan yksinkertainen kokonaisuus:

```
apache2asennus:
  pkg.installed:
    - name: apache2

index_ulos:
  file.absent:
    - name: /var/www/html/index.html

tilalle:
  file.managed:
    - name: /var/www/html/automatisoitukorvaaja.html

demonitoimii:
  cmd.run:
   - name: sudo systemctl status apache2 | grep "Active"
```

`Alla: Kuvana vielä sama init.sls-tiedostoa muokatessa.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/4ee8bf84-0c5a-44d5-b02a-bdbb400833ae)

---

Sitten testataan toimivuutta :D Ajan komennon `$ sudo salt 't001* state.apply hello` , jolla kohdistan tilan vain t001-orjaan. Vastaukseksi saan jokaisesta kohdasta vihreätä ja lopputulokseksi succeeded: 4 (changed = 4)! Homma siis toimii ainakin näin yksinkertaisella tasolla. 

`Alla: Pari kuvaa jossa näkyy tilan ajaminen ja onnistunut lopputulos!`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/869f7221-20b1-4e57-b802-40814c23fd4d)

---

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/120dd4d2-c06b-41c1-8c17-f89038ed3e4d)

---








 
## Lähteet.

https://askubuntu.com/questions/176964/permanently-removing-apache2







