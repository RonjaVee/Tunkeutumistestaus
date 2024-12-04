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
