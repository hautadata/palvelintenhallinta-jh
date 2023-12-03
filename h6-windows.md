# H6- Windows

Tein tehtävää itsenäisesti kotonani sunnuntaina 3.12. Tein tehtävät HP pavilion kannettavallani, jossa on Intel i5-9300H prosessori, 16gt RAM, 512gt SSD ja Windows 10 home-käyttöjärjestelmä.

## x) Lue ja tiivistä.

### [JRissanen](https://github.com/JRissanen) - h5 - Windows (Marraskuu 2022)

```
-Tilan ajaminen koko tiedostopolulla syntaksissa --file-root=
-PowerShellissä komennolla $ Get-ComputerInfo koneen tekniset tiedot.
-Porttien testaus komennolla $ tnc <ip-osoite> -p <portin numero>
-Tietoa porteista komennolla $ netstat 
```

---

### Halonen, Rajala ja Ollikainen 2023: Installing Windows 10 on a virtual machine

```
-ISO-tiedoston lataus vain viralliselta Microsoftin verkkosivulta!
-Virtual Hard Driven tila hyvä olla n. 50gt.
-RAM-muisti 8gt tai hostikoneen oman raudan verran.
-Prosessoreja mielellään vähintään 4kpl.
-"Domain join instead" , ei s-postilla kirjautumista.
```

---

### LSB Workgroup, The Linux Foundation 2015: Filesystem Hierarchy Standard

```
-/etc -hierarkia sisältää koneen ohjelmistojen konfiguraatiotiedostot.
-/srv -hierarkia sisältää eri palvelujen datatiedostot, ja on jaoteltu yleensä protokollien mukaan, esim. /var/www/
-/var -hierarkia sisältää muuttuvia ja tilapäisiä datatiedostoja (variable datafiles), kuten esim. /var/log/ ja /var/cache/
-/usr -hierarkia sisältää jaettavaa ja read-only dataa. Ei saa sisältää yhdellekkään hostille yksityistä dataa, vaan pitää olla jaettavissa kaikkien hostien välillä.
```

## a) Asenna Windows virtuaalikoneeseen.

Asensin Windowsin virtuaalikoneeseen VirtualBoxissa tiistain luennolla 28.11. Asennuksessa meni toistakymmentä minuuttia, joten en lähde sitä nyt uudelleen asentamaan. Muistan kuitenkin tarkasti speksit, jotka asennuksessa annoin, joten käydään niitä läpi sen sijaan. Käytin asennuksessa apuna _"x) Lue ja tiivistä."_ kohdan toista artikkelia _"Halonen, Rajala ja Ollikainen 2023: Installing Windows 10 on a virtual machine"_.  

Latasin Windows 10-käyttöjärjestelmän ISO-tiedoston Microsoftin virallisilta sivuilta. Valitsin "English (Great Britain) ja tiedoston ISO – Enterprise downloads, 64-bit download. Asennuksessa kesti puolisen tuntia hieman hitaan verkkoni takia. 

Ladattuani ISO-tiedoston valitsin VirtualBoxissa "new", josta pääsin määrittelemään uutta virtuaalikonetta. Annoin koneelle nimen "Winukka", valitsin lataamani ISO-tiedoston, valitsin "skip unattended install", laitoin RAM-muistia 8192mt, prosessoreja 2, sekä virtuaalisen kovalevyn muistia 50gt. 


![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/da451828-6083-4990-8eb7-fb76b441b0f1)
>Yllä: Aiemmin luomani Windows 10-virtuaalikone, ja sen muistimäärä.

## b) Asenna Salt Windowsille.

Lähdin asentamaan Saltia Windowsille lataamalla sen Salt Projectin Salt Install Guide Windows -sivulta. Tiedoston nimi oli https://repo.saltproject.io/salt/py3/windows/latest/Salt-Minion-3006.4-Py3-AMD64-Setup.exe . Menin setupin läpi oletusasetuksilla, ja Master hostnamea ja minion namea kysyttäessä jätin nekin oletusarvoisiksi. Lopussa Salt ilmoitti vielä, että asennukseen vaadittavaa vcredistiä ei ole asennettuna, joten asensin senkin samalla.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/03204717-d442-4699-bfd7-58228f34575b)
>Yllä: Saltin asennusta, ja hostin ja minionin nimeäminen.

---

Avasin Windows Powershellin ja lähdin testaamaan Saltin toimivuutta aluksi komennolla `$ salt-call --local` , joka antoi pitkän listan eri syntakseja. Kokeilin listalta löytyvää --version syntaksia, joka tulostaa Saltin version komennolla `$ salt-call --local --version` .

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/e972603f-b834-4909-9ae5-b6bd715b58bb)
>Yllä: Ajetut komennot Saltin testauksessa.

---

### c) Kerää Windows-koneesta tietoa

Lähdin keräämään tietoa Windows-koneesta komennolla `$ salt-call --local grains.items` . Ihmettelin miksi sain errorin, mutta nopeasti huomasin että kyseessä oli "permission denied". En ollut avannut Powershelliä adminina, joten suljin nykyisen ja hain uudestaan Powershellin, ja tällä kertaa "run as administrator". Ajoin sen jälkeen komennon uudestaan, ja tällä kertaa se toimi moitteettomasti!

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/d16924d1-5753-47b3-9804-f681c7afc1cf)
>Yllä: grains.items komento ajettuna adminina.

---

Tehtävässä oli tarkoitus poimia vielä muutamia keskeisiä tietoja, joita saamme syöttämällä komentoon vielä jonkun spesifoivan syntaksin. Kun poimitaan vain yksittäisiä tietoja, täytyy komentoa muokata ja käyttää "grains.item" ilman s-kirjainta, jolloin komento ei ole monikossa eikä printtaa kaikkia tietoja. Ajattelin että printataan vaikka pelkästään käyttöjärjestelmän versio, IPv4-osoite ja RAM-muistin määrä. Ajoin näitä varten komennot `$ salt-call --local grains.item osversion/ipv4/memtotal` (3 eri komentoa, loppuosa jokaisella eri)

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/8f5c3720-cabd-48fb-ac0c-440dbcfb5aa4)
>Yllä: grain.itemillä haettuja yksittäisiä tietoja koneesta.

---

## d) Kokeile Saltin file -toimintoa Windowsilla.

Lähdin kokeilemaan Saltin file-toimintoa hyvin perinteisellä tavalla. Päätin katsoa kurssin ensimmäistä tehtävää, eli omaa H1 - Viisikkoa, jossa olimme käyttäneet eri toimintoja lokaalisti, ja joukossa oli juurikin tuo file-toiminto myös. Halusin tehdä uuden tekstitiedoston, ja koittaa lisätä sinne vielä jotain tekstiä. 

Käytin tähän komentoa `$ sal-call --local state.single file.managed C:\users\joo\testi.txt contents="Tämä..."` 

Komento näytti toimivan onnistuneesti! Halusin vielä tarkistaa tallentuiko sisältökin, ja hetken pähkäiltyäni päätin kokeilla ihan perinteistä cat-komentoa, jonka en uskonut toimivan Windowsilla, mutta positiivisena yllätyksenä se toimi hyvin. 

Eli tämän jälkeen vielä tarkistettiin sisältö komennolla `$ cat C:\users\joo\testi.txt` .

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/db60002b-e6f0-4b7c-830e-c011f8c4d389)
>Yllä: file-toiminnon kokeilua uuden tiedoston merkeissä, ja sisällön tarkastus cat-komennolla.

---

## e) Kokeile jotain itsellesi uutta toimintoa Saltista Windowsilla.

Päätin kokeilla jotain uutta toimintoa etsimällä aluksi jonkun aivan tuntemattoman toiminnon. Päätin hakea inspiraatiota Salt Projectin [State Modules](https://docs.saltproject.io/en/latest/ref/states/all/)-sivulta, josta löytyy kaikki Saltin toiminnot. 

Hetken toimintoja ihmetellessäni tuli eteeni "win_path"-niminen toiminto, joka vaikutti suoritettavalta, ja en ollut toiminnosta ennen kuullutkaan. Painoin tilan nimestä, josta pääsin win_pathin omalle salt.states-sivulle. Nopeasti selvisikin, kuten nimestä voi päätellä, että toiminnolla hallitaan Windowsin polkuja (patheja).

Päätin luoda uuden pathin kotihakemistooni komennolla `$ salt-call --local state.single win_path.exists C:\users\joo\onkolemassa` . Sain komennolla positiivisen tuloksen, succeeded: 1 (changed=1). Päätin vielä ajaa komennon uudelleen tarkistaakseni idempotentin. Nyt vastauksena oli, että polku löytyy jo, ja täten ollen mitään ei muuttunut toimintoa ajettaessa.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/4e6cb2d9-0178-431f-9094-a69cb2fa84db)
>Yllä: win_path.exist kokeilua.

---

