# DNS en SSH
---
## Domain Name System
---
domein (inclusief prefix) ≈ IP-adres

note:
- beter leesbaar
- beter te onthouden
- **niet** geschikt voor routering
- complexere configuraties waarbij dit niet zomaar geldt (load balancers,...)
---
ARPANET

note:
- één master copy tekstbestand
- "surfers" (avant la lettre) moesten enkel het adres van die server kennen
- duidelijk niet robuust / schaalbaar
- DNS maakt dit idee wel robuust en schaalbaar
---
![DNS root server](./afbeeldingen/dns-root-server.png)
note:
- Wat is de indruk op basis van deze figuur?
---
![domain name space](./afbeeldingen/Domain_name_space.svg)
note:
- lijkt op vorige figuur, maar technischer
- omvat verdeling van verantwoordelijkheid
- hier staat bovenaan nog steeds maar één root server, maar er zijn er eigenlijk een paar 100
  - en die zijn dan load balanced over 13 IP-adressen
- je kan zeggen dat deze met geen enkel domein geassocieerd zijn, of met elk domein
---
<iframe width="560" height="315" src="https://www.youtube.com/embed/27r4Bzuj5NQ?si=dgP3K7T8dTUtqYqV"
                    title="YouTube video player" frameborder="0"
                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                    allowfullscreen />

note:
- eindgebruiker moet een DNS-resolver instellen ≠ name server
- zie netwerksettings
- normaal mee ingesteld met IP-adres, maar manueel aanpasbaar
- bv. switchen naar Google DNS
- voorgesteld als IP-adres, want niet raadpleegbaar via naam
- luistert op poort 53
  - name servers doen dit ook, maar hebben andere **verantwoordelijkheid**
---
Caching

note:
- DNS-records hebben een TTL
- "non-authoritative" = afkomstig uit een cache, niet recht van de bron
---
Load balancing

note:
- voor wanneer meerdere servers dezelfde dienst aanbeiden
- heel makkelijk: gewoon afwisselend andere antwoorden geven op queries
---
- Open het Wireshark bestand DNS1 van op DigitAP
- Stel een filter in zodat je enkel HTTP en DNS ziet
  - Een OR van filters doe je met `||`
- Beantwoord volgende vragen:
  - Wat is het IP-adres van de DNS server die bevraagd wordt?
  - Hoeveel IP adressen worden er door de DNS server gevonden voor host www.ietf.org?
  - Welke adressen zijn dit?
  - Welk wordt uiteindelijk door de host gebruikt om te surfen?
  - Hoe lang mag de host dit adres beschouwen als geldig?
---
- Om een DNS server te bevragen, gebruik je `nslookup`
  - Mogelijk moet je hiervoor je Windows Terminal als admin openen
- Vraag het DNS record op voor www.ap.be
  - Welke DNS server heeft de vraag beantwoord?
  - Welk adres krijg je?
  - Is de server authoritative voor dit domein?
- Vraag het DNS record op voor learning.ap.be
  - Welke DNS server heeft de vraag beantwoord?
  - Welk adres krijg je?
  - Is de server authoritative voor dit domein?
  - Wat valt op in vergelijking met de eerdere query? Wat zou dit kunnen betekenen?
---
- Je DNS-server is automatisch toegekend
- Je kan hem systeembreed wijzigen of je kan bij `nslookup` een andere gebruiken
- Probeer uit: nslookup `www.ap.be 8.8.8.8`
- Krijg je hetzelfde antwoord als eerder?
- Vraag aan volgende 4 DNS-servers waar www.amazon.com staat. Hou alle resultaten bij in één file:
  - 8.8.8.8
  - 195.130.130.4
  - 204.13.250.31
  - 195.238.2.21
- Wat verklaart dat de resultaten gelijk of verschillend zijn?
---
- A
- AAAA
- CNAME
- MX
- NS
  - SOA

note:
- belangrijkste records voor programmeurs
- je ziet deze zaken als je bv. zelf een domein registreert
- A = meest klassieke mapping (domeinnaam op IPv4-adres)
- Dus je vult iets in van de vorm example.com = 192.168.0.2
- AAAA = IPv6-versie
- CNAME = alias, i.e. ander domein (dat we dan zullen resolven)
  - kan bv. van pas komen om `www` en leeg prefix gelijk te schakelen
