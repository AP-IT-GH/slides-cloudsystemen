CI/CD¹

note:
- allemaal zelfde "C"
- integration: voortdurend afstemmen dmv samenvoegen broncode
- delivery: een volledige build afleveren
- deployment: die build ook in productie plaatsen
---
![Pipeline](./afbeeldingen/pipeline.png)
---
Git hooks

note:
- githooks.com (ook: https://kinsta.com/blog/git-hooks/)
- ingebouwd in Git, op zich niets extra nodig
- gewoon uitvoerbare scripts met vaste namen in de map `.git/hooks`, in programmeertaal naar keuze
  - pre-commit (kan via exit code commit tegenhouden)
  - post-commit
  - pre-receive (ook via exit code, maar voor accepteren push)
  - post-receive
- staan (omwille van locatie) **standaard niet** in versiebeheer en dus ook niet gelinkt aan branches
- kunnen op developermachines staan, **maar ook op remotes**
- runnen lokaal
  - niets verhindert dat een hook zelf een container of VM opstart...
- runnen **synchroon**, dus liefst wel snappy
---
"grotere" CI/CD tools

![Jenkins](./afbeeldingen/jenkins-logo.svg)
![Github Actions](./afbeeldingen/github-actions-logo.png)
![Travis](./afbeeldingen/travis-logo.png)

note:
- hosted (en dus sowieso asynchroon)
- zowat allemaal plugingebaseerd
  - minder zelf code schrijven
  - gestructureerder
  - higher level (standaard gekoppeld aan branches,...)
---
Github Actions

note:
- wel onder versiebeheer en branchspecifiek, dus goed voor teams
- heel populair: zonder setup geïntegreerd met populaire hosting dienst
- gratis voor publieke code (zoals vaker bij Github)
- zelf scripten blijft mogelijk, maar er is bijkomende structuur ("kant-en-klare" actions)
- runnen standaard in specifieke VM, resultaat is "voorspelbaarder" dan bij lokale scripts
---
workflows / jobs / steps

note:
- workflow = 1 proces dat doorlopen moet worden
- job = 1 taak binnen dat proces, normaal parallel
- step = 1 stap binnen een job, altijd sequentieel
  - steps kunnen scripts of actions (voorverpakte handelingen) zijn
  - typische actions kunnen uit soort "app store" gehaald worden
  - hoef dan zelf geen volledig script te schrijven
---
Samen doorlopen: [Building a workflow with Github Actions: Essentials](https://resources.github.com/learn/pathways/automation/essentials/automated-application-deployment-with-github-actions-and-pages/) (guide 1 en 2)
---
Opdrachten
---
Schrijf een Workflow om de tests voor de voorbeeldapplicatie te runnen voor elke commit. Test uit.
---
Zet de in-memory versie van de gastenboekapplicatie in een Git repository. Zet die op Github. Zorg dat een Docker image gebouwd wordt wanneer je main uitbreidt.
---
Act

note:

- kunnen "doen alsof" event plaatsvindt om juiste workflows te runnen
- kunnen dit automatiseren met een heel korte Git hook (bijvoorbeeld: pre-commit Act dat event laten simuleren)
- vermijdt ontdubbeling tussen Git hooks en Github Actions
---
- Installeer en probeer met de twee eerdere workflows.
- Gebruik Windows Terminal voor de duidelijkste output!
- Geen optie om image te kiezen? Gebruik `-P ubuntu-latest=ghcr.io/catthehacker/ubuntu:act-22.04`
---
Verdere guides

note:
- 3, 4, 5, secrets.
- 5 heeft vreemde volgorde.
---
Web hooks

note:
- anders dan Git hooks, maar ook manier om te zorgen dat "als A gebeurt, dan moet B ook gebeuren"
- "reverse API call"
- handig in CI/CD: "build is gelukt" ⇒ kan bijvoorbeeld container updaten
  - er is een Github Action voor, maar eigenlijk is `curl` al genoeg
---
Opdracht
---
- lokale web hook (om te testen)
- vervolledig de code op DigitAP
- voer de workflow uit via Act
- lees de opties via `act --help`
- gebruik je kennis over Docker
- hoe zie je dat de web hook succesvol is gebruikt...?
---
Voor het project:

- run web hook (buiten container) achter Traefik (met authenticatiemiddleware)
- combineer met [Docker Compose package](https://www.npmjs.com/package/docker-compose) voor automatische deployment
