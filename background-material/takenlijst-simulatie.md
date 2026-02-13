# Takenlijst: Interactieve Digital Twin Simulatie

Gebaseerd op [PRD-digital-twin-simulatie.md](PRD-digital-twin-simulatie.md)

---

## Structuur & Basis

- [ ] **1. HTML-skelet Sectie 8** — Sectie 8 aanmaken tussen Sectie 7 en Quiz, inclusief titel, "Start Simulatie"-knop en containerstructuur voor alle 7 stappen
- [ ] **2. Voortgangsindicator** — 7 dots in 2 fasen (Detectie & Analyse + Agentic AI Actie) met verbindingslijnen en visuele scheiding tussen stap 4 en 5
- [ ] **3. SVG Ontologiediagram** — 9 entity-nodes (VerpakkingsMachine, Klant, Onderdeel, Sensor, Storing, Onderhoudsbeurt, Magazijn, Leverancier, Monteur) met gelabelde relaties en unieke id-attributen

## Detectie & Analyse (stap 1–4)

- [ ] **4. Stap 1 – Ruwe Sensordata** — 3 sensorcards met live-geanimeerde waarden (600ms interval), temperatuur stijgt van 175.2 naar 192°C met kleurcodering (oranje >185, rood >190)
- [ ] **5. Stap 2 – Ontologie Verrijkt** — SVG-nodes highlighten in volgorde met puls-animatie, voor/na vergelijkingsweergave, typewriter-effect voor verrijkt JSON-object
- [ ] **6. Stap 3 – AI Analyseert** — Risicobalk animatie van 0% naar 82% met kleurovergang (groen→oranje→rood), risicopercentage en factorenlijst
- [ ] **7. Stap 4 – Aanbeveling** — Groene aanbevelingscard met machine, locatie, actie, urgentie, geschatte downtime en besparing

## Agentic AI Actie (stap 5–7)

- [ ] **8. Stap 5 – Magazijncontrole & Logistiek** — Onderdelenlijst met live statusupdates, scanning-effect, voorraadstatus, groen/oranje/rood indicators, leveranciersbestelling voor ontbrekend onderdeel
- [ ] **9. Stap 6 – Monteurplanning** — 3 monteur-profielcards met certificering, locatie en weekplanning, AI-beoordeling (grijs/oranje/groen), selectiebevestiging
- [ ] **10. Stap 7 – Klantnotificatie** — E-mail-achtige notificatiekaart met verzendanimatie, samenvattende tijdlijn van alle 7 stappen

## Afronding

- [ ] **11. Reset-functionaliteit** — Knop die alle timers opruimt, waarden terugzet, highlights verwijdert en simulatie opnieuw startbaar maakt
- [ ] **12. Takeaway-box** — Samenvatting na de simulatie over ontologie + agentic AI
- [ ] **13. CSS styling** — Alle nieuwe klassen met `.sim-` prefix, hergebruik bestaande custom properties, puls-glow animatie via `@keyframes`
- [ ] **14. Responsive design** — Sensorcards en vergelijkingsweergave stacked bij viewport < 560px
- [ ] **15. Testen & debuggen** — Simulatie meerdere keren draaien, geen console-errors, alle tekst Nederlands, timing ~30s totaal
