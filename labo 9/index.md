Workshop (deel 3)

note:
- Azure Container Instance ≅ Amazon ECS
 - meer "managed"
- Azure Virtual Machines ≅ Amazon EC2
 - meer eigen controle, maar ook meer eigen werk
- Doorlopen "Deploy a Docker image to an Azure Container Instance". Niet overal hetzelfde als in schriftelijke uitleg ⇒ vraag het aan de lector!
- Wissen resource group op het einde: moet specifiek op detailpagina van die groep staan.
---
[Oefening](https://github.com/v-nys/BaseApp)

note:
- hou geheime info uit versiebeheer d.m.v. een file met omgevingsvariabelen!
---
Docker (Compose) op Azure Virtual Machines

note:
- neem eerdere applicatie als voorbeeld
- maak een VM aan
- installeer Docker met Compose
- zorg dat alle nodige bestanden op de VM staan (`scp` of `git`)
- controleer dat de applicatie bereikbaar is
---
Traefik

note:
- reverse proxy
  - kan dus kenmerken van inkomend verkeer bekijken (hostnaam,... zelfs als IP gelijk is)
  - kan deze gebruiken om de juiste achterliggende service aan te spreken
  - verschillende voordelen
    - extra tussenstop is handig voor monitoren / beveiligen / ... diensten
    - mogelijk meerdere applicaties op dezelfde server te runnen zonder dat gebruiker hier iets van merkt (domeinnamen zijn anders)
    - kan dit via configuratiefile **of** door metadata die aan containers wordt gekoppeld ("labels")
- **erg uitgebreid, we bekijken gewoon basis**
---
![architectuur Traefik](./afbeeldingen/traefik-architecture.png)
---
- entrypoints
- routers
- middleware
- services

note:
- entrypoints geven aan hoe de setup bereikt kan worden (protocol, poort,...)
- routers geven aan hoe een inkomend request doorgestuurd kan worden naar een dienst achter de proxy
- middleware herschrijft een request (bv.: intern wordt geen HTTPS gebruikt en web servers kennen dat meestal ook niet, dus herschrijf naar HTTP)
- services: wat achter de proxy zit
  - Traefik "wrapt" deze, bijvoorbeeld door meerdere instanties met load balancing te behandelen als één service
---
```yaml
services:
  reverse-proxy:
    image: traefik:v3.2
    # dit voegt extra opties toe aan het defaultcommando
    # in de Dockerfile voor Traefik staat "entrypoint"
    # dat deel is vast
    command: --api.insecure=true --providers.docker
    ports:
      - "80:80"
      - "8080:8080" # web UI, kan door api.insecure=true
    volumes:
      # zo kan Traefik meteen reageren op wijzigingen containers
      - /var/run/docker.sock:/var/run/docker.sock
```

note:
- Traefik kan ook met andere systemen werken, maar hier wordt het nu eenmaal opgestart in Docker (Compose)
- Run dit al een keer, toon web UI
  - bekijk entrypoints (HTTP entrypoint = TCP op poort 80,...)
  - tabs bovenaan: HTTP, TCP, UDP geven routers, services en middleware **voor die protocols**
    - M.O.: HTTP krijgt dus speciale behandeling want is bovenop TCP (en HTTP3 uiteindelijk bovenop UDP)
  - klik op een router om in detail te bekijken
    - rule drukt uit wanneer die router zal doorsturen naar bepaalde service
      - `Host` voor wanneer we de reverse proxy bereikt hebben via bepaalde hostnaam
      - `PathPrefix` voor wanneer onderdeel voorkomt vooraan in pad (na domeinnaam): zie bv. `http://localhost:8080/api/entrypoints`, is niet op dashboard
---
```yaml
services:
  reverse-proxy:
    image: traefik:v3.2
    command: --api.insecure=true --providers.docker
    ports:
      - "80:80"
      - "8080:8080" # web UI, kan door api.insecure=true
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
  # labels zijn voor metadata
  # Traefik kan deze uitlezen (omwille van docker.sock)
  whoami:
    image: traefik/whoami
    labels:
      - "traefik.http.routers.whoami.rule=Host(`whoami.localhost`)"
```

note:
- toegelaten subdomeinen voor localhost te voorzien
- toon effect van toevoeging: surf naar `whoami.localhost`
  - officieel voorbeeld gebruikt `.docker.localhost` maar redundant en introduceert verwarring over rol
- check met `docker inspect`
- check ook via web UI
  - Docker provider ⇒ metadata van hierboven wordt gebruikt om regel te communiceren naar Traefik
---
Oefening:
- zorg dat een Apache web server toegankelijk is via mywebsite.localhost
- zorg dat hier een eigen indexpagina in staat, niet de default
---
![statische en dynamische configuratie](./afbeeldingen/static-dynamic-configuration.png)

note:
- wijziging statische informatie vereist restart
- wijziging dynamische config kan na deployment gebeuren
---
Statische configuratie

note:
- meerdere opties, maar mutually exclusive
  - CLI
  - YAML file
  - omgevingsvariabelen
- eerdere voorbeelden gebruikten CLI: `providers.docker`
  - veel meer opties: https://doc.traefik.io/traefik/reference/static-configuration/cli/
  - entrypoint HTTP is er sowieso, "traefik" komt van insecure API
---
Dynamische configuratie

note:
- veel concepten: bekijk in termen van "hoe komt iets binnen en waar moet het naartoe?"
- we bespreken hier alleen dynamische configuratie via file en met Docker (inclusief Compose); er zijn andere manieren
  - vereist toegang tot Docker socket
    - dit omvat eigenlijk een security risico (succesvolle aanval op Traefik = op heel de **host**)
      - workarounds in Traefik docs, kan ook via dynamische config file werken
---
![overzicht architectuur Traefik](./afbeeldingen/architecture-overview.png)
note:
- check zaken in de UI!
---
```yaml
entryPoints:
  web:
    address: :8081
providers:
  file:
    filename: /path/to/dynamic/conf
```
note:
statisch
---
```yaml
http:
  routers:
    to-whoami:
      rule: "Host(`example.com`) && PathPrefix(`/whoami/`)"
      middlewares:
        - test-user # zie onder
      service: whoami
  middlewares:
    test-user:
      basicAuth:
        users:
        # dit is niet het wachtwoord, maar de hash
        # run htpasswd -nb usernaamnaarkeuze wachtwoordnaarkeuze
        - user:hashcode
  services:
    whoami:
      loadBalancer: # zie Traefik docs
        servers:
        - url: http://private/whoami-service
```
note:
- dynamisch
- vergelijk met onderdelen dashboard
- htpasswd staat op httpd images
---
Oefening

note:
- start deze config samen met website in Docker Compose
- gebruik example.localhost in plaats van example.com
- zie Docker Hub voor gebruik statische config file
---
TLS (manueel)

note:
- gebruikt poort 443, dus vereist extra entrypoint
- router omvat een sectie `tls` met waarde `{}` (geen opties)
- sectie voor het **protocol** TLS omvat `certificates`, zie "User defined" gedeelte in docs HTTPS & TLS → TLS
  - genoeg om "cert" en "privkey" van traefik.me te halen en als enige certificate / private key pair te voorzien, andere files niet nodig
---
Oefening

note:
- deploy eigen site op een Azure VM
- gebruik een self-signed certificate
- **niet alle poorten zijn altijd bruikbaar, bv. web UI gebruikt 8080...**
---
TLS (automatisch)

note:
- https://doc.traefik.io/traefik/https/acme/
- eigenlijk maar paar zaken nodig
  - certificatesResolvers gedeelte in statische config
    - start eerst met de staging server!
  - optie certresolver in dynamische config
  - initieel lege acme.json file (moet containers overleven) met permissies 600
---
Oefening

note:
- registreer een gratis domeinnaam (via Github Student Developer Pack) voor je Azure machine
- configureer automatische aanvraag en renewal LetsEncrypt, eerst met de test service
- switch uiteindelijk naar echte service
