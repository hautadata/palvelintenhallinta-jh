# H3 - Versio

Tein tehtävää koululla keskiviikkona 8.11. ja perjantaina 10.11. Tein tehtävät HP pavilion kannettavallani, jossa on Intel i5-9300H prosessori, 16gt RAM, 512gt SSD ja Windows 10 home-käyttöjäjestelmä. 

Tehtäviä on tehty yhdessä Thomas Helmisen ja Essi Krakaun kanssa. 

## a) Online. Tee uusi varasto GitHubiin.

Lähdin tekemään tehtäviä Teron _"Infra as Code 2023"_-ohjeen H3-kohdan mukaisesti (Karvinen, 2023). Aloitin luomalla itselleni uuden varaston GitHubiin Google Chrome-verkkoselaimella.

GitHubin pääsivulla klikkasin oikeasta yläkulmasta plus-merkistä, ja valtisin "Create new repository". Annoin varaston nimeksi winterclassic, sillä sen nimessä piti olla sana "winter".

Kirjoitin pienen kuvauksen varastosta, lisäsin sille Readme-tiedoston ja valitsin lisenssiksi "GNU General Public License v3.0". Tämän jälkeen valitsin oikeasta alakulmasta "Create repository", joka loi uuden varaston. 

``Alla: Uuden varaston luominen ja määrittely.``
![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/bd2e467d-2d51-459d-bf72-fbf8fe5d5c45)

## b) Dolly

Lähdin aluksi kloonaamaan varastoa omalle koneelleni Windowsin terminalissa. Käytin tähän verkosta löytyvää ohjetta _How to SSH into GitHub on Windows example_. (McKenzie, 2022).

Loin itselleni uuden kansion GitHubille terminalissa komennolla `$ mkdir github/h3winter` . 

Lähdin luomaan julkista avainta, jonka yhdistän GitHubiin. Käytin komentoa `$ ssh-keygen -o -t rsa -C "windows-ssh@github"` , jossa -o tarkoittaa OpenSSH-formaattia, -t RSA/SHA256-avaintyypin valintaa ja -C on kommentti.

Tiedosto tallentuu kotihakemistooni .ssh/id_rsa -kansioon.

`Alla: Julkisen avaimen luominen yllämainitulla komennolla `

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/a7cae877-89a8-4b54-823d-7af0b4f28996)

---

Tämän jälkeen menin lisäämään julkisen avaimeni GitHub varastooni. Valitsin winterclassic-varastoni, ja valitsin yläpalkista oikealta ensimmäisenä "settings", josta pääsin varastoni asetuksiin. Valitsin sieltä vasemmalta alhaalta "Security" alaotsikosta "Deploy keys", jossa syötin avaimeni valitsemalla oikealta ylhäältä löytyvän "Add deploy key"-kohdan. Tämän jälkeen avain näkyi listattuna asetuksissa. 

Annoin avaimen nimeksi "SSH-key-JH, ja kopion avaimen tekstikenttään juuri luomani julkisen SSH-avaimen. 

`Alla: Julkisen SSH-avaimen lisääminen GitHubissa winterclassic-varastoon.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/5f058e9f-d5fc-4283-aae2-24586ea5b642)

---

`Alla: Julkinen avain lisätty onnistuneesti.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/0596051e-cbb3-45db-81c1-f44e3baabf72)

---

Tämän jälkeen lähdin asentamaan Gitiä, sillä sitä minulla ei vielä ollut. Lähdin siten siis asentamaan itselleni Git Bashia. Latasin sen Gitin omilta sivuilta, josta valitsin Windows-versioiden lataussivun. Valitsin sieltä viimeisimmän 64-bittisen version (2.42.0), joka on julkaistu 30.08.2023. (Git, s.a.)

`Alla: Git-version valitseminen Gitin omillta sivuilla.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/8bbc3bd9-5936-4e32-aeef-f2a67e5fa2e0)

---

Latauksessa kesti n. puoli minuuttia, jonka jälkeen lähdin ajamaan sitä ja valitsemaan Gitin. Sain pitkän liudan kysymyksiä SSH client programista aina git pull-komennon toimintoon. Menin kaikki kysymykset vakiovastauksilla, paitsi tekstieditorin kohdassa muutin vimin Windowsin omaan notepad.exeen. Etenin kysymyksissä kunnes kaikkiin oli vastattu ja Git Bash oli asennettu. 

Sitten itse kloonaamiseen. Käytin ensin komentoa `$ git version` tarkistamaan, että git asentui onnistuneesti. Vastaus oli "git version 2.42.0.windows.2", joten ok! Sitten kloonasin kansioni koneelleni komennolla
``$git clone git@github.com:hautadata/winterclassic.git``

`Alla: Onnistunut kansion kloonaus koneelle.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/b305b031-128d-4d39-b18a-6c3a357f1c03)


