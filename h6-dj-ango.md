# h6 DJ Ango

## x) Tiivistelmä

### Tero Karvinen - Django 4 Instant Customer Database Tutorial
- 
- 

### Tero Karvinen - Deploy Django 4 - Production Install
- 
- 

## Rauta & HostOS
- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Asus ROG Strix Nvidia RTX 4090
- Windows 11 Home 23H2

## a)
Käytin ensimmäiseen tehtävään tuoretta viime tehtävässä tehtyä virtuaalikonetta. Hommat käyntiin asentamalla Virtualenv

        sudo apt-get -y install virtualenv

![H6](H6_1.png)

Seuraavaksi luotiin uusia virtualenv asennus Python paketeille. 

        virtualenv --system-site-packages -p python3 env/

![H6](H6_2.png)

Virtuaalinen ympäristö käyntiin

        source env/bin/activate

![H6](H6_3.png)

Samalla oli myös hyvä tarkistaa se, mihin aloitetaan asennustoimet.

        which pip

![H6](H6_4.png)

Seuraavaksi asennetaan Micro, jotta päästään asentamaan Python paketin kautta Django. Loin Requirements.txt tiedoston ja lisäsin sinne sisälle tekstin Django.

        sudo apt-get install micro
        micro requirements.txt

![H6](H6_5.png)
![H6](H6_6.png)

Käynnistetään itse Djangon asennus ja tarkistetaan, asentuiko oikea versio.

        pip install -r requirements.txt
        django-admin --version

![H6](H6_7.png)
![H6](H6_8.png)

Taron ohjeistuksessa käytettiin versiota 4.0.2, mutta itselle asentui 5.0.2 ja annoin sen olla. Seuraavaksi siirryttiin itse Django projektin luomisen pariin. 

        django-admin startproject maikki

![H6](H6_9.png)

Kokeilin, että serveri ja Django lähtee normaalisti toimimaan.

        ./manage.py runserver

![H6](H6_10.png)

Admin käyttöliittymän rakentamisen aloitin heti perään.

        ./manage.py makemigrations
        ./manage.py migrate

![H6](H6_11.png)

Lisäsin käyttäjän ja käynnistin serverin uudetsaan, jotta pääsen kirjautumaan sisään. 

        sudo apt-get install pwgen
        pwgen -s 20 1
        ./manage.py createsuperuser
        ./manage.py runserver

![H6](H6_12.png)
![H6](H6_13.png)
![H6](H6_14.png)
![H6](H6_15.png)

Kirjautuminen sisäänkin onnistui moitteitta. Lisäsin samalla uuden käyttäjän, kenelle annoin staff ja superuser oikeudet. Testasin vielä, että pääsen kirjautumaan myös uudelle käyttäjälle sisään.

![H6](H6_16.png)
![H6](H6_17.png)
![H6](H6_18.png)

Seuraavaksi siirrin itse asiakastietokannan rakentamisen kimppuun.

        ./manage.py startapp crm

![H6](H6_19.png)

Lisäsin ohjelman settings.py tiedostoon.

        micro maikki/settings.py

![H6](H6_20.png)

Samalla lisäsin muutaman mallin, jotta Django voi automaattisesti luoda tietokannat ja näkymät.

        micro crm/models.py

![H6](H6_21.png)

Rekisteröidään vielä muutokset käyttöön.

        ./manage.py makemigrations
        ./manage.py migrate

        micro crm/admin.py

![H6](H6_22.png)
![H6](H6_23.png)

Seuraavaksi testailin, näkyykö CRM Djangon Admin sivustolla. 

        ./manage.py runserver

![H6](H6_24.png)

Hienosti toimii, muokataan vielä nimet näkyviin ja luodaan uusi asiakas.

        micro crm/models.py

![H6](H6_25.png)

Serveri takaisin käyntiin ja tarkastetaan homman toimivuus.

        ./manage.py runserver

![H6](H6_26.png)

## b)


### Lähdeluettelo

Karvinen, T. H6 - DJ Ango, Linux-palvelimet kurssi. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/2024/linux-palvelimet-2024-alkukevat/ Luettu 29.02.2024.

Karvinen, T. Django 4 Instant Customer Database Tutorial. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/2022/django-instant-crm-tutorial/ Luettu 29.02.2024.

Karvinen, T. Deploy Django 4 - Production Install. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/2022/deploy-django/ Luettu 29.02.2024.
