# h6 DJ Ango

## x) Tiivistelmä

### Tero Karvinen - Django 4 Instant Customer Database Tutorial
- Artikkelissa opastetaan rakentamaan asiakastietokanta Djangoa käyttäen 
- Lisäksi siinä käydään läpi Django-admin liittymän käyttöä.

### Tero Karvinen - Deploy Django 4 - Production Install
- Artikkelin opastaa asentamaan ja konfiguroimaan Django tuotantoympäristön käyttäen Apachea ja mod_wsgi:tä.
- Myös ohjeita Apache-serverin asentamisesta ja peruskonfiguraatiosta sekä Django-projektin luomisesta ja yhdistämisestä Apacheen mod_wsgi:n avulla.

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
Siirryin tuotantotyyppistä Djangoa varten toiselle virtuaalikoneelle, jotta pystyin alottamaan koko homman alusta. Apache2 ja Micro tälle koneelle oli jo kummiskin asennettuna. 

        echo "See you at Pelottavapontso.me"|sudo tee /var/www/html.index.html

![H6](H6_27.png)

Luodaan kansio projektille. 

        mkdir -p publicwsgi/pontso/static/
        echo "Statically see you at Pelottavapontso.me."|tee publicwsgi/pontso/static/index.html

![H6](H6_28.png)

Luodaan samalla uusi VirtualHost. 

        sudoedit /etc/apache2/sites-available/pontso.conf

![H6](H6_29.png)

Listataan uusi configuraatio käyttöön ja poistetaan nykyinen käytöstä. Samalla käynnistin myös apache2 uudestaan.

        sudo a2ensite pontso.conf
        sudo a2dissite hattu.example.com
        sudo systemctl restart apache2

![H6](H6_30.png)

Testasin, että Syntax antaa OK eli vihreetä valoa jatkaa.

        /sbin/apache2ctl configtest

![H6](H6_31.png)

Homma ok, joten yritin testata localhost osoitetta. Tällä kertaa kuitenkin epäonnistuneesti.

        curl http://localhost/static/

![H6](H6_32.png)

Mietin, missä voisi olla vika ja yritin lisätä täydet oikeudet kansioille. 

        chmod -R 755 /home/pontso/publicwsgi/pontso/static/

Tämä ei kuitenkaan auttanut.

![H6](H6_33.png)

Yritin hetken mietiskellä ja googlettaa ongelmaa. En kuitenkaan tehnyt mitään muuta, kuin lukenut error logit ja vahingossa käynnistin Terminalin uudestaan. Tämän jälkeen yllättäen toimikin normaalisti? 

![H6](H6_34.png)

Eipä mitään, en jäänyt sen enempää ihmettelemään asiaa vaan siirryin heti Djangon asentamiseen VirtualEnvillä.

        sudo apt-get -y install virtualenv
        cd publicwsgi/
        virtualenv -p python3 --system-site-packages env

![H6](H6_35.png)
![H6](H6_36.png)

Virtuaalinen ympäristö käyntiin. Samalla oli myös hyvä tarkistaa se, mihin aloitetaan asennustoimet.

        source env/bin/activate
        which pip

![H6](H6_37.png)

Tein uuden tiedoston requirements.txt Django asennusta varten.

        micro requirements.txt

![H6](H6_38.png)

Django asennus käyntiin.

        pip install -r requirements.txt

![H6](H6_39.png)

Testaus vielä. Käytössäni versio 5.0.2.

![H6](H6_40.png)

Seuraavaksi Django projektin pariin. 

        django-admin startproject pontsome

Ja lisäksi yhdistetään Python Apacheen käyttämällä mod_wsgi:tä.

        sudoedit /etc/apache2/sites-available/pontso.conf

![H6](H6_41.png)

Asennetaan Apache WSGI moduuli.

        sudo apt-get -y install libapache2-mod-wsgi-py3

![H6](H6_42.png)

Testataan Syntax, mikä näyttää OK:lla vihreetä valoa.

        /sbin/apache2ctl configtest

![H6](H6_43.png)

Testataan toimivuus.

        curl -s localhost|grep title

![H6](H6_44.png)

403 Forbidden, eli jossain ollaan menty pieleen. Takaisin pontso.conf pariin katsomaan, että kaikki on oikein ja huomasinkin yhden virheen minkä korjasin.

![H6](H6_45.png)

Testataan toimivuus uudestaan.

        curl -s localhost|grep title

![H6](H6_46.png)

500 Internal Server Error. Virhe vaihtunut, mutta en vieläkään tiedä missä vika. Yritin hieman katsoa error logeja, mutta ne löytänyt mitään selittävää tälle. Tällä kertaa tehtävä jäi keskeneräisenä tähän, vielä en kerennyt aloittaa alusta koko hommaa. 

### Lähdeluettelo

Karvinen, T. H6 - DJ Ango, Linux-palvelimet kurssi. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/2024/linux-palvelimet-2024-alkukevat/ Luettu 29.02.2024.

Karvinen, T. Django 4 Instant Customer Database Tutorial. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/2022/django-instant-crm-tutorial/ Luettu 29.02.2024.

Karvinen, T. Deploy Django 4 - Production Install. Tero Karvisen verkkosivut. Luettavissa: https://terokarvinen.com/2022/deploy-django/ Luettu 29.02.2024.
