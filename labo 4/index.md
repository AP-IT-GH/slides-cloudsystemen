# Transportlaag en netwerklaag
---
## Transportlaag

note:
- dient voor communicatie tussen *processen* op machines, niet gewoon tussen machines
- hier spreken we over "segmenten" voor datapakketjes
---
Poorten

note:
- zoals "postvakjes" op hetzelfde adres
- nodig voor zender ("outbox") en ontvanger ("inbox")
- een proces "luistert" naar een poort
---
- 0 tot 1023: "well-known"
- 1024 tot 49151: in principe voor vaste keuzes
- 49152 tot 65535: voor willekeurige toekenning
---
Voorbeelden:

- 22
- 80
- 443
- 3306
Sockets

note:
- unieke combinatie van **transportprotocol (TCP/UDP), IP-adres en poortnummer**
- eindpunt voor communicatie, zoals een stopcontact
- voor programmeurs: bytes in wegschrijven of bytes uit lezen
  - m.a.w. hier gaat de data van een applicatielaagprotocol in (of komt ze uit)
---
UDP

```mermaid
packet-beta
title UDP Packet
0-15: "Source Port"
16-31: "Destination Port"
32-47: "Length"
48-63: "Checksum"
64-95: "Data (variable length)"
```

note:
- denkvraag: waarom 2 bytes voor poorten?
---
- simpel
- snel
- "onbetrouwbaar"

note:
- UDP voegt eigenlijk enkel het concept van "poorten" toe aan IP.
- Pakketverlies of volgorde van ontvangst wordt niet geregeld.
- Dit protocol is vooral handig als er af en toe wat data verloren mag gaan of uit volgorde mag aankomen, maar weinig overhead gewenst is.
- Sommige netwerken blokkeren dit gewoonweg!
---
TCP

```mermaid
packet-beta
0-15: "Source Port"
16-31: "Destination Port"
32-63: "Sequence Number"
64-95: "Acknowledgment Number"
96-99: "Data Offset"
100-105: "Reserved"
106: "URG"
107: "ACK"
108: "PSH"
109: "RST"
110: "SYN"
111: "FIN"
112-127: "Window"
128-143: "Checksum"
144-159: "Urgent Pointer"
160-191: "(Options and Padding)"
192-255: "Data (variable length)"
```

---
- complexer
- connectiegeoriënteerd
  - volgnummers
  - bevestiging ontvangst
  - start met een "handshake"
- nog steeds onderworpen aan fysica
- TLS zit hier meestal "bovenop"

note:
- connectiegeoriënteerd, maar nog steeds packet switching, geen circuit switching!
- **when in doubt, use TCP!**
---
## Netwerklaag

note:
- functie was: data van machine A naar machine B krijgen
- vraag: waarom niet gewoon MAC-adressen en switches?
- antwoord: switch onthoudt **alle** adressen achter een bepaalde poort (binnen netwerk afgebakend door router)
- heel het Internet zou één lokaal netwerk zijn, dus...
---
| kenmerk | MAC | IP |
|-|-|-|
| sterkte associatie | "vast" | "veranderlijk" |
| structuur | vlak | hiërarchisch |
| afhankelijk van | fabrikant | subnet |
| vergelijk met | rijksregisternummer | thuisadres |
---
basisidee bereikbaarheid

note:
- vinden niet de machine, wel het (meest specifieke) subnet
- op subnet gebeurt een "broadcast"
- dus IP-adres wordt op het einde omgezet naar MAC-adres
---
IPv4 (classless)

note:

Bestaat ook ouder systeem met "klassen". Niet belangrijk voor programmeurs.

---
- 192.168.0.222
- 11000000.101...

note:
- 4 bytes = 32 bits
- je mag rekenmachine in programming mode gebruiken
---
- hostgedeelte
- netwerkgedeelte

note:
- machines op zelfde netwerk hebben zelfde netwerkgedeelte (ander hostgedeelte)
  - **in binair**, zelfde netwerkgedeelte kan voorgesteld zijn met andere reeks getallen in decimaal!
- om de twee te scheiden: netmasker nodig
---
netmasker

note:
- zelfde notatie als IP-adres, maar...
- binair altijd reeks 1'tjes gevolgd door reeks 0'en, zonder afwisseling
- afgekort tot slash-notatie
- **hoe lager de waarde, hoe groter het netwerk**
---

```text
11000000.1010000.00000000.11011110 # 192.168.0.222
11111111.1111111.00000000.00000000 # 255.255.0.0
11000000.1010000.00000000.00000000
```

note:

hoe zou je dan hostgedeelte kunnen krijgen?

---
                        speciale adressen
                        <aside class="notes">
                            <ul>
                                <li>netwerkadres (hostbits op 0) wordt eigenlijk niet als dusdanig gebruikt</li>
                                <li>maar netmasker toepassen doet niets, dus zou niet te onderscheiden zijn van andere
                                    adressen binnen zelfde netwerk</li>
                                <li>broadcastadres is wat naam zegt, verkregen door alle hostbits op 1 te zetten</li>
                                <li>denkvraag: aantal hosts op netwerk met netmasker van lengte N?</li>
                            </ul>
                        </aside>
---
Opdrachten

note:
Gebruik je rekenmachine in programmeermodus.

