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



b) Django-projektikansio väärässä paikassa (siis se, jossa on manage.py)
c) Projektikansiolla väärät oikeudet ('chmod ugo-rwx teroco/', 'chmod u+rx teroco/')
d) Kirjoitusvirhe Apachen asetustiedostossa (/etc/apache2/sites-available/terokarvinen.conf tms)
e) Apachen WSGI-moduli puuttuu ('sudo apt-get purge libapache2-mod-wsgi-py3' tms)
f) Väärät domain-nimet ALLOWED_HOSTS-kohdassa (settings.py, ja DEBUG=False)


## Lopetus
Tämä harjoitus opetti minut konfiguroimaan Djangon tuotantokäyttöön, harjoitteisiin meni tällä erää noin 2,5h.

## Lähteet:
Karvinen, Tero, 2022 - Deploy Django 4 - Production Install (https://terokarvinen.com/2022/deploy-django/)