---

Git clone jälkeen tulevan uniikin ssh-linkin löytää GitHubista haluamasta varastosta valitsemalla oikealta ylhäältä "code", josta "local" ja "ssh". Liittämällä tämän git clone-komentoon, komento osaa kloonata oikean kansion sille syötettävällä osoitteella. 

`Alla: Oman ssh-kloonauskoodin löytäminen.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/67f9acf7-5d6a-4fb2-9992-f5033b8be239)

---

Tajusin hieman hävettyneenä että tein juuri kloonauksen vielä Windowsin terminalissa vaikka asensin jo Gitin. Vaihdoin sinne avaamalla juuri asentuneen Git Bash-sovelluksen. Yhdistin varastoon SSH-yhteydellä komennolla `$git ssh -T git@github.com` . Navigoin oikeaan kansioon komennolla `$ cd /github/h3winter` , jossa ajoin komennon `$ ls` , jolla listasin tiedostot ja totesin että kloonaus on todellakin onnistunut. 

Lähdin tekemään muutoksia muokkaamalla readme-tiedostoa komennolla `$ nano README.md` , ja lisäsin sinne hieman tekstiä että muokkaan tiedostoa Git Bashilla Windowsilla. Tallensin muutokset nanossa pikanäppäimillä ctrl + o , joka pyytää vahvistamaan muutokset tiedostoon ja sitten poistuin nanosta komennolla ctrl + x.

Sitten itse showtime, eli muutoksien puskeminen GitHubiin. Ihan monnina yritin ajaa aluksi komennon `$git commit` , mutta eihän se toimi koska en ole syöttänyt käyttäjänimeäkään vielä. Tästä Git ohjeistaakin minua käyttämään --global user.name-komentoa, jolla voin sellaisen itselleni luoda. Ajoin komennon `$ git config --global user.name "winukka"` . Pistin hieman tyhmän nimen tässä kohtaa, mutta se muokataan myöhemmässä vaiheessa. Lisäsin vielä "sähköpostiosoitteenkin" komennolla `$ git config --global user.email joonas@windows.com`. Kyseessä ei ole mikään oikea säpo-osoite, vaan pistin vaan lähinnä sellaisen josta tunnistan itseni ja käyttöjärjestelmäni. 

Sitten ajoin komennon `$git add .` , joka lisää kaikki tehdyt muutokset uuteen branchin tilaan. Sitten komennolla `$git commit -m "muokattu 8nov23 1001` teen commitin tilasta. (eli käytännössä valmistelen branchin nykyisen tilan valmiiksi jotta se voidaan puskea/tallentaa GitHubiin). -m lisää haluamani kommentin tilanteesta. Sitten pusken muutoksen komennolla `$git push main` , joka puskee muutokset main-branchiin, jossa yleisesti kaikki tiedostoni sijaitsevat varaston sisällä. Kävin GitHubissa katsomassa, ja muutokset olivat onnistuneet.

Halusin vielä tehdä uuden muutoksen lisäämällä täysin uuden tiedoston. Loin kokonaan uuden tekstitiedoston komennolla `$ nano pushday.txt` . Lisäsin sinne tekstiä, jossa kerron ajavani muutokset GitHubiin. Tiedoston tallennus samalla tavalla kuin ylemmässä kohdassa ctrl + o ja ctrl + x. Sitten taas `$git add .` , `$git commit -m "yritetään puskea pushday webbiin"` ja lopulta `$ git push main.`

`Alla: pushday.txt-tiedoston muokkausta nanolla.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/4d9c7ca4-67b2-4cb0-af28-f3be0023e0eb)

---

`Alla: Pushdayn muutoksien puskeminen GitHubiin.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/e934f0c2-d21b-41e5-8ff1-bb2fe410e9c9)

-

`Alla: Onnistuneet muutokset GitHubissa. (Kuva otettu 2pv myöhemmin perjantaina)`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/e23171a6-ebf0-464d-b765-41535e0da202)

## c) Doh!

Lähdin tekemään tyhmää muokkausta Gitiin. Päätin muokata aiemman luomaani pushday-tiedostoa ja lisätä sinne mukamas yrityksen luottokortin numeron plaintextinä. Tämän tein komennolla `$ nano pushday.txt` , jossa kirjoitin muutoksen ja tallensin sen ctrl + o ja ctrl + x. 

