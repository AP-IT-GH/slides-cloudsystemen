CI/CD¹
<aside class="notes">
    <ul>
        <li>allemaal zelfde "C"</li>
        <li>integration: voortdurend afstemmen dmv samenvoegen broncode</li>
        <li>delivery: een volledige build afleveren</li>
        <li>deployment: die build ook in productie plaatsen</li>
    </ul>
</aside>
---
Pipeline
<img src="./afbeeldingen/ci-cd-flow-desktop.png" />
---
Git hooks
<aside class="notes">
    <!-- https://githooks.com/ -->
    <ul>
        <li>gewoon uitvoerbare scripts met vaste namen in de map <code>.git/hooks</code></li>
        <ul>
            <li>pre-commit (kan via exit code commit tegenhouden)</li>
            <li>post-commit</li>
            <li>pre-receive (ook via exit code, maar voor accepteren push)</li>
            <li>post-receive</li>
        </ul>
        <li>staan (omwille van locatie) <strong>standaard niet</strong> in versiebeheer en dus ook niet
            gelinkt aan branches</li>
        <li>kunnen in allerlei scriptingtalen geschreven zijn (Bash, PowerShell, Python, PHP,...)</li>
        <li>kunnen op developermachines staan, maar ook op remotes</li>
        <li>runnen lokaal</li>
    </ul>
</aside>
---
Github Actions
<aside class="notes">
    <ul>
        <li>wel onder versiebeheer en branchspecifiek, dus goed voor teams</li>
        <li>niet de enige tool voor dit soort taken</li>
        <li>maar wel dominant: zonder setup geïntegreerd met populaire dienst</li>
        <li>gratis voor publieke code (zoals vaker bij Github)</li>
        <li>zelf scripten blijft mogelijk, maar er is bijkomende structuur ("kant-en-klare" actions)</li>
        <li>runnen standaard in specifieke VM, scripts runnen lokaal en (tenzij ze zelf VM of container
            starten) kan resultaat dus verschillen</li>
    </ul>
</aside>
---
workflows / jobs / steps
<aside class="notes">
    <ul>
        <li>workflow = 1 proces dat doorlopen moet worden</li>
        <li>job = 1 taak binnen dat proces, normaal parallel</li>
        <li>step = 1 stap binnen een job, altijd sequentieel</li>
        <ul>
            <li>steps kunnen scripts of actions (voorverpakte handelingen) zijn</li>
            <li>typische actions kunnen uit soort "app store" gehaald worden</li>
            <li>hoef dan zelf geen volledig script te schrijven</li>
        </ul>
    </ul>
</aside>
---
<section>Opdrachten</section>
<section>Schrijf een Workflow om de tests voor de voorbeeldapplicatie te runnen voor elke commit. Test uit.</section>
<section>Zet de in-memory versie van de gastenboekapplicatie in een Git repository. Zet die op Github. Zorg dat een Docker image gebouwd wordt wanneer je main uitbreidt.</section>
