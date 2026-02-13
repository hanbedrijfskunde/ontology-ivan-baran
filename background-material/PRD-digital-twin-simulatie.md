# PRD: Interactieve Digital Twin / Ontologie Simulatie

## 1. Achtergrond & Aanleiding

De bestaande tutorialpagina (`index.html`) legt ontologie-concepten uit via tekst, diagrammen en een quiz. Studenten lezen *over* een ontologiesysteem, maar ervaren niet *hoe* het werkt. Er ontbreekt een interactief element dat het abstracte concept tastbaar maakt.

## 2. Doel

Een interactieve simulatie toevoegen (Sectie 8) waarin de student met één klik een werkend digital twin / ontologiesysteem ziet opereren — van ruwe sensordata tot AI-aanbeveling. Dit demonstreert het kernprincipe: een ontologie geeft ruwe data betekenis, waardoor AI intelligente beslissingen kan nemen.

## 3. Doelgroep

HBO Bedrijfskunde-studenten die werken aan het Connected Products-project. Geen technische voorkennis vereist — de simulatie moet visueel en intuïtief zijn.

## 4. Domein & Scenario

**Sector:** Fabrikant van verpakkingsmachines voor de FMCG-sector (Fast-Moving Consumer Goods).

**Scenario:** Een "FlowPack 3000" verpakkingsmachine staat bij Unilever Rotterdam. De temperatuursensor op het sealmechanisme detecteert een afwijking. Het ontologiesysteem verrijkt de data, AI analyseert het risico, adviseert preventief onderhoud, en handelt vervolgens autonoom: het controleert het magazijn op benodigde onderdelen, regelt logistiek (inclusief directe levering vanuit leverancier voor ontbrekende onderdelen), plant de optimale onderhoudsmonteur in, en brengt de klant op de hoogte. Een volledig agentic AI-scenario.

### Domein-entiteiten

| Entiteit | Voorbeeld | Kleur (bestaand thema) |
|----------|-----------|----------------------|
| VerpakkingsMachine | FlowPack 3000 (FP3K-0042) | Donkerblauw (primary fill) |
| Klant | Unilever Rotterdam | Lichtblauw (primary border) |
| Onderdeel | Sealmechanisme | Lichtblauw (primary border) |
| Sensor | Temperatuursensor SENS-TMP-0847 | Lichtblauw (primary border) |
| Storing | Sealfout | Rood (danger) |
| Onderhoudsbeurt | Preventief onderhoud | Groen (success) |
| Magazijn | Centraal Magazijn Eindhoven | Lichtblauw (primary border) |
| Leverancier | SealTech GmbH (Düsseldorf) | Lichtblauw (primary border) |
| Monteur | Jan de Vries, Fatima El Amrani, Pieter Bakker | Lichtblauw (primary border) |

### Relaties

- Machine **staat bij** Klant
- Machine **bevat** Onderdeel
- Machine **meet via** Sensor
- Onderdeel **kan hebben** Storing
- Onderdeel **vraagt om** Onderhoudsbeurt
- Sensor **detecteert** Storing (gestippelde lijn)
- Magazijn **heeft op voorraad** Onderdeel
- Leverancier **levert** Onderdeel
- Monteur **voert uit** Onderhoudsbeurt
- Monteur **is gecertificeerd voor** Machine

### Sensordata

| Sensor-ID | Type | Normaal bereik | Eenheid | Gedrag in simulatie |
|-----------|------|---------------|---------|-------------------|
| SENS-TMP-0847 | Temperatuur | 170–185 | °C | Stijgt geleidelijk naar 192 (anomalie) |
| SENS-SPD-1203 | Snelheid | 44–52 | m/min | Fluctueert normaal |
| SENS-PRS-0551 | Druk | 2.8–3.5 | bar | Fluctueert normaal |

## 5. Functionele Eisen

### 5.1 Simulatieflow (7 stappen)

De simulatie doorloopt automatisch 7 stappen na het klikken op "Start Simulatie". Stappen 1–4 vormen de **detectie- en analysefase**, stappen 5–7 de **agentic AI-actiefase** waarin het systeem autonoom handelt:

#### Stap 1: Ruwe Sensordata (~8 seconden)
- Drie sensorcards tonen live-geanimeerde waarden (update elke 600ms)
- Waarden worden getoond als kale getallen met sensor-ID's — geen eenheden of context
- Temperatuur stijgt lineair van 175.2°C naar 192°C
- Snelheid en druk fluctueren willekeurig binnen normaal bereik
- Kleurcodering: temperatuur wordt oranje boven 185°C, rood boven 190°C
- Caption: "Ruwe getallen... maar wat betekenen ze?"

#### Stap 2: Ontologie Verrijkt (~4 seconden)
- SVG-ontologiegrafiek: nodes lichten op in volgorde (Sensor → Onderdeel → Machine → Klant) met puls-animatie (600ms interval)
- Vergelijkingsweergave (voor/na):
  - **Ruwe data** (rood kader): `{ "sensor": "SENS-TMP-0847", "waarde": 191.8 }`
  - **Verrijkte data** (groen kader): volledig JSON-object met ontologie-context, gevuld met typewriter-effect
- Het verrijkte JSON-object bevat: type, eenheid, normaal bereik, onderdeel, onderdeel-leeftijd, machine, klant, eerdere storingen, laatste onderhoud

**Verrijkt JSON-object:**
```json
{
  "sensor": "SENS-TMP-0847",
  "waarde": 191.8,
  "type": "temperatuur",
  "eenheid": "°C",
  "normaal_bereik": [170, 185],
  "onderdeel": "sealmechanisme",
  "onderdeel_leeftijd": "11 maanden",
  "machine": "FlowPack 3000",
  "klant": "Unilever Rotterdam",
  "eerdere_storingen": ["sealfout"],
  "laatste_onderhoud": "2025-09-14"
}
```

#### Stap 3: AI Analyseert (~3,5 seconden)
- Risicobalk animeert van 0% naar 82%
- Kleur gaat van groen → oranje (>30%) → rood (>60%)
- Risicopercentage wordt groot weergegeven
- Factorenlijst verschijnt:
  - Temperatuur 191.8°C overschrijdt normaal bereik (170–185°C)
  - Sealmechanisme is 11 maanden oud (verwachte levensduur: 14 mnd)
  - Eerdere sealfout geregistreerd op deze machine
  - Vergelijkbare patronen bij 3 andere FlowPack-machines leidden tot storing

#### Stap 4: Aanbeveling (~2 seconden)
- Groene aanbevelingscard verschijnt met:
  - **Machine:** FlowPack 3000 (FP3K-0042)
  - **Locatie:** Unilever Rotterdam, Lijn 7
  - **Actie:** Sealmechanisme vervangen
  - **Urgentie:** Binnen 10 werkdagen
  - **Geschatte downtime:** 2 uur (gepland)
  - **Besparing:** ca. €8.500 en 14 uur productieverlies voorkomen t.o.v. ongeplande stilstand
- Overgang naar de agentic AI-fase (stappen 5–7)

---

#### Stap 5: Magazijncontrole & Logistiek (~5 seconden)

De AI-agent controleert automatisch het magazijn en regelt de logistiek voor alle benodigde onderdelen.

**Visueel: onderdelenlijst met live statusupdates**

De volgende onderdelen zijn nodig voor het vervangen van het sealmechanisme:

| Onderdeel | Artikelnr. | Magazijn Eindhoven | Actie | Status-animatie |
|-----------|-----------|-------------------|-------|----------------|
| Sealunit compleet | SU-3000-A | 2 op voorraad | Reserveren & verzenden naar Rotterdam | Vinkje (groen) verschijnt met "Verzonden vanuit Eindhoven — ETA morgen 08:00" |
| Teflonband (3m) | TB-200-TF | 14 op voorraad | Reserveren & verzenden naar Rotterdam | Vinkje (groen) verschijnt met "Verzonden vanuit Eindhoven — ETA morgen 08:00" |
| Thermokoppel sensor | TK-NTC-80 | 0 op voorraad | Bestellen bij leverancier | Oranje indicator verschijnt, gevolgd door: "Besteld bij SealTech GmbH Düsseldorf — directe levering Rotterdam — ETA overmorgen 11:00" |
| O-ring set (hittebestendig) | OR-SIL-42 | 8 op voorraad | Reserveren & verzenden naar Rotterdam | Vinkje (groen) verschijnt met "Verzonden vanuit Eindhoven — ETA morgen 08:00" |

