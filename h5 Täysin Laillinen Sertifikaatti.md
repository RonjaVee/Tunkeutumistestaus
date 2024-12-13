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

- **Cross-site scripting (XSS)**: verkkoturvahaavoittuvuus, jossa hyökkääjä upottaa haitallista JavaScriptiä verkkosivustolle



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

**Insecure direct object references**

Tehtävä: [https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)

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


**Simple case**: 
Tehtävä: [https://portswigger.net/web-security/file-path-traversal/lab-simple](https://portswigger.net/web-security/file-path-traversal/lab-simple).

Tehtävässä piti ladata /etc/passwd -tiedosto käyttäen tuotteiden kuvapyynnöistä löytyvää haavoittuvuutta. Avasin siis labran verkkokaupan sivulta tuotteen ja etsin ZAPista .jpg -tiedoston pyynnön.

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

Tehtävä: [https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)

Myös tässä tehtävässä haavoittuvuus on kuvissa, eli etsin ZAPissa .jpg -pyynnön.

![image](https://github.com/user-attachments/assets/cf182d80-38b5-477a-a142-fbfea8baeca8)

Luntattuani taas ratkaisukohdasta: Filename-kohdan tiedostonimen vaihtamalla /etc/passwd pääsi haluttuun tiedostoon käsiksi. Kuva piti taas vaihtaa tekstiksi body-kohdasta ZAPissa.

![image](https://github.com/user-attachments/assets/c643c8c4-25a9-4833-beee-3ece47a9e85c)

![image](https://github.com/user-attachments/assets/bb035296-ed50-46ba-9f56-8cf31e9594a3)



**Traversal sequences stripped non-recursively**

Tehtävä: [https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)

Tavoite oli taas sama, päästä käsiksi /etc/passwd, ja haavoittuvuus löytyi samasta kohtaa. Eli etsin ensin oikean hakupyynnön ZAPissa.

![image](https://github.com/user-attachments/assets/858a8892-ed0a-4d7d-a34e-84569517d5a5)

Kurkkasin ratkaisuun, ja nyt eroa olikin hakemistorakenteen ".." -sekvensseissä, eli path traversalin sekvenssit poistetaan toistuvasti (eli kuten tehtävän nimessä lukee, rekursiivisesti), kunnes niitä ei ole enää jäljellä. Tässä tehtävässä siis riitti, että .. toistettiin kahdesti, jotta suojauksen pystyi ohittamaan.

![image](https://github.com/user-attachments/assets/a6ee6922-4f72-4000-ad7c-2c3410e19e42)

![image](https://github.com/user-attachments/assets/502a41ef-9bd7-4572-ac36-70b44b37a16a)


### Server Side Template Injection (SSTI)

**g. Server-side template injection with information disclosure via user-supplied objects (SSTI**

Tehtävä: [https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-with-information-disclosure-via-user-supplied-objects](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-with-information-disclosure-via-user-supplied-objects)

Tehtävässä tuli napata frameworkin salainen avain. Kirjauduin ensin sisään tehtävänannosta löytyvillä tunnuksilla.

![image](https://github.com/user-attachments/assets/bf2b5ba6-a2dd-46c7-9a54-fa9d0f4b979a)

Ja taas kurkkaamaan ratkaisua. Tehtävänannosta en oikein osannut päätellä, mitä lähteä tarkastelemaan. Eli jonkin tuotteen kuvaustemplaattia piti muokata. Liitin tehtävänannossa vinkatun stringin templaattiin ja tallensin, jolloin esiin tuli virheilmoitus. Virheilmoituksesta pystyi bongaamaan, että käytössä on Django framework. Tässä vaiheessa olisi pitänyt tutustua Djangoon syvemmin, mutta jatkoin nyt tehtävää ihan vain tehtävän ratkaisuohjeella.

![image](https://github.com/user-attachments/assets/89f34576-36a0-4b60-bb8c-c8248dcc1b0b)

Eli vaihdoin tuotetta ja sen kuvaustemplaattiin syötin {% debug %} ja tallensin. Tämä sylkäisi ulos litanian, josta olisi pitänyt bongata, että settings-objektiin pääsee käsiksi. En ihan tätä hiffannut kokonaisuudessaan.

![image](https://github.com/user-attachments/assets/bb734b5b-9b95-4199-8646-a578af6e43a0)

Syötin suoritusohjeen mukaisesti templaattiin {{settings.SECRET_KEY}}.

![image](https://github.com/user-attachments/assets/45a2bc35-1497-40b8-ae3f-4cf4d8b557d4)

Syötin ratkaisun submit solution -kenttään, tehtävä suoritettu.

![image](https://github.com/user-attachments/assets/d24b65a4-908d-4bd5-a67a-bef3562dc9e5)


### Server Side Request Forgery (SSRF)

**h. Basic SSRF against the local server**

Tehtävä: [https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost](https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost)

Tehtävässä piti muuttaa URLia siten, että saa pääsyn admin interfaceen ja sitä kautta poistaa carlosin käyttäjä. Haavoittuvuus löytyi varaston tarkituskohdasta (stock check). Kohta löytyi tuotesivun alalaidasta.

![image](https://github.com/user-attachments/assets/6e8fbfd3-5eb0-4d21-bd4e-3b811c7a11e0)

ZAPissa pyyntö näytti tältä: 

![image](https://github.com/user-attachments/assets/401f5028-f96d-485f-8d27-04cc16732d5c)

Pähkäilin sitten, kuinka muokata pyyntöä. Ratkaisun mukaisesti muokkasin stockApia, mutta tuli virheilmoitus 405. Tutkin ZAPissa lisää, miten tämä kohta erosi aikaisemmista, ja kekkasin! Pyyntö oli siis POST, ei GET, ja sen korjaamalla sainkin tavoitellun tuloksen, eli nyt voidaan muodostaa URL, jolla poistaa carlosin käyttäjä.

![image](https://github.com/user-attachments/assets/697b4b8b-62fd-4e07-8aa6-ac79c2f0cc38)


![image](https://github.com/user-attachments/assets/7c4237c4-c9c6-4345-b24b-e7341c18a619)


Tässä muokattu pyyntö, joka poisti carlosin käyttäjän:

![image](https://github.com/user-attachments/assets/3fbadf2d-6507-47e5-b873-298ca0ada4bf)


![image](https://github.com/user-attachments/assets/5d0b1c95-5235-4572-b5b3-41df9db584d2)


### Cross Site Scripting (XSS)

**i. Reflected XSS into HTML context with nothing encoded**

Tehtävä: [https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)

Tässä tehtävässä haavoittuvuus löytyi hakutoiminnosta, ja tarkoitus oli saada aikaan alert-ikkuna. Tein siis hakupyynnön ja tutkin hetken ZAPissa sitä, eikä ollut käryäkään, mitä tehdä. Vilkaisin ratkaisua, ja se olikin hyvin yksinkertainen. Hakukenttään tulikin siis vain syöttää ``<script>alert(1)</script>`` (tuohon 1 kohdalle voi kai laittaa jonkun omankin viestin).

![image](https://github.com/user-attachments/assets/8d36a337-d4c2-4d9d-937e-a745a8472567)

![image](https://github.com/user-attachments/assets/d5761167-8af3-4169-a5aa-3393920d09dd)

Eli sivusto suorittaa hakukenttään syötetyn tekstin osana koodia.



**j.  Stored XSS into HTML context with nothing encoded**

Tehtävä: [https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)

Tässä tehtävässä haavoittuvuus löytyi kommenttiosion tekstikentästä. Tavoitteena oli aiheuttaa alert silloin, kun blogia katsellaan. Avasin blogipostauksen ja etsin kommenttikentän, ja päätin kokeilla samaa koodinpätkää. Sivusto vaati kommentoijan tietojen syöttämiseen oikeaa formaattia, eli oli tärkeää, että nettisivu ja sähköpostiosoite muotoiltiin oikein, jotta kommentin pystyi lähettämään.

![image](https://github.com/user-attachments/assets/a8d54d0d-9f41-4487-96e7-9e94ca7782a4)

Ja sitten etsin sivulta samaisen blogipostauksen. Kyllä, nyt alert ilmestyi. Eli koodinpätkä on nyt upotettu sivulle ja se häiriköi, kunnes joku siivoaa sen pois.

![image](https://github.com/user-attachments/assets/827a87d9-60a1-473d-bcdf-62272233eb12)


![image](https://github.com/user-attachments/assets/88d8f85d-e09a-4963-a735-34a038ad0759)



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
