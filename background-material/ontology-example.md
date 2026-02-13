# Ontology: Praktische Technische Voorbeelden

## Drie laag-technische voorbeelden:

### 1. Ontologie als Data Model (JSON Schema)

Dit is hoe je de structuur definiëert die uit de tutorial komt:
```json
{
  "entities": {
    "Machine": {
      "properties": {
        "id": "string",
        "modelNaam": "string",
        "aanschafDatum": "date",
        "status": ["actief", "onderhoud", "defect"]
      },
      "relations": {
        "staatBij": "Klant",
        "bevat": ["Onderdeel"]
      }
    },
    "Sensor": {
      "properties": {
        "id": "string",
        "type": ["trilling", "temperatuur", "druk"],
        "lastMeasurement": "float"
      },
      "relations": {
        "zitOp": "Onderdeel"
      }
    },
    "Storing": {
      "properties": {
        "id": "string",
        "severity": ["laag", "gemiddeld", "kritiek"],
        "detecteerddatum": "datetime"
      },
      "relations": {
        "gebeurtOp": "Onderdeel"
      }
    }
  }
}
```

### 2. Ontologie in een Knowledge Graph (RDF/Semantic Web)

Dit is wat Palantir intern gebruikt — linked data waar je directe verbanden query't:
```turtle
# Machine #42 is van klant Janssen
:Machine42 rdf:type :Machine ;
          :eigenaarVan :KlantJanssen ;
          :bevat :LagerblokA ;
          :status "actief" .

# Lagerblok zit in machine
:LagerblokA rdf:type :Onderdeel ;
           :zitIn :Machine42 ;
           :leeftijd "14 maanden" ;
           :heeftSensor :TrillingSensor_001 .

# Sensor meet trillingen
:TrillingSensor_001 rdf:type :Sensor ;
                   :meetType "trilling" ;
                   :laatste_waarde 4.2 ;
                   :eenheid "mm/s" .
```

### 3. Praktische Implementatie: Python + Graph Database

Hier zie je hoe AI daadwerkelijk de ontologie gebruikt:
```python
# Pseudo-code met Neo4j (populaire graph database)
from neo4j import GraphDatabase

driver = GraphDatabase.driver("bolt://localhost:7687")

def get_machine_context(machine_id):
    """Ontologie in actie: geef een machine alle context"""
    query = """
    MATCH (m:Machine {id: $machine_id})
    OPTIONAL MATCH (m)-[:staat_bij]->(klant:Klant)
    OPTIONAL MATCH (m)-[:bevat]->(onderdeel:Onderdeel)
    OPTIONAL MATCH (onderdeel)-[:zit_op]->(sensor:Sensor)
    OPTIONAL MATCH (onderdeel)-[:kan_hebben]->(storing:Storing)
    RETURN m, klant, onderdeel, sensor, storing
    """
    
    with driver.session() as session:
        result = session.run(query, machine_id=machine_id)
        return result.data()

# Voorbeeld: Machine #42 bij Janssen
context = get_machine_context("Machine_42")
# Resultaat bevat ALLES: welke klant, welke onderdelen, 
# welke sensoren, welke storingen eerder gehad

# Nu kan je AI-model dit gebruiken:
def predict_maintenance(context):
    """AI gebruikt ontologie om voorspelling te doen"""
    onderdeel = context['onderdeel']
    sensoren = context['sensor']
    klant = context['klant']
    
    # AI kent nu:
    # - Wanneer dit onderdeel geplaatst is
    # - Wat normale trillingen zijn voor dit type
    # - Wat deze klant gebruikt (draaiuren, temperatuur)
    # - Welke storingen op dit onderdeel eerder voorkwamen
    
    risk_score = ai_model.predict(
        sensor_trend=sensoren['waarde'],
        part_age=onderdeel['leeftijd'],
        customer_usage_pattern=klant['gemiddeld_gebruik']
    )
    
    return {
        "risk": risk_score,
        "recommendation": "preventief onderhoud volgende week dinsdag"
    }
```

### 4. Embedded Ontologie in Actie

Dit is hoe het automatic "ingebouwd" werkt — elke keer als data binnenkomt:
```python
def ingest_sensor_data(raw_data):
    """Raw sensordata wordt automatisch aan ontologie gekoppeld"""
    
    # Ruwe data van sensor: {"sensor_123": 3.8, "timestamp": ...}
    # → Wij weten via ontologie: sensor_123 zit op lagerblok
    # → lagerblok zit in machine_42 van klant Janssen
    
    sensor_id = raw_data['sensor_id']
    
    # Ontologie lookup: wat hoort bij deze sensor?
    ontology_lookup = {
        'sensor_123': {
            'type': 'trilling',
            'eenheid': 'mm/s',
            'zitOp': 'lagerblok_A',
            'lagerblok_A': {'zitIn': 'machine_42'},
            'machine_42': {'eigenaarVan': 'klant_janssen'}
        }
    }
    
    # Nu kun je continu context toevoegen
    enriched_data = {
        **raw_data,
        'sensor_type': ontology_lookup['sensor_123']['type'],
        'machine_id': 'machine_42',
        'customer': 'janssen',
        'part_age_months': 14
    }
    
    # Dit verrijkte data gaat naar je AI-systeem
    return enriched_data
```

## Waarom is dit zo krachtig?

### Zonder ontologie:
```python
# AI ziet alleen:
data = [3.8, 4.2, 4.1, 3.9]  # cijfers zonder betekenis
```

### Met ontologie:
```python
# AI ziet:
{
    "trillingen_machine_42_lagerblok": [3.8, 4.2, 4.1, 3.9],
    "machine_eigenaar": "Janssen B.V.",
    "onderdeel_leeftijd_maanden": 14,
    "vorige_storingen_dit_onderdeel": ["lagerslijtage", "temperatuurstijging"],
    "normale_range": [2.0, 3.5]
}
```

De ontologie is dus het "brein" dat rauwdata van betekenis voorziet — en daarom kunnen AI-systemen echt intelligente voorspellingen doen.