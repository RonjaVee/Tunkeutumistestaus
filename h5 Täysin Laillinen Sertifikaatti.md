# h5 Täysin Laillinen Sertifikaatti

Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024](https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024)

## x) Lue/katso ja tiivistä



## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Laita ZAP sieppaamaan myös kuvat, niitä tarvitaan tämän kerran kotitehtävissä. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään

Löysin ohjeeksi TheDutchHackerin [Configure OWASP Zap with Firefox -oppaan](https://thedutchhacker.com/configure-owasp-zap-with-firefox/).

OWASP ZAP:in asensin ``sudo apt-get install zaproxy`` ja käynnistin sen komennolla ``zaproxy``. Sitten etsinkin jonkin aikaa, mistä pääsen luomaan sertifikaatin (luki tehtävänannon ohjeissa): Tools -> Options -> Network -> Server Certificates. Tuli ilmoitus, että Root CA Sertificate on jo olemassa. Loin uuden tilalle ja tallensin sen kotihakemistooni.

Firefoxissa menin kohtaan Settings -> Privacy and Security -> Certificates. View Certificates -kohdassa Authorities-välilehdellä valitsin luomani sertifikaatin kotihakemistostani ja hyväksyin kohdan "Trust this CA to identify websites". Sitten loin tunnilla asentamaani FoxyProxyyn Zap-nimisen proxyn (Hostname 127.0.0.1, portti 8080).

![image](https://github.com/user-attachments/assets/1226b538-1ad0-4341-be13-1bcf1febd3f9)


![image](https://github.com/user-attachments/assets/097945e7-e9d0-4ca7-90c2-c6a9b245b91c)

Kokeilin sitten katsoa localhostilla, näkyykö Zapissa mitään. Tuli virheilmoitus 502, ja tajusin sitten, että Apache täytyy varmaankin käynnistää ensin. ``sudo systemctl start apache2`` ja nyt localhostissa näkyi oletussivu. 

![image](https://github.com/user-attachments/assets/b934bb70-1003-475a-88f8-6d0caefbd3d0)

Jotta kuvatkin näkyisivät, tehtävänannossa vinkattiin valitsemaan asetuksista Tools -> Options -> Display. Sieltä löytyikin täppä "Process images in HTTP requests/responses. CTRL + F5 latasin sivun uudelleen, ja nyt kuvakin näkyi Zapissa.

![image](https://github.com/user-attachments/assets/7855659a-717c-4f79-92ae-a43eac070ddd)


## b) Kettumaista. Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen. Käytä FoxyProxyn "Patterns" -toimintoa, niin että vain valitsemasi weppisivut ohjataan Proxyyn

Tutkin ensin, miten käyttää FoxyProxyn Patternsia FoxyProxyn ohjesivulta [URL Patterns](https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/). Ohjeessa kerrottiin, kuinka URL pitäisi muotoilla. Localhost: ``*://localhost/*``, PortSwigger: ``*.portswigger.net/*``. 

![image](https://github.com/user-attachments/assets/45e448e2-9d37-421b-8d22-d7886cafb939)


## c) PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu



## k) Asenna pencode ja muunna sillä jokin merkkijono (encode a string)

Luin sivulta [https://github.com/ffuf/pencode](https://github.com/ffuf/pencode) mitä pencoden asennus vaatii. Tarkastinko, onko minulla go:ta ``go version``-> ei ollut, ehdotus oli asentaa se ``sudo apt-get install golang-go``. Sitten ``go install github.com/ffuf/pencode/cmd/pencode@latest``. Tämä ei kuitenkaan asentunut, kun kokeilin ``pencode version``. Ehdotuksena oli asentaa se ``sudo apt install golang-github-ffuf-pencode-dev``, valitsin yes ja kokeilin uudelleen, asentuiko. Asentuihan se. 

![image](https://github.com/user-attachments/assets/0ff11f6a-776d-425c-929e-939a0e4ae2c3)


Sivulta löytyvän ohjeen mukaan kokeilin muuntaa merkkijonon moimoi. Käytin komentoa ``echo 'olen omena'|pencode [muoto]``. Muodot: urlencode b64encode hexencode. Urlencode: Koodaa merkkijonon URL-muotoon, b64encode: Koodaa merkkijonon Base64-muotoon, hexencode: Koodaa merkkijonon heksadesimaalimuotoon.


![image](https://github.com/user-attachments/assets/c21fe116-370a-4495-ae34-091a17d43896)




## Lähteet

Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024](https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024)

https://thedutchhacker.com/configure-owasp-zap-with-firefox/

https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/
