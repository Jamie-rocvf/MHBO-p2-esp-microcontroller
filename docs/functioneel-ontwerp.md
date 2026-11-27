# Functioneel ontwerp

**Project:** MHBO-p2-esp-microcontroller
**Leden:** Ian, Jamie
**Datum:** 13-11-2026

---

## Versiebeheer

| Versie | Datum      | Auteur(s)  |
|--------|------------|------------|
| 0.1    | 13-11-2026 | Ian, Jamie |

---

## Inhoudsopgave

1. [Inleiding](#1-inleiding)
2. [Doel](#2-doel)
3. [Functionele eisen](#3-functionele-eisen)
   - 3.1 [Must have](#31-must-have)
   - 3.2 [Should have](#32-should-have)
   - 3.3 [Could have](#33-could-have)
   - 3.4 [Won't have](#34-wont-have)
4. [Ethische analyse](#4-ethische-analyse)
   - 4.1 [Belanghebbenden](#41-belanghebbenden)
   - 4.2 [Ethische vraagstukken](#42-ethische-vraagstukken)
   - 4.3 [Normen en waarden](#43-normen-en-waarden)
   - 4.4 [Ontwerpkeuzes](#44-ontwerpkeuzes)
   - 4.5 [Gevolgen](#45-gevolgen)
   - 4.6 [Conclusie](#46-conclusie)

---

## 1. Inleiding

Dit document beschrijft het functioneel ontwerp van het prototype voor het visualiseren van snelheidsgegevens. Een ESP-microcontroller meet regelmatig de snelheid van een voertuig en stuurt deze metingen naar een computer. Op dit moment worden de metingen alleen als losse waarden weergegeven, waardoor het lastig is om snel te zien hoe de snelheid zich ontwikkelt of welke metingen opvallen.

De opdrachtgever, [naam opdrachtgever], heeft gevraagd om een gebruiksvriendelijk dashboard dat de metingen overzichtelijk presenteert. Het dashboard moet de actuele snelheid tonen, eerdere metingen bewaren en beschikbaar maken, metingen laten zien in een grafiek, snelheden indelen in categorieën en waarschuwen bij een hoge snelheid. Ongeldige metingen moeten worden herkend en mogen niet tussen de geldige metingen terechtkomen.

Het functioneel ontwerp is gebaseerd op de casus en de requirements volgens de MoSCoW-methode. Het beschrijft wat het systeem moet doen, niet hoe het technisch wordt gebouwd. De ethische analyse in hoofdstuk 4 beschrijft welke gevolgen het ontwerp kan hebben voor gebruikers en andere betrokkenen.

---

## 2. Doel

### 2.1 Doel van het systeem

Het doel is een overzichtelijk en betrouwbaar prototype waarmee een gebruiker actuele en historische snelheidsgegevens van een voertuig kan bekijken en analyseren. Het systeem hoeft geen professioneel verkeerssysteem te zijn.

### 2.2 Concrete doelen

- De gebruiker ziet in één oogopslag de meest recente snelheid en het tijdstip van de meting.
- De gebruiker begrijpt het verloop van de snelheid via een grafiek.
- De gebruiker kan snelheden beoordelen aan de hand van duidelijke categorieën (laag, normaal, hoog).
- De gebruiker krijgt een duidelijke waarschuwing bij een opvallend hoge snelheid.
- Onjuiste of onvolledige metingen worden herkend en niet zomaar geaccepteerd.
- Metingen blijven bewaard, ook na het opnieuw starten van de applicatie.
- De gebruiker kan foutieve metingen corrigeren of verwijderen.

### 2.3 Afbakening

- Het prototype legt geen boetes of sancties op.
- Het prototype wordt niet gekoppeld aan politie- of verkeerssystemen.
- Het prototype doet geen persoonsgerichte profilering.

---

## 3. Functionele eisen

De functionele eisen zijn opgesteld op basis van de requirements volgens de MoSCoW-methode. Per eis is een beschrijving opgenomen, gevolgd door de functionele uitwerking en de acceptatiecriteria. Eisen zijn nummeriek gelabeld met het prefix M (Must), S (Should), C (Could) en W (Won't).

### 3.1 Must have

#### M1. Actuele snelheid weergeven

**Beschrijving:** Het dashboard toont de meest recente snelheidsmeting duidelijk, inclusief het tijdstip waarop deze is gemeten.

**Functionele uitwerking:**
- Het dashboard toont de snelheid in km/h als grootste element op het scherm.
- Het tijdstip van de meting staat direct onder of naast de snelheid.
- De weergave wordt automatisch bijgewerkt wanneer een nieuwe geldige meting binnenkomt.
- Als er nog geen meting is, toont het dashboard een duidelijke melding dat er nog geen gegevens zijn.

**Acceptatiecriteria:**
- De meest recente geldige meting is zichtbaar zonder dat de gebruiker iets hoeft te doen.
- Het tijdstip wordt in hetzelfde formaat getoond als de meting is ontvangen.

#### M2. Snelheidsmetingen bewaren

**Beschrijving:** De applicatie slaat ontvangen snelheidsmetingen blijvend op, zodat deze beschikbaar blijven nadat de applicatie opnieuw is gestart.

**Functionele uitwerking:**
- Elke ontvangen meting wordt opgeslagen met de waarde, het tijdstip en een uniek identificatienummer.
- Opgeslagen metingen blijven beschikbaar na een herstart van de applicatie.
- Metingen worden alleen opgeslagen nadat ze zijn gecontroleerd op geldigheid (zie M9).

**Acceptatiecriteria:**
- Een meting die is ontvangen blijft zichtbaar na het afsluiten en opnieuw starten van de applicatie.
- Er gaat geen meting verloren bij het ontvangen van opeenvolgende metingen.

#### M3. Historische metingen bekijken

**Beschrijving:** De gebruiker kan eerder opgeslagen snelheidsmetingen bekijken.

**Functionele uitwerking:**
- Het dashboard toont een overzicht van opgeslagen metingen, gesorteerd op tijdstip.
- Het overzicht toont per meting minimaal de snelheid, het tijdstip en de categorie.
- Bij een groot aantal metingen is het overzicht gepagineerd of scrollbaar.

**Acceptatiecriteria:**
- De gebruiker kan alle opgeslagen metingen terugvinden in het overzicht.
- De volgorde van de metingen klopt met het tijdstip waarop ze zijn gemeten.

#### M4. Een meting aanpassen

**Beschrijving:** De gebruiker kan een aantoonbaar onjuiste meting aanpassen.

**Functionele uitwerking:**
- De gebruiker selecteert een meting in het overzicht en kiest voor aanpassen.
- De gebruiker kan de snelheid en/of het tijdstip wijzigen.
- Bij het opslaan wordt de nieuwe waarde gecontroleerd op geldigheid (zie M9).
- De applicatie registreert dat de meting is aangepast, inclusief het tijdstip van de wijziging.

**Acceptatiecriteria:**
- Een aangepaste meting toont de nieuwe waarde in het overzicht en in de grafiek.
- Een ongeldige waarde wordt niet opgeslagen, en de gebruiker krijgt een melding met de reden.

#### M5. Een meting verwijderen

**Beschrijving:** De gebruiker kan een niet-relevante of foutieve meting verwijderen.

**Functionele uitwerking:**
- De gebruiker selecteert een meting en kiest voor verwijderen.
- Het systeem vraagt om bevestiging voordat de meting definitief wordt verwijderd.
- Na verwijdering verdwijnt de meting uit het overzicht, de grafiek en de samenvatting.

**Acceptatiecriteria:**
- Een verwijderde meting komt niet meer terug na het opnieuw starten van de applicatie.
- Zonder bevestiging wordt er niets verwijderd.

#### M6. Het snelheidsverloop visualiseren

**Beschrijving:** Het dashboard toont het verloop van de snelheid in een grafiek of een andere passende visualisatie.

**Functionele uitwerking:**
- De grafiek toont de snelheid op de verticale as en het tijdstip op de horizontale as.
- De grafiek toont de grenzen van de snelheidscategorieën als horizontale lijnen of kleurvlakken.
- Met de periode-selectie (zie S3) past de grafiek zich aan.

**Acceptatiecriteria:**
- De grafiek is leesbaar en de assen zijn voorzien van eenheden en labels.
- Bij een geselecteerde periode toont de grafiek alleen metingen binnen die periode.

#### M7. Snelheidscategorieën tonen

**Beschrijving:** Het dashboard deelt iedere geldige meting in een snelheidscategorie in. Er worden minimaal drie categorieën gebruikt: lage snelheid (15 km/h), normale snelheid (30 km/h) en hoge snelheid(40 km/h).

**Functionele uitwerking:**
- Elke geldige meting krijgt precies één categorie toegewezen.
- De grenswaarden van de categorieën worden door de projectgroep vastgesteld: 15 km/h en 40 km/h.
- De grenswaarden worden zichtbaar gemaakt voor de gebruiker, bijvoorbeeld in een legenda of op een informatiepagina.
- Elke categorie heeft een eigen kleur én een eigen tekstlabel, zodat de categorie niet alleen via kleur te herkennen is.

**Acceptatiecriteria:**
- Iedere geldige meting heeft precies één categorie.
- De grenswaarden staan zichtbaar in het dashboard.

#### M8. Waarschuwen bij een hoge snelheid

**Beschrijving:** Het dashboard geeft een duidelijke waarschuwing wanneer een snelheid binnen de hoogste categorie valt.

**Functionele uitwerking:**
- Bij een meting in de categorie hoge snelheid verschijnt een opvallende melding op het dashboard.
- De melding bevat de gemeten snelheid, het tijdstip en de categorie.
- De waarschuwing blijft zichtbaar tot de gebruiker deze bevestigt of de volgende meting binnenkomt.
- De waarschuwing is niet alleen op kleur gebaseerd, maar ook als tekst.

**Acceptatiecriteria:**
- Een meting in de hoge categorie leidt altijd tot een zichtbare waarschuwing.
- Een meting buiten de hoge categorie leidt niet tot een waarschuwing.

#### M9. Ongeldige metingen afhandelen

**Beschrijving:** De applicatie herkent onvolledige, foutieve of onrealistische metingen en voorkomt dat deze tussen de geldige metingen terechtkomen.

**Functionele uitwerking:**
- Een meting wordt als ongeldig beschouwd wanneer:
  - een waarde ontbreekt of niet numeriek is;
  - de snelheid negatief is;
  - de snelheid boven een realistische bovengrens ligt (grenswaarde te bepalen door de projectgroep: 100 km/h);
  - het tijdstip ontbreekt of niet geldig is;
  - het tijdstip in de toekomst ligt of duidelijk niet bij de rest van de metingen past.
- Ongeldige metingen worden niet opgeslagen in het hoofdoverzicht of de grafiek.
- De gebruiker krijgt een duidelijke uitleg van wat er niet klopt, bijvoorbeeld: "Snelheid ontbreekt" of "Snelheid van 350 km/h ligt boven de realistische bovengrens."
- Ongeldige metingen worden apart bewaard in een overzicht van afgewezen metingen, zodat de gebruiker ze kan controleren.

**Acceptatiecriteria:**
- Een ongeldige meting verschijnt niet in de grafiek, de samenvatting of het hoofdoverzicht.
- Bij elke afgewezen meting is zichtbaar welke reden is gebruikt.

#### M10. Een duidelijk dashboard aanbieden

**Beschrijving:** Het dashboard is zonder uitgebreide instructie bruikbaar.

**Functionele uitwerking:**
- De belangrijkste informatie (actuele snelheid, categorie en waarschuwing) staat bovenaan het scherm.
- Knoppen en acties hebben duidelijke labels in begrijpelijke taal.
- Het scherm bevat niet meer informatie dan nodig; secundaire functies staan op een aparte pagina of in een uitklapbaar deel.
- Er is een korte, optionele uitleg beschikbaar van het dashboard en de categorieën.

**Acceptatiecriteria:**
- Een gebruiker die het dashboard voor het eerst ziet, kan de actuele snelheid en de categorie zonder uitleg aflezen.
- Acties zoals bewerken en verwijderen zijn zonder handleiding te vinden.

### 3.2 Should have

#### S1. Metingen filteren

**Beschrijving:** De gebruiker kan metingen filteren op datum of periode, minimale en maximale snelheid en snelheidscategorie.

**Functionele uitwerking:**
- Filters zijn combineerbaar, bijvoorbeeld: alleen metingen van vandaag in de categorie hoge snelheid.
- Actieve filters zijn zichtbaar en kunnen met één actie worden gewist.
- Het overzicht en de grafiek passen zich aan op de actieve filters.

**Acceptatiecriteria:**
- Met elk filter toont het overzicht alleen de metingen die aan de filtervoorwaarden voldoen.
- Het wissen van filters herstelt het volledige overzicht.

#### S2. Samenvattende informatie tonen

**Beschrijving:** Het dashboard toont over de geselecteerde metingen de laagste, hoogste en gemiddelde snelheid en het aantal metingen.

**Functionele uitwerking:**
- De samenvatting wordt bijgewerkt bij elke wijziging van de selectie of filters.
- Bij een selectie zonder metingen toont de samenvatting "geen metingen" in plaats van een getal.
- Het gemiddelde wordt afgerond op één decimaal.

**Acceptatiecriteria:**
- De samenvatting klopt met de metingen in de geselecteerde periode of selectie.
- Bij nul metingen wordt geen gemiddelde of minimum getoond.

#### S3. Een periode selecteren

**Beschrijving:** De gebruiker kan zelf een periode selecteren die in het overzicht en de visualisatie wordt getoond.

**Functionele uitwerking:**
- De periode wordt ingesteld met een begin- en eindtijdstip via een datumkiezer.
- Snelkeuzes zijn beschikbaar, bijvoorbeeld "laatste uur", "vandaag" en "laatste 7 dagen".
- Een ongeldige periode (eindtijdstip vóór begintijdstip) wordt geweigerd met een melding.

**Acceptatiecriteria:**
- Het overzicht en de grafiek tonen alleen metingen binnen de gekozen periode.
- Een ongeldige periode kan niet worden toegepast.

#### S4. Waarschuwingen terugvinden

**Beschrijving:** De gebruiker kan een overzicht bekijken van alle metingen waarbij een hoge snelheid is vastgesteld.

**Functionele uitwerking:**
- Er is een aparte weergave of filter met alleen metingen in de categorie hoge snelheid.
- Per waarschuwing zijn snelheid, tijdstip en eventuele wijzigingen zichtbaar.

**Acceptatiecriteria:**
- Het overzicht bevat alle en alleen metingen in de hoge categorie.

#### S5. Metingen zoeken

**Beschrijving:** De gebruiker kan een specifieke meting zoeken op datum of tijdstip.

**Functionele uitwerking:**
- Een zoekveld accepteert een datum of een datum met tijdstip.
- Bij een treffer wordt de meting geselecteerd en gemarkeerd in het overzicht.
- Bij geen treffer toont het systeem een melding.

**Acceptatiecriteria:**
- Een bestaande meting wordt gevonden met de juiste datum of tijd.
- Een ingevoerde datum die niet bestaat in de data geeft een duidelijke melding.

### 3.3 Could have

#### C1. Perioden vergelijken

**Beschrijving:** De gebruiker kan snelheidsmetingen uit twee verschillende perioden met elkaar vergelijken.

**Functionele uitwerking:**
- De gebruiker kiest twee periodes; de samenvatting en grafiek tonen beide naast elkaar.

#### C2. Gegevens exporteren

**Beschrijving:** De gebruiker kan een selectie van de metingen exporteren voor verder gebruik.

**Functionele uitwerking:**
- Export naar een CSV-bestand met de kolommen: id, tijdstip, snelheid, categorie, status (geldig/aangepast).

#### C3. Grenswaarden aanpassen

**Beschrijving:** Een bevoegde gebruiker kan de grenzen van de snelheidscategorieën aanpassen.

**Functionele uitwerking:**
- Een beheerscherm met instellingen voor de grenswaarden, met een wachtwoord of andere bevestiging.
- Na een wijziging worden de categorieën van bestaande metingen opnieuw berekend.

#### C4. Afwijkende metingen markeren

**Beschrijving:** Het dashboard kan sterk afwijkende metingen herkenbaar maken.

**Functionele uitwerking:**
- Metingen die ver afwijken van het gemiddelde van de geselecteerde periode krijgen een extra markering.
- De markering is alleen informatief en verwijdert de meting niet automatisch.

#### C5. Meerdere voertuigen onderscheiden

**Beschrijving:** De gebruiker kan metingen van meerdere voertuigen afzonderlijk bekijken.

**Functionele uitwerking:**
- Elke meting krijgt een voertuig-identificatie.
- Het overzicht en de grafiek kunnen op voertuig worden gefilterd.

### 3.4 Won't have

- **W1.** Het prototype legt geen boetes of sancties op.
- **W2.** Het prototype wordt niet gekoppeld aan politie- of verkeerssystemen.
- **W3.** Er vindt geen etnische profilering plaats.

---

## 4. Ethische analyse

### 4.1 Belanghebbenden

| Belanghebbende | Betrokkenheid | Belangen |
|----------------|---------------|----------|
| Bestuurder van het voertuig | Wordt gemeten | Privacy, eerlijke weergave van de eigen gegevens, geen onterechte beschuldigingen |
| Gebruiker van het dashboard (bijv. fleetbeheerder of analist) | Bekijkt, past aan en verwijdert metingen | Betrouwbare data, overzicht, bruikbaar gereedschap |
| Opdrachtgever [naam] | Heeft de opdracht gegeven | Een werkend en overtuigend prototype, binnen de afgesproken kaders |
| Projectgroep | Ontwikkelt het systeem | Haalbaarheid, leerdoelen, een beoordeelbaar resultaat |
| Passagiers en andere weggebruikers | Worden indirect beïnvloed | Veiligheid, privacy, geen onnodige monitoring |
| Docent / beoordelaar | Beoordeelt het ontwerp | Volledigheid, kwaliteit, onderbouwing van keuzes |
| Ontwikkelaars van het ESP-systeem | Leveren de meetgegevens | Correcte werking van de hardware, betrouwbaarheid van de sensor |

### 4.2 Ethische vraagstukken

#### Vraagstuk 1: Privacy

Snelheidsgegevens van een voertuig kunnen herleidbaar zijn tot een persoon, zeker in combinatie met tijdstip en locatie. Het bewaren van historische metingen maakt het mogelijk om bewegingspatronen te reconstrueren. Dit kan leiden tot ongewenste monitoring van de bestuurder.

#### Vraagstuk 2: Betrouwbaarheid van data

Een sensor kan foutieve waarden leveren, en de gebruiker kan metingen aanpassen. Wanneer aangepaste of verwijderde metingen niet zichtbaar worden bijgehouden, kunnen conclusies onjuist of zelfs bewust gemanipuleerd zijn. Bovendien kan een onterechte waarschuwing voor hoge snelheid leiden tot een onterechte beschuldiging.

#### Vraagstuk 3: Veiligheid en interpretatie van waarschuwingen

Een waarschuwing bij hoge snelheid suggereert een oordeel. Als de drempelwaarden niet goed zijn onderbouwd, kan het systeem te vaak of te weinig waarschuwen, met risico's voor het vertrouwen in het dashboard en de veiligheid op de weg.

#### Vraagstuk 4: Gegevensbeveiliging

Opgeslagen metingen zijn gevoelige gegevens. Zonder beveiliging kan iemand zonder recht toegang krijgen tot de bewegingsgegevens van een voertuig.

#### Vraagstuk 5: Toegankelijkheid en inclusie

Een dashboard dat alleen op kleur onderscheid maakt tussen categorieën, of dat niet leesbaar is voor mensen met een visuele beperking, sluit gebruikers uit.

### 4.3 Normen en waarden

De projectgroep hanteert de volgende normen en waarden:

- **Privacy:** de minimale hoeveelheid persoonsgegevens verzamelen en bewaren die nodig is voor het doel.
- **Transparantie:** de gebruiker moet begrijpen hoe categorieën tot stand komen en waarom een meting is afgewezen.
- **Betrouwbaarheid:** onjuiste gegevens mogen niet onopgemerkt in analyses terechtkomen.
- **Verantwoordelijkheid:** aanpassingen en verwijderingen zijn traceerbaar.
- **Inclusie:** het dashboard is bruikbaar voor zoveel mogelijk gebruikers, ongeacht visuele beperking of technische ervaring.

Deze normen en waarden zijn van invloed op de ontwerpkeuzes hieronder.

### 4.4 Ontwerpkeuzes

| Ontwerpkeuze | Norm of waarde | Toelichting |
|--------------|----------------|-------------|
| Geen koppeling met locatiegegevens of voertuig-identiteit in de basisversie (C5 is optioneel) | Privacy | Hierdoor is het moeilijker om bewegingspatronen van een persoon te reconstrueren. |
| Bewerkte en verwijderde metingen worden gemarkeerd met een wijzigingstijdstip | Transparantie, verantwoordelijkheid | De gebruiker en eventuele controleurs kunnen zien wat er is veranderd. |
| Ongeldige metingen worden apart bewaard met een reden | Betrouwbaarheid, transparantie | Fouten blijven zichtbaar en worden niet stilzwijgend weggegooid. |
| Waarschuwingen zijn tekst én kleur | Inclusie | Mensen met kleurenblindheid kunnen de categorie toch herkennen. |
| Grenswaarden zijn zichtbaar voor de gebruiker | Transparantie | De gebruiker kan begrijpen waarom een meting in een categorie valt. |
| Grenswaarden zijn beheerd door een bevoegde gebruiker (C3) | Verantwoordelijkheid | Voorkomt dat iedere gebruiker de beoordeling stilzwijgend verandert. |
| Bevestiging bij verwijderen van een meting | Betrouwbaarheid | Voorkomt onbedoeld verlies van gegevens. |
| Het prototype koppelt niet aan politie- of verkeerssystemen (W2) en legt geen sancties op (W1) | Veiligheid, verantwoordelijkheid | Het systeem wordt niet gebruikt als basis voor handhaving, wat de kans op onterechte gevolgen verkleint. |
| Opslag en toegang zijn beperkt tot het lokale prototype [te bevestigen door projectgroep] | Gegevensbeveiliging | Vermindert het risico op datalekken. |

### 4.5 Gevolgen

**Positieve gevolgen:**
- Gebruikers krijgen een overzicht dat snel en begrijpelijk is, waardoor ze sneller de juiste conclusies trekken.
- Door ongeldige metingen apart te tonen, wordt de betrouwbaarheid van de analyses verhoogd.
- Door transparantie over grenswaarden en wijzigingen, kan de gebruiker de resultaten beter beoordelen en ter discussie stellen.
- Door inclusief ontwerp (tekst naast kleur, duidelijke labels) is het dashboard bruikbaar voor een breder publiek.

**Negatieve gevolgen:**
- Het bewaren van metingen blijft gevoelig, ook zonder locatiegegevens. Een datalek kan alsnog nadelig zijn voor de bestuurder.
- Een waarschuwing kan onterecht zijn, bijvoorbeeld door een defecte sensor of een verkeerd ingestelde grenswaarde, met mogelijk onterechte conclusies als gevolg.
- Het aanpassen van metingen kan misbruikt worden om resultaten te verdraaien, ondanks de logging.
- Een gebruiker die sterk vertrouwt op het dashboard, kan vergeten zelf kritisch te kijken naar de data.

### 4.6 Conclusie

De ethische analyse laat zien dat het grootste risico zit in de combinatie van opgeslagen snelheidsgegevens, mogelijke foutieve metingen en het gebruik van waarschuwingen als oordeel. De projectgroep heeft daarom gekozen voor ontwerpkeuzes die transparantie, betrouwbaarheid en privacy centraal stellen: ongeldige metingen worden niet stilzwijgend verwijderd, wijzigingen worden gelogd, grenswaarden zijn zichtbaar en het prototype wordt niet gekoppeld aan handhavingssystemen.

Een aanbeveling voor de vervolgstappen is om de grenswaarden en de bewaartermijn van metingen te onderbouwen met bronnen en om de beveiliging van de opslag nader uit te werken in het technisch ontwerp.