Ja kuten aina, niin nytkin tarkistetaan lopputulos jotain muuta kautta. Yritin ajaa komennon `$ dir C:\users\joo\` tarkistaakseni tuloksen, mutta siellä ei näkynyt mitään onkoolemassa-nimistä polkua. Sentään aiemmin luotu testi.txt näkyi.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/5a7e6d6c-d2ab-424c-bae8-730f91e2aafe)
>Yllä: Missä path?

---

Päätin vielä yrittää ihan komentoa `$ path`, mutta se ei PowerShellissä toiminut. Siirryin komentokehotteeseen, jossa ajoin komennon `$ path C:\users\joo\`, ja nyt sain onnistuneesti listan patheistani kotihakemistossani, jossa joukossa \onkoolemassa. 

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/b526a9d6-79c3-4a69-aaa0-9d6eaeabe9c1)
>Yllä: path-komennolla onnistunut listaus.

---

Lista näytti erilaiselta, kuin mitä dir-komento antoi. Tässä vaiheessa hiffasin, että path ja directory eivät olekkaan sama asia. 

Nopealla googlauksella selvitin eron, joka on siinä että directory on aina kansio. Path sen sijaan on kuin "road map", joka on polku hakemistoon, kansioon tai yksittäiseen tiedostoon, mutta se ei kuitenkaan ole mitään näistä. (Corel, s.a.)

---

## g) Vapaaehtoinen: käytä Saltin service-funktiota Windowsilla. 

Lähdin kokeilemaan service-funktiota Windowsilla. Lähdin ensin hakemaan tietoa eri palveluista verkosta, ja löysin tiedon, että komentokehotteella komennolla `$ services.msc` saa auki listan palveluista. (Wikihow, 2023)

Katsoin jonkun palvelun, joka ei ole päällä. Koitin aluksi käynnistää Remote Desktop Serviceä komennolla `$ salt-call --local state.single service.running Remote Desktop Services enable=True` , mutta se ei toiminut. Vastauksena oli vain että service remote not present, ja mitään ei muuttunut.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/7f7f8dad-e9aa-4e7a-a308-6ccc888b7e66)
>Yllä: Ei toiminut

---

En kuitenkaan lannistunut, vaan yritin vielä toista palvelua. Tällä kertaa kohteena webclient. Ajoin komennon `$ salt-call --local state.single service.running webclient enable=True` . Tällä kertaa sain positiivisen tuloksen, vastauksena että "Started service webclient" ja "Succeeded: 1 (changed= 1).

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/7a983155-499a-4d6d-8c5f-8d9711cdd710)
>Yllä: Onnistunut komennon ajo.

---

Menin servicesiin tarkistamaan vielä, että palvelu meni varmasti päälle. Status-kohdassa ei lue että "running" mutta vasemmalla Webclient-otsikon alapuolella on vaihtoehdot lopettaa ja uudelleenkäynnistää palvelu. Nämä + aiemmin saamani onnistunut tulos salt-tilasta, ja voidaan siis olettaa että palvelu tosiaankin on päällä.

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/2f971c1a-4a10-4ecf-a814-8082b797c576)
>Yllä: Palvelu päällä.

---

Pistetään se vielä pois päältä, ja katsotaan muuttuuko mikään services-ikkunassa. Ajan komennon `$ salt-call --local state.single service.dead webclient enable=False` . Tästä tuli myös onnistunut lopputulos, kommenttina että "webclien has been disabled, and is dead". Succeeded: 1 (Changed= 1).

![image](https://github.com/hautadata/palvelintenhallinta-jh/assets/148875340/a90d5b09-9fa8-4432-819f-72fb716b2b83)
>Yllä: Onnistuneesti lopetettu webclient-palvelu.

---











## Lähteet

Corel. s.a. What are "directory" and "path"? Luettavissa: https://kb.corel.com/en/125970#:~:text=(Alternate%20definition%3A%20A%20directory%20is,a%20file%20or%20another%20folder.). Luettu: 3.12.2023.

Halonen, Rajala ja Ollikainen. 2023. Installing Windows 10 on a virtual machine. Luettavissa: https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md. Luettu: 3.12.2023.

JRissanen. h5 - Windows. Luettavissa: https://github.com/JRissanen/h5-Windows. Luettu: 3.12.2023.

LSB Workgroup, The Linux Foundation. 2015. Filesystem Hierarchy Standard. Luettavissa: https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html. Luettu: 3.12.2023.

WikiHow. 11.7.2023. How to Open Windows Services. Luettavissa: https://www.wikihow.com/Open-Windows-Services. Luettu: 3.12.2023.
