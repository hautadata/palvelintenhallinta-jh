# H2 - karjaa 

Tein tehtävää itsenäisesti kotona 4.11.2023 HP Pavilion Gaming Laptop 15-kannettavallani. 

## x) Lue ja tiivistä

### What is the definition of “cattle not pets”? 

Tässä käyttäjän Richard Slater vastaus tiivistettynä:

- Randy Biasin mukaan käsite on syntynyt Bill Bakerin esitellessä scale-up- (lisätään tehoa yhdelle koneelle) ja scale-out-arkkitehtuureita (lisätään enemmän koneita). 

- Lemmikillä tarkoitetaan yhtä tai kahta palvelinta, usein manuaalisesti rakennettuja, joilla ei ole varaa olla alhaalla. Jos toinen niistä olisi alhaalla, seuraus olisi merkittävä ja voi haitata koko palvelimen toimintatarkoitusta (esim. sähköpostipalvelin alhaalla = sähköpostiliikenne ei toimi).

- Karjalla tarkoitetaan useamman kuin kahden palvelimen laumaa, joka on usein rakennettu automatiikalla, ja on suunniteltu kestämään useammankin palvelimen alhaallaolon. Jos palvelin menee alas, sen korjaamisessa ei usein tarvita ihmiskäsiä, vaan automatiikka ratkaisee tilanteen. (Bias, 2016)

- Karjalaumassa yksi tai useampi menetys ei ole maailmanloppu, vaan korjattavissa oleva tilanne. Rakkaan lemmikin menetyksen kohdalla tilanne on toinen.

  ---

### Vagrant Revisited - Install & Boot New Virtual Machine in 31 seconds

- Vagrant luo uuden virtuaalikoneen automaattisesti.
  
- Vagrantin ja virtualboxin asennus komennolla $ sudo apt-get -y install vagrant virtualbox. (Karvinen, 2017)
  
- Uuden palvelimen luonti $ vagrant init bento/haluttupalvelin

- Vagrant päälle $ vagrant up
  
- SSH-yhteys vagrantille $ vagrant ssh

- Koneen ja tiedostojen tuho $ vagrant destroy.

  ---

### Salt vagrant -automatically provision one master and two slaves

- Kolmen virtuaalikoneen keskitetyn hallintajärjestelmän luominen, jolle Teron luoma valmis ajettava vagrant-tiedosto.
- Orjien avaimien hyväksyminen herra-koneelta, ja yhteyksien testaus.
- Shell-komentojen ajo ja tietojen kerääminen orjakoneilta. ($ sudo salt '*' grains.items)
- Idempotentin ajo ja palvelujen asennus, testaus sekä sammutus. (apache2)
- Infra as code, ajettavat komennot .sls-tekstitiedostossa. Sisennyksellä ON väliä. (Karvinen, 2023)
- Meillä ei ole lemmikkejä, vaan karjaa josta voi huoletta luopua. $ vagrant destroy.

## a) Asenna Vagrant

Asensin Vagrantin Teron “Infra as Code 2023”-sivun H2-vinkit kohdassa olevasta Hashicorpin linkistä omalle raudalleni. Valitsin käyttöjärjestelmäksi siis Windowsin (en ole vielä vaihtanut kokonaan Linuxiin), ja sille AMD64-version.
Latauksessa meni pari minuuttia, jonka jälkeen avasin installerin. Hyväksyin käyttöehdot, ja Vagrant lähti asentumaan. Tässäkin meni pari minuuttia. 

Alla: Vagrant-tiedoston valitseminen.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/6e9f0b59-55d2-4564-967b-1f379b9be808)

Alla: Käynnistin vielä asennuksen lopussa koko järjestelmän uudestaan Vagrantin suosituksesta. 

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/1e3863cd-be98-480d-9f2c-05597b85b2bd)

## b) Yksi maankiertäjä

