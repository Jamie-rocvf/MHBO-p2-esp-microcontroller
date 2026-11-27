# Plan van aanpak – Snelheidsdashboard

Project MHBO-p2-esp-microcontroller · Casus 1 · Versie 0.1 · 27 november 2026

## Voorblad

| | |
|---|---|
| **Documenttitel** | Plan van aanpak – Snelheidsdashboard |
| **Casus** | Casus 1: Visualisatie van snelheidsdata in een dashboard |
| **Project** | MHBO-p2-esp-microcontroller |
| **Projectgroep** | snelheidsmeterdashboard |
| **Teamleden** | Jamie Leijst, Ian van Nieuwkerk |
| **Docenten** | Hans Rijk & Marcel Runte |
| **Datum** | 27 november 2026 |
| **Versie** | 0.1 |

## Versiebeheer

| Versie | Datum | Auteur(s) | Wijziging |
|---|---|---|---|
| 0.1 | 27-11-2026 | Ian, Jamie | Eerste opzet van het plan van aanpak |
| | | | |

## Inhoudsopgave

1. Voorblad
2. Versiebeheer
3. Inhoudsopgave
4. Inleiding
5. Projectopdracht en doel
6. Op te leveren producten
7. Aanpak en fasering
8. Planning
9. Organisatie en taakverdeling
10. Communicatie en samenwerkingsafspraken
11. Kwaliteit
12. Risico's en maatregelen

## 1. Inleiding

Dit plan van aanpak beschrijft hoe de projectgroep Casus 1 uitvoert: de visualisatie van snelheidsdata in een dashboard. Het document legt vast wat er wordt opgeleverd, in welke stappen er wordt gewerkt, wie waarvoor verantwoordelijk is en hoe het team samenwerkt. Het plan sluit aan op het functioneel ontwerp en het technisch ontwerp.

## 2. Projectopdracht en doel

### 2.1 Achtergrond

Een mobiliteitsbedrijf onderzoekt hoe actuele gegevens uit voertuigen gebruikt kunnen worden om beter inzicht te krijgen in rijgedrag en vervoersbewegingen. Er is een prototype waarbij een ESP regelmatig de snelheid van een voertuig meet en naar een computer verstuurt. Die metingen worden nu alleen als losse waarden weergegeven, waardoor het lastig is om snel te zien hoe de snelheid zich ontwikkelt en welke metingen opvallen.

### 2.2 Doel

Het doel is een overzichtelijk en betrouwbaar prototype waarmee een gebruiker actuele en historische snelheidsgegevens kan bekijken en analyseren. Het dashboard toont de actuele snelheid, bewaart metingen blijvend, laat het snelheidsverloop zien in een grafiek, deelt snelheden in categorieën in, waarschuwt bij een hoge snelheid en herkent ongeldige metingen.

### 2.3 Afbakening

- De Must haves (M1 t/m M10) en Should haves (S1 t/m S5) worden gerealiseerd. De Could haves (C1 t/m C5) worden alleen opgepakt als daar tijd voor is.
- Het prototype legt geen boetes of sancties op, wordt niet gekoppeld aan politie- of verkeerssystemen en doet geen persoonsgerichte profilering (W1 t/m W3).
- Het prototype is geen professioneel verkeerssysteem.
- Het enige beschikbare materiaal is een ESP8266. Omdat er geen snelheidssensor is, worden de snelheidswaarden gesimuleerd.

## 3. Op te leveren producten

| Product | Omschrijving |
|---|---|
| Functioneel ontwerp | Beschrijving van wat het systeem moet doen, met functionele eisen, acceptatiecriteria en ethische analyse |
| Technisch ontwerp | Beschrijving van hardware, software en softwarecomponenten |
| Plan van aanpak | Dit document |
| ESP8266-firmware | Software op de ESP die snelheidsmetingen verstuurt |
| Server met database | Ontvangt, valideert en bewaart de metingen |
| Dashboard | Webpagina met actuele snelheid, grafiek, historie, filters, samenvatting en waarschuwingen |

## 4. Aanpak en fasering

Het project wordt in vijf fasen uitgevoerd. De volgorde van het bouwen volgt de MoSCoW-prioriteit: eerst de Must haves, daarna de Should haves en pas daarna eventuele Could haves.

