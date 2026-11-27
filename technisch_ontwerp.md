**Technisch ontwerp**

Snelheidsdashboard

Casus 1: Visualisatie van snelheidsdata in een dashboard

| **Projectgroep**     | snelheidsmeterdashboard         |
|----------------------|---------------------------------|
| **Teamleden**        | Jamie Leijst, Ian van Nieuwkerk |
| **Opleiding / klas** | Beste mhbo groepje              |
| **Docent**           | Hans Rijk & Marcel Runte        |
| **Datum**            | 20 november 2026                |
| **Versie**           | 0.1                             |

# Versiebeheer

| **Versie** | **Datum**  | **Auteur**        | **Wijziging**                          |
|------------|------------|-------------------|----------------------------------------|
| 0.1        | 20-11-2026 | Ian van nieuwkerk | Eerste opzet van het technisch ontwerp |
|            |            |                   |                                        |

# Inhoudsopgave

1.  Voorblad

2.  Versiebeheer

3.  Inhoudsopgave

4.  Inleiding en doel

5.  Technische specificaties

    - 5.1 Wat het project beoogt te bereiken

    - 5.2 Hardware

    - 5.3 Software, besturingssysteem en server

    - 5.4 Database

    - 5.5 Netwerk

6.  Softwarecomponenten

    - 6.1 Overzicht en gegevensstroom

    - 6.2 ESP8266-firmware

    - 6.3 Validatiemodule

    - 6.4 Categoriemodule

    - 6.5 Databasemodule

    - 6.6 API (backend)

    - 6.7 Dashboard (frontend)

7.  Conclusie

# Inleiding en doel

Een mobiliteitsbedrijf wil actuele gegevens uit voertuigen gebruiken om beter inzicht te krijgen in rijgedrag. Het bedrijf heeft een prototype waarbij een ESP de snelheid van een voertuig meet en naar een computer verstuurt. Op dit moment worden de metingen alleen als losse waarden getoond, waardoor het lastig is om te zien hoe de snelheid zich ontwikkelt en of er opvallende metingen zijn.

Het doel van dit technisch ontwerp is om vast te leggen hoe het snelheidsdashboard technisch wordt opgebouwd. Het document beschrijft welke hardware en software nodig zijn en hoe de softwarecomponenten samenwerken, zodat het dashboard voldoet aan de requirements uit de MoSCoW-analyse van Casus 1.

Het document behandelt:

- de technische specificaties: doel, hardware, software, besturingssysteem, server, database en netwerk;

- een gedetailleerde beschrijving van elke softwarecomponent;

- een conclusie.

Het ontwerp richt zich op de Must haves (M1 t/m M10) en de Should haves (S1 t/m S5). Het resultaat is een overzichtelijk en betrouwbaar prototype en geen professioneel verkeerssysteem.

# Technische specificaties

## 5.1 Wat het project beoogt te bereiken

Het project levert een dashboard waarmee een gebruiker actuele en historische snelheidsgegevens kan bekijken en analyseren. Concreet betekent dit:

- De ESP stuurt regelmatig snelheidsmetingen naar een computer (M1).

- De metingen worden blijvend opgeslagen en blijven beschikbaar na een herstart van de applicatie (M2, M3).

- De gebruiker kan metingen aanpassen en verwijderen (M4, M5).

- Het verloop van de snelheid wordt getoond in een grafiek (M6).

- Elke geldige meting krijgt een categorie: laag, normaal of hoog. Bij een hoge snelheid verschijnt een duidelijke waarschuwing (M7, M8).

- Onvolledige, foutieve of onrealistische metingen worden herkend, niet opgeslagen en voorzien van een uitleg over wat er niet klopt (M9).

- Het dashboard is rustig opgebouwd en zonder uitleg te gebruiken (M10).

- Daarnaast kan de gebruiker filteren, zoeken, een periode kiezen, een samenvatting zien en waarschuwingen terugvinden (S1 t/m S5).

