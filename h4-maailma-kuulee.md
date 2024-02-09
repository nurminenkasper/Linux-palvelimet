# h4 Maailma kuulee

## x) Tiivistelmä

### Susanna Lehto - Teoriasta käytäntöön pilvipalvelimen avulla (h4)
-
-
-

### Tero Karvinen - First Steps on a New Virtual Private Server
-
-
-

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


## c) Weppipalvelin
