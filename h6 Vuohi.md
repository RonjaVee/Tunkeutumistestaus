# h6 Vuohi

Tehtävänanto: Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. Julkaistu 10.5.2024.[https://terokarvinen.com/tunkeutumistestaus/#h6-vuohi](https://terokarvinen.com/tunkeutumistestaus/#h6-vuohi)

## x) Lue/katso ja tiivistä

Karvinen, Tero. Try Web Hacking on New Webgoat 2023.4. 13.11.2023. [https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/](https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/)

- Asenna Java ja palomuuri ja pistä muuri pystyyn
  ```
  sudo apt-get update
  sudo apt-get install openjdk-17-jre
  sudo apt-get install ufw
  sudo ufw enable
  ```
- Lataa WebGoat.jar `wget https://github.com/WebGoat/WebGoat/releases/download/v2023.4/webgoat-2023.4.jar`
- Aja WebGoat `java -Dfile.encoding=UTF-8 -Dwebgoat.port=8888 -Dwebwolf.port=9090 -jar webgoat-2023.4.jar`; valitaan muu kuin ZAPin oletusportti 8080, koska se on käytössä
- WebGoat löytyy osoitteesta `http://127.0.0.1:8888/WebGoat`

## a) Asenna Webgoat 2023.4

Asensin WebGoatin x. -kohdan komennoilla.

![image](https://github.com/user-attachments/assets/14831161-2027-49f1-93f1-fa48a8e94bfd)

Tarkistin palomuurin statuksen `sudo ufw status`, sitten tein käyttäjän ja kirjauduin sisään.

Vielä piti saada ZAPissa näkymään WebGoat. Käynnistin ZAPin, tein FoxyProxyyn uudet asetukset, mutten tiedä oliko se tarpeen. Jostain syystä sivu antoi 404, joten käynnistin WebGoatin uudestaan. Sitten sivu toimi ja näkyi ZAPissa.

![image](https://github.com/user-attachments/assets/8dc6011c-deb0-4ba1-87f3-41cb5d6e3d2f)

![image](https://github.com/user-attachments/assets/6fe0219a-f07c-4524-9f68-331e2355f429)

![image](https://github.com/user-attachments/assets/17234f02-b22e-4f03-9d6d-22f86193d527)



## Ratkaise WebGoat 2023.4


### b) (A1) Broken Access Control

**Hijack a session (1)**

**Insecure Direct Object References (4)**

Ensimmäisessä osiossa tuli kirjautua sisään tunnuksilla tom, cat. Seuraavalla sivulla piti etsiä, mitä lisätietoja profiilista löytyi raw responsesta sivulla näkyvän lisäksi. Löytyi role ja userId.

![image](https://github.com/user-attachments/assets/d50e10c9-3da7-499c-bd2d-6290059d93fa)

![image](https://github.com/user-attachments/assets/a76aca43-d7de-4e28-85d9-46769eb13d2d)

Seuraavassa osiossa piti miettiä, mikä URL on suora polku käyttäjälle tom. Vastaus oli /WebGoat/IDOR/profile/2342384. Mallina tuli käyttää aiemman tehtävän requestia, ja vinkkinä oli, että muutos olisi vain pieni lisäys. Kokeilemalla löytyi ratkaisu.

![image](https://github.com/user-attachments/assets/c2d1cff8-6902-40a1-be6a-b57d348ec5cf)

Seuraavan tehtävän kohdalla tutkin taas vinkkejä. View profile antoi 502 erroria. Eli piti etsiä muita profiileita, ja vinkkinä oli käyttää fuzzeria. Niinpä askartelin ZAPin fuzzerilla hetken aikaa, kunnes sain sen toimimaan. Tarkoituksena oli kokeilla userId-kohtaan eri numeroita. 


![image](https://github.com/user-attachments/assets/72607ca2-c465-430d-89f9-4a2e0c4857e6)


![image](https://github.com/user-attachments/assets/bf50ea31-98ac-4534-8e4f-cf1b363c2345)


![image](https://github.com/user-attachments/assets/5054fd5e-f5c7-4524-8b02-e6b2c410e8c2)

Kokeiltavia lukuja oli kyllä ihan liikaa, joten rajasin scopea. Koneeni puhkui ja puhisi.

![image](https://github.com/user-attachments/assets/7b3fb828-1aad-4c80-96b0-cb5829ba761a)





**Missing Function Level Access Control (2)**


### c) (A7) Identity & Auth Failure

**Authentication Bypasses (1)**

Tehtävässä piti ohittaa 2FA ja asettaa uusi salasana käyttäjälle. Vinkkinä oli muokata secQuestion-parametrejä.

Käytin tukena ohjevideota [Non-Functional Club. Assignment 1 | Authentication Bypass | WebGoat | OWASP TOP 10 | Broken Authentication. Youtube. 1.11.2021.](https://www.youtube.com/watch?v=DErUuNMHgJo)

Muokkasin secQuestion0 ja secQuestion1 -parametreja.

![image](https://github.com/user-attachments/assets/3b49c167-1dce-4a96-9365-3765a46b8f2c)

![image](https://github.com/user-attachments/assets/a770d99b-3743-4ec4-8e02-697b0c0f6df2)

Tehtävä muuttui vihreäksi, mutten päässyt sivulla laittamaan uutta salasanaa?

![image](https://github.com/user-attachments/assets/52cb1178-103c-4bfc-82ac-8ae073df6643)





**Insecure Login (1)**


### d) (A10) Server-side Request Forgery

**Server-Side Request Forgery (2)**


### e) Client side

**Bypass front-end restrictions (2)**


## f) Editmenu. Lisää uusi oma komento micro:n palettero-lisäkkeellä käytettäväksi

[https://github.com/terokarvinen/palettero](https://github.com/terokarvinen/palettero)


## Lähteet


Tehtävänanto: Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. Julkaistu 10.5.2024.[https://terokarvinen.com/tunkeutumistestaus/#h6-vuohi](https://terokarvinen.com/tunkeutumistestaus/#h6-vuohi)

Karvinen, Tero. Try Web Hacking on New Webgoat 2023.4. 13.11.2023. [https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/](https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/)

Non-Functional Club. Assignment 1 | Authentication Bypass | WebGoat | OWASP TOP 10 | Broken Authentication. Youtube. 1.11.2021.[https://www.youtube.com/watch?v=DErUuNMHgJo](https://www.youtube.com/watch?v=DErUuNMHgJo)

[Non-Functional Club. Assignment 1 | Authentication Bypass | WebGoat | OWASP TOP 10 | Broken Authentication. Youtube. 1.11.2021.](https://www.youtube.com/watch?v=DErUuNMHgJo)