Buiten scope zijn het opleggen van boetes of sancties, koppelingen met politie- of verkeerssystemen en etnisch profileren (W1 t/m W3). De Could haves (C1 t/m C5) zijn in deze versie niet uitgewerkt.

## 5.2 Hardware

Het project gebruikt de volgende hardware. Er zijn geen andere materialen beschikbaar.

| **Component**   | **Specificatie**                                       | **Functie**                                               |
|-----------------|--------------------------------------------------------|-----------------------------------------------------------|
| Microcontroller | ESP8266 met ingebouwde WiFi                            | Genereert en verstuurt snelheidsmetingen                  |
| Computer        | Laptop of pc van een teamlid (Windows, macOS of Linux) | Draait de server, de database en het dashboard            |
| USB-kabel       | USB naar micro-USB                                     | Voeding en het uploaden van de firmware vanaf de computer |

**Beperking:** er is geen snelheidssensor beschikbaar. De ESP8266 meet daarom geen echte voertuigsnelheid, maar genereert in de firmware realistische testwaarden (een geleidelijk stijgende en dalende snelheid met af en toe een uitschieter). De testwaarden worden op dezelfde manier verstuurd als echte metingen. Wanneer later een echte sensor wordt aangesloten, hoeft alleen de functie die de snelheid bepaalt te worden vervangen. De rest van het systeem blijft gelijk.

Om ook foutieve metingen te kunnen testen, kan de firmware bewust af en toe een ongeldige waarde versturen (bijvoorbeeld een negatieve snelheid of een ontbrekend veld).

## 5.3 Software, besturingssysteem en server

| **Onderdeel**              | **Keuze**                                          | **Toelichting**                              |
|----------------------------|----------------------------------------------------|----------------------------------------------|
| Besturingssysteem computer | Windows, macOS of Linux                            | De gekozen software werkt op alle drie       |
| Besturingssysteem ESP8266  | Geen (firmware draait direct op de chip)           | Programmeren via Arduino IDE of PlatformIO   |
| Programmeertaal firmware   | C++ (Arduino-framework voor ESP8266)               | Standaard voor de ESP8266                    |
| Programmeertaal backend    | Python 3 met het framework Flask                   | Eenvoudig te leren en snel een API te bouwen |
| Frontend                   | HTML, CSS en JavaScript                            | Draait in elke moderne browser               |
| Grafiekbibliotheek         | Chart.js                                           | Toont het snelheidsverloop als lijngrafiek   |
| Server                     | De computer zelf (lokale Flask-server, poort 5000) | Er is geen aparte serverhardware nodig       |

De Flask-server draait lokaal op de computer en levert zowel de API als de dashboardpagina. De gebruiker opent het dashboard in een browser via het adres van de computer.

## 5.4 Database

Voor de blijvende opslag wordt SQLite gebruikt. SQLite slaat de gegevens op in één bestand op de computer, vereist geen aparte databaseserver en is daardoor geschikt voor een prototype. Omdat de gegevens in een bestand staan, zijn ze ook na het herstarten van de applicatie nog beschikbaar (M2).

Tabel metingen:

| **Veld**   | **Type**                                        | **Beschrijving**                              |
|------------|-------------------------------------------------|-----------------------------------------------|
| id         | INTEGER, primaire sleutel, automatisch oplopend | Unieke identificatie van de meting            |
| snelheid   | REAL                                            | Gemeten snelheid in km/u                      |
| gemeten_op | TEXT                                            | Tijdstip van meting in UTC (ISO 8601)         |
| aangepast  | INTEGER (0 of 1)                                | Geeft aan of de meting handmatig is aangepast |

De categorie wordt niet opgeslagen, maar berekend op basis van de snelheid en de actuele grenswaarden. Zo blijft de indeling altijd consistent met de grenswaarden.

## 5.5 Netwerk