Lähdin asentamaan yhtä konetta Vagrantille, ja ennen kaikkea opettelemaan sen käyttöä, sillä kyseessä on minulle täysin uusi ohjelma. 
Käytin asennuksessa apuna Teron ”_Vagrant revisited_”-ohjetta, sekä Hashicorpin sivuilta löytyvää laajaa ”_Get started_”-julkaisukantaa. (Vagrant s.a.)

Koska asensin Vagrantin omalle raudalle, lähdin ajamaan sitä Windowsin powershellillä. Tarkastin ensin Vagrantin version komennolla ``$ vagrant -v`` , ja loin uuden kansion kotihakemistooni komennolla ``$ mkdir vagrant_joonas`` , ja siirryin sinne cd-komennolla Vagrantin ”Initialize a Project Directory”-ohjeen mukaisesti. (Vagrant s.a.)

Alla: Version tarkastus ja uuden kansion luominen. Kansio on luotu onnistuneesti, katso yläpalkki. 

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/fd737fd1-febf-4681-8839-c8c75ed4a2e3)

Näiden jälkeen loin uuden koneen kansioon komennolla ``$ vagrant init bento/ubuntu-22.04`` , joka luo Ubuntun 22.04-virtuaalikoneen.

Samalla kansiooni ilmestyi Vagrantfile-tekstitiedosto, johon en vielä sen pahemmin koskenut. Tämän jälkeen Vagrant päälle komennolla ``$ vagrant up`` . Tämä kesti yllättävän kauan, n. 3 minuuttia.

Sitten ssh-yhteys uudelle koneelleni komennolla ``$ vagrant ssh`` . Sisällä ollaan, ja pingataan seuraavaksi Haaga-Helian tietojenkäsittelijöiden pyhää sivua komennolla ``$ ping terokarvinen.com`` . Netti toimii onnistuneesti.

Alla: SSH-yhteydellä sisällä, verkko toimii ja vasemmassa reunassa näkyy Vagrantilla automaattisesti luotu virtuaalikone Virtualbox-ympäristössä.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/cea71eef-d16e-4b87-b1a2-514ceed9636a)

Lopuksi poistan vielä koneen kokonaan komennolla ``$ vagrant destroy`` . Toistoja sisään aina koneen asennuksesta alkaen niin oppii! 

## c) Oma orjansa

Loin uuden ympäristön eri kansioon samalla tavalla kuin ylemmässäkin. Ei siitä siis sen enempää, vaan homman kimppuun.

Kun olin SSH-yhteydellä sisällä uudessa koneessa, latasin herran komennolla`` $ sudo apt install salt-master`` , ja orjan komennolla ``$ sudo apt install salt-minion`` . 

Tarkastetaan että molemmat palvelut ovat päällä komennolla ``$ sudo service salt-master status ; sudo service salt-minion status`` , joka tarkastaa molempien statukset ja printtaa ne peräkkäin. 

Alla: Hyvältä näyttää! Tässä yhden koneen ympäristössä ei tulla paljoa kikkailemaan joten jatkan suoraan seuraaviin tehtäviin, ja poistan tämänkin koneen komennolla ``$ vagrant destroy`` .

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/38640fcb-8bf9-40fa-8c51-25708da2e626)

## d) Asenna Saltin herra-orja arkkitehtuuri toimimaan verkon yli

Lähdin luomaan uutta Vagrant-ympäristöä, jossa on samalla koneella herra ja orja. Pysyn samassa kansiossa kuin ylemmässä tehtävässä, mutta poistan siitä jääneen Vagrantfile-tiedoston, koska se oli poistettuun koneeseen liitettynä.

Luon tiedoston uudestaan ajamalla Powershellissä komennon ``$ notepad.exe Vagrantfile`` (Nyt on erittäin ikävä linuxia ja nanoa, Powershelliin ei oikeen saa tekstipohjaista editoria…). Muistio avautuu, ja kopioin sinne suoraan Teron sivulta ”_Salt Vagrant - automatically provision one master and two slaves_” löytyvän valmiin Vagrantfilen.