---
                            <ul>
                                <li>Is 10.4.3.0/16 een geldig netwerkadres?</li>
                                <li>Is 192.168.0.0/16 een geldig netwerkadres?</li>
                                <li>Is 192.168.0.0/8 een geldig netwerkadres?</li>
                                <li>Is 10.4.224.0/18 een geldig netwerkadres?</li>
                                <li>Is 172.25.13.0/24 een subnet van 172.16.0.0/12?</li>
                                <li>Is 172.16.0.0/13 een subnet van 172.16.0.0/12?</li>
                                <li>Met welke eerste bytewaarden kunnen subnetten van 10.0.0.0/8 allemaal beginnen?</li>
                                <li>Met welke eerste twee bytewaarden kunnen subnetten van 200.16.0.0/12 allemaal beginnen?</li>
                            </ul>
---
                            <p>Gegeven:</p>
                            <ul>
                                <li>IP-adres: 192.168.1.13</li>
                                <li>lengte netmasker: 27</li>
                            </ul>
                            <p>Gevraagd:</p>
                            <ul>
                                <li>Netwerkadres</li>
                                <li>Broadcastadres</li>
                                <li>Hostgedeelte (binair)</li>
                                <li>Eerste en laatste hostadres subnet</li>
                                <li>Maximaal aantal hosts</li>
                            </ul>
---
                            <p>Gegeven:</p>
                            <ul>
                                <li>IP-adres: 172.16.5.88</li>
                                <li>lengte netmasker: 20</li>
                            </ul>
                            <p>Gevraagd:</p>
                            <ul>
                                <li>Netwerkadres</li>
                                <li>Broadcastadres</li>
                                <li>Hostgedeelte (binair)</li>
                                <li>Eerste en laatste hostadres subnet</li>
                                <li>Maximaal aantal hosts</li>
                            </ul>
---
                            <p>Gegeven:</p>
                            <ul>
                                <li>IP-adres: 10.4.3.2</li>
                                <li>lengte netmasker: 18</li>
                            </ul>
                            <p>Gevraagd:</p>
                            <ul>
                                <li>Netwerkadres</li>
                                <li>Broadcastadres</li>
                                <li>Hostgedeelte (binair)</li>
                                <li>Eerste en laatste hostadres subnet</li>
                                <li>Maximaal aantal hosts</li>
                            </ul>
---
IP-adressen voor:

- subnetten (netwerk en broadcast)
- routers
- hosts
- **niet** voor modems, hubs of switches

note:
voor routers minstens twee

---
                Routers
                <aside class="notes">
                    <ul>
                        <li>in essentie gewoon toestel met twee of meer fysieke netwerkinterfaces (i.e. loopback
                            etc. telt niet)</li>
                        <li>soort verkeersagent: inspecteert adresinformatie en stuurt door op juiste interface</li>
                        <li>gewone hardware kan als router optreden (als je twee netwerkinterfaces hebt).</li>
                        <li>draadloze hotspot is vorm hiervan</li>
                    </ul>
                </aside>
---
                (Default) gateway
                <aside class="notes">
                    <ul>
                        <li>"gateway" wijst op grens tussen systemen.</li>
                        <li>in netwerkconfiguratie wordt hiermee bedoeld: router die dit netwerk van de rest afbakent
                        </li>
                        <li>Moet dus bereikbaar zijn op het netwerk.</li>
                    </ul>
                </aside>
---
![ARP](./afbeeldingen/ARP.png)

note:

- Eens op juiste netwerk doet machine een broadcast om te vragen wie een bepaald IP heeft.
- Het is dus genoeg om iets tot bij de router te krijgen die aangesloten is op dat subnet.

---
Demo: invullen routeringstabel
---
                Toekenning van adressen
                <aside class="notes">
                    <ul>
                        <li>kan statisch</li>
                        <li>kan geautomatiseerd (typisch gewoon <strong>een</strong> vrij adres nodig)</li>
                        <li>werking DHCP is niet te kennen, als je weet dat het er is, zet je het gewoon aan</li>
                        <li>ISP,... heeft blokken gekocht</li>
                        <li>netmasker in PT <strong>klopt niet altijd</strong></li>
                    </ul>
                </aside>
---
Opdracht

note:
Ken adressen toe. Maak connectiviteit mogelijk.

---
Achilleshiel IPv4

note:

Stel dat heel het Internet één groot lokaal netwerk was (en dat we toch IPv4 gebruikten), hoe veel adressen dan? (toon met rekenmachine)
---
NAT (PAT)

note:
- Netwerkadresvertaling / Network Address Translation
- Vergelijk: ipconfig en myip.com
- (indien IPv4): zal voor iedereen **zelfde** zijn!
---
                <iframe width="560" height="315"
                    src="https://www.youtube.com/embed/2t8-WRZWCq8?si=IbcDw07DOS24lm2u&amp;start=47"
                    title="YouTube video player" frameborder="0"
                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                    allowfullscreen></iframe>
                <aside class="notes">
                    <ul>
                        <li>bestaan variaties op het concept, maar dit is wat je typisch moet weten als programmeur</li>
                        <ul>
                            <li>sommige zaken moeten <strong>eerst</strong> de connectie opstarten</li>
                            <li>poort die zichtbaar is aan ontvangerkant is niet noodzakelijk echte poort van de
                                afzender</li>
                            <li>PAT doorbreekt protocolstapel</li>
                            <li>nadeel: routers moeten details van TCP/UDP kennen, zijn wel dominant maar hindert andere
                                zaken die bovenop IP staan</li>
                            <li>screenshot port forwarding Telenet settings = <strong>dit</strong></li>
                            <li>NAT/PAT doet al vrij lang goed dienst, maar is een "hack". Manuele port forwarding is
                                soms nodig, nieuwe protocols bovenop IP implementeren is lastig.</li>
                            <li>truukje "hole punching" (en reden) moet je kennen voor applicaties</li>
                        </ul>
                    </ul>
                </aside>
---
![port forwarding bij Telenet](./afbeeldingen/portforwarding.png)