**Animatieflow:**
1. Header verschijnt: "Magazijn Eindhoven wordt gecontroleerd..."
2. Onderdelen verschijnen één voor één (400ms interval) met een "scanning"-effect
3. Per onderdeel verschijnt de voorraadstatus
4. Beschikbare onderdelen krijgen een groen vinkje + verzendbevestiging
5. Het ontbrekende onderdeel (thermokoppel) krijgt eerst een rode "Niet op voorraad" melding
6. Na 1 seconde verandert dit in een oranje "Besteld bij leverancier" met directe leveringsbevestiging
7. Onderaan verschijnt een samenvattingsregel: "Alle onderdelen geregeld — beschikbaar op locatie: [datum]"

**Ontologie-verbindingen die hier zichtbaar worden:**
- Onderdeel → Magazijn (voorraadrelatie)
- Onderdeel → Leverancier (inkooprelatie)
- Leverancier → locatie → logistieke route

#### Stap 6: Monteurplanning (~5 seconden)

De AI-agent doorzoekt de planning van alle gecertificeerde onderhoudsmonteurs en selecteert de optimale match.

**Visueel: monteurselectie met planningsoverzicht**

Drie monteurs worden geëvalueerd (verschijnen als compacte profielcards):

| Monteur | Certificering | Huidige locatie | Planning komende dagen | AI-beoordeling |
|---------|--------------|----------------|----------------------|----------------|
| Jan de Vries | FlowPack 3000 ✓ | Amsterdam | Ma: Nestlé Breda, Di: vrij, Wo: FrieslandCampina Utrecht | **Beste match** — Di vrij, route Amsterdam → Rotterdam 1u15 |
| Fatima El Amrani | FlowPack 3000 ✓ | Eindhoven | Ma: Heineken Zoeterwoude, Di: Danone Utrecht, Wo: vrij | Wo mogelijk, maar onderdelen al Di beschikbaar |
| Pieter Bakker | CartonneerMachine X2 ✓ | Rotterdam | Ma–Wo: beschikbaar | Niet gecertificeerd voor FlowPack 3000 |

**Animatieflow:**
1. Header verschijnt: "Beschikbare monteurs worden geanalyseerd..."
2. Drie monteurskaarten verschijnen met basisinfo (naam, certificering, locatie)
3. Per monteur verschijnt de weekplanning als compacte tijdlijn
4. AI-beoordelingsindicatoren verschijnen:
   - Pieter Bakker wordt grijs/doorgestreept: "Niet gecertificeerd voor FlowPack 3000"
   - Fatima El Amrani krijgt een oranje "Mogelijk" indicator: "Wo beschikbaar maar vertraagd"
   - Jan de Vries krijgt een groene highlight + "Beste match" badge
5. Selectiebevestiging verschijnt: "Jan de Vries ingepland — dinsdag 09:00, Unilever Rotterdam Lijn 7"

**Ontologie-verbindingen die hier zichtbaar worden:**
- Monteur → certificering → Machine-type
- Monteur → planning → beschikbaarheid
- Monteur → locatie → reistijd naar klant
- Onderhoudscontract → klant → planning-prioriteit

#### Stap 7: Klantnotificatie (eindstatus)

De klant wordt automatisch op de hoogte gebracht met alle details.

**Visueel: notificatiekaart (blauw/primary thema)**

Een "verzonden bericht"-card verschijnt, gestyled als een e-mail/notificatie:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📧  Automatische servicemelding verzonden
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Aan:        operations@unilever-rotterdam.nl
Betreft:    Gepland preventief onderhoud — FlowPack 3000 (Lijn 7)

Geachte heer/mevrouw,

Op basis van onze continue monitoring hebben wij een
verhoogd slijtagepatroon gedetecteerd in het sealmechanisme
van uw FlowPack 3000 op Lijn 7.

📅  Datum:       Dinsdag 18 februari 2025, 09:00 uur
🔧  Monteur:     Jan de Vries
⏱️  Geschatte duur: 2 uur
📦  Onderdelen:  Worden vooraf geleverd

Geen actie vereist van uw kant. Neem contact op met uw
servicemanager bij vragen.

