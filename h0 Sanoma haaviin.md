## h0 Sanoma haaviin

a. Sieppaa ja analysoi verkkoliikennettä.

Tero Karvisen [kurssitehtävässä](https://terokarvinen.com/tunkeutumistestaus/) esimerkkityökaluiksi annettiin ngrep, Wireshark, Firefox F12 Network, tcpdump. Näistä valitsin Firefox F12 Networkin, sillä käytän Firefoxia selaimena,
ngrep ei auennut minulle ja Wiresharkilta en löytänyt Linuxille versiota. Ohje löytyi sivulta [https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/). Käytännössä Firefoxissa saa paneelin auki näppäimillä ``ctrl + shift + E``. Näkymä Network-välilehdellä oli sivun päivityksen 
jälkeen seuraava:

![image](https://github.com/user-attachments/assets/cddc199d-70d1-4abe-a6b7-59e180a2d94e)

Vasemmassa sarakkeessa näkyy pyyntöjen statukset: esim. vihreällä taustalla 200 tarkoittaa onnistunutta vastausta. Method-sarakkeella näkyy pyynnön tyyppi, GET-pyyntöjä näkyy enimmäkseen eli informaatiota noudetaan API:sta. Domain-sarakkeessa näkyy pyydetyn polun domain, Filestä eteenpäin tietoja tiedostotyypistä.

Klikkaamalla yhtä pyyntöä aukesi seuraava, yksityiskohtaisempia tietoja sisältävä näkymä välilehtineen (Headers, Cookies, Request, Response, Timings, Security).

![image](https://github.com/user-attachments/assets/2bf73d0e-dcc1-4d05-ad14-0ef2d72dfab2)

Aukeavalla Headers-välilehdellä on esim. tietoa pyynnöstä sekä vastauksesta (Response Headers ja Request Headers), HTTP:n versio.

## Lähteet

Karvinen, Tero. Tunkeutumistestaus-kurssi. Kurssisivu: [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/) Luettu 25.10.2024.

Firefox Source Docs. Network monitor. [https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/) Luettu 25.10.2024.

Lähteet lisätty arviointien jälkeen, unohtui kiireessä merkata.
