# H12_Vianselvitys

## Ympäristö

Hyper-V kotikoneella (Host):

- CPU: i5-9600K
- RAM: 16Gb
- HDD: 120Gb
- Win 11 Pro x64

Virtuaalikoneen speksit:

- 2 x CPU
- 4 Gb RAM
- 50 Gb HDD
- Generation 2 (Hyper-V pyytää määrittelemään)


## Aloitus 
Aloitin työt 4.3.2023 klo 12:49 käynnistämällä virtuaalikoneen ja kirjautumalla siihen sisälle. Jotta pääsin harjoitteiden saloihin, tuli minun ensin aktivoida Virtualenv ympäristö komennolla ```source env/bin/activate```: </br>
![Kuva1](https://user-images.githubusercontent.com/122887740/222895936-28d41814-1de5-42d5-9bf6-bb93613ee152.png)</br>

### a) Kirjoitusvirhe Python-tiedostossa
Otin uhriksi settings.py tiedoston, johon kävin tekemässä kirjoitusvirheen muuttamalla INSTALLED_APPS kohtaa:</br>
![Kuva2](https://user-images.githubusercontent.com/122887740/222896586-b2d714c6-236a-434f-b847-7bb22bc93bd1.png)</br>

Vaihdoin arvoksi ```'dango.contrib.admin'```, heti tallennettuani tiedosto, rupesi Djangon palvelin valittamaan virheistä: </br>
![Kuva3](https://user-images.githubusercontent.com/122887740/222896667-90a0906a-6199-4c3f-ba39-4d0292127954.png)</br>

Käynnistin vielä erikseen Apachen palvelimen uudelleen ja testasin mennä webselaimella Djangon hallintaan: </br>
![Kuva4](https://user-images.githubusercontent.com/122887740/222896929-8a1cd4eb-067c-41e8-8305-73dfd90e0aba.png) </br>


Jotain on selvästi rikki. Ei kun selvittämään lokeista: </br>

![Kuva5](https://user-images.githubusercontent.com/122887740/222897018-3a01608e-f242-499b-aefc-fdfc60b95d70.png)</br>
Lokit ilmoittaa, ettei järjestelmä löydä ```dango``` nimistä modulia. Lopulta päätin vielä korjata tilanteen korjaamalla kirjoitusvirheen, käynnistämällä Apache2:n palvelimen ja ajamalla configtestin: </br>
![Kuva6](https://user-images.githubusercontent.com/122887740/222897137-78bf171c-2ea6-4854-90c8-391f07fd072d.png)</br>

Ja vielä testit selaimessa: </br>
![Kuva7](https://user-images.githubusercontent.com/122887740/222897190-a9e508d3-8465-4468-805e-e3ec5198fe1a.png)</br>


Testit jatkuvat myöhemmin.


### b) Django-projektikansio väärässä paikassa (siis se, jossa on manage.py) 
Klo 17:45 </br>
Tässä tehtävässä aloitin siirtämällä yritysoy-kansion käyttäjän mattis juureen komennolla ```mv yritysoy /mattis```. Tämän jälkeen käynnistin uudelleen Apache2:n komennolla ```sudo systemctl restart apache2``` sekä Djangon oman serverin ``` ./manage.py start server```: </br>
![Kuva8](https://user-images.githubusercontent.com/122887740/222915571-76902f15-83ef-4669-8eaa-b34201856306.png)</br>
![Kuva9](https://user-images.githubusercontent.com/122887740/222915617-70214f46-25ff-410e-a6fa-798bf99adcfa.png)</br>

Nyt tuli uudenlaista virheilmoitusta, sitten vielä tarkistamaan lokit sekä korjaamaan tilanne. </br>
![Kuva10](https://user-images.githubusercontent.com/122887740/222929264-f4c55598-729c-4999-b607-b467d400d839.png)</br>

Kuvan lokin perusteella, client on hylätty server configurationin puolesta. Oli aika palauttaa konfiguraatio jälleen normaaliksi.
Etsiessäni tahalteen siirrettyä tiedostoa, huomasin vahingossa siirtäneeni sen juureen ja se oli uudella nimellä /mattis. Tilanne palautui, kun siirsin kansion takaisin omalle paikalleen.</br>

Ja vielä lopuksi serverin käynnistys, jonka yhteydessä on system checkit: </br>
![Kuva11](https://user-images.githubusercontent.com/122887740/222929361-69c0bf8d-8b8c-4b3c-b0f5-7cacd967e2ec.png)</br>


Jälleen jatketaan myöhemmin.


### c) Projektikansiolla väärät oikeudet ('chmod ugo-rwx teroco/', 'chmod u+rx teroco/')
SU 5.3.2023 Klo 0:04 </br>
Seuraavaksi minun tuli testata poistaa oikeuksia yritysoy-kansiosta komennolla: ```chmod ugo-rwx yritysoy/```, jonka jälkeen koitin ```cd yritysoy```:</br>
![Kuva12](https://user-images.githubusercontent.com/122887740/222930671-ba5a88c8-7e72-4dd7-beae-f2cf96dede6a.png)

Testasin myös käynnistää Djangon: </br>
![Kuva13](https://user-images.githubusercontent.com/122887740/222930722-575ba277-ae04-4942-a3f7-dab96c38d7b0.png) </br>


ja lopulta sivuja: </br>


![Kuva14](https://user-images.githubusercontent.com/122887740/222930746-70c8e622-3528-4572-968f-94f4996cd1a5.png) </br>


Sehän on RIKKI!, ei kun katsomaan lokeja:</br>
![Kuva15](https://user-images.githubusercontent.com/122887740/222930785-0fdd42a4-c4ee-4893-b5d9-49c52b46aa3d.png) </br>


Lokien perusteella Apachella ei ole oikeuksia käynnistää kansiossa olevia sivustoja. Nyt oli aika palauttaa tilanne ja aloitinkin sen komennolla ```chmod ugo+rwx yritysoy/```. Oikeuksien lisäyksen jälkeen palvelin lähti hyvin käyntiin: </br>


![Kuva16](https://user-images.githubusercontent.com/122887740/222930923-e3e0bf8f-aaa3-44c9-955a-dab444930931.png) </br>

ja vielä lokien tarkistus: </br>
![Kuva17](https://user-images.githubusercontent.com/122887740/222930932-9dca763a-2637-4969-8134-d53f063696fb.png)

Kaikki toimii ja tilanne palautettu. Aika siirtyä seuraavaan harjoitukseen.


### d) Kirjoitusvirhe Apachen asetustiedostossa (/etc/apache2/sites-available/terokarvinen.conf tms)
Klo 0:15 </br>
Tässä harjoitteessa oli tarkoituksena rikkoa Apacheen syötetty asetustiedosto:</br>
![Kuva18](https://user-images.githubusercontent.com/122887740/222931085-6d14b636-ac58-48b7-8330-4deec0f2b75b.png)</br>
Syötin tiedostoon liikakirjaimia, kuten kuvasta näkee.


Seuraavaksi käynnistin uudelleen Apachen palvelut ja testasin sivuston toimintaa:</br>
![Kuva19](https://user-images.githubusercontent.com/122887740/222931100-51b76fa4-f860-485b-b8af-c6fe6a05c64c.png)</br>
Rikkihän se oli.


Ja nyt katsomaan lokeja:</br>
![Kuva20](https://user-images.githubusercontent.com/122887740/222931152-7d59a2a4-56e0-45d8-931b-7ee5f26660b8.png)</br>
Lokeista huomaa, että client on jälleen hylätty serverin määritteistä. Nyt olikin taas aika palauttaa tilanne normaaliksi.


Muutin Apachen määritetiedoston vielä takaisin ja käynnistin onnistuneesti palvelut: </br>
![Kuva21](https://user-images.githubusercontent.com/122887740/222931300-f4235715-38ad-48aa-8808-cc2974ffb0ed.png) </br>
![Kuva22](https://user-images.githubusercontent.com/122887740/222931303-e5915f1a-8c35-4081-86ff-0262cf8181f2.png) </br>


### e) Apachen WSGI-moduli puuttuu ('sudo apt-get purge libapache2-mod-wsgi-py3' tms)
Klo 10:03 </br>
Nyt oli vuorossa poistaa WSGI-moduli ja tarkistaa, miten Django toimii sen jälkeen. Ajoin komennon ```sudo apt-get purge libapache2-mod-wsgi-py3```,
poiston lomassa Apache yritti käynnistää jo itseään uudelleen ja alkoikin heti ilmoittamaan virheistä:</br>
![Kuva23](https://user-images.githubusercontent.com/122887740/222949106-8deb4c83-3706-48e5-ba80-15a63ac2aad8.png)</br>


Koitin vielä käynnistää itse Apachen palvelut:</br>
![Kuva24](https://user-images.githubusercontent.com/122887740/222949162-2aa59e85-5363-48bd-8acd-f2c9035b7841.png)</br>


Kävin vielä koittamassa websivua:</br>
![Kuva25](https://user-images.githubusercontent.com/122887740/222949250-27bf1db7-7856-4b42-a648-42722054a2e2.png)</br>

Ei toimi. Oli aika siirtyä tarkistamaan vielä lokit: </br>
![Kuva26](https://user-images.githubusercontent.com/122887740/222949339-cf6b4aba-4590-4257-8af0-824d2450cd17.png)</br>
Ainoastaan yksi rivi oli generoitunut.

Testien ja tutkimusten jälkeen oli aika palauttaa tilanne takaisin normaaliksi komennolla ```sudo apt-get install libapache2-mod-wsgi-py3```, jonka jälkeen koitin käynnistää Apachen uudelleen, tällä kertaa ei tullut virheilmoituksia ja websivukin toimi normaalisti: </br>
![Kuva27](https://user-images.githubusercontent.com/122887740/222949529-d5673e7b-892a-4644-96f0-c10ebd30ee67.png)</br>


### f) Väärät domain-nimet ALLOWED_HOSTS-kohdassa (settings.py, ja DEBUG=False)
Klo 10:16 </br>
Viimeisenä harjoitteena oli tarkoitus rikkoa settings.py tiedosto muuttamalla määritteet vääriksi. Aloitin käynnistämällä microlla settings.py tiedoston ja muokkaamalla kohtaa:</br>
```
ALLOWED_HOSTS = ["localhost"] -> ALLOWED_HOSTS = ["localhos","ehkäei"]
```
Tallensin konfiguraation, käynnistin Apachen palvelut uudelleen ja lähdin testaamaan webselaimella:</br>
![Kuva28](https://user-images.githubusercontent.com/122887740/222949720-1f2200ad-9137-41f8-bb77-ba4e41221481.png)</br>


Rikkihän se oli. Oli aika tarkistaa lokit: </br>
![Kuva29](https://user-images.githubusercontent.com/122887740/222949821-0b24400e-d85d-49a6-943d-112961490a12.png)</br>


Lokeissa ei ollut mitään merkintää, koska Apachen mielestä kaikki toimii moitteetta. Apachen toimintaan ei näytä vaikuttava ALLOWED_HOSTS kohta lainkaan. Joka tapauksessa, nyt oli vielä tarkoitus palauttaa tilanne normaaliksi vaihtamalla määritteet takaisin sekä testata jälleen toiminta: </br>

![Kuva30](https://user-images.githubusercontent.com/122887740/222949952-c8908a13-a985-422a-909c-5ed74a8aa052.png)


## Lopetus
Tämä harjoitus opetti paljon erilaisista virhetilanteista sekä vianmäärityksestä kokonaisuudessaan, harjoitteisiin meni tällä erää noin 2,5h.

## Lähteet:
Karvinen Tero, 2023 - Linux Palvelimet 2023 alkukevät (https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/)