Met vriendelijke groet,
PackTech Solutions — Afdeling Predictive Service
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Animatieflow:**
1. Een "verzenden..."-animatie (envelop-icoon met pulseffect)
2. Na 1,5 seconde verschijnt de volledige notificatiekaart met een slide-in-effect
3. Onderaan verschijnt een samenvattende tijdlijn van alles wat het systeem autonoom heeft gedaan

**Afsluitende samenvattingsregel:**

Na de notificatiekaart verschijnt een compacte tijdlijn die alle 7 stappen samenvat:

```
Sensordata → Ontologie → Analyse → Aanbeveling → Onderdelen → Monteur → Klant geïnformeerd
   [0s]       [9s]       [13s]     [17s]         [22s]        [27s]      [30s]
```

- Reset-knop verschijnt

### 5.2 Voortgangsindicator

Horizontale stappenbalk (7 dots met verbindingslijnen), visueel opgedeeld in twee fasen:
- **Dots 1–4:** Detectie & Analyse (label erboven)
- **Dots 5–7:** Agentic AI Actie (label erboven)
- Inactieve stap: grijs
- Actieve stap: blauw (primary)
- Voltooide stap: groen (success)
- Verbindingslijnen worden actief naarmate stappen vorderen
- Tussen dot 4 en 5 zit een visuele scheiding (dubbele lijn of gap) om de fase-overgang te markeren

### 5.3 SVG Ontologiediagram

Nieuw SVG-diagram specifiek voor het verpakkingsmachinedomein, in dezelfde visuele stijl als het bestaande diagram in Sectie 2:
- **9 entity-nodes** met gelabelde relaties:
  - Bovenste rij: VerpakkingsMachine (centraal, primary fill)
  - Tweede rij: Klant, Onderdeel, Sensor
  - Derde rij: Storing, Onderhoudsbeurt
  - Vierde rij / zijkant: Magazijn, Leverancier, Monteur
- Nodes krijgen unieke `id`-attributen voor JavaScript-highlighting
- Highlight-animatie: puls-glow effect via CSS `@keyframes`
- Tijdens stap 5-6 lichten de nieuwe nodes (Magazijn, Leverancier, Monteur) op om te tonen dat de ontologie ook deze entiteiten verbindt

### 5.4 Reset-functionaliteit

- Reset-knop zet alles terug naar beginstatus
- Alle timers worden opgeruimd
- Sensor-waarden keren terug naar startwaarden
- Grafiek-highlights worden verwijderd
- Simulatie kan opnieuw gestart worden

### 5.5 Takeaway

Na de simulatie verschijnt een takeaway-box die het volledige plaatje samenvat: de ontologie verbindt niet alleen sensoren met machines en klanten, maar ook met magazijnen, leveranciers en monteurs. Hierdoor kan een AI-agent het hele proces autonoom afhandelen — van detectie tot actie — zonder menselijke tussenkomst. Dát is de kracht van een embedded ontologie in een agentic AI-systeem.

## 6. Niet-functionele Eisen

### 6.1 Technische beperkingen
- Alle code inline in het bestaande `index.html`-bestand (CSS in `<style>`, JS in `<script>`)
- Geen externe frameworks of libraries — alleen vanilla JavaScript
- Animaties via CSS transitions en `requestAnimationFrame`
- Sensordata-updates via `setInterval` (600ms)

### 6.2 Stijl & Consistentie
- Alle nieuwe CSS-klassen beginnen met `.sim-` prefix
- Hergebruik van bestaande CSS custom properties (`--primary`, `--success`, `--danger`, `--warning`, etc.)
- Bestaande component-patronen volgen (`.card`, `.takeaway`, `.analogy`, `.ontology-diagram`)
- Alle tekst in het Nederlands

### 6.3 Responsive Design
- Sensorcard-grid stacked naar enkelkoloms bij viewport < 560px
- Verrijkings-vergelijking stacked naar enkelkoloms bij viewport < 560px

### 6.4 Performance
- Totale simulatietijd: ~30 seconden
- Maximaal 1 actieve `setInterval` tegelijkertijd
- Alle timers worden netjes opgeruimd bij reset

## 7. Plaatsing in de pagina