Het netwerk bestaat uit een bestaand WiFi-netwerk (thuisrouter of mobiele hotspot) waarop zowel de ESP8266 als de computer zijn aangesloten. Er is geen extra netwerkapparatuur nodig.

| **Onderdeel**      | **Specificatie**                                                                      |
|--------------------|---------------------------------------------------------------------------------------|
| Verbinding         | WiFi 2,4 GHz (de ESP8266 ondersteunt geen 5 GHz)                                      |
| Protocol           | HTTP over het lokale netwerk                                                          |
| Gegevensformaat    | JSON                                                                                  |
| Adressering        | De computer heeft een vast of bekend lokaal IP-adres dat in de firmware is ingesteld  |
| Tijdsynchronisatie | De ESP8266 haalt de tijd op via NTP, zodat elke meting een betrouwbaar tijdstip heeft |

Het netwerk is niet gekoppeld aan het internet voor de werking van het systeem, behalve voor de NTP-tijdsynchronisatie en eventueel het laden van de grafiekbibliotheek.

# Softwarecomponenten

## 6.1 Overzicht en gegevensstroom

Het systeem bestaat uit zes softwarecomponenten die samen één keten vormen:

1.  De **ESP8266-firmware** meet (simuleert) een snelheid en verstuurt die als JSON naar de server.

2.  De **API** ontvangt de meting en geeft die aan de **validatiemodule**.

3.  Een geldige meting gaat via de **databasemodule** naar de SQLite-database. Een ongeldige meting wordt afgewezen, niet opgeslagen, en de API geeft een duidelijke foutmelding terug.

4.  Het **dashboard** vraagt de API regelmatig om de gegevens, laat die met behulp van de **categoriemodule** zien als categorie en waarschuwing, en tekent de grafiek.

## 6.2 ESP8266-firmware

**Doel:** regelmatig een snelheidsmeting met tijdstip naar de server sturen.

**Eisen:** M1, M2 (aanlevering van gegevens).

| **Onderdeel**      | **Beschrijving**                                                                                                                                        |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| WiFi-verbinding    | Maakt bij het opstarten verbinding met het WiFi-netwerk. Bij verlies van verbinding wordt automatisch opnieuw verbonden.                                |
| Tijdsynchronisatie | Haalt de actuele tijd op via NTP en voorziet elke meting van een UTC-tijdstip.                                                                          |
| Snelheidsfunctie   | Geeft de huidige snelheid terug. Nu een simulatie, later vervangbaar door een echte sensor.                                                             |
| Verzendfunctie     | Stuurt elke 2 seconden een HTTP POST-verzoek naar /api/metingen.                                                                                        |
| Foutafhandeling    | Als de server niet bereikbaar is of een foutmelding terugstuurt, wordt dit in de seriële monitor getoond en wordt de volgende meting normaal verstuurd. |

Voorbeeld van een bericht:

> { "snelheid": 87.5, "gemeten_op": "2026-11-20T10:15:30Z" }

## 6.3 Validatiemodule

**Doel:** voorkomen dat onbruikbare gegevens tussen de geldige metingen terechtkomen en uitleggen wat er niet klopt.

**Eisen:** M9, M10 (en ondersteunt M4 bij het aanpassen van metingen).

De module controleert elke meting, zowel bij binnenkomst vanaf de ESP als bij het aanpassen door de gebruiker:

| **Controle**       | **Regel**                                                     | **Voorbeeld van foutmelding**                      |
|--------------------|---------------------------------------------------------------|----------------------------------------------------|
| Volledigheid       | snelheid en gemeten_op zijn beide aanwezig                    | “Het veld 'snelheid' ontbreekt.”                   |
| Datatype           | Snelheid is een getal                                         | “De snelheid moet een getal zijn.”                 |
| Realistisch bereik | Snelheid ligt tussen 0 en 250 km/u                            | “Een snelheid van -12 km/u is niet mogelijk.”      |
| Tijdstip           | Tijdstip heeft een geldig formaat en ligt niet in de toekomst | “Het tijdstip is ongeldig of ligt in de toekomst.” |
| Duplicaat          | Er bestaat nog geen meting met exact hetzelfde tijdstip       | “Er is al een meting voor dit tijdstip.”           |

