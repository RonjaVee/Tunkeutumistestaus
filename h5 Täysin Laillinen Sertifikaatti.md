# h5 Täysin Laillinen Sertifikaatti



## x) Lue/katso ja tiivistä



## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Laita ZAP sieppaamaan myös kuvat, niitä tarvitaan tämän kerran kotitehtävissä. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään

Löysin ohjeeksi ZAP:n virallisen [Server Certificates -oppaan] (https://www.zaproxy.org/docs/desktop/addons/network/options/servercertificates/).

OWASP ZAP:in asensin ``sudo apt-get install zaproxy`` ja käynnistin sen komennolla ``zaproxy``. Sitten etsinkin jonkin aikaa, mistä pääsen luomaan sertifikaatin. Tools -> Options -> Network -> Server Certificates. Tuli ilmoitus, että Root CA Sertificate on jo olemassa. Loin uuden tilalle ja tallensin sen kotihakemistooni.

Firefoxissa menin kohtaan Settings -> Privacy and Security -> Certificates. View Certificates -kohdassa Authorities-välilehdellä valitsin luomani sertifikaatin kotihakemistostani ja hyväksyin kohdan "Trust this CA to identify websites". Sitten loin tunnilla asentamaani FoxyProxyyn Zap-nimisen proxyn.

![image](https://github.com/user-attachments/assets/097945e7-e9d0-4ca7-90c2-c6a9b245b91c)

Kokeilin sitten katsoa localhostilla, näkyykö Zapissa mitään. Tuli virheilmoitus 502, ja tajusin sitten, että Apache täytyy varmaankin käynnistää ensin. ``sudo systemctl start apache2`` ja nyt localhostissa näkyi oletussivu. 

![image](https://github.com/user-attachments/assets/b934bb70-1003-475a-88f8-6d0caefbd3d0)


Kysyin sitten ChatGPT:ltä, "miten saan kuvat näkymään history-välilehdellä zapissa" ja ohjeeksi tuli katsoa asetukset Tools -> Options -> Display. Sieltä löytyikin täppä "Process images in HTTP requests/responses. CTRL + F




## b) Kettumaista. Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen. Käytä FoxyProxyn "Patterns" -toimintoa, niin että vain valitsemasi weppisivut ohjataan Proxyyn



## c) PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu



## k) Asenna pencode ja muunna sillä jokin merkkijono (encode a string)



## Lähteet
