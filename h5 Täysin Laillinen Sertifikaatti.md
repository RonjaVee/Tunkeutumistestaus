# h5 Täysin Laillinen Sertifikaatti

Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024](https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024)

## x) Lue/katso ja tiivistä

### OWASP. A01:2021-Broken Access Control. 2021.

- Broken Access Control yleisin haavoittuvuus
- Johtaa tietovuotoon, tietojen muokkaamiseen luvatta tai oikeuksien laajentamiseen
- Tyypillisiä ongelmia: parametrimanipulaatio, API-pyyntöjen väärinkäyttö ja pakotettu selaaminen
- Yleisiä virheitä ovat CORS-konfiguraatiovirheet ja liian laajat käyttöoikeudet
- Ennaltaehkäisy: "deny by default", pääsynvalvonnan keskittäminen ja lokit
- Tapahtuu esim. niin, että hyökkääjä muuttaa URL-parametria ja pääsee sitä kautta toisen käyttäjän tietoihin

### OWASP. A10:2021-Server-Side Request Forgery (SSRF). 2021.

- SSRF (Server-Side Request Forgery): sovellus ohjataan tekemään pyyntöjä odottamattomiin kohteisiin
- Aina käyttäjän syöttämiä URL-osoitteita ei validoida; modernit web-sovellukset alttiimpia
- Ehkäisy: Verkon tasolla estetään liikenne oletuksena "deny by default" ja eristetään resurssipyyntöjä eri verkkoihin
- Sovellustasolla validoidaan ja sallitaan vain turvalliset URL-mallit, estetään HTTP-uudelleenohjaukset ja käsitellään tietoturvapalvelut erillisillä järjestelmillä
- Johtaa esim. arkaluontoisiin tietoihin pääsyyn tai sisäisten palveluiden hyväksikäyttöön, esim. etäkoodin suoritus (RCE) tai palvelunestohyökkäys (DoS)

### PortSwigger. Web Security Academy. 

Kohdat: Insecure direct object references (IDOR), Path traversal, Server-side template injection, Server-side request forgery (SSRF), Cross-site scripting

- **Insecure Direct Object References (IDOR)**: Haavoittuvuus, jossa sovellus käyttää käyttäjän syöttämää tietoa jolloin käyttäjä pääsee käsiksi resursseihin ilman riittävää pääsynhallintaa
    - Esimerkkejä: IDOR voi ilmetä esimerkiksi muuttamalla URL-parametria (esim. asiakasnumero) tai staattisen tiedoston nimeä (esim. chat-lokitiedosto) ja saada luvattoman pääsyn muiden käyttäjien tietoihin

- **Path traversal**: hyökkäyksillä hyökkääjä voi lukea palvelimen tiedostoja, esim. sovelluskoodia, järjestelmätietoja, arkaluonteisia käyttöjärjestelmän tiedostoja
    - Hyödynnetään hakemistorakenteen "../" tai ".." -sekvenssejä ohittamalla suojaukset, kuten tiedostopolun rajoitukset
    - Esimerkkejä: URL-pyynnöllä, kuten ?filename=../../../etc/passwd, voidaan lukea tiedostoja palvelimen juurihakemistosta

- **Server-side template injection (SSTI)**: hyökkäyksessä hyökkääjä injektoi haitallisen koodin palvelinpuolen templaattiin, joka suoritetaan palvelimella ja voi johtaa koodin suoritukseen
    - Hyökkäykset syntyvät, kun käyttäjien syötteet liitetään suoraan templaattiin ilman riittäviä suojauksia

- **Server-side request forgery (SSRF)**: haavoittuvuus, jossa hyökkääjä pakottaa palvelimen tekemään pyynnön ei-toivottuun sijaintiin, mikä voi paljastaa arkaluontoista tietoa tai suorittaa toimintoja
    - Hyökkäyksessä hyödynnetään luottamusvälejä, esim. sisäisten järjestelmien pääsyä
    - Esimerkki: hyökkääjä muuttaa palvelimelle lähetetyn pyynnön osoitteen paikalliseksi (localhost tai 127.0.0.1) -> palvelin tekee pyynnön itseensä ja ohittaa pääsynvalvonnan