De simulatie wordt ingevoegd als **Sectie 8**, tussen de huidige Sectie 7 ("Wat betekent dit voor jullie project?") en de Quiz-sectie. Rationale:
- Secties 1–7 bouwen conceptueel begrip op (theorie)
- Sectie 8 laat de student de theorie *ervaren* (praktijk)
- De quiz toetst daarna begrip van zowel theorie als de observaties uit de simulatie

## 8. Ontwerpbeslissingen

| Beslissing | Rationale |
|-----------|-----------|
| Fasen stapelen (eerdere fasen blijven zichtbaar) | Student ziet de volledige pipeline aan het einde — van ruwe data bovenaan tot klantnotificatie onderaan. Dit versterkt het end-to-end "agentic AI"-concept. |
| Alleen temperatuur als anomalie, andere sensoren normaal | Maakt het educatieve punt duidelijker: AI kijkt niet naar alle getallen, maar gebruikt de ontologie om te begrijpen *welk* getal ertoe doet en *waarom*. |
| Typewriter-effect voor verrijkt JSON | Creëert het gevoel dat het systeem context "opbouwt" rondom het ruwe datapunt — precies wat een embedded ontologie doet. |
| Eén ontbrekend onderdeel in het magazijn | Toont dat de ontologie niet alleen interne systemen verbindt, maar ook externe leveranciers. Maakt het scenario realistischer dan "alles is op voorraad". |
| Drie monteurs met verschillende geschiktheid | Demonstreert hoe de ontologie certificeringen, planning en locatie combineert voor optimale beslissingen. De niet-gecertificeerde monteur die wél in Rotterdam zit toont dat nabijheid alleen niet genoeg is. |
| 7 stappen in twee fasen (detectie + actie) | Het visuele onderscheid tussen "begrijpen" (stap 1–4) en "handelen" (stap 5–7) maakt het agentic-karakter expliciet. |
| `innerHTML` voor dynamische content | Veilig in deze context (geen gebruikersinput wordt gerenderd). Houdt de code simpel. |

## 9. Acceptatiecriteria

**Detectie & Analyse (stap 1–4):**
1. [ ] Sectie 8 verschijnt correct tussen Sectie 7 en de Quiz
2. [ ] SVG-ontologiediagram toont 9 nodes met gelabelde relaties
3. [ ] "Start Simulatie"-knop start de animatie
4. [ ] Stap 1: Sensorwaarden animeren, temperatuur wordt oranje/rood
5. [ ] Stap 2: Grafieknodes highlighten in volgorde, verrijkt JSON typt in
6. [ ] Stap 3: Risicobalk animeert naar 82%, factoren verschijnen
7. [ ] Stap 4: Aanbevelingscard verschijnt met details en besparing

**Agentic AI Actie (stap 5–7):**
8. [ ] Stap 5: Onderdelenlijst verschijnt met voorraadstatus per onderdeel
9. [ ] Stap 5: Beschikbare onderdelen tonen groene verzendbevestiging (3 van 4)
10. [ ] Stap 5: Ontbrekend onderdeel toont eerst rode melding, dan oranje leveranciersbestelling
11. [ ] Stap 5: Samenvattingsregel met beschikbaarheidsdatum verschijnt
12. [ ] Stap 6: Drie monteurkaarten verschijnen met certificering en planning
13. [ ] Stap 6: Niet-gecertificeerde monteur wordt grijs/doorgestreept
14. [ ] Stap 6: Beste match krijgt groene highlight en wordt ingepland
15. [ ] Stap 7: Klantnotificatie verschijnt als e-mail-achtige kaart
16. [ ] Stap 7: Samenvattende tijdlijn van alle 7 stappen verschijnt
17. [ ] Ontologiediagram highlight relevante nodes per stap (incl. Magazijn, Leverancier, Monteur in stap 5-6)

**Algemeen:**
18. [ ] Voortgangsindicator (7 dots in 2 fasen) werkt correct door alle stappen
19. [ ] Reset-knop zet alles terug naar beginstatus (inclusief stap 5-7 content)
20. [ ] Simulatie kan meerdere keren opnieuw gedraaid worden
21. [ ] Responsive layout werkt bij viewport < 560px
22. [ ] Alle tekst is in het Nederlands
23. [ ] Geen console-errors in de browser
