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
- Ladataan ja kootaan John the Ripperin jumbo-versio ``git clone --depth=1 https://github.com/openwall/john.git`` ``cd john/src/`` ``./configure`` -> configure-komennon outputista saa kokoamiskomennon,  ``cd ../run/`` näkyy kaikki suoritettavat tiedostot ja scriptit
- Testitiedosto ladataan ``wget https://TeroKarvinen.com/2023/crack-file-password-with-john/tero.zip ``
- Otetaan tiedostosta hash ja siirretään se tiedostoon tero.zip.hash ``$HOME/john/run/zip2john tero.zip >tero.zip.hash``
- Hashia vastaan tehdään sanakirjahyökkäys ``$HOME/john/run/john tero.zip.hash``
- Puretaan tiedosto ``unzip tero.zip`` annetaan saatu salasana



Santos et al 2017: Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials (8 videos, about 30 min)
[https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/](https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/)

-
-
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



## d) Fuffme. Asenna Ffufme harjoitusmaali paikallisesti omalle koneellesi

Harjoitusmateriaali: [https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)


## e) Tiedosto. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki. Murra sen salaus.


## f) Tiiviste. Tee itse tai etsi verkosta salasanan tiiviste, jonka saat auki. Murra sen salaus


## g) Tee msfvenom-työkalulla haittaohjelma, joka soittaa kotiin (reverse shell). Ota yhteys vastaan metasploitin multi/handler -työkalulla


## Lähteet

- Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. Julkaistu 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024)

- Karvinen, Tero. Cracking Passwords with Hashcat. 6.4.2022. [https://terokarvinen.com/2022/cracking-passwords-with-hashcat/](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)

- Karvinen, Tero. Crack File Password With John. 9.2.2023. [https://terokarvinen.com/2023/crack-file-password-with-john/](https://terokarvinen.com/2023/crack-file-password-with-john/)

- Santos et al 2017: Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials (8 videos, about 30 min)
[https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/](https://www.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00/)

- Polop et al 2024: HackTricks: MSFVenom - CheatSheet [https://book.hacktricks.xyz/generic-methodologies-and-resources/reverse-shells/msfvenom](https://book.hacktricks.xyz/generic-methodologies-and-resources/reverse-shells/msfvenom)

- Karvinen, Tero. Fuffme - Install Web Fuzzing Target on Debian. 30.10.2023. [https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)