De module geeft een lijst met fouten terug. De API stuurt deze lijst als antwoord mee, zodat het dashboard de gebruiker kan tonen wat er niet klopt. Een meting met één of meer fouten wordt niet opgeslagen.

## 6.4 Categoriemodule

**Doel:** elke geldige meting indelen in een snelheidscategorie en bepalen of er een waarschuwing nodig is.

**Eisen:** M7, M8, S4.

De grenswaarden staan in één configuratiebestand, zodat ze op één plek zijn aan te passen en zichtbaar zijn voor de gebruiker. De projectgroep stelt de volgende grenzen voor:

| **Categorie** | **Snelheid**      | **Kleur in dashboard** |
|---------------|-------------------|------------------------|
| Laag          | 0 t/m 29 km/u     | Blauw                  |
| Normaal       | 30 t/m 99 km/u    | Groen                  |
| Hoog          | 100 km/u en hoger | Rood, met waarschuwing |

De module heeft één functie die een snelheid ontvangt en de categorie teruggeeft. Bij de categorie 'hoog' wordt ook een waarschuwing gemarkeerd. De grenswaarden worden onder het dashboard weergegeven, zodat de gebruiker weet waarop de indeling is gebaseerd.

## 6.5 Databasemodule

**Doel:** metingen blijvend opslaan, opvragen, aanpassen en verwijderen.

**Eisen:** M2, M3, M4, M5, S1, S2, S3, S5.

| **Functie**     | **Beschrijving**                                                                                                                                 |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| Initialiseren   | Maakt de tabel metingen aan als die nog niet bestaat.                                                                                            |
| Toevoegen       | Slaat een gevalideerde meting op.                                                                                                                |
| Laatste ophalen | Geeft de meest recente meting op basis van gemeten_op.                                                                                           |
| Lijst ophalen   | Geeft metingen terug, optioneel gefilterd op periode, minimale en maximale snelheid en categorie (via snelheidsgrenzen), gesorteerd op tijdstip. |
| Zoeken          | Zoekt metingen op datum of tijdstip.                                                                                                             |
| Aanpassen       | Wijzigt de snelheid en/of het tijdstip van een meting en zet aangepast op 1.                                                                     |
| Verwijderen     | Verwijdert een meting op basis van het id.                                                                                                       |
| Samenvatten     | Berekent voor een selectie de laagste, hoogste en gemiddelde snelheid en het aantal metingen.                                                    |

Alle databasevragen gebruiken geparametriseerde queries, zodat de database beschermd is tegen SQL-injectie.

## 6.6 API (backend)

**Doel:** de verbinding tussen de ESP, de database en het dashboard.

**Eisen:** M1 t/m M5, M9, S1 t/m S5.

De API is gebouwd met Flask en gebruikt JSON.

| **Methode** | **Route**             | **Functie**                                                              |
|-------------|-----------------------|--------------------------------------------------------------------------|
| POST        | /api/metingen         | Nieuwe meting ontvangen, valideren en opslaan                            |
| GET         | /api/metingen         | Metingen ophalen, met optionele filters (van, tot, min, max, categorie)  |
| GET         | /api/metingen/laatste | Meest recente meting ophalen, inclusief categorie                        |
| GET         | /api/metingen/zoek    | Meting zoeken op datum of tijdstip                                       |
| PUT         | /api/metingen/\<id\>  | Meting aanpassen (met validatie)                                         |
| DELETE      | /api/metingen/\<id\>  | Meting verwijderen                                                       |
| GET         | /api/samenvatting     | Laagste, hoogste, gemiddelde snelheid en aantal metingen van de selectie |
| GET         | /api/waarschuwingen   | Overzicht van metingen in de categorie 'hoog'                            |
| GET         | /api/grenswaarden     | Actuele categoriegrenzen voor weergave in het dashboard                  |

