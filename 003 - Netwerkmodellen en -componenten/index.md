Netwerkmodellen

note:
- abstractie
- schept orde
- allerlei apparaten
- allerlei media
- allerlei programma's en taken
---
Lagenschema

note:
- zelfde laag = zelfde soort taak
- kunnen rekenen op ondersteuning *onderliggende* laag
- werking hogere laag is "onzichtbaar"
- één interactie op één laag volgt één protocol
- kunnen meerdere protocols op dezelfde laag zijn
- **even conceptueel, zullen zien wat dit concreet betekent met Wireshark**
---
Voorbeeld: filosofen

![filosofen communiceren in verschillende talen](./afbeeldingen/filosofen.png)
<!-- figuur overgenomen uit Tanenbaum, Computer Networks, 5e editie -->
note:
- 3-lagenmodel
- analogie stopt hier, maar kan verder toegepast worden
  - we zouden een "faxlaag" kunnen voorzien, maar de fax is hier "black box"
---
Enkele termen:

- simplex
- duplex
- half-duplex

note:
- hangt af van communicatiekanaal en protocol
  - bv. walkie-talkie
---
- conceptueel overzicht
- relaties tussen gehelen zijn duidelijk
- "swappable" (met beperkingen, bv. bedraad naar draadloos of interplanetair...)

note:
- Dit zijn voordelen van het lagenmodel
---
- Theorie: OSI-model
- Praktijk: TCP/IP-model
---
<!-- .slide:data-background="#ffffff"-->
![OSI-model](./afbeeldingen/osi.svg)
<!--<a href="https://commons.wikimedia.org/wiki/File:Osi-model-jb.svg">SVG edition:Gorivero</a>,<a href="http://creativecommons.org/licenses/by-sa/3.0/">CC BY-SA 3.0</a>,via Wikimedia Commons-->
note:

- verwacht niet dat je ze na deze slide allemaal uit het hoofd kent, gewoon dat je je een beeld vormt!
---
![TCP/IP](./afbeeldingen/TCP-IP_Model_-_en.png)
<!--afgeleid van afgeleid van <a
								href="https://commons.wikimedia.org/wiki/File:TCP-IP_Model_-_en.png">Michel Bakni</a>,
							<a href="https://creativecommons.org/licenses/by-sa/4.0">CC BY-SA 4.0</a>, via Wikimedia
							Commons-->
note:

- spreken soms over "laag 1", "laag 2",... en betreft dan meestal dit model, van onder naar boven
- verwacht nog steeds niet dat je ze allemaal uit het hoofd kent!
- de concrete protocols zouden kunnen zijn "Filosofie", "Recht", "Geneeskunde",... of "Engels", "Frans", "Duits" of "Fax", "Telefoon", "e-mail",...
---
![Vergelijking OSI en TCP/IP](./afbeeldingen/vergelijking-osi-tcpip.png)
<!--<a
								href="https://commons.wikimedia.org/wiki/File:Comparaison_des_mod%C3%A8les_OSI_et_TCP_IP.png">tomsgued</a>,
							<a href="https://creativecommons.org/licenses/by-sa/3.0">CC BY-SA 3.0</a>, via Wikimedia
							Commons-->

note:
- fysieke laag wordt soms wel, soms niet vermeld, hier dus niet
---
<!-- .slide:data-background="#ffffff"-->
![netwerkcomponenten en situering](./afbeeldingen/netwerkcomponenten.png)
note:
- elke component ruwweg geassocieerd met bepaalde laag
- in principe enkel toegang tot eigen laag en lagere lagen
- metadata van lagere lagen mag worden aangepast, inhoud normaal niet
- hub: kopieert gewoon bits (in dat opzicht fysiek)
- switch: werkt alleen binnen één lokaal netwerk (kent geen laag 3 protocol), maar kopieert niet domweg
- router: inspecteert IP-adressen, dus kent het IP-protocol, dus laag 3
---
<!-- .slide:data-background="#ffffff"-->
![encapsulatie in het TCP/IP-model](./afbeeldingen/encapsulatie.svg)
<!--<a href="https://commons.wikimedia.org/wiki/File:Inkapseling_bij_tcp-ip-nl.svg">David Mudrák (mudrdmz)</a>, <a href="https://creativecommons.org/licenses/by-sa/3.0">CC BY-SA 3.0</a>, via Wikimedia Commons-->
note:
- allerlaagst: bits op het medium
- data van de hogere laag = "payload"
- "payload" is dus verschillend naargelang de laag
- dit is wat je in Wireshark te zien krijgt als je een pakket in detail bekijkt, je kan de verschillende laagjes data bekijken
---
<!-- .slide:data-background="#ffffff"-->
![encapsulatie doorheen het netwerk](./afbeeldingen/encapsulatie-doorheen-netwerk.svg)

note:
- constant proces van "inpakken, uitpakken, inpakken, uitpakken,..." van data
---
Client vs. server
![client-server interactie](./afbeeldingen/client-server.png)
<!--Client-server, afbeelding van <a href="https://pixabay.com/users/sandra_schoen-53876/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=341420">Sandra Schön</a> from <a href="https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=341420">Pixabay</a>-->
---
Demonstratie Wireshark

note:
- surf naar website zonder TLS, bijvoorbeeld neverssl.com of flowgorithm.org en toon de inhoud in Wireshark
- surf ook naar www.ubuntu.com of iets dergelijks en toon structuur
---
Opdracht met trace

- Welke lagen van TCP/IP zijn hier vertegenwoordigd?
- Uit hoeveel bytes bestaan de volledige pakketjes?
- Hoeveel bytes bedraagt de payload in elk pakket?
- Van waar komen deze pakketjes eigenlijk? Wat heeft ze doen genereren?
- Wat is het laag 3-adres van de client in deze interactie?
- Wat is het laag 3-adres van de server in deze interactie?
- Is deze interactie "simplex", "duplex" of "half-duplex"?
---
Opdracht eigen traces

- Start een capture in Wireshark
- Surf naar neverssl.com
- Stop meteen de capture en sla op
- Start een nieuwe capture
- Surf naar www.ap.be
- Stop meteen de capture en sla op
- Zoek in beide captures de HTML voor de pagina
  - Verklaar
---
Opdracht Azure

- [deze pagina](https://azure.microsoft.com/nl-nl/free/students)
- kies "gratis aan de slag"
- probeer met `s...@ap.be` of met `voornaam.familienaam@student.ap.be` indien dat eerste niet werkt
- upload tegen volgende les screenshot
  - geen tegoed / geweigerd? upload de foutmelding.
---
Voorbeeld:

![voorbeeld met belangrijke onderdelen](./afbeeldingen/azure-for-students.png)

