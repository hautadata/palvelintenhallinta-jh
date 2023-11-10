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

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/5f754162-61f9-4fba-854a-18ec00eea161)

---

Git clone jälkeen tulevan uniikin ssh-linkin löytää GitHubista haluamasta varastosta valitsemalla oikealta ylhäältä "code", josta "local" ja "ssh". Liittämällä tämän git clone-komentoon, komento osaa kloonata oikean kansion sille syötettävällä osoitteella. 

`Alla: Oman ssh-kloonauskoodin löytäminen.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/67f9acf7-5d6a-4fb2-9992-f5033b8be239)

---

Tajusin hieman hävettyneenä että tein juuri kloonauksen vielä Windowsin terminalissa vaikka asensin jo Gitin. Vaihdoin sinne avaamalla juuri asentuneen Git Bash-sovelluksen. Yhdistin varastoon SSH-yhteydellä komennolla `$git ssh -T git@github.com` . Navigoin oikeaan kansioon komennolla `$ cd /github/h3winter` , jossa ajoin komennon `$ ls` , jolla listasin tiedostot ja totesin että kloonaus on todellakin onnistunut. 

Lähdin tekemään muutoksia muokkaamalla readme-tiedostoa komennolla `$ nano README.md` , ja lisäsin sinne hieman tekstiä että muokkaan tiedostoa Git Bashilla Windowsilla. Tallensin muutokset nanossa pikanäppäimillä ctrl + o , joka pyytää vahvistamaan muutokset tiedostoon ja sitten poistuin nanosta komennolla ctrl + x.

Sitten itse showtime, eli muutoksien puskeminen GitHubiin. Ihan monnina yritin ajaa aluksi komennon `$git commit` , mutta eihän se toimi koska en ole syöttänyt käyttäjänimeäkään vielä. Tästä Git ohjeistaakin minua käyttämään --global user.name-komentoa, jolla voin sellaisen itselleni luoda. Ajoin komennon `$ git config --global user.name "winukka"` . Pistin hieman tyhmän nimen tässä kohtaa, mutta se muokataan myöhemmässä vaiheessa. Lisäsin vielä "sähköpostiosoitteenkin" komennolla `$ git config --global user.email joonas@windows.com`. Kyseessä ei ole mikään oikea säpo-osoite, vaan pistin vaan lähinnä sellaisen josta tunnistan itseni ja käyttöjärjestelmäni. 

Sitten ajoin komennon `$git add .` , joka lisää kaikki tehdyt muutokset uuteen välitilaan. Sitten komennolla `$git commit -m "muokattu 8nov23 1001` teen commitin välitilasta. (eli käytännössä valmistelen välitilan valmiiksi jotta se voidaan puskea/tallentaa GitHubiin). -m lisää haluamani kommentin tilanteesta. Sitten pusken muutoksen komennolla `$git push main` , joka puskee muutokset main-branchiin, jossa yleisesti kaikki tiedostoni sijaitsevat varaston sisällä. Kävin GitHubissa katsomassa, ja muutokset olivat onnistuneet.

Halusin vielä tehdä uuden muutoksen lisäämällä täysin uuden tiedoston. Loin kokonaan uuden tekstitiedoston komennolla `$ nano pushday.txt` . Lisäsin sinne tekstiä, jossa kerron ajavani muutokset GitHubiin. Tiedoston tallennus samalla tavalla kuin ylemmässä kohdassa ctrl + o ja ctrl + x. Sitten taas `$git add .` , `$git commit -m "yritetään puskea pushday webbiin"` ja lopulta `$ git push main.`

`Alla: pushday.txt-tiedoston muokkausta nanolla.`

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/4d9c7ca4-67b2-4cb0-af28-f3be0023e0eb)

---

``Alla: Onnistuneet muutokset GitHubissa. (Kuva otettu 2pv myöhemmin perjantaina)``

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/e23171a6-ebf0-464d-b765-41535e0da202)





