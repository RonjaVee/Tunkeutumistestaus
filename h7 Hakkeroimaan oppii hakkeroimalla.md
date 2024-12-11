# h7 Hakkeroimaan oppii hakkeroimalla

Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. Julkaistu 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/#h4-marraskuu2024) Etsi vapaavalintainen review eli katsausartikkeli, joka liittyy kurssin aiheisiin.

## x) Lue/katso ja tiivistä

Bergman, J., & Popov, O. B. (2023). Exploring dark web crawlers: a systematic literature review of dark web crawlers and their implementation. IEEE Access, 11, 35914-35933. [https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=10064292](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=10064292)
Julkaisun JUFO ID 78297, tasoluokka 1.


Tiivistelmä:

Dark web crawler (suomeksi indeksoija, hakurobotti) on ohjelma, joka liikkuu dark webissa eli pimeässä verkossa sivustolta toiselle linkkejä pitkin ja kerää löytämäänsä salaamatonta dataa sekä tallentaa kulkemansa reitin ja tallentaa ne tietokantaan. Carwlereita hyödynnetään mm. rikostutkinnassa johtolankojen ja todisteiden etsintään.  Kyberrikollisuuden tutkinta on yhä haastavampaa, kun data liikkuu verkossa salattuna ja rikolliset hyödyntävät reitityksiä, jotka mahdollistavat anonymiteetin.

Tutkimusartikkeli Exploring dark web crawlers: a systematic literature review of dark web crawlers and their implementation jakautui kahteen osaan: systemaattiseen kirjallisuuskatsaukseen ja käytännön toteutukseen, jossa hyödynnettiin katsaukseen valittuja artikkeleita. Kirjallisuuskatsauksessa tutkittiin Tor-verkkoa ja tiedonkeruuta koskevia vertaisarvioituja artikkeleita. Haku tehtiin Scopus-tietokannasta, ja 35 artikkelia valittiin tarkempaan analyysiin: vain kuudessa oli saatavilla myös lähdekoodi. Tulokset osoittivat, että Python oli yleisin crawlereiden ohjelmointikieli, ja Selenium sekä Scrapy olivat suosituimmat kirjastotyökalut. Apache Nutch oli yleisimmin käytetty valmiiksi rakennettu crawler. Mainittujen lisäksi yksittäisiä muita ohjelmointikieliä käytettiin, ja 12 artikkelissa ohjelmointikieltä ei mainittu. 

Systemaattisessa kirjallisuuskatsauksessa todettiin, että akateemisessa tutkimuksessa yleisin tapa Tor-onionsivustojen ryöminnän toteuttamiseen oli kehittää ohjelma Pythonilla ja käyttää Selenium-web-testauskirjastoa. Kirjallisuuskatsauksen pohjalta kehitettiin Tor-verkkoon sopiva crawler, joka siis käytti Seleniumia. Työkalun tavoitteena oli tukea kyberrikollisuuden torjuntaa ja digitaalista forensiikkaa. Sen toimivuutta testattiin laboratorio-olosuhteissa kahdella tavalla: tavallisten verkkosivujen ryöminnässä ja pimeän verkon suojattujen markkinapaikkojen käsittelyssä. Pimeän verkon testit sisälsivät monimutkaisempia toimenpiteitä, kuten käyttäjän autentikoinnin ja CAPTCHA-tunnistuksen hallinnan, jolloin saatiin pääsy myös kirjautumista vaativille sivustoille ja markkinapaikoille.

Kokeet osoittivat, että työkalu pystyi tehokkaasti ryömimään sekä tavallisen että pimeän verkon verkkosivustoilla. Verkkosivujen sisältövertailussa huomattiin, että työkalun lataamat sivut olivat lähes identtisiä molemmissa verkoissa. Lisäksi työkalun kehitykselle asetetut vaatimukset täyttyivät. Se pystyi käsittelemään tavallisen ja pimeän verkon sivustoja odotetusti. Seleniumin avulla voitiin simuloida ihmiskäyttäjän toimintaa, mikä teki työkalusta monipuolisen. Työkalu oli avoimen lähdekoodin ohjelma, tuki rinnakkaisprosessointia ja toimi omavaraisesti. Suorituskykyä rajoitti kuitenkin Tor-verkon hitaus ja Seleniumin yksisäikeinen arkkitehtuuri. 

Tutkimuksen perusteella ryömijä oli käyttökelpoinen työkalu kyberrikollisuuden torjuntaan. Se teki datankeruusta tehokasta ja tarkkaa, mikä säästi tutkijoiden aikaa. Jatkossa työkalua voisi parantaa esimerkiksi suorituskyvyn optimoinnilla, kuten monisäikeistämisellä ja estomekanismien kiertämisellä, mikä auttaisi erityisesti pimeän verkon ryöminnässä.

## TLDR

- Dark web crawler on ohjelma, joka liikkuu pimeässä verkossa, kerää salaamatonta dataa ja tallentaa reitit tietokantaan, ja sitä hyödynnetään mm. rikostutkinnassa.
- Systemaattisessa kirjallisuuskatsauksessa tutkittiin Tor-verkossa käytettävien crawlerien toteutusta: yleisin ohjelmointikieli oli Python, jossa käytettiin Selenium- ja Scrapy-kirjastoja.
- Kehitettiin Tor-verkkoon sopiva crawler, joka testattiin laboratorio-olosuhteissa sekä tavallisilla että pimeän verkon verkkosivustoilla, mukaan lukien suojatut markkinapaikat.
- Kokeet osoittivat, että työkalu täytti vaatimukset ja oli käyttökelpoinen kyberrikollisuuden torjunnassa, mutta suorituskyvyn parantaminen, kuten monisäikeistys, voisi tehostaa sen toimintaa.


### Muut lähteet:

Tiivistelmää varten etsin suomennoksen web crawler-sanasta, ja siitä löytyikin suppea Wikipedia-artikkeli suomennoksineen.

Wikipedia. Hakurobotti. Wikipedia. Haettu 11.12.2024. [https://fi.wikipedia.org/wiki/Hakurobotti](https://fi.wikipedia.org/wiki/Hakurobotti)