- **Cross-site scripting (XSS)**: verkkoturvahaavoittuvuus, jossa hyökkääjä upottaa haitallista JavaScriptiä verkkosivustolle, jolloin hän voi kaapata käyttäjän tietoja



## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Laita ZAP sieppaamaan myös kuvat, niitä tarvitaan tämän kerran kotitehtävissä. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään

Löysin ohjeeksi TheDutchHackerin [Configure OWASP Zap with Firefox -oppaan](https://thedutchhacker.com/configure-owasp-zap-with-firefox/).

OWASP ZAP:in asensin ``sudo apt-get install zaproxy`` ja käynnistin sen komennolla ``zaproxy``. Sitten etsinkin jonkin aikaa, mistä pääsen luomaan sertifikaatin (luki tehtävänannon ohjeissa): Tools -> Options -> Network -> Server Certificates. Tuli ilmoitus, että Root CA Sertificate on jo olemassa. Loin uuden tilalle ja tallensin sen kotihakemistooni.

Firefoxissa menin kohtaan Settings -> Privacy and Security -> Certificates. View Certificates -kohdassa Authorities-välilehdellä valitsin luomani sertifikaatin kotihakemistostani ja hyväksyin kohdan "Trust this CA to identify websites". Sitten loin tunnilla asentamaani FoxyProxyyn Zap-nimisen proxyn (Hostname 127.0.0.1, portti 8080).

![image](https://github.com/user-attachments/assets/1226b538-1ad0-4341-be13-1bcf1febd3f9)


![image](https://github.com/user-attachments/assets/097945e7-e9d0-4ca7-90c2-c6a9b245b91c)

Kokeilin sitten katsoa localhostilla, näkyykö Zapissa mitään. Tuli virheilmoitus 502, ja tajusin sitten, että Apache täytyy varmaankin käynnistää ensin. ``sudo systemctl start apache2`` ja nyt localhostissa näkyi oletussivu. 

![image](https://github.com/user-attachments/assets/b934bb70-1003-475a-88f8-6d0caefbd3d0)

Jotta kuvatkin näkyisivät, tehtävänannossa vinkattiin valitsemaan asetuksista Tools -> Options -> Display. Sieltä löytyikin täppä "Process images in HTTP requests/responses. CTRL + F5 latasin sivun uudelleen, ja nyt kuvakin näkyi Zapissa.

![image](https://github.com/user-attachments/assets/7855659a-717c-4f79-92ae-a43eac070ddd)


## b) Kettumaista. Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen. Käytä FoxyProxyn "Patterns" -toimintoa, niin että vain valitsemasi weppisivut ohjataan Proxyyn

Koetin tehdä tätä jo 28.11., mutta en saanut Patternsia toimimaan, ja sitten koneeni ylikuumeni ja kaatui jälleen, jolloin myös tallentematon raportti hävisi... Siispä uudestaan.

Tutkin ensin, miten käyttää FoxyProxyn Patternsia FoxyProxyn ohjesivulta [URL Patterns](https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/). Ohjeessa kerrottiin, kuinka URL pitäisi muotoilla. Localhost: ``*://localhost/*``, PortSwigger: ``*.portswigger.net/*``. 

![image](https://github.com/user-attachments/assets/45e448e2-9d37-421b-8d22-d7886cafb939)

Tuli ongelmia. Mitään ei näkynyt ZAPissa.

![image](https://github.com/user-attachments/assets/97820f35-8c88-44e4-bbad-1ffd63b9c5a6)

Kysyin ChatGPT:ltä miksei toimi, ja se ehdotti toisenlaista tapaa muotoilla URL.

![image](https://github.com/user-attachments/assets/cea49cdf-dfe1-411f-9134-7ad7bcf01a31)

Sivu aukesi nyt normaalisti, mutta ZAPissa ei näy vieläkään mitään. Pidin kokoajan huolta, että FoxyProxylla on valittuna proxy by patterns.

![image](https://github.com/user-attachments/assets/d7873fe6-89be-469a-ba1b-4a6641a59e3c)

Käynnistin ZAPin uudelleen, nyt eri tavalla. Laitoin Firefoxiin uuden sertifikaatin jonka tallensin ZAPista.

![image](https://github.com/user-attachments/assets/97bb8206-a5b2-4a86-929c-09d255b0c60b)

Kokeilin selaimella valita FoxyProxysta ensin Proxy by patterns ja sitten pelkkä ZAP. Ahaa, nyt onnistuikin. Ei hajuakaan mistä kiikasti, mutta tämän ongelman takia en päässyt suorittamaan noita c-kohdan tehtäviä ajallaan. Siispä teen ne myöhemmin.

![image](https://github.com/user-attachments/assets/34eb6fc4-a404-41a9-b07f-e0658f8ec767)

![image](https://github.com/user-attachments/assets/54cb641c-af8b-4c54-9ef9-036950a60a53)


## PortSwigger Labs. Ratkaise tehtävät. Selitä ratkaisusi: mitä palvelimella tapahtuu, mitä eri osat tekevät, miten hyökkäys löytyi, mistä vika johtuu

Nämä tehtävät tehty 1.12.2024.

### c. IDOR

Tehtävässä piti kaapata carlos-käyttäjän salasana ja kirjautua tämän tilille. Tehtävänanto vinkkasi tarkastelemaan sivuston chat-toimintoa: chat-logit tallennetaan suoraan palvelimen hakemistoon.

Chatissa pystyi keskustelemaan botin kanssa. View transcript -kohdasta klikkaamalla latautui transcript-tiedosto.

![image](https://github.com/user-attachments/assets/48631008-e693-43ac-a54f-d8272a62d4c4)

![image](https://github.com/user-attachments/assets/41b02ef0-859f-450a-922b-0175e88eed7b)

Etsin ZAPista pyynnön, jolla tiedosto ladattiin.

![image](https://github.com/user-attachments/assets/8b7fb6db-3d5f-4c4f-87c9-d493cf4be376)

Katsoin Portswiggeristä ratkaisun tehtävään: eli requestia piti muokata siten, että se haki palvelimelta aiemman chat login, 1.txt. Logista löytyi salasana.

![image](https://github.com/user-attachments/assets/a1239775-a340-4d40-a25c-273112a780f3)

Kirjauduin carlosin tilille salasanalla ja tehtävä suoritettu.

![image](https://github.com/user-attachments/assets/ac5e739f-2b6f-47f3-8d66-6ccd7caf7b00)


### d. Path traversal


**Simple case**: Tehtävässä piti ladata /etc/passwd -tiedosto käyttäen tuotteiden kuvapyynnöistä löytyvää haavoittuvuutta. Avasin siis labran verkkokaupan sivulta tuotteen ja etsin ZAPista .jpg -tiedoston pyynnön.

![image](https://github.com/user-attachments/assets/f5920cf4-bbfd-4878-b564-df13033df5c4)

Koska tehtävät olivat haastavia, käytin taas kävelykeppinä tehtävän ratkaisuohjetta. Eli hakupyynnön filename-parametrille annetaan arvo ../../../etc/passwd.

Kokeilin muokata pyyntöä ZAPissa, mutten tiedä oliko hakupyyntö oikea, content type image? Kokeilin myös selaimella, mutten löytänyt tiedostoa.

![image](https://github.com/user-attachments/assets/1db9bf97-8c53-4502-9ad6-d5e4a7595bbe)

![image](https://github.com/user-attachments/assets/c32cbf28-6eb6-43df-a4d4-2c282a02f1e6)

Muokkailin URLia, kunnes tajusin, että labra näytti solved?

![image](https://github.com/user-attachments/assets/c80578da-f49c-40aa-bc39-aabf1f4ddde4)

Aivan, ZAPissa voi vaihtaa hakupyynnön tulos tekstiksi. Kesti hetki tutkia ZAPin ominaisuuksia.

![image](https://github.com/user-attachments/assets/3264de5d-97e2-4f7e-864a-f22ce69c2e47)


**Traversal sequences blocked with absolute path bypass**

Myös tässä tehtävässä haavoittuvuus on kuvissa, eli etsin ZAPissa .jpg -pyynnön.

![image](https://github.com/user-attachments/assets/cf182d80-38b5-477a-a142-fbfea8baeca8)

Luntattuani taas ratkaisukohdasta: Filename-kohdan tiedostonimen vaihtamalla /etc/passwd pääsi haluttuun tiedostoon käsiksi. Kuva piti taas vaihtaa tekstiksi body-kohdasta ZAPissa.

![image](https://github.com/user-attachments/assets/c643c8c4-25a9-4833-beee-3ece47a9e85c)

![image](https://github.com/user-attachments/assets/bb035296-ed50-46ba-9f56-8cf31e9594a3)



**Traversal sequences stripped non-recursively**

Tavoite oli taas sama, päästä käsiksi /etc/passwd, ja haavoittuvuus löytyi samasta kohtaa. Eli etsin ensin oikean hakupyynnön ZAPissa.

![image](https://github.com/user-attachments/assets/858a8892-ed0a-4d7d-a34e-84569517d5a5)

Kurkkasin ratkaisuun, ja nyt eroa olikin hakemistorakenteen ".." -sekvensseissä, eli path traversalin sekvenssit poistetaan toistuvasti (eli kuten tehtävän nimessä lukee, rekursiivisesti), kunnes niitä ei ole enää jäljellä. Tässä tehtävässä siis riitti, että .. toistettiin kahdesti, jotta suojauksen pystyi ohittamaan.

![image](https://github.com/user-attachments/assets/a6ee6922-4f72-4000-ad7c-2c3410e19e42)

### g. Server-side template injection with information disclosure via user-supplied objects (SSTI)




## k) Asenna pencode ja muunna sillä jokin merkkijono (encode a string)

Luin sivulta [https://github.com/ffuf/pencode](https://github.com/ffuf/pencode) mitä pencoden asennus vaatii. Tarkastinko, onko minulla go:ta ``go version``-> ei ollut, ehdotus oli asentaa se ``sudo apt-get install golang-go``. Sitten ``go install github.com/ffuf/pencode/cmd/pencode@latest``. Tämä ei kuitenkaan asentunut, kun kokeilin ``pencode version``. Ehdotuksena oli asentaa se ``sudo apt install golang-github-ffuf-pencode-dev``, valitsin yes ja kokeilin uudelleen, asentuiko. Asentuihan se. 

![image](https://github.com/user-attachments/assets/0ff11f6a-776d-425c-929e-939a0e4ae2c3)


Sivulta löytyvän ohjeen mukaan kokeilin muuntaa merkkijonon "olen omena". Käytin komentoa ``echo 'olen omena'|pencode [muoto]``. Muodot: urlencode b64encode hexencode. Urlencode: Koodaa merkkijonon URL-muotoon, b64encode: Koodaa merkkijonon Base64-muotoon, hexencode: Koodaa merkkijonon heksadesimaalimuotoon.


![image](https://github.com/user-attachments/assets/c21fe116-370a-4495-ae34-091a17d43896)




## Lähteet

- Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024](https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024)

- OWASP. A10:2021-Server-Side Request Forgery (SSRF). 2021. [https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/](https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/)

- OWASP. A01:2021-Broken Access Control.2021. [https://owasp.org/Top10/A01_2021-Broken_Access_Control/](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

- PortSwigger. Web Security Academy. [https://portswigger.net/web-security](https://portswigger.net/web-security)
    Kohdat:
    - Insecure direct object references (IDOR)
    - Path traversal
    - Server-side template injection
    - Server-side request forgery (SSRF)
    - Cross-site scripting

- Dutch Hacker. Configure OWASP ZAP with Firefox. The Dutch Hacker. Luettu 27.11.2024. [https://thedutchhacker.com/configure-owasp-zap-with-firefox/](https://thedutchhacker.com/configure-owasp-zap-with-firefox/)

- FoxyProxy. URL patterns. FoxyProxy Knowledge Base. Luettu 27.11.2024. [https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/](https://help.getfoxyproxy.org/index.php/knowledge-base/url-patterns/)

- Hoikkala, Joona. Pencode. GitHub. Luettu 27.11.2024. [https://github.com/ffuf/pencode](https://github.com/ffuf/pencode)

- OpenAI. ChatGPT. 2024.  [https://chat.openai.com](https://chat.openai.com) Kysytty URL:n muotoilu FoxyProxyyn ja muita asetuksia FoxyProxyyn, Firefoxiin ja ZAPiin
