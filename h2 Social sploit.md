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


## c) Etsi Metasploitable porttiskannaamalla (db_nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable


## d) Porttiskannaa Metasploitable perusteellisesti. Tallenna tulokset Metasploitin tietokantoihin (db_nmap) ja tiedostoihin (nmap -oA foo)


## e) Tarkastele Metasploitin tietokantoihin tallennettuja tietoja komennoilla "hosts" ja "services". Kokeile suodattaa näitä listoja tai hakea niistä


## f) Vertaile nmap:n omaa tiedostoon tallennusta ja db_nmap:n tallennusta tietokantoihin. Mitkä ovat eri tiedostomuotojen ja Metasploitin tietokannan hyvät puolet?


## g) Murtaudu Metasploitablen vsftpd-palveluun


## h) Päivitä äskeisen vsftpd-murron yhteydessä syntynyt sessio meterpretriin


## i) Kerää levittäytymisessä (lateral movement) tarvittavaa tietoa metasploitablesta. Analysoi tiedot. Selitä, miten niitä voisi hyödyntää


## j) Murtaudu Metasploitableen jollain toisella tavalla


## k) Demonstroi Meterpretrin ominaisuuksia


## l) Tallenna shell-sessio tekstitiedostoon script-työkalulla (script -fa log001.txt)


## Lähteet

Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit
