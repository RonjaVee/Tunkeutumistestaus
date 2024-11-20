# h4 Marraskuu2024!

Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. Julkaistu 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024)

## x) Lue/katso ja tiivistä

Karvinen, Tero. Cracking Passwords with Hashcat. 6.4.2022. [https://terokarvinen.com/2022/cracking-passwords-with-hashcat/](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)

- Asennetaan tarvittavat paketit ``sudo apt-get -y install hashid hashcat wget``
- Tehdään kansio hommille ``mkdir hashed`` ``cd hashed``
- Hankitaan sanakirjatiedosto esim. Rockyou `` wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz`` ``tar xf rockyou.txt.tar.gz``  ``rm rockyou.txt.tar.gz``
- Määritellään hash-tyyppi ``hashid -m 6b1628b016dff46e6fa35684be6acc96``
- Käytetään sanakirjaa ja selvitetään hashista salasana ja tallennetaan tulos tiedostoon solved ``hashcat -m 0 '6b1628b016dff46e6fa35684be6acc96' rockyou.txt -o solved``

Karvinen, Tero. Crack File Password With John. 9.2.2023. [https://terokarvinen.com/2023/crack-file-password-with-john/](https://terokarvinen.com/2023/crack-file-password-with-john/)

- Asennetaan tarvittavat paketit ``sudo apt-get -y install micro bash-completion git build-essential libssl-dev zlib1g zlib1g-dev libbz2-1.0 libbz2-dev atool zip wget ``
- Ladataan ja kootaan John the Ripperin jumbo-versio ``git clone --depth=1 https://github.com/openwall/john.git`` ``cd john/src/`` ``./configure`` -> configure-komennon outputista saa kokoamiskomennon,  ``cd ../run/`` näkyy kaikki suoritettavat tiedostot ja scriptit: valitaan sen mukaan, millainen tiedostomuoto
- Testitiedosto ladataan ``wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip ``
- Otetaan tiedostosta hash ja siirretään se tiedostoon tero.zip.hash ``$HOME/john/run/zip2john tero.zip >tero.zip.hash``
- Hashia vastaan tehdään sanakirjahyökkäys ``$HOME/john/run/john tero.zip.hash``
- Puretaan tiedosto ``unzip tero.zip`` annetaan saatu salasana



Santos et al 2017: Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials (8 videos, about 30 min)
[https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/](https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/)

- Salasanat tallennetaan palvelimille hash-funktioina tiedostoihin tai tietokantoihin
- Heikot salasanat ja oletussalasanat on helpointa murtaa 
-

Polop et al 2024: HackTricks: MSFVenom - CheatSheet [https://book.hacktricks.xyz/generic-methodologies-and-resources/reverse-shells/msfvenom](https://book.hacktricks.xyz/generic-methodologies-and-resources/reverse-shells/msfvenom)

- 
-
-

## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana

Ekana ``sudo apt-get update``. Sitten asensin [ohjeen](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/) mukaiset paketit komennolla ``sudo apt-get -y install hashid hashcat wget``. Loin kansion hashed ja siirryin kansioon ``mkdir hashed`` ``cd hashed``. Latasin sanakirjatiedoston ``wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz``ja purin tiedoston kansioon. ``tar xf rockyou.txt.tar.gz`` ``rm rockyou.txt.tar.gz``.

![image](https://github.com/user-attachments/assets/90307991-b219-4901-8b1d-f72eb0554695)

Sitten loin hashin keksitystä salasanasta, jonka murran. Naisten nimiä löytyy rockyou:sta, joten kokeilin sellaista.

![image](https://github.com/user-attachments/assets/08e3e558-fa8e-4f02-82d1-8d21b0ba7f53)

Tutkin hashid:llä tunnistaako se hashtyypin ``hashid -m``. MD5 oli oikea, Hashcat mode 0.

![image](https://github.com/user-attachments/assets/bbe25f37-6694-4f4d-a8dc-7ccedf9841e5)

Sitten komennolla ``hashcat -m 0 'tähän hash' rockyou.txt -o solved`` mursin testisalasanan. Solved-tiedostosta löytyi vastaus.

![image](https://github.com/user-attachments/assets/093a1259-9f88-4a43-a0e0-29a519170dc4)


![image](https://github.com/user-attachments/assets/18734ecc-ed1f-4936-9414-841744483045)





## c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana

Päivitin taas paketit ja lähdin asentamaan paketteja, osa löytynee jo mutta kopioin komennon kuitenkin [ohjeen](https://terokarvinen.com/2023/crack-file-password-with-john/) mukaan: ``sudo apt-get -y install micro bash-completion git build-essential libssl-dev zlib1g zlib1g-dev zlib-gst libbz2-1.0 libbz2-dev atool zip wget``. Tuli virheilmoitus E: unable to locate package zlib-gst. Otin sen pois pakettilistalta, ja asensin muut samalla kun aloin tutkimaan, mistä on kyse. En löytänyt vastausta, joten jatkoin tehtävää.

Latasin John the Ripperin ja annoin ohjeen mukaiset komennot. ``git clone --depth=1 https://github.com/openwall/john.git`` ``cd john/src/`` ``./configure``. Configure-komennon outputista sain kokoamiskomennon: ``make -s clean && make -sj4``. -> "Make process complete". ``cd ../run/`` tästä kansiosta löytyi scriptejä ym.

![image](https://github.com/user-attachments/assets/41c28c5b-a940-4aa5-938e-a3d7c793a20b)

Annoin komennon ``$HOME/john/run/john`` ja tajusin tässä vaiheessa, että olin vielä hashed-kansiossa. Siirsin johnin kotihakemistooni ja homma toimi.

![image](https://github.com/user-attachments/assets/4d7bd963-9d49-4de3-a3e2-0ced9a8377a0)

Sitten latasin testitiedoston komennolla ``wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip``. Zip-tiedosto oli lukittu salasanalla.

Murtamiseksi, ensin otin tiedostosta hash ja sitten siirsin sen erilliseen tiedostoon tero.zip.hash: ``$HOME/john/run/zip2john tero.zip >tero.zip.hash``. Itse murtaminen tapahtui komennolla ``$HOME/john/run/john tero.zip.hash``. Salasana oli butterfly.

![image](https://github.com/user-attachments/assets/5b973bf0-001e-4fa5-be3e-0d3a09700ff3)

Purin zip-tiedoston ja sain sisällön haltuuni.

![image](https://github.com/user-attachments/assets/fb54e855-efcb-4312-bdda-37b49ce752f1)



## d) Fuffme. Asenna Ffufme harjoitusmaali paikallisesti omalle koneellesi ja ratkaise tehtävät

Harjoitusmateriaali ja ohjeet: [https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)

Asensin ensin ohjeen mukaisesti paketit ``sudo apt-get install docker.io git ffuf``; näistä tosin docker.io oli ainoa, joka puuttui. Komennoilla `` git clone https://github.com/adamtlangley/ffufme`` ``cd ffufme/`` ``sudo docker build -t ffufme`` rakensin harjoitusmaalin. Komennolla ``sudo docker run -d -p 80:80 ffufme`` maali laitettiin päälle. Testasin, että se näkyy curlilla ja localhostissa selaimella.

![image](https://github.com/user-attachments/assets/b5bd79e4-667c-4fb8-8396-4678095d092d)

Seuraavaksi tein kansion sanalistoille ``mkdir $HOME/wordlists`` ja siirryin kansioon ``cd $HOME wordlists``. Latasin sinne ohjeessa annetut sanalistatiedostot ``wget http://ffuf.me/wordlist/common.txt`` ``wget http://ffuf.me/wordlist/parameters.txt`` ``wget http://ffuf.me/wordlist/subdomains.txt``. Palasin sen jälkeen kotihakemistoon.

Testaamalla ``ffuf -w $HOME/wordlists/common.txt -u http://ffuf.me/cd/basic/FUZZ`` löytyi class ja development.log.

![image](https://github.com/user-attachments/assets/29ae57b8-0f8e-46ef-b12c-cd32ae6921c7)
![image](https://github.com/user-attachments/assets/da786df0-1454-4b5e-b3b2-49385ea6a532)


1. Sitten lähdin tekemään ffufin tehtäviä. Basic Content Discovery -tehtävän komento oli sama kuin ylläoleva.

2. Content Discovery With Recursion -tehtävän komento oli ``ffuf -w ~/wordlists/common.txt -recursion -u http://localhost/cd/recursion/FUZZ``, eli aikaisempaan lisätään -recursion. Tällä löytyi /admin, /admin/users ja /admin/users/96. Eli jos ffuf löytää hakemiston, se skannaa hakemiston sisällön ja toistaa tätä samaa kunnes ei löydy enempää skannattavaa.

![image](https://github.com/user-attachments/assets/2ec67391-2b3b-419e-9cef-980b088cdd22)
![image](https://github.com/user-attachments/assets/6084ab29-2a36-4334-b04a-d27e633587e0)


3. Tehtävässä Content Discovery With File Extensions komento oli ``ffuf -w ~/wordlists/common.txt -e .log -u http://localhost/cd/ext/logs/FUZZ``. -e .log tarkoittaa, että ffuf liittää common.txt-tiedostossa oleviin sanoihin .log-päätteen. Tehtävässä sanottiin, että ollaan löydetty /logs -hakemisto, jonka sisältöä ei voi katsoa, mutta voidaan olettaa, että se sisältää tiedostoja .log -päätteellä. Näin löytyi users.log.

![image](https://github.com/user-attachments/assets/f7881455-9e63-47df-b5af-eb2425992f97)
![image](https://github.com/user-attachments/assets/1ddaa8d3-d4b8-4be1-9c44-bc7b543e66f4)


4. No 404 Status -tehtävässä annettiin ensin peruskomento ``ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ``, joka tuotti pitkän listan tuloksia. Page Cannot be Found -sivujen koko oli kaikilla sama, joten tämän kokoiset tiedostot filtteröitiin ffufauksesta: ``ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ -fs 669``. Löytyi secret.

![image](https://github.com/user-attachments/assets/8ea2dc93-0245-4d11-8143-00d400b755f3)
![image](https://github.com/user-attachments/assets/beb356d1-7acd-46f7-8b73-95b0a5a47503)

![image](https://github.com/user-attachments/assets/43323438-0bba-4292-9b34-adadb066fc5a)
![image](https://github.com/user-attachments/assets/51438b00-f7bf-4228-b62d-74ed9da76d64)

5. Param Mining -tehtävässä haettiin ensin sivu localhost/cd/param/data. HTTP-statuskoodi oli 400 eli Bad Request.

![image](https://github.com/user-attachments/assets/706d7640-e7b5-45d3-b1df-1e4b096735a2)
![image](https://github.com/user-attachments/assets/f0451b92-aac6-43ed-8bf9-4c803a5422df)

Komennolla ``fuf -w ~/wordlists/parameters.txt -u http://localhost/cd/param/data?FUZZ=1`` sain puuttuvan parametrin: debug.

![image](https://github.com/user-attachments/assets/b54c3b55-cff5-438f-a64b-61df0d38e884)
![image](https://github.com/user-attachments/assets/28ef2cda-e5e2-47cc-9737-0239d64931af)

6. Rate Limited -tehtävän komennot antoivat pelkkää erroria?

7. Virtual Host Discovery -tehtävässä ``ffuf -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://localhost`` komennolla etsitään piilotettuja domaineja. Kaikki löydökset olivat samankokoisia, joten komennolla ``ffuf -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://localhost -fs 1495`` filtteröitiin 1495 tavun tulokset. Löytyi redhat.

![image](https://github.com/user-attachments/assets/d1dccc6b-2190-49f8-b4e3-a17e5349392d)

En tiedä löysinkö oikeaa sivua, mutta redhat.localhost oli sama kuin etusivu: 
![image](https://github.com/user-attachments/assets/8ffdaa77-6bb5-4f67-aa79-2e23ec5abb68)



## e) Tiedosto. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki. Murra sen salaus.

Tämän tehtävän tein Johnilla. Kysyin ChatGPT:ltä tapoja salakirjoittaa tiedosto (muu kuin zip) ja päätin kokeilla ehdotuksista 7zipiä. Asensin sen ensin komennolla sudo apt-get install p7zip-full, ja loin sitten tiedoston salaisuus.txt, jonne laitoin salaisen tekstin. Sitten ``7z a -p lukko.7z salaisuus.txt`` laitoin tiedostolle salasanan. Salattu tiedosto oli nyt nimeltään lukko.7z. Kysyin vielä ChatGPT:ltä, kuinka tälläinen tiedosto avataan, ja kokeilin komentoa ``7z x lukko.7z``, ja tiedosto oli toden totta salasanan takana.

![image](https://github.com/user-attachments/assets/6018dc6c-1a92-4785-a913-e873d85f94aa)
![image](https://github.com/user-attachments/assets/d63d0bb4-7be6-4555-8d59-95be78db5518)

Minun täytyi ensin selvittää, miten saan Johnilla käsiteltyä 7Zip-tiedostoja, joten etsin "7p" ../john/run/ -hakemistosta, ja kokeilin ensimmäistä tulosta. Loin Johnilla tiedostosta hashin komennolla ``$HOME/john/run/7z2john.pl lukko.7z > lukko.7z.hash``.

![image](https://github.com/user-attachments/assets/71d3039d-f25e-408e-a868-8b4cc4ecf869)
![image](https://github.com/user-attachments/assets/953aeaba-5240-4c07-8c30-d594e84a325f)

Sitten mursin salasanan hashin kautta komennolla ``$HOME/john/run/john lukko.7z.hash``. Tässä kohtaa koneen tuulettimet tekivät kovasti töitä ja aikaa kului. Lopulta salasana selvisi: 123456.

![image](https://github.com/user-attachments/assets/5691faaa-95fa-4b77-ae94-0c87d78147a0)

Kokeilin salasanaa. Olin unohtanut poistaa salaisuus.txt omasta kotihakemistosta, joten annoin tämän puretun tiedoston korvata nyt sen. ``cat salaisuus.txt`` nähtiin sitten sen sisältö.

![image](https://github.com/user-attachments/assets/736eb244-cb68-44b9-8af2-562053242be8)
![image](https://github.com/user-attachments/assets/d063a566-2495-4382-b4aa-8851faca9ebf)



## f) Tiiviste. Tee itse tai etsi verkosta salasanan tiiviste, jonka saat auki. Murra sen salaus

Hyödynsinpä tässäkin tekoälyä: kysyin ChatGPT:ltä murrettavaa hashia. Nyt on jokin yllätysmomentti tässä tehtävässä.

![image](https://github.com/user-attachments/assets/7952eb91-f678-4412-8f74-2a66a9829dd1)

Siirryin hashed-kansioon, jonka loin aiemmassa tehtävässä ja joka sisältää rockyou.txt -tiedoston. Tutkin sitten tiivisteen tyyppiä ``hashid -m``. SHA-1 oli ensimmäinen vaihtoehto, joten valitsin sen (Hashcat Mode 100).

![image](https://github.com/user-attachments/assets/dee70c29-2a8e-42f5-a9e7-e3f95d481c66)

``hashcat -m 100 'tiiviste' rockyou.txt -o solved2`` lähdin murtamaan salasanaa, tulokset tiedostoon solved2. Statukseksi tuli exhausted, eli salasana ei selvinnyt. Päätin vaihtaa funktiota, eli hashid seuraava vaihtoehto (4500). Taas tuli exhausted. Kokeilin seuraavaa (6000). Taas sama. Aloin epäillä, käyttikö ChatGPT jotain suomalaista salasanaa, jota ei löydy rockyousta. Pyysin siis englanniksi uuden hashin.

![image](https://github.com/user-attachments/assets/a60e4a05-604b-42be-ad81-f6a133caf1c2)

Hashid antoi tutumpia tuloksia. Valitsin MD5 eli 0, koska se on yleinen.

![image](https://github.com/user-attachments/assets/45d6db9d-2ec6-4f00-8e57-edf8628e7f92)

Onnistui ekalla. Salasana oli abc123.

![image](https://github.com/user-attachments/assets/a12d1620-f919-421a-a73e-b5ab60ac8fac)



## g) Tee msfvenom-työkalulla haittaohjelma, joka soittaa kotiin (reverse shell). Ota yhteys vastaan metasploitin multi/handler -työkalulla


## Lähteet 

- Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. Julkaistu 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024)

- Karvinen, Tero. Cracking Passwords with Hashcat. 6.4.2022. [https://terokarvinen.com/2022/cracking-passwords-with-hashcat/](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)

- Karvinen, Tero. Crack File Password With John. 9.2.2023. [https://terokarvinen.com/2023/crack-file-password-with-john/](https://terokarvinen.com/2023/crack-file-password-with-john/)

- Santos et al 2017: Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials (8 videos, about 30 min)
[https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/](https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/)

- Polop et al 2024: HackTricks: MSFVenom - CheatSheet [https://book.hacktricks.xyz/generic-methodologies-and-resources/reverse-shells/msfvenom](https://book.hacktricks.xyz/generic-methodologies-and-resources/reverse-shells/msfvenom)

- Karvinen, Tero. Fuffme - Install Web Fuzzing Target on Debian. 30.10.2023. [https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)