Mennään tässä siis Debianilla ja orjia tulee olemaan 2 kappaletta. Ajan komennon ``$ vagrant up`` , ja saan virheilmoituksen. Tämä johtui siitä, että tiedoston nimi päättyi .txt-muotoon. Otin päätteen pois, ja ajoin komennon uudestaan jonka jälkeen onnistui. Tässä kesti aika kauan, n. 8min.

Alla: Teron valmis Vagrantfile ja sen käyttöönotto.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/8e682942-9f6a-4a01-869c-601797d77a23)

Alla: Onnistunut $ vagrant run . Virtualboxiin ilmestynyt 1 herra ja 2 orjaa. 

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/a76e1a3f-e779-4b71-9843-a903d33be969)

Otin SSH-yhteyden herralle komennolla ``$ vagrant ssh tmaster`` , ja ensimmäisiksi hyväksyin orjien avaimet komennolla ``$ sudo salt-key -A`` .

Alla: Orjien avainten hyväksyminen onnistuneesti.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/30f5b7fd-2a8b-48c5-a00e-cf52f1d1b77f)

Alla: Testailin vielä verkon toimivuutta pingaamalla jälleen terokarvinen.com sekä orjien IP-osoitteet. Hyvin toimii.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/60c5adb7-694b-403d-a69f-8ca79f28d897)

Ehdinkin jo tässä hieman testailemaan verkon toimivuutta, mutta testaillaan vielä lisää niin saadaan rutiinia touhuun! 

Alla: Jatkamalla Teron saman sivun ohjeiden mukaan, ajan komennot ``$ sudo salt '*' test.ping`` (Ping ja palaute siitä) ja ``$ sudo salt '*' cmd.run 'hostname -I'`` (Palauttaa hostnamen IP-osoitteen)

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/a1bfe91d-49f3-4b46-9fc4-b27fa2c62146)

## e) Aja useita idempotentteja (state.single) komentoja verkon yli

Lähdin kokeilemaan idempotenttien ajoa ihan perinteisillä esimerkeillä. Ensin kokeilin apache2 asennusta komennolla ``$ sudo salt ’*’ state.single pkg.installed apache2`` . Komento onnistui, ja sain ruudulle pitkän listan changes-kohtaan molemmille orjille. 

Alla: Apache2 asennus idempotenttina. Kuvassa näkyy t002-orjan yhteenveto, ja heti perään jatkuu sama syntaksi t001-orjalle (jonka yhteenvedossa myös changed=1). Onnistui!

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/cca18ac7-90e0-4a2b-adc0-a4427c25edc3)

Alla: Ajamalla komennon uudestaan, saadaan idempotentti. Molemmilla orjilla on jo ylempänä tapahtuneesta apache2 asennettuna, joten ei muutoksia. Kommentti: All specified packages are already installed.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/cb61a7df-13fa-4e65-ab11-d047a21ef69f)

Kokeillaan seuraavaksi Apache2 toimivuutta. Haluan kokeilla omaa tiedostoa, joten luon sellaisen komennolla ``$ sudo salt ’*’ state.single file.managed /var/www/html/testi`` .

Alla: Tiedoston luominen.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/4b004a01-f435-4930-b6f9-90a3adb89089)

Seuraavaksi lisään sinne olemattomia HTML-elementtejä komennolla ``$ sudo salt ’*’ state.single file.managed /var/www/html/testi contents=”<html> h1> Joonaksen testi vain! </h1> </html>”`` . Sen jälkeen tarkistan tuloksen komennolla ``$ curl -s 192.168.12.102/testi`` . Hyvin pelaa!

Alla: testi-tiedoston muokkauksen lopputulos ja curlin ajo toisen orjan IP-osoitteella. Toivottavasti näyttäisi GUI:lla oikealta!

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/b6e24275-5b3f-4e3b-a9d3-38071ee49394)

