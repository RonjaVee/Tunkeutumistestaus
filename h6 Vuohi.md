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

Asensin WebGoatin x. -kohdan komennoilla. Tarkistin palomuurin statuksen `sudo ufw status`, sitten tein käyttäjän ja kirjauduin sisään.

![image](https://github.com/user-attachments/assets/14831161-2027-49f1-93f1-fa48a8e94bfd)


Vielä piti saada ZAPissa näkymään WebGoat. Käynnistin ZAPin. Jostain syystä sivu antoi 404, joten käynnistin WebGoatin uudestaan. Sitten sivu toimi ja näkyi ZAPissa.


![image](https://github.com/user-attachments/assets/17234f02-b22e-4f03-9d6d-22f86193d527)



## Ratkaise WebGoat 2023.4


### b) (A1) Broken Access Control

**Hijack a session (1)**

Tämä oli liian vaikea :(

**Insecure Direct Object References (4)**

Kohta 2. Ensimmäisessä osiossa tuli kirjautua sisään tunnuksilla tom, cat. 
Kohta 3. Seuraavalla sivulla piti etsiä, mitä lisätietoja profiilista löytyi raw responsesta sivulla näkyvän lisäksi. Löytyi role ja userId.

![image](https://github.com/user-attachments/assets/d50e10c9-3da7-499c-bd2d-6290059d93fa)

![image](https://github.com/user-attachments/assets/a76aca43-d7de-4e28-85d9-46769eb13d2d)

**KOHTA 4.** Seuraavassa osiossa piti miettiä, mikä URL on suora polku käyttäjälle tom. Vastaus oli /WebGoat/IDOR/profile/2342384. Mallina tuli käyttää aiemman tehtävän requestia, ja vinkkinä oli, että muutos olisi vain pieni lisäys. Kokeilemalla löytyi ratkaisu.

![image](https://github.com/user-attachments/assets/c2d1cff8-6902-40a1-be6a-b57d348ec5cf)



![image](https://github.com/user-attachments/assets/72607ca2-c465-430d-89f9-4a2e0c4857e6)

**KOHTA 4 LOPPU**

Kohta 5. (En saanut tehtyä loppuun) Seuraavan tehtävän kohdalla tutkin taas vinkkejä. View profile antoi 500 erroria. Eli piti etsiä muita profiileita, ja vinkkinä oli käyttää fuzzeria. Niinpä askartelin ZAPin fuzzerilla hetken aikaa, kunnes sain sen toimimaan. Tarkoituksena oli kokeilla userId-kohtaan eri numeroita. 


![image](https://github.com/user-attachments/assets/72607ca2-c465-430d-89f9-4a2e0c4857e6)


![image](https://github.com/user-attachments/assets/bf50ea31-98ac-4534-8e4f-cf1b363c2345)


![image](https://github.com/user-attachments/assets/5054fd5e-f5c7-4524-8b02-e6b2c410e8c2)

Kokeiltavia lukuja oli kyllä ihan liikaa, joten rajasin scopea. Koneeni puhkui ja puhisi, jos lukuja piti tykittää liikaa.

![image](https://github.com/user-attachments/assets/00ca6a62-45b1-4b6a-a12a-acf0933f227c)

Osumia tuli useampia.

![image](https://github.com/user-attachments/assets/85cd26e2-79df-4468-9c2b-0602cf9b39b8)

Ei hajuakaan, kuinka tässä edetä. Etsin ohjeita, mutta niistäkään ei ollut apua nyt. Mutta tämähän oli ylimääräistä työtä, tehtävänannossa oli vain kohta 4!



**Missing Function Level Access Control (2)**


### c) (A7) Identity & Auth Failure

**Authentication Bypasses (1)**

Kohta 2. Tehtävässä piti ohittaa 2FA ja asettaa uusi salasana käyttäjälle. Vinkkinä oli muokata secQuestion-parametrejä.

Käytin tukena ohjevideota [Non-Functional Club. Assignment 1 | Authentication Bypass | WebGoat | OWASP TOP 10 | Broken Authentication. Youtube. 1.11.2021.](https://www.youtube.com/watch?v=DErUuNMHgJo)

Muokkasin secQuestion0 ja secQuestion1 -parametreja.

![image](https://github.com/user-attachments/assets/3b49c167-1dce-4a96-9365-3765a46b8f2c)

![image](https://github.com/user-attachments/assets/a770d99b-3743-4ec4-8e02-697b0c0f6df2)

Tehtävä muuttui vihreäksi, mutten päässyt sivulla laittamaan uutta salasanaa?

![image](https://github.com/user-attachments/assets/52cb1178-103c-4bfc-82ac-8ae073df6643)



**Insecure Login (1)**

Kohta 1. oli vain ohjeita.

Kohta 2. Tässä tehtävässä piti ZAPilla napata toisen henkilön tunnus ja salasana. Käyttäjätunnus ja salasana näkyivät selkokielisenä pyynnössä.

![image](https://github.com/user-attachments/assets/33743e5c-45b3-43ed-bd64-c1107ab84ea1)

![image](https://github.com/user-attachments/assets/6c9821fd-f5b3-432a-9d6a-1dcc35b8580b)



### d) (A10) Server-side Request Forgery

**Server-Side Request Forgery (2)**

Kohta 1. oli vain ohjeita.

Kohta 2. Ensin tuli löytää juusto sivulta. Tomin sijaan etsittiin Jerryn kuvaa, jerry.jpg. kuva löytyi, mutta miten sen sai sivulle näkyviin? Kokeilin ZAPilla monenlaista, mutten onnistunut.

![image](https://github.com/user-attachments/assets/22d975d7-ab29-42c7-84b9-509fb6dcb7de)

Etsin siis ohjevideon. [PseudoTime. WebGoat 8 Server Side Request Forgery 2. Youtube, 12.6.2021.](https://www.youtube.com/watch?v=ccCP4STs968) Tässä hyödynnettiin Firefoxin omaa dev toolsia: klikkaamalla oikealla hiiren painikkeella Steal the cheese -kohtaa ja avaamalla Inspect pystyi muokata kohtaa value=images/tom.jpg. Muokkaamisen jälkeen enter -> kun Steal the cheese -kohdasta klikkasi nyt, saatiin juusto napattua ja Jerry näkyviin.

![image](https://github.com/user-attachments/assets/49b0d9db-7213-4851-b084-e1208b52bcf9)

![image](https://github.com/user-attachments/assets/a76ecc58-a060-42ac-af86-2aa48a2b4bf3)


Kohta 3. Seuraavaksi piti muuttaa pyyntöä niin, että palvelin saa informaatiota http://ifconfig.prosta. (tämä olikin näköjään ylimääräinen tehtävä)

![image](https://github.com/user-attachments/assets/ee264bbb-0cf5-4582-bfe5-50b4e7d2d095)


![image](https://github.com/user-attachments/assets/3922b409-3495-414d-85d6-d429881207c2)

Kokeilin samaa metodia kuin aiemmin, ja se toimi!

![image](https://github.com/user-attachments/assets/75bf9813-a90f-402d-a2f5-7afb86ec6e7f)

![image](https://github.com/user-attachments/assets/c33e98b1-81fb-45d9-b2c2-266b6e2bd5a3)



### e) Client side

**Bypass front-end restrictions (2)** 

Kohta 2. Tehtävänä oli ohittaa kenttien rajoitukset. Painoin submit ja etsin pyynnön ZAPista ja siirsin sen Requesteriin.

![image](https://github.com/user-attachments/assets/a5c2649e-5ca7-428c-b778-77bea47584ae)

![image](https://github.com/user-attachments/assets/bffa7ad0-3e2c-4d03-b8fb-bf4bdec1a03e)

En tiennyt, mitä tehdä, sillä mitään vinkkejä ei ollut. Niinpä etsin taas opasvideon: [PseudoTime. WebGoat 8 Bypass front end restrictions Field Restrictions. Youtube, 13.6.2021.](https://www.youtube.com/watch?v=731dsnlKoFk) Eli muokkasin 

![image](https://github.com/user-attachments/assets/29b63ed7-8943-4c94-b940-12d6e43a594b)







## f) Editmenu. Lisää uusi oma komento micro:n palettero-lisäkkeellä käytettäväksi

[https://github.com/terokarvinen/palettero](https://github.com/terokarvinen/palettero) Palettero oli jo asennettuna. `micro plugin-install palettero`. Tein tiedoston `micro testi` ja tallensin tiedostoon tekstiä.

![image](https://github.com/user-attachments/assets/bd50b72a-550b-4af8-940e-7db8b6e57170)


CTRL + välilyönti aukesi valikko, sieltä editmenu.

![image](https://github.com/user-attachments/assets/7ab5b872-78b2-42a3-a80c-e83264247487)

Keksin satunnaisen greppauksen ja kokeilin sitä, mutten saanut sitä tallentumaan tai toimimaan? Kokeilin valmiita komentoja, ja ainakin osa niistä toimi, esim. colorschemet. En enää muista, mitä tunnilla tehtiin :(

![image](https://github.com/user-attachments/assets/07789eea-37e1-4537-93b8-87f46fbb008f)

Kokeilin sitten muokata tuota palettero.cfg -tiedostoa, kun se tuossa näkyy.

![image](https://github.com/user-attachments/assets/e2a613d9-3b71-4736-9df8-2e588f7779d2)

Nyt toimi, tein ihan hölmön komennon ihan vain testiksi. Eli ainakin tätä kautta onnistuu.

![image](https://github.com/user-attachments/assets/aa44ed8b-29a2-4f61-a610-88f559f82b7e)

![image](https://github.com/user-attachments/assets/2a280be9-b93b-4aff-b9c4-37aef7f04f13)



## Lähteet


Tehtävänanto: Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. Julkaistu 10.5.2024.[https://terokarvinen.com/tunkeutumistestaus/#h6-vuohi](https://terokarvinen.com/tunkeutumistestaus/#h6-vuohi)

Karvinen, Tero. Try Web Hacking on New Webgoat 2023.4. 13.11.2023. [https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/](https://terokarvinen.com/2023/webgoat-2023-4-ethical-web-hacking/)

Non-Functional Club. Assignment 1 | Authentication Bypass | WebGoat | OWASP TOP 10 | Broken Authentication. Youtube, 1.11.2021.[https://www.youtube.com/watch?v=DErUuNMHgJo](https://www.youtube.com/watch?v=DErUuNMHgJo)

PseudoTime. WebGoat 8 Server Side Request Forgery 2. Youtube, 12.6.2021. [https://www.youtube.com/watch?v=ccCP4STs968](https://www.youtube.com/watch?v=ccCP4STs968)

PseudoTime. WebGoat 8 Bypass front end restrictions Field Restrictions. Youtube, 13.6.2021.[https://www.youtube.com/watch?v=731dsnlKoFk](https://www.youtube.com/watch?v=731dsnlKoFk)