`Alla: Tyhmä muutos pushday-tiedostoon.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/81506678-1d5e-4454-9cea-af17107183ee)

Onneksi tilanne huomataan, ja siitä ei aiheudu haittaa koska emme ole puskeneet sitä GitHubiin. Tarkistan komennolla `$ cat pushday.txt` että tyhmä muutos on vielä tiedostossa. Tämän jälkeen käytän komentoa `$ git reset --hard` , joka nollaa kaikki muutokset viimeisimpään branchin tilaan. Tarkistan tämän jälkeen pushday tiedoston uudelleen komennolla `$ cat pushday.txt` , ja huomaan että muutokset ovat nollaantuneet.

`Alla: Tyhmä muokkaus ja sen nollaaminen. Nollauksen päätteeksi muokkausta ei enää ole.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/87de8258-cb29-4b61-8fb1-d4f16dc54034)

## d) Tukki

Tässä tehtävässä lähdin tarkastelemaan lokiani. Lokin avaamiseen on hyvin helppo komento, ja se kuuluu näin: `$ git log`. Sillä pääsee näkemään lokin, jossa näkyy eri vaiheissa tallennetut branchit. 

Lokia lukiessani huomasin, että sitä luetaan alhaalta ylöspäin, jossa vanhimmat muutokset ovat aina alimpana. Huomasin että minulla on 3 branchin committia tallennettuna. Ensimmäisen nimi on initial commit, ja authorina näkyy GitHub-käyttäjätunnukseni. En muista kirjoittaneeni kumpaakaan näistä, ja aikaleimasta päätellen oletan tämän tulleen automaattisesti silloin kun olen kloonannut varaston GitHubista koneelleni. Tämän jälkeenhän vasta asetin käyttäjänimeni, ja seuraavassa branchissa huomaan että authorina toimii asettamani käyttäjänimi "winukka", ja asettamani sähköpostiosoite. Minulla on vielä yksi commit tallenettuna lokissa, ja se on viimeisin jossa puskin pushday-tiedostoni GitHubiin. 

Itse lokista voidaan huomata että se tallentaa jokaisen commitin ja antaa sille tietynlaisen ID-numeron heti commitin perään. Commitin tiedoissa näkyy authorin nimi ja sähköpostiosoite, sekä tarkka aikaleima päivämäärällä ja oikealla aikavyöhykkeellä. Näiden alla näkyy commitin message, jonka käyttäjä on lisännyt. 

Halusin vaihtaa winukka käyttäjänimen, joten ajoin komennon `$ git config --global user.name "joonas"` , joka muuttaa käyttäjätunnukseni. Tarkastan vielä muutoksen komennolla `$ git config user.name` , joka vastaa nykyisellä käyttäjänimelläni. (Screen, 2020) Huomaan kuitenkin, että vaikka ajan `$ git log` -komennon uudelleen, commiteissa näkyy kuitenkin vielä vanha käyttäjänimi, joka on ihan loogista. Lokeissahan on tärkeää se, että tallennettuja tietoja ei voi jälkikäteen peukaloida. 

Alla: Lokitiedostojen tarkastelua, ja käyttäjätunnuksen vaihtaminan ja varmistaminen. 

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/1ce09434-fc39-448b-9386-17df28156e54)

## e) Vapaaehtoinen: yhteistyötä

Lähdimme Thomaksen kanssa kokeilemaan yhteistyötä. Minä loin julkisen avaimen jonka annoin Thomakselle, ja hän lisäsi sen hänen GitHubiinsa. Loin Windows terminalissa kotikansioni /github kansioon uuden alakansion komennolla `$ mkdir /github/thomaskey` , jossa lähdin luomaan uutta julkista avainta. Tein tämän komennolla `$ ssh-keygen -o -t rsa` , jossa -o tarkoittaa OpenSSH-formaattia ja -t RSA/SHA256-avaintyypin valintaa, aivan kuten oman varaston avaimenikin luomisessa.

`Alla: Uuden julkisen avaimen luominen onnistuneesti.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/63afbbb5-aa40-44b3-bbab-3ca2e1808ee6)

--- 

Tämä komento loi avaimen, joka löytyi /github/thomaskey-kansiosta nimellä thomaskey.pub. Kyseessä on microsoft publisher-tiedosto, jonka avasin muistiolla. Kopioin koko avaimen, ja lähetin sen Thomakselle. Hän lisäsi avaimeni hänen tämän tehtävän GitHub-varastoon. 

Sitten oli aika kloonata varasto. Vaihdoin Git Bashiin takaisin, johon kirjauduin komennolla `$ ssh -T git@github.com`  Varmistin että olen haluamassani /thomaskey-kansiossa, ja aloin kloonaamaan Thomaksen kansiota. Tein tämän komennolla `$ git clone git@github.com:ThomasHelminen/winter-management.git` .

`Alla: Thomaksen varaston kloonaaminen koneelleni.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/53145d72-58d9-4796-aca9-98f6fd516b06)