## f) Kerää teknistä tietoa orjista verkon yli (grains.item)

Kerätään tietoa jo edellisestä H1-tehtävästä tutulla grans.items-tilalla. Ajan komennon ``$ sudo salt ’*’ grains.items`` , joka antaa jälleen erittäin pitkän listan tietoa molemmista orjakoneista. Vilkaisen tietoja, ja yritän huomata eroavaisuuksia.

Yksi mielenkiintoinen näyttäisi olevan server_id. Välimatkaa näiden välillä scrollatessa on kuitenkin paljon joten tarkastetaan asia komennolla ``$ sudo salt ’*’ grains.item server_id`` , joka näyttää kyseiset ID:t.

Kuten allaolevassa kuvassa näkyy, toisen ID on 2 merkkiä pidempi kuin toisen. Noh, ei kai siinä mitään ihmeellistä. Pisti vain silmään :D

Alla: server_id

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/1959b8f6-aa63-456e-ac2d-1d4b45e32ea0)

## g) Aja shell-komento orjalla verkon yli.

Päivitykset ovat tärkeitä, joten ei unohdeta niitä.

Alla: Ajan komennon ``$ sudo salt ’*’ state.single cmd.run ’sudo apt update’`` , joka asentaa päivitykset automaattisesti. Changes-kohdassa näkyy että lopussa mainostetaan myös -upgrade-mahdollisuutta. Kokeillaan sitä seuraavaksi. 

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/d65b4db3-64f1-4c72-a876-e243f344e3f9)

Alla: Komennon ``$ sudo salt ’*’ state.single cmd.run ’sudo apt upgrade’`` ajaminen ei onnistu (Failed. 1), sillä tavallisesti Linuxissa komennon lopussa pyydetään vahvistus yes/no-tyylillä. Ja tässä komentoa ajaessa emme pääse lopussa sitä erikseen vahvistamaan taikka kirjoittamaan.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/16b54bec-dee7-426f-9b6c-fc51de4dee63)

Tähänhän on naurettavan helppo ratkaisu. Lisätään komennon perään -y eli ”yes”, joka vastaa ”yes” automaattisesti kysymyksiin. Eli onnistuu komennolla ``$ sudo salt ’*’ state.single cmd-run ’sudo apt upgrade -y’`` .
Halusin tässä vain lähinnä korostaa sitä että Saltissa erityisesti cmd.runilla pitää saada koko ”syntaksi” kerralla ajettua, etkä voi jälkikäteen alkaa erikseen valitsemaan vaihtoehtoja.

## h) Hello, IaC

Tämä kohta vaikutti itselleni aika monimutkaiselta, joten lähdin siinä lähinnä toistamaan samaa jota Tero oli tehnyt ohjeessaan ”_Salt Vagrant - automatically provision one master and two slaves_”.

Yleensä tykkään hieman kikkailla ja kokeilla omia juttuja, enkä mennä suoraan samoilla esimerkeillä joita opettaja on käyttänyt mutta tässä koin sen fiksuimmaksi jotta ymmärrän miten homma toimii.

Loin uuden kansion hakemiston /srv/salt/hello komennolla ``$ sudo mkdir -p /srv/salt/hello`` . Loin sen jälkeen init-tiedoston komennolla ``$ sudoedit /srv/salt/hello/init.sls`` , ja lisäsin sinne saman tekstin jonka Tero on lisännyt (muutin vain nimeä) eli 

/tmp/infrakoodina:

  file.managed

Kun init.sls-tiedoston ajaa, se luo uuden tiedoston /tmp/infrakoodina. 

Alla: Kokeilin tätä ajamalla komennon ``$ sudo salt '*' state.apply hello`` , ja kyseessähän on idempotentti joka luo tuon yllä mainitsemani tiedoston, kuten muutoksista ja yhteenvedosta näkyy.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/eb483b13-f5c7-4e26-850a-e5213d31eec3)

