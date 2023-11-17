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





