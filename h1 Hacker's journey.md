## h1 Hacker's journey




### x) Lue/katso/kuuntele ja tiivistä.

1. Hyppönen, Mikko & Tuominen, Tomi. Herrasmieshakkerit. Rikos, jonka voi tilata netistä | Yhteistyössä Kyberrosvot. Julkaistu 21.3.2024. [https://podcasts.apple.com/fi/podcast/rikos-jonka-voi-tilata-netist%C3%A4-yhteisty%C3%B6ss%C3%A4-kyberrosvot/id1479000931?i=1000650652103](https://podcasts.apple.com/fi/podcast/rikos-jonka-voi-tilata-netist%C3%A4-yhteisty%C3%B6ss%C3%A4-kyberrosvot/id1479000931?i=1000650652103)
   

2. Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain. [https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf](https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf)
   

3. Santos et al: The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance. [https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/](https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/)
   

4. Korkeimman oikeuden ratkaisu 2003:36. [https://finlex.fi/fi/oikeus/kko/kko/2003/20030036](https://finlex.fi/fi/oikeus/kko/kko/2003/20030036)


### a) Asenna Kali virtuaalikoneeseen.


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



### b) Irrota Kali-virtuaalikone verkosta. Todista testein, että kone ei saa yhteyttä Internetiin (esim. 'ping 8.8.8.8')

Testasin ensin pingata vielä, kun kone oli verkossa. ``ping 8.8.8.8``

![image](https://github.com/user-attachments/assets/7aa07fcc-f0d5-410e-9057-58370768de63)

Irrotin sitten koneen verkosta VirtualBoxissa.

![image](https://github.com/user-attachments/assets/eed98eba-7fee-4947-8342-83d4200b5b74)

![image](https://github.com/user-attachments/assets/bb2a167e-4e42-4223-9779-31e22061bf6f)


### c) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (nmap -A localhost). Analysoi tulokset.


Päivitin ensin Kalilla paketit ``sudo apt-get update``. Sitten asensin nmapin ``sudo apt-get install nmap``. Asennuksen jälkeen irrotin koneen netistä ja kokeilin skanneria ``nmap -A localhost``. Oletetusti kaikki portit olivat inaktiivisia. Network distance 0 hops kertoo, että yhteys on paikallinen, eli pyyntö ei kulje muiden laitteiden kautta (esim. reititin).

![image](https://github.com/user-attachments/assets/c50bc715-60e9-400f-913a-bf4fd9ae1ad7)



### d) Asenna kaksi vapaavalintaista demonia ja skannaa uudelleen. Analysoi ja selitä erot.

Asensin seuraavat demonit: ``sudo apt-get install apache2`` ja ``sudo apt-get install openssh-server``. Otin koneen taas pois verkosta ja potkaisin demonit käyntiin ``sudo service apache2 start``, ``sudo systemctl start ssh.service``. Skannasin portit uudestaan, ja siellä oli kuin olikin auenneet seuraavat: portti 22 on varattu SSH-yhteyksille. SSH-yhteyden avulla voidaan ottaa salattu etäyhteys koneeseen. Portti 80 aukesi Apachen myötä, sillä Apache kuuntelee tätä porttia: sen kautta HTTP-pyynnöt kulkevat selaimelta palvelimelle.

![image](https://github.com/user-attachments/assets/f411c6b9-f525-4a9a-8db5-472719dd8c29)



### e) Asenna Metasploitable 2 virtuaalikoneeseen


### f) Tee koneiden välille virtuaaliverkko.


### g) Etsi Metasploitable porttiskannaamalla (nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable.


### h) Porttiskannaa Metasploitable huolellisesti ja kaikki portit (nmap -A -p-). Poimi 2-3 hyökkääjälle kiinnostavinta porttia. Analysoi ja selitä tulokset näiden porttien osalta.

### Lähteet

- Karvinen, Tero. Tunkeutumistestaus. [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/)

- Hyppönen, Mikko & Tuominen, Tomi. Herrasmieshakkerit. Rikos, jonka voi tilata netistä | Yhteistyössä Kyberrosvot. Julkaistu 21.3.2024. [https://podcasts.apple.com/fi/podcast/rikos-jonka-voi-tilata-netist%C3%A4-yhteisty%C3%B6ss%C3%A4-kyberrosvot/id1479000931?i=1000650652103](https://podcasts.apple.com/fi/podcast/rikos-jonka-voi-tilata-netist%C3%A4-yhteisty%C3%B6ss%C3%A4-kyberrosvot/id1479000931?i=1000650652103)

- Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain. [https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf](https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf)
  
- Santos et al: The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance. [https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/](https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/)

- Korkeimman oikeuden ratkaisu 2003:36. [https://finlex.fi/fi/oikeus/kko/kko/2003/20030036](https://finlex.fi/fi/oikeus/kko/kko/2003/20030036)

-  [https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)