Statuscodes: 201 bij een opgeslagen meting, 200 bij een geslaagde opvraging of wijziging, 400 met een lijst van fouten bij een ongeldige meting en 404 als een meting niet bestaat. Bij het verwijderen vraagt het dashboard eerst om bevestiging, zodat een meting niet per ongeluk verdwijnt.

## 6.7 Dashboard (frontend)

**Doel:** de gegevens overzichtelijk en rustig presenteren, zodat de gebruiker zonder uitleg begrijpt wat er gebeurt.

**Eisen:** M1, M3, M4, M5, M6, M7, M8, M10, S1 t/m S5.

Het dashboard is één webpagina met de volgende onderdelen, van boven naar beneden in volgorde van belangrijkheid:

| **Onderdeel**           | **Beschrijving**                                                                                                                                       |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| Actuele snelheid        | Grote weergave van de laatste snelheid met tijdstip en gekleurde categorie. De pagina ververst deze waarde elke 2 seconden.                            |
| Waarschuwing            | Bij categorie 'hoog' verschijnt een duidelijke rode melding bij de actuele snelheid.                                                                   |
| Samenvatting            | Laagste, hoogste en gemiddelde snelheid en aantal metingen van de geselecteerde periode.                                                               |
| Grafiek                 | Lijngrafiek van het snelheidsverloop (Chart.js) met de grenzen van de categorieën als referentielijnen.                                                |
| Filters en periode      | Keuze van datum of periode, minimale en maximale snelheid en categorie. De filters werken op de samenvatting, de grafiek en de tabel.                  |
| Zoeken                  | Zoekveld om een meting te vinden op datum of tijdstip.                                                                                                 |
| Metingentabel           | Tabel met tijdstip, snelheid en categorie per meting, met knoppen om te bewerken en te verwijderen. Aangepaste metingen krijgen een label 'aangepast'. |
| Waarschuwingenoverzicht | Aparte weergave van alle metingen met een hoge snelheid.                                                                                               |
| Foutmeldingen           | Bij een ongeldige invoer, bijvoorbeeld bij het aanpassen van een meting, toont het dashboard de uitleg van de API in gewone taal bij het invoerveld.   |
| Grenswaarden            | Korte uitleg onder het dashboard van welke snelheden bij welke categorie horen.                                                                        |

Ontwerpuitgangspunten: een rustige lay-out met veel witruimte, maximaal drie kleuren voor de categorieën (blauw, groen, rood), duidelijke labels in het Nederlands en een weergave die ook op een kleiner scherm bruikbaar blijft.

# Conclusie

Met alleen een ESP8266 en een computer is het mogelijk om het snelheidsdashboard uit Casus 1 als prototype te realiseren. De ESP8266 verstuurt via WiFi regelmatig snelheidswaarden naar een lokale Flask-server. Omdat er geen snelheidssensor beschikbaar is, worden deze waarden gesimuleerd. Dit is later te vervangen door een echte sensor zonder dat de rest van het systeem hoeft te veranderen.

De validatiemodule zorgt ervoor dat alleen geldige metingen in de SQLite-database terechtkomen en dat de gebruiker te zien krijgt wat er niet klopt. De categoriemodule deelt elke meting in als laag, normaal of hoog en markeert hoge snelheden met een waarschuwing. Het dashboard toont de actuele snelheid, het verloop in een grafiek, de historie, filters en een samenvatting, en laat de gebruiker metingen aanpassen en verwijderen.

Hiermee zijn alle Must haves (M1 t/m M10) en Should haves (S1 t/m S5) gedekt. De Could haves en de Won't haves vallen buiten dit ontwerp. De gekozen technieken zijn eenvoudig en gratis beschikbaar, waardoor het ontwerp haalbaar is voor de projectgroep.
