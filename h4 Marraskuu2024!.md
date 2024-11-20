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
- Ladataan ja kootaan John the Ripperin jumbo-versio ``git clone --depth=1 https://github.com/openwall/john.git`` ``cd john/src/`` ``./configure`` ``make -s clean && make -sj4`` ``cd ../run/``
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


## c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana


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







