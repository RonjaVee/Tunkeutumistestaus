## h1 Hacker's journey




### x) Lue/katso/kuuntele ja tiivistä.

1. Hyppönen, Mikko & Tuominen, Tomi. Herrasmieshakkerit. Rikos, jonka voi tilata netistä | Yhteistyössä Kyberrosvot. Julkaistu 21.3.2024. [https://podcasts.apple.com/fi/podcast/rikos-jonka-voi-tilata-netist%C3%A4-yhteisty%C3%B6ss%C3%A4-kyberrosvot/id1479000931?i=1000650652103](https://podcasts.apple.com/fi/podcast/rikos-jonka-voi-tilata-netist%C3%A4-yhteisty%C3%B6ss%C3%A4-kyberrosvot/id1479000931?i=1000650652103)
   

2. Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains, chapters Abstract, 3.2 Intrusion Kill Chain. [https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf](https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf)
   

3. Santos et al: The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance. [https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/](https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00/)
   

4. Korkeimman oikeuden ratkaisu 2003:36. [https://finlex.fi/fi/oikeus/kko/kko/2003/20030036](https://finlex.fi/fi/oikeus/kko/kko/2003/20030036)


### a) Asenna Kali virtuaalikoneeseen.


Latasin Kalin ISO-imagen sivulta [https://www.kali.org/get-kali/#kali-installer-images](https://www.kali.org/get-kali/#kali-installer-images).


![image](https://github.com/user-attachments/assets/0c23e440-328f-4f9e-944d-67dab80fefd8)

![image](https://github.com/user-attachments/assets/d9c5c679-5161-46b7-b1ff-4bc4221c77b2)


Odotellessani latausta loin Virtual Boxilla uuden koneen johon tulisin asentamaan Kalin. Etsin tueksi vielä oppaan, jos asennuksessa olisi ongelmia: [https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)


![image](https://github.com/user-attachments/assets/5b3b3670-ea17-4592-b597-d3998c73dd9f)

![image](https://github.com/user-attachments/assets/7f1dafdd-0bf0-49df-93eb-d0ca411cada9)

![image](https://github.com/user-attachments/assets/91c404da-4708-40f1-a436-3d79d663d169)

Tutkimani asennusohjeen perusteella valisin boottausjärjestyksen, kovalevy 1. ja optinen levy 2. Muita asetuksia koneeseen en tehnyt.

![image](https://github.com/user-attachments/assets/495d1436-5f7a-4dd8-8ca0-d95d4c0cf9bd)

Lataus tuli valmiiksi, ja iskin levyn sisään ja lähdin asentamaan Kalia.

Olen aiemminkin asennellut Linuxeja, joten tein asennusta koskevat valinnat sen perusteella, mitä olen oppinut aiemmilta kerroilta. Olen raportoinut yhden asennuksen johon olen joitakin valintoja perustellut, ja se löytyy [täältä](https://github.com/RonjaVee/Installing-Linux/blob/main/Installing%20process.md). 

Ja näin, asennus onnistui ja Kali Linux on valmiina käyttöön.

![image](https://github.com/user-attachments/assets/79c43762-d15f-4b02-9fba-26c2b2b40763)



### b) Irrota Kali-virtuaalikone verkosta. Todista testein, että kone ei saa yhteyttä Internetiin (esim. 'ping 8.8.8.8')


### c) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (nmap -A localhost). Analysoi tulokset.


### d) Asenna kaksi vapaavalintaista demonia ja skannaa uudelleen. Analysoi ja selitä erot.


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