Mutta ajoin komennon kohteessa hello, joka on hakemisto ja sisältää vielä aiemmin luomani init.sls tiedoston? Miten tämä syntaksi ajoi kuitenkin haluamani lopputuloksen?

VMware Salt Project kertoo artikkelissaan ”_STATES TUTORIAL, PART 1 - BASIC USAGE_” että jos sls-tiedoston nimi on init.sls, siihen viitataan pelkällä hakemiston polulla. (VMware Salt Project s.a.) 

Joten tässä tapauksessa tiedostoa /hello/init.sls voidaan kutsua pelkällä hakemistolla eli hello:lla. Jos meillä olisi taas tiedosto nimeltä hello.sls ja luomamme /hello/init.sls, helloa kutsumalla viitattaisiin vain hello.sls-tiedostoon. 

Eli siis oudolta vaikuttava komento … state.apply hello on nyt ihan looginen.

Tarkistetaan nyt vielä, että tiedosto kuitenkin varmasti luotiin. Tämä komennolla ``$ sudo salt ’*’ state.single cmd.run ’ls /tmp/ | grep infr’`` , eli listataan /tmp/hakemiston tiedostot ja haetaan vain ne jotka sisältävät tekstin ”infr”.

Alla: Tarkistus onnistunut. Molemmilla koneilla kyseinen tiedosto.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/c189c842-cca9-44c1-b266-4fae3be996a3)

Koitetaan vielä infra koodina-ajoa vain toiselle orjalle. Muokkaan aiemman hello/init.sls tiedoston file.managed tilan file.absent tilaksi. Eli tämän ajaminen seuraavaksi pitäisi tuhota aiemmin luomamme tiedoston.

Alla: hello/init.sls muokkaus.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/e2159007-9a3e-4f63-84a1-1261d94b94e4)

Ajan seuraavaksi komennon ``$ sudo salt ’t001’ state.apply hello`` , joka toteuttaa ajon vain toisella orjalla.

Alla: Lopputulos, vain t001-orjalla tiedosto on poistettu. 

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/a73c8b67-372f-4f75-8c89-40c60e890fc4)

Tarkastetaan tilanne vielä komennolla ``$ sudo salt --state-output=terse '*' state.single cmd.run ’ls /tmp/ | grep infr`` , joka hakee nyt molemmilta orjilta infr-sisältävää tiedostoa /tmp/-kansiosta. 

Alla: Koska äsken poistimme tiedoston t001-orjalta, saamme sieltä failurea. t002:lla taas komento onnistuu, sillä se löytää vielä tiedoston itseltään.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/e97269de-ec7f-4c71-8b1d-b5e5d5a0f0b8)

That's it. Poistutaan tmasterista ja vagrantista komennolla `$ exit` ja sammutetaan vielä koneet komennolla `$ vagrant halt`

-Joonas H

## Lähteet

Bias, R. 29.11.2016. The History of Pets vs Cattle and How to Use the Analogy Properly. Luettavissa: https://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/. Luettu: 4.11.2023. (via https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654) 

Karvinen, T. 11.4.2017. Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds. Luettavissa: https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/. Luettu: 4.11.2023.

Karvinen, T. 28.3.2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/. Luettu: 4.11.2023.

Vagrant s.a. Get Started. Luettavissa: https://developer.hashicorp.com/vagrant/tutorials/getting-started. Luettu: 4.11.2023.

Vagrant s.a. Initialize a Project Directory. Luettavissa: https://developer.hashicorp.com/vagrant/tutorials/getting-started/getting-started-project-setup. Luettu: 4.11.2023.

VMware Salt Project s.a. STATES TUTORIAL, PART 1 - BASIC USAGE. Luettavissa: https://docs.saltproject.io/en/latest/topics/tutorials/states_pt1.html#sls-file-namespace. Luettu: 4.11.2023.































