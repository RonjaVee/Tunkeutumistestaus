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



Testasin sitten, toimiiko. Valitsin FoxyProxysta Proxy By Patterns ja 




## c) PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu



## k) Asenna pencode ja muunna sillä jokin merkkijono (encode a string)



## Lähteet

Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024](https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024)

https://thedutchhacker.com/configure-owasp-zap-with-firefox/

https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/