- MX = geassocieerde mail server (als je er zelf een hebt)
- NS = domeinnaam van een name server voor een domein
  - als je zelf alle namen onder een domein wil kunnen beheren
- SOA is vooral belangrijk als je een eigen zone managet, bevat zaken zoals contactadres beheerder en TTL
---
![voorbeeld DNS in GoDaddy](./afbeeldingen/voorbeeld-dns-godaddy.png)
---
Klassikale opdracht

note:
- klanten.netnoobs.be en bestellingen.netnoobs.be bereikbaar maken
- start zo kleinschalig mogelijk
- i.e. surf eerst van binnen Netnoobs netwerk via één server naar andere
- daarna vanaf .be name server
- doorgaan tot het lukt vanaf clients
---
Uitbreiding: winkelen.netnoobs.nl
---
## SSH

note:
- secure shell (remote login)
- vereist client en server
- applicatielaagprotocol
- standaard luistert server op poort 22
- client is voorzien bij Git Bash
- vaak niet mogelijk via wachtwoord, wel asymmetrische sleutels (zie TLS)
- klassiek gebruik = shell opstarten, maar ook voor Github, voor remote sessie VSC,...
  - alomtegenwoordig in cloud infrastructuur!
---
Gegeven:

- server IP (zelfde voor iedereen)
- container ID (individueel)
- loginnaam (individueel)
- poortnummer (individueel)
- wachtwoord (individueel)
---
![temp mail](./afbeeldingen/tempmail.png)

note:

- geen "leerstof"
- wel heel nuttig
- kwestie van privacy en digitale hygiëne
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/dm8i4IFTA7k?si=oG083hSw_HZHSzG2"
    title="YouTube video player" frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen></iframe>

note:
- aanzienlijke beperkingen, maar gratis en geen CC vereist
- kan ook TLS koppelen
---
- `ssh USERNAAM@SERVERIP`
- `docker exec -it CONTAINERID bash`
- `apt update; apt install -y nano; nano htdocs/index.html`
- bewerk de HTML en sla op zodat je je eigen site kan herkennen
- surf naar de website (hoe?)
- registreer via FreeDNS (niet toegankelijk via netwerk school, gebruik hotspot)
- controleer propagatie via WhatsMyDNS
- surf via domeinnaam naar de website
---
![interactie client-server](./afbeeldingen/SSHkeydiagram.webp)

note:
- zullen iets verderop zelf sleutels maken!
---
authorized_keys

note:
- zit op de server!
- betreft dus publieke sleutels!
---
known_hosts

note:
- op de client!
- omvat adres en signatuur
- waarschuwing bij nieuwe bestemming
  - waarom denk je dat dit belangrijk is?
---
config

note:
- meerdere SSH key pairs mogelijk
- kan default settings koppelen
  - bv. deze sleutel gebruiken wanneer we met dat IP-adres verbinden
  - te veel verschillende sleutels en geen config ⇒ te veel pogingen, weigering
---

- `scp /path/to/local/file username@remotehost:/path/to/remote/directory`
- `scp username@remotehost:/path/to/remote/file /path/to/local/directory`

note:
- Op Linux begint een filesysteem bij `/`, niet `C:`
- forward slash ipv backslash
---
Opdracht (Git bash)
- sleutelpaar aanmaken: `ssh-keygen -t rsa -b 4096` met defaults en lege passphrase
- kijk in (verborgen map) `.ssh`
- kopieer naar server: `ssh-copy-id -i ~/.ssh/id_rsa.pub username@remotehost`
  - dit combineert `scp` met een append in `authorized_keys`
- log in op de server
- `chmod 700 ~/.ssh` beperkt rechten op deze map
- `chmod 600 ~/.ssh/authorized_keys` beperkt rechten file
- test login zonder wachtwoord
- registreer public key in Github
- maak een niet-lege repo, clone via SSH, commit iets, push

