# h1 Hacker's journey


## x) Lue/katso/kuuntele ja tiivistä.

1. Hyppönen, Mikko & Tuominen, Tomi. Herrasmieshakkerit. Rikos, jonka voi tilata netistä | Yhteistyössä Kyberrosvot. Julkaistu 21.3.2024. [https://podcasts.apple.com/fi/podcast/rikos-jonka-voi-tilata-netist%C3%A4-yhteisty%C3%B6ss%C3%A4-kyberrosvot/id1479000931?i=1000650652103](https://podcasts.apple.com/fi/podcast/rikos-jonka-voi-tilata-netist%C3%A4-yhteisty%C3%B6ss%C3%A4-kyberrosvot/id1479000931?i=1000650652103)

- Verkkorikollisten toiminta on nykypäivänä varsin organisoitunutta: rikollisyrityksillä saattaa olla jopa HR-osastot yms.
- Esim. palvelunestohyökkäykset ovat halpoja ostaa, eikä tarvitse hakea Tor-verkosta asti vaan niitä on yleisesti saatavilla.
- CaaS (Crime as a Service): esim. palvelut, joilla myydään varastettua dataa, DdoS-hyökkäykset ym.
- RaaS (Ransomware as a Service): Ransomwaren (esim. kiristystroijalainen) myynti tai vuokraaminen. Haittaohjelmaa ei tarvitse itse koodata, kun sen ostaa. RaaS kuuluu CaaS-katon alle.
- Motivaationa rahan lisäksi poliittinen vaikuttaminen, kybersodankäynti.
   

2. Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain. [https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf](https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf)


   

5. Santos et al: The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance. [https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/](https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/)
   

6. Korkeimman oikeuden ratkaisu 2003:36. [https://finlex.fi/fi/oikeus/kko/kko/2003/20030036](https://finlex.fi/fi/oikeus/kko/kko/2003/20030036)



## a) Asenna Kali virtuaalikoneeseen.


Latasin Kalin ISO-imagen sivulta [https://www.kali.org/get-kali/#kali-installer-images](https://www.kali.org/get-kali/#kali-installer-images).


![image](https://github.com/user-attachments/assets/d9c5c679-5161-46b7-b1ff-4bc4221c77b2)


Odotellessani latausta loin Virtual Boxilla uuden koneen johon tulisin asentamaan Kalin. Hyödynsin koneen luomisessa aiemmilta kursseilta oppimaani. Aina voi luoda uudenkin jos tämä ei riitä, mutta toivotaan, että näillä statseilla Kali pyörii hyvin. 


![image](https://github.com/user-attachments/assets/5b3b3670-ea17-4592-b597-d3998c73dd9f)

![image](https://github.com/user-attachments/assets/7f1dafdd-0bf0-49df-93eb-d0ca411cada9)

![image](https://github.com/user-attachments/assets/91c404da-4708-40f1-a436-3d79d663d169)


Etsin tueksi vielä oppaan, jos asennuksessa olisi ongelmia: [https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/) Tutkimani asennusohjeen perusteella valisin boottausjärjestyksen, kovalevy 1. ja optinen levy 2. Muita asetuksia koneeseen en tehnyt.

![image](https://github.com/user-attachments/assets/495d1436-5f7a-4dd8-8ca0-d95d4c0cf9bd)

Lataus tuli valmiiksi, ja iskin levyn sisään ja lähdin asentamaan Kalia.

Olen aiemminkin asennellut Linuxeja, joten tein asennusta koskevat valinnat sen perusteella, mitä olen oppinut aiemmilta kerroilta. Olen raportoinut yhden asennuksen johon olen joitakin valintoja perustellut, ja se löytyy [täältä](https://github.com/RonjaVee/Installing-Linux/blob/main/Installing%20process.md). 

Ja näin, asennus onnistui ja Kali Linux on valmiina käyttöön.

![image](https://github.com/user-attachments/assets/79c43762-d15f-4b02-9fba-26c2b2b40763)




## b) Irrota Kali-virtuaalikone verkosta. Todista testein, että kone ei saa yhteyttä Internetiin (esim. 'ping 8.8.8.8')

Testasin ensin pingata vielä, kun kone oli verkossa. ``ping 8.8.8.8``

![image](https://github.com/user-attachments/assets/7aa07fcc-f0d5-410e-9057-58370768de63)

Irrotin sitten koneen verkosta VirtualBoxissa.

![image](https://github.com/user-attachments/assets/eed98eba-7fee-4947-8342-83d4200b5b74)

![image](https://github.com/user-attachments/assets/bb2a167e-4e42-4223-9779-31e22061bf6f)




## c) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (nmap -A localhost). Analysoi tulokset.


Päivitin ensin Kalilla paketit ``sudo apt-get update``. Sitten asensin nmapin ``sudo apt-get install nmap``. Asennuksen jälkeen irrotin koneen netistä ja kokeilin skanneria ``nmap -A localhost``. Porttien tila oli ignored state = demoneita ei ole asennettu, jotka kuuntelisivat tiettyjä portteja ja kaikki skannatut portit olivat kiinni. Network distance 0 hops kertoo, että yhteys on paikallinen, eli pyyntö ei kulje muiden laitteiden kautta (esim. reititin).

![image](https://github.com/user-attachments/assets/c50bc715-60e9-400f-913a-bf4fd9ae1ad7)




## d) Asenna kaksi vapaavalintaista demonia ja skannaa uudelleen. Analysoi ja selitä erot.

Asensin seuraavat demonit: ``sudo apt-get install apache2`` ja ``sudo apt-get install openssh-server``. Otin koneen taas pois verkosta ja potkaisin demonit käyntiin ``sudo service apache2 start``, ``sudo systemctl start ssh.service``. Skannasin portit uudestaan, ja siellä oli kuin olikin auenneet seuraavat: portti 22 on varattu SSH-yhteyksille. SSH-yhteyden avulla voidaan ottaa salattu etäyhteys koneeseen. Portti 80 aukesi Apachen myötä, sillä Apache kuuntelee tätä porttia: sen kautta HTTP-pyynnöt kulkevat selaimelta palvelimelle.

![image](https://github.com/user-attachments/assets/f411c6b9-f525-4a9a-8db5-472719dd8c29)




## e) Asenna Metasploitable 2 virtuaalikoneeseen

Käytin [https://docs.rapid7.com/metasploit/metasploitable-2/](https://docs.rapid7.com/metasploit/metasploitable-2/) sivustolta löytyvää ohjetta. Sivustolta löytyvää [Sourceforge](https://docs.rapid7.com/metasploit/metasploitable-2/)-linkkiä käytin Metaspoitable 2:n lataamiseen.

En tiennyt, kuinka jatkaa tästä eteenpäin, joten etsin ohjevideon Youtubesta: [install Kali Linux 2024.1 & Metasploitable2 on VirtualBox 7 Step By Step : Cyber Security Lab 2024](https://www.youtube.com/watch?v=yf3jetn4tN8).

Siirsin lataamani kansion Virtual Boxin kansioon josta muutkin kovalevyt löytyvät, ja valitsin Extract here.

![image](https://github.com/user-attachments/assets/e7b814eb-a7ff-44eb-9a83-173a7db62ec2)

Loin VirtualBoxissa koneen, jossa käytin tätä kovalevyä.

![image](https://github.com/user-attachments/assets/6245ccdf-cb51-444f-a048-2f533b79db96)

Avasin koneen ja kirjauduin sisään Metasploitablen sivulla annetuilla tunnuksilla ``msfadmin:msfadmin``. Näyttää toimivan kuten pitää.

![image](https://github.com/user-attachments/assets/914424e2-b38a-4cf5-894f-6013a1ec3dd6)




## f) Tee koneiden välille virtuaaliverkko.

Tehtävän ohjeen mukaan loin host only networkin Virtualboxissa (File -> Host Network Manager -> Create). Verkon IP-osoite on 192.168.56.1/24.

![image](https://github.com/user-attachments/assets/9582c702-0d53-48af-900a-7ba981fbe174)

Sitten loin Kalille toisen verkkosovittimen. Attached to -kohdassa valitsin Host-Only Adapter.
Name-kentässä oli oletuksena vboxnet0 eli juuri luomani verkko. 

![image](https://github.com/user-attachments/assets/9f1ab2a7-5770-42b6-b80f-fb266e3aa972)

Metasploitable-koneella muokkasin Adapter1 suoraan, sillä sen ei tarvitse olla yhteydessä Internetiin. Kalin voi erikseen irrottaa Internetistä kun porttiskannailee.

![image](https://github.com/user-attachments/assets/50b4bf87-5ee1-40ef-ade0-d2995183273e)

Sitten tarkastin komennolla ``ifconfig``, että Kalilla löytyy yhteys luotuun verkkoon. Eth1-sovittimessa IP-osoite 192.168.56.101 täsmää luotuun verkkoon, eli Kali on tässä verkossa.

![image](https://github.com/user-attachments/assets/ce937b63-dffb-41f6-bd47-307873b029f7)

Saman tarkastuksen tein Metasploitable-koneella. Verkko löytyi eth0-verkkosovittimesta: IP-osoite täsmää (192.168.56.102).

![image](https://github.com/user-attachments/assets/faa5862d-118f-48d4-8540-97b8a1a81088)

Pingasin sitten Kalilla Metasploitablea ``ping 192.168.56.102``. Paketteja liikkui.

![image](https://github.com/user-attachments/assets/f426b5fb-d02d-4194-b606-5ab30ca4bd0c)




## g) Etsi Metasploitable porttiskannaamalla (nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable.

Porttiskannauksen ajaksi otin Kalin irti Internetistä (eli Adapter1 Not Attatched). Skannasin luodun Host only -verkon komennolla ``nmap -sn 192.168.56.0/24``.

![image](https://github.com/user-attachments/assets/eb4dd802-da02-4844-8bf1-cdc370eb4fb7)

Metasploitable näkyi IP-osoitteessa 192.168.56.102.

Komennolla ``curl http://192.168.56.102`` näkyi seuraavaa:

![image](https://github.com/user-attachments/assets/83dfdde2-5838-4971-8503-bd895d3686a2)

Ja selaimessa Metasploitable näkyi myös:

![image](https://github.com/user-attachments/assets/98e6e1be-1a24-48e2-8732-d1450380923b)




## h) Porttiskannaa Metasploitable huolellisesti ja kaikki portit (nmap -A -p-). Poimi 2-3 hyökkääjälle kiinnostavinta porttia. Analysoi ja selitä tulokset näiden porttien osalta.

Skannasin Metasploitablen komennolla ``nmap -A -p- 192.168.56.102``. Portit 21, 22, 23, 25, 53, 80, 111, 139, 445, 512, 513, 514, 1099, 1524, 2049, 2121, 3306, 3632, 5432, 5900, 6000, 6667, 6697, 8009, 8180, 8787, 45552, 50229, 55445 ja 56092 olivat auki. Kaikkia en saanut kuvaan, mutta tässä kuva siitä, että skannaus tehty. 

![image](https://github.com/user-attachments/assets/45612f62-c05e-4e12-ba39-151357f15b27)

Kysyin ChatGPT:ltä, mitkä näistä porteista olisi sen mukaan mielenkiintoisimpia hyökkääjän näkökulmasta. Se ehdotti portteja 22, 80 ja 445. Kaksi ensimmäistä ovatkin jo tuttuja: Portin 22 kautta saa etäyhteyden koneeseen, ja hyökkääjälle SSH-yhteys onkin tapa hallita murrettua konetta etäältä. Portin 80 läpi kulkee HTTP-pyyntöjä, ja huonosti suunniteltujen web-sovellusten heikkouksia hyödyntämällä (esim. SQL-injektiolla päästään käsiksi tietokantaan) tai esimerkiksi DDoS-hyökkäyksellä (HTTP-pyynnöt tukkivat palvelimen) voi aiheuttaa uhrille haittaa.

Portti 445 ei ollut minulle tuttu, joten tutustuin siihen käyttäen lähteenä [SecurityScoreboardin](https://securityscorecard.com/blog/navigating-the-risks-of-tcp-445-strategies-for-secure-network-communication/) blogia. Portti 445 on siis SMB (Server Message Block) -protokollaa hyödyntävä verkkoportti ja sitä käytetään tiedostojen jakamiseen laitteiden välillä samassa verkossa, esim. printtereille tulostamista varten. Portin heikkouksia voidaan hyödyntää esimerkiksi ransomware-hyökkäysten toteuttamiseen. Porttia ei tulisi avata ulkoiseen verkkoon, vaan sen käyttö tulisi rajoittaa ainoastaan sisäiseen verkkoon. Sisäiseen verkkoon päästessään hyökkääjä voi hyödyntää tätä porttia siirtyessään laitteelta toiselle.



## Lähteet

- Karvinen, Tero. Tunkeutumistestaus. Julkaistu 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/)

- Hyppönen, Mikko & Tuominen, Tomi. Herrasmieshakkerit. Rikos, jonka voi tilata netistä | Yhteistyössä Kyberrosvot. Julkaistu 21.3.2024. [https://podcasts.apple.com/fi/podcast/rikos-jonka-voi-tilata-netist%C3%A4-yhteisty%C3%B6ss%C3%A4-kyberrosvot/id1479000931?i=1000650652103](https://podcasts.apple.com/fi/podcast/rikos-jonka-voi-tilata-netist%C3%A4-yhteisty%C3%B6ss%C3%A4-kyberrosvot/id1479000931?i=1000650652103)

- Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain. [https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf](https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf)
  
- Santos et al: The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance. [https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/](https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/)

- Korkeimman oikeuden ratkaisu 2003:36. [https://finlex.fi/fi/oikeus/kko/kko/2003/20030036](https://finlex.fi/fi/oikeus/kko/kko/2003/20030036)

- Kali Linux. Install VirtualBox guest VM. [https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)

- Rapid7. Metasploitable 2. [https://docs.rapid7.com/metasploit/metasploitable-2/](https://docs.rapid7.com/metasploit/metasploitable-2/)

- SourceForge. Metasploitable2 latauslinkki.[https://sourceforge.net/projects/metasploitable/](https://sourceforge.net/projects/metasploitable/) Ladattu 30.10.2024.

- Sunny Dimalu The Cyborg. install Kali Linux 2024.1 & Metasploitable2 on VirtualBox 7 Step By Step : Cyber Security Lab 2024. Julkaistu 14.3.2023. [https://www.youtube.com/watch?v=yf3jetn4tN8](https://www.youtube.com/watch?v=yf3jetn4tN8)

- SecuritScorecard. Navigating the Risks of TCP 445: Strategies for Secure Network Communication. [https://securityscorecard.com/blog/navigating-the-risks-of-tcp-445-strategies-for-secure-network-communication/](https://securityscorecard.com/blog/navigating-the-risks-of-tcp-445-strategies-for-secure-network-communication/).



