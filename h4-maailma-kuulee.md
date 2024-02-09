# h4 Maailma kuulee

## x) Tiivistelmä

### Susanna Lehto - Teoriasta käytäntöön pilvipalvelimen avulla
- Palvelimen vuokraamiseen ja käyttönottoon liittyy paljon eri vaiheita ja tekemistä. 
- Päivittäminen ja suojaus on tärkeässä asemassa jokaista työvaihetta.

### Tero Karvinen - First Steps on a New Virtual Private Server
- GitHub Education tarjoaa opiskelijoille lyhytaikaisesti ilmaisia palvelimia ja domain-nimiä.
- Sivustolla on hyvin kootut ohjeet tärkeimmistä ensiaskelista uuden virtuaalipalvelimen käyttöönottoon.

## Rauta & HostOS
- Asus X570 ROG Crosshair VIII Dark Hero AM4
- AMD Ryzen 5800X3D
- G.Skill DDR4 2x16gb 3200MHz CL16
- 2x SK hynix Platinum P41 2TB PCIe NVMe Gen4
- Asus ROG Strix Nvidia RTX 4090
- Windows 11 Home 23H2

## a) + d) Virtuaalipalvelin & Domain-nimi
Virtuaalinpalvelimen vuokraus käynnistyi tekemällä tunnukset valitsemaani palveluun, mikä oli tällä kertaa DigitalOcean, koska GitHub Student Pack tarjosi ilmaisia credittejä vuodeksi. 

![H4](H4_1.png)

Rekisteröitymisen, kirjautumisen ja onnistuneen GitHub integraation jälkeen oli aika ruveta rakentamaan uutta Droplettiä valitsemalla "Create Droplet"

![H4](H4_2.png)

Dropletin datacenteriksi valikoitu Euroopasta Amsterdam, jotta se olisi mahdollisimman lähellä.

![H4](H4_3.png)

Seuraavaksi asetuksista valittiin alustaksi Debian 12 vastaamaan virtuaalikoneeni järjestelmää. Droplet tyypiksi vaihdoin Basic ja CPU vaihtoehdosita etsin Tero Karvisen tunnilla ohjeistaman 1GB/1CPU vaihtoehdon. 

![H4](H4_4.png)

Vielä ennen viimeistelyä asetin hyvän salasanan sekä nimen virtuaalipalvelimelle.

![H4](H4_5.png)
![H4](H4_6.png)

Näin saimmekin ensimmäisen oman Dropletin, eli virtuaalipalvelimen alun valmiiksi. 

![H4](H4_7.png)

Siirryin heti seuraavaksi vuokraamaan omaa domain-nimeä ja tähän valikoitui Namecheap, ihan vain sen takia että .me päätteisen domain-nimen sai vuodeksi ilmaiseksi GitHub Education kautta. Tunnusket tekoon ja GitHub integraatio onnistuneesti tulille. Domain-nimen vuokraaminen oli melko yksinkertainen prosessi.

Vapaan domain-nimen etsintä ja lisääminen ostoskoriin.

![H4](H4_9.png)
![H4](H4_8.png)

Vuokrauksen yhteydessä kysyi vielä GitHub Educationiin liitettyä sähköpostia, mutta sen jälkeen domain-nimi oli vuokrattuna. 

![H4](H4_10.png)
![H4](H4_11.png)
![H4](H4_12.png)

Tarkistin vielä suoraan Namecheapin tunnuksillani, että pelottavapontso.me oli vuokrattuna, siirryin seuraavaan kohtaan eli muokkaamaan itse domain-nimeä valitsemalla "Manage"

![H4](H4_13.png)

Managen "Advanced DNS" alta löytyi kenttä, mistä alettiin rakentamaan tietoja virtuaalipalvelimen tiedoille.

![H4](H4_14.png)

Syötin Host kohdalle @ & www alle IP-osoitteen, jonka DigitalOceanista sain ja domain-nimen syöttäminen oli sillä valmis. 

![H4](H4_15.png)

## b) Alkutoimet
Virtuaalikone auki ja ensimmäisenä asensin openssh-clientin, että pääsen ottamaan yhteyden uuteen virtuaalipalvelimeeni.

        sudo apt-get install openssh-client

![H4](H4_16.png)

Seuraavaksi otinkin heti root yhteyden suoraan virtuaalipalvelimeeni onnistuneesti, tässä komennossa käytin DigitalOceanilta löytyvää virtuaalipalvelimen IP-osoitetta.

        ssh root@174.138.15.127

![H4](H4_17.png)

Aloitin päivittämällä virtuaalipalvelimen ja käynnistämällä sen uudestaan. 

        sudo apt-get update
        sudo apt-get dist upgrade
        sudo systemctl reboot

![H4](H4_18.png)
![H4](H4_19.png)
![H4](H4_20.png)

Seuraavaksi oli vuorossa tulimuurin päälle laittaminen ja siihen tarpeellisten reikien tekeminen

        sudo apt-get install ufw
        sudo ufw allow 22/tcp; sudo ufw enable; sudo ufw allow 80/tcp

![H4](H4_21.png)
![H4](H4_22.png)

Seuraavaksi lisäsin itselleni käyttäjän palvelimelle. 

        sudo adduser pontso

![H4](H4_23.png)
![H4](H4_24.png)

Lisäsin vielä sudo oikeudet tehdylle käyttäjälle

        sudo adduser pontso sudo

![H4](H4_25.png)

Avasin toisen terminaalin, missä testasin miten kirjaudun sisälle uudella käyttäjällä ja sen, että sudo oikeudet toimivat.

![H4](H4_26.png)

Seuraavaksi suljin kokonaan root käyttäjän pois käytöstä. sshd_config tiedoista kävin vaihtamassa PermitRootLogin kohdan Yes -> No.

        sudo usermod --lock root
        sudoedit /etc/ssd/sshd_config

![H4](H4_27.png)
![H4](H4_28.png)

## c) Weppipalvelin
Seuraavaksi asensin apache2 weppisivun rakentamista palvelimelle varten.

      sudo apt-get install apache2

![H4](H4_29.png)

Testasin vielä sen toiminnan asentamisen jälkeen

        sudo systamctl status apache2

![H4](H4_30.png)

Testasin seuraavaksi Firefoxilla, että palvelin vastaa molemmista sekä domain-nimestä ja IP-osoitteesta.

![H4](H4_31.png)

Viimeisimpänä tehtävänä korvasin vielä etusivun uudella ja testasin sen toimivuuden.

        echo Maailma kuulee! |sudo tee /var/www/html/index.html

![H4](H4_32.png)

## Lähteet
- Susanna Lehto, Teoriasta käytäntöön pilvipalvelimen avulla. https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/ Luettu 9.2.2024
- Tero Karvinen, First Steps on a New Virtual Private Server. https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/ Luettu 9.2.2024
- DigitalOcean. https://www.digitalocean.com/ Luettu 9.2.2024
- Namecheap. https://www.namecheap.com/ Luettu 9.2.2024
- GitHub Education. https://education.github.com/ Luettu 9.2.2024
- Tero Karvinen, Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. https://terokarvinen.com/2018/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/ Luettu 9.2.2024
