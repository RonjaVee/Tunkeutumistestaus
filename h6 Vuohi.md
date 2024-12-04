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

Tein käyttäjän ja kävin tehtävien kimppuun.

## Ratkaise WebGoat 2023.4


### b) (A1) Broken Access Control

**Hijack a session (1)**

**Insecure Direct Object References (4)**

**Missing Function Level Access Control (2)**


### c) (A7) Identity & Auth Failure

**Authentication Bypasses (1)**

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

