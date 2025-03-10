# Docker
---
<iframe width="560" height="315" src="https://www.youtube.com/embed/J0NuOlA2xDc?si=hFJDadYzxPoWgbA8"
    title="YouTube video player" frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen></iframe>
---
container vs. VM

note:
- VM = van de grond op (simulatie HW)
- container = enkel exact wat applicatie nodig heeft
  - tweede VM = dubbele kost
  - tweede container = minimale extra kost
- In de eerste plaats **Linux**, omdat images dus die kernel delen.
---
(installatie Docker Desktop)
---
image vs. container

note:
- "template" vs. instantie
- vergelijk met "klasse" vs. "object"
- filmpje: image = onderste lagen (liggen vast), container heeft jouw eigen extra bovenste laag
---
Workshop (deel 1)

- Doorlopen "Build a containerized..." op Azure tot en met "Customize a Docker image to run your own web app, enkel "Retrieve..."."
---
- download de officiële `nginx` image van Docker Hub
- maak er een container mee en link poort 80 van host naar container
- log in op de container
- installeer een editor: `apt update; apt install nano`
- bewerk `/usr/share/nginx/html/index.html`
- controleer resultaat op `localhost`

note:
- inloggen via `docker exec -it CONTAINERNAAM sh` is **supernuttig**, noteer dit commando goed
---
Workshop (deel 2)

note:
- doe stappen "customize a Docker image..."
---
- Maak een nieuwe Dockerfile.
- Zorg dat hij voortbouwt op `ubuntu:latest`
- Zorg dat hij beschikt over `curl`. In Ubuntu installeer je dit via `apt-get update && apt-get install -y curl`.
- Zorg dat hij de broncode van neverssl.com toont in de terminal wanneer hij start.
- Test uit door hem te builden en uit te voeren.

note:
- pro tip: neem de basisimage en zet het `CMD` op `sleep infinity`, log dan in met `docker exec -it CONTAINERNAAM sh` (of `bash` indien ondersteund), **test een stap uit en voeg hem dan pas toe aan de Dockerfile**