---

Kloonaus onnistui, ja lähdin tekemään sinne omia muutoksia. Loin uuden tiedoston komennolla `$ nano joonastesti.md` , ja lisäsin sinne hieman tekstiä. Tallennus ja ulos ctrl + o ja ctrl + x. Käytin vielä komentoja `$ ls ` ja `$ cat joonastesti.md` , joilla tarkistin että tiedostoni on siellä ja sisältää tekstini. 

`Alla: Luomani tiedosto ja sen sisällön tarkastus Thomaksen varastossa omalla koneellani.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/12ddcbf4-4f71-4063-ad34-8f28176d541d)

---

Valmistelin muutokset komennoilla ` $ git add .` ja `$ git commit -m "joonaksen testi"` , joiden jälkeen lähdin puskemaan muutoksia Thomaksen GitHubiin.

Yritin tätä komennolla `$ git push` sekä `$ git push git@github.com:ThomasHelminen/winter-management.git` mutta emme saaneet pushia toimimaan. Saimme erroria että "permission denied" ja "could not read from remote repository". Pähkäilimme asian kanssa hetken aikaa, tarkistimme Thomaksen GitHubista oikeudet ja yritimme säätää SSH-asetuksia minun koneella Windows Powershellissä adminina `$ Start-Service ssh-agent` , jolla käynnistimme SSH-agentin uudelleen. Etsimme verkosta ohjeita ja kokeilimme GitHubin ohjetta _"Generating a new SSH key and adding it to the ssh-agent"_ , jonka avulla loimme minulle kokonaan uuden avainparin Windows terminalissa. Lisäsimme näistä yksityisen avaimeni Windows Powerhsellissä SSH-agenttiin komennolla `$ ssh-add C:\Users\minä/.ssh/testiavain` . (GitHub, s.a.) 

Emme kuitenkaan tälläkään saaneet pushia onnistumaan. 

`Alla: Epäonnistunut yritys puskea muutokset Thomaksen GitHubiin...`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/0e0ace8d-a715-4e8f-af87-bcd9a89fe384)

## g) Vapaaehtoinen: Se toinen järjestelmä

Lähdin kokeilemaan Gittiä Debian 12 "Bookworm"-virtuaalikoneella, jonka loin tehtävässä H1. Avasin koneen Virtualboxissa ja kirjauduin sisään käyttäjälleni. Siirryin heti terminaliin, jossa ajoin päivitykset komennolla `$ sudo apt update && sudo apt upgrade` . Tämän jälkeen loin uuden kansion kotihakemistooni komennolla `$ mkdir github` , ja siirryin kansioon komennolla `$ cd github` .

Tämän jälkeen lähdin noudattamaan samaa kaavaa, mitä tämän tehtävän ensimmäisissä kohdissa. Loin uuden avainparin komennolla `$ ssh-keygen -o -t rsa`

`Alla: Avainparin luonti.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/c34537fd-12be-4a81-8cfd-eec753d79323)

---

Sitten lisäsin avaimen GitHubissa winterclassic-varastolleni kuten aikaisemmin. Valitsin yläpalkista oikealta ensimmäisenä "settings", josta pääsin varastoni asetuksiin. Valitsin sieltä vasemmalta alhaalta "Security" alaotsikosta "Deploy keys", jossa syötin avaimeni valitsemalla oikealta ylhäältä löytyvän "Add deploy key"-kohdan. Tämän jälkeen avain näkyi listattuna asetuksissa. Valitsin vielä että "allow write access".

`Alla: Julkisen avaimen lisääminen GitHubiin`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/33d4836f-df34-4ae3-8d8c-c206d352134d)

---

Sitten testaamaan että toimiiko. Ja ai niin, eihän meillä ole Gittiäkään vielä :D Korjataan asia komennolla `$ sudo apt install git` . Tarkistan asennuksen version komennolla `$ git -v` . Saan vastaukseksi "git version 2.39.2".

Avaan SSH yhteyden GitHubiin komennolla `$ ssh -T git@github.com` . Sen jälkeen 























