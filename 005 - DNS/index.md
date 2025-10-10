# Domain Name System
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
- Krijg je hetzelfde antwoord als met je default server?
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
Klassikale demonstratie

note:
- surf naar klanten.netnoobs.be
- bekijk hoe het DNS-verkeer vloeit
- surf ook naar personeel.netnoobs.be
- idem
---
Klassikale oefening: winkelen.netnoobs.nl

note:
- duid mensen in de klas aan om tot antwoord te komen
- hints:
  - klanten.netnoobs.be zit al in systeem, we willen dat iets **hetzelfde** voorstelt
  - wat is het "belangrijkste" deel dat anders is tegenover wat al werkt?
    - trek daaruit conclusie rond waar we eerst iets kunnen aanpassen
    - wat is het tweede belangrijkste deel van het domein?
  - denk na over wanneer we al iets kunnen testen
    - het kan zijn dat winkelen.netnoobs.nl al sneller bereikbaar is van andere plaatsen dan van bv. PC0
---
<iframe width="560" height="315" src="https://www.youtube.com/embed/dm8i4IFTA7k?si=oG083hSw_HZHSzG2"
    title="YouTube video player" frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen></iframe>

note:
- aanzienlijke beperkingen, maar gratis en geen CC vereist
- kan ook TLS koppelen
- **op school: bletchley netwerk gebruiken, AP wifi zal adressen blokkeren**
- er bestaan alternatieven zoals DuckDNS
---
<iframe width="946" height="532" src="https://www.youtube.com/embed/kKsHo6r4_rc" title="Using Pi-Hole for Local DNS - Fast, Simple, and Easy Guide" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

note:
- lokale DNS-server (handig als je thuis meerdere machines hebt en je wil niet telkens met hun IP werken)
- ad blocking voor je thuisnetwerk
  - via DNS, maar niet alle ads kunnen op deze manier geblokkeerd worden
