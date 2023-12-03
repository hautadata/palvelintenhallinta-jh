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

Ladattuani ISO-tiedoston valitsin VirtualBoxissa "new", josta pääsin määrittelemään uutta virtuaalikonetta. 




## Lähteet

Halonen, Rajala ja Ollikainen. 2023. Installing Windows 10 on a virtual machine. Luettavissa: https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md. Luettu: 3.12.2023.

JRissanen. h5 - Windows. Luettavissa: https://github.com/JRissanen/h5-Windows. Luettu: 3.12.2023.

LSB Workgroup, The Linux Foundation. 2015. Filesystem Hierarchy Standard. Luettavissa: https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html. Luettu: 3.12.2023.