| Fase | Activiteiten | Resultaat |
|---|---|---|
| 1. Analyse en ontwerp | Casus en requirements bestuderen, functioneel en technisch ontwerp opstellen, ethische analyse uitvoeren | Functioneel ontwerp en technisch ontwerp |
| 2. Bouw ESP en server | Firmware voor de ESP8266 maken, server en database opzetten, validatie van metingen bouwen | ESP stuurt metingen die worden gevalideerd en opgeslagen (M1, M2, M9) |
| 3. Bouw dashboard | Actuele snelheid, historie, aanpassen en verwijderen, grafiek, categorieën en waarschuwing bouwen | Werkend dashboard met alle Must haves (M1 t/m M10) |
| 4. Uitbreiding en testen | Should haves toevoegen (filteren, samenvatting, periode, waarschuwingen terugvinden, zoeken) en alles testen aan de hand van de acceptatiecriteria | Geteste versie met Should haves (S1 t/m S5) |
| 5. Afronding | Feedback verwerken, documenten bijwerken, resultaat presenteren aan de docenten | Opgeleverd prototype en bijgewerkte documentatie |

## 5. Planning

De data worden in overleg met de docenten ingevuld.

| Fase | Start | Einde |
|---|---|---|
| 1. Analyse en ontwerp | [datum] | [datum] |
| 2. Bouw ESP en server | [datum] | [datum] |
| 3. Bouw dashboard | [datum] | [datum] |
| 4. Uitbreiding en testen | [datum] | [datum] |
| 5. Afronding | [datum] | [datum] |

## 6. Organisatie en taakverdeling

| Rol | Naam | Verantwoordelijkheid |
|---|---|---|
| Teamlid | Ian van Nieuwkerk | [taken invullen] |
| Teamlid | Jamie Leijst | [taken invullen] |
| Docent / beoordelaar | Hans Rijk | Begeleiding en beoordeling |
| Docent / beoordelaar | Marcel Runte | Begeleiding en beoordeling |

De taken worden aan het begin van elke fase verdeeld. Er wordt goed afgestemd wie wat doet, zodat iedereen een eerlijke bijdrage levert en niemand profiteert van het werk van anderen.

## 7. Communicatie en samenwerkingsafspraken

| Onderwerp | Afspraak |
|---|---|
| Communicatiemiddelen | WhatsApp en Discord |
| Reactietijd op berichten | Zo snel mogelijk |
| Ziekte | Overleggen wat de persoon vanuit huis kan doen |
| Na afwezigheid | Overleggen wat de persoon vanuit huis kan doen |
| Eerlijke bijdrage | Goed afstemmen wie wat doet |
| Taak niet op tijd af | Overleggen hoe de achterstand kan worden ingehaald |
| Deadline in gevaar | Elkaar direct aanspreken |
| Afspraken niet nagekomen | Aanspreken |
| Eigen stappen vooraf | Elkaar aanspreken en samen een duidelijk stappenplan maken |
| Docent inschakelen | Als het niet lukt om samen tot een oplossing te komen |
| Voorkomen dat iemand meelift | Een goede werkverdeling |
| Zelfde beoordeling voor iedereen | Als er goed wordt samengewerkt en gecommuniceerd |
| Individuele beoordeling | Als er aanwijzingen zijn dat iemand aanzienlijk meer werk heeft gedaan dan de rest |
| Feedback geven | Via WhatsApp en/of Discord |
| Omgaan met feedback | Verwerken in het project, of meenemen naar volgende projecten als het niet over code gaat |
| Irritaties of conflicten | Een gesprek plannen, indien nodig met de docent, en kort aangeven waar het over gaat |

## 8. Kwaliteit

- In groepsverband streven we naar het maximale resultaat (100%).
- Elke functionele eis uit het functioneel ontwerp wordt getest aan de hand van de bijbehorende acceptatiecriteria.
- Ongeldige metingen worden getest met bewust foute waarden vanuit de ESP, zoals een negatieve snelheid of een ontbrekend veld.
- Ontvangen feedback wordt verwerkt in het project.
- De ontwerpdocumenten worden bijgewerkt wanneer er tijdens het bouwen iets verandert.

## 9. Risico's en maatregelen

| Risico | Maatregel |
|---|---|
| Er is geen snelheidssensor beschikbaar | De ESP8266 simuleert snelheidswaarden. De functie die de snelheid bepaalt is los gehouden, zodat later een echte sensor kan worden aangesloten. |
| Een teamlid is ziek of afwezig | Overleggen wat de persoon vanuit huis kan doen en de taken indien nodig herverdelen |
| Een taak is niet op tijd af | Direct overleggen hoe de achterstand wordt ingehaald, zo nodig met prioriteit voor de Must haves |
| Het werk is te groot voor de beschikbare tijd | Volgorde van bouwen volgens MoSCoW; Could haves vervallen als er geen tijd voor is |
| Het WiFi-netwerk werkt niet goed met de ESP8266 | Een 2,4 GHz-netwerk of mobiele hotspot gebruiken |
| Onenigheid in het team | Aanspreken, een gesprek plannen en zo nodig de docent inschakelen |
