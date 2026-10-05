Lingvo — News Triplets

Lingvo represents news and web information using triplets.

Core representation

The fundamental structure is:

SUBJECT → RELATION → OBJECT


Example:

Water → has_formula → H2O
Earth → has_mass → 5.972e24_kg


The Subject, Relation, and Object are the essential semantic content of a triplet.

Additional information such as time, numbers, location, source, confidence, and status is attached after the triplet on the same line.

Example:

USA → announced → Alaska_LNG_project | time: 2026-10-05 | amount: 200_billion_USD | source: Reuters


The first three elements remain the triplet:

USA → announced → Alaska_LNG_project


Everything after | is metadata or an attribute of the statement/event.

News triplet format

The preferred format is:

SUBJECT → RELATION → OBJECT | attribute: value | attribute: value | ...


For example:

China → suspended → oil_product_exports | time: 2026-10-05 | source: Reuters

France → proposed → EU_trade_tool | time: 2026-10-05 | target: unfair_trade_practices | source: Reuters

Poland → extended → border_controls | time: 2026-10-01 | until: 2027-03-30 | source: Reuters

England → defeated → Croatia | time: 2026-10-03 | score: 7-0 | competition: UEFA_Nations_League


The triplet itself should remain simple.

Do not put information such as dates, prices, percentages, locations, or source names into the Subject, Relation, or Object unless that information is genuinely part of the entity.

Attributes

Common attributes include:

time:
date:
timestamp:
location:
number:
amount:
percentage:
duration:
until:
from:
cause:
reason:
context:
source:
url:
publication_date:
extraction_date:
confidence:
status:
evidence:


Not every triplet needs every attribute.

Example:

USA → imposed → tariffs | time: 2026-10-01 | rate: 25_percent | target: China | source: Reuters

China → responded_to → US_tariffs | time: 2026-10-02 | action: retaliation | source: Reuters

Keep the triplet atomic

One line should preferably represent one relationship.

Good:

USA → imposed → tariffs | rate: 25_percent | target: China

China → responded_to → US_tariffs | action: retaliation


Avoid combining several relationships into one triplet:

USA → imposed_tariffs_on_China_and_caused_trade_pressure → China


Instead, split it:

USA → imposed → tariffs | target: China
Tariffs → increased → trade_pressure


Atomic triplets are easier to search, validate, deduplicate, and use for inference.

Time-dependent information

Time should normally be an attribute rather than part of the core triplet.

Example:

Bitcoin → had_price → 100000_USD | time: 2026-10-05T14:30


Later:

Bitcoin → had_price → 95000_USD | time: 2026-10-06T14:30


The system should not overwrite the previous observation.

Both statements represent different observations at different times.

For news:

CompanyA → plans_to_acquire → CompanyB | time: 2026-01-10 | source: Reuters


Later:

CompanyA → acquired → CompanyB | time: 2026-05-20 | source: Reuters


The first triplet should remain in the database.

The second triplet represents a later state.

Claims versus facts

News frequently reports statements, allegations, predictions, and assessments.

Therefore, an extracted news triplet should not automatically be treated as an unquestionable fact.

Example:

Ukraine → accused → Russia | time: 2026-10-05 | status: allegation | source: Reuters

Russia → denied → accusation | time: 2026-10-05 | status: denial | source: Reuters


Both triplets should be preserved.

Other useful status values include:

confirmed
reported
claimed
alleged
estimated
forecast
assessment
disputed
denied


This allows Lingvo to represent conflicting information without arbitrarily choosing one statement.

Provenance

Every news triplet should ideally retain its provenance.

The core representation remains:

SUBJECT → RELATION → OBJECT


Additional provenance can follow it:

subject → relation → object | source: Reuters | url: ... | publication_date: 2026-10-05 | extraction_date: 2026-10-05 | confidence: 0.95


Recommended provenance attributes:

source:
url:
publication_date:
extraction_date:
evidence:
confidence:
status:


The original article or document remains the evidence.

The triplet is an extracted representation of that evidence.

Numbers and measurements

Numbers should normally be attributes or objects, depending on their semantic role.

Example where the number is the object:

US_inflation → reached → 4_percent | time: 2026-09


Example where the number describes an event:

USA → announced → energy_funding | amount: 150_million_USD | time: 2026-10-05


Example with several measurements:

England → received → rainfall | amount: 65_percent | compared_with: long_term_average | time: 2026-10-01


The important distinction is that the semantic relationship remains visible in S → R → O.

Entity naming

Entities should use stable, normalized names where possible.

Prefer:

United_States
Donald_Trump
China
European_Commission
Alaska_LNG_project


rather than:

The_US_government
Trump's_administration
the_Chinese_side
that_EU_project


This makes duplicate detection and graph queries easier.

Names can later be mapped to canonical IDs.

Mermaid compatibility

The news triplet format is not Mermaid syntax.

The triplet format is the canonical Lingvo representation.

A separate parser can convert triplets into Mermaid.

Lingvo data:

USA → announced → Alaska_LNG_project | time: 2026-10-05 | amount: 200_billion_USD
China → responded_to → US_tariffs | time: 2026-10-02 | action: retaliation


Generated Mermaid:

announced
responded_to
USA
Alaska_LNG_project
China
US_tariffs

This separation is intentional.

Triplets are the data. Mermaid is a visualization.

Metadata should normally remain outside the Mermaid graph unless it is specifically needed for visualization.

Parsing

The intended parser can interpret a line as:

SUBJECT → RELATION → OBJECT | ATTRIBUTE: VALUE | ATTRIBUTE: VALUE


For example:

USA → imposed → tariffs | time: 2026-10-01 | rate: 25_percent | target: China | source: Reuters


can be parsed as:

subject = USA
relation = imposed
object = tariffs
attributes:
    time = 2026-10-01
    rate = 25_percent
    target = China
    source = Reuters


The parser should treat the first three semantic fields as the triplet and everything after | as attributes.

Database representation

The textual representation can be stored in a database as separate fields:

subject
relation
object
attributes
source
url
publication_date
extraction_date
confidence
status
evidence


For example:

subject: USA
relation: imposed
object: tariffs
attributes: rate=25_percent; target=China; time=2026-10-01
source: Reuters
confidence: 0.96
status: reported


This allows the same information to be represented both as human-readable text and as structured database records.

Rules and inference

Triplets remain the basic units used by the reasoning engine.

Example:

USA → imposed → tariffs | target: China
China → imports → goods_from_USA


A rule may derive another triplet:

X → imposes → tariffs_on → Y
Y → imports → goods_from → X
--------------------------------
X → affects → trade_with → Y


The exact rule representation can evolve later.

The important principle is:

Input triplets are facts/claims extracted from sources.
Rules operate on triplets.
Derived triplets should be distinguishable from source triplets.

News knowledge graph

The intended news pipeline is:

Internet/news
      ↓
article/document
      ↓
parser / LLM
      ↓
SUBJECT → RELATION → OBJECT
      ↓
attributes / provenance
      ↓
validation
      ↓
database
      ↓
deduplication
      ↓
rules / inference
      ↓
derived triplets
      ↓
graph / search / natural-language queries


Daily news collection should:

extract atomic triplets

preserve the original source

preserve publication and event dates

attach numbers and measurements as attributes

distinguish claims from confirmed information

detect duplicates

preserve updates

preserve contradictions

avoid overwriting historical observations

assign extraction confidence

allow later conversion to Mermaid or another graph format

Conceptual architecture
TEXT
  ↓
PARSER
  ↓
TRIPLETS
  ↓
ATTRIBUTES + PROVENANCE
  ↓
VALIDATION
  ↓
DATABASE / GRAPH
  ↓
RULES / INFERENCE
  ↓
DERIVED TRIPLETS


The long-term goal is:

TRIPLETS = structured information
ATTRIBUTES = context and metadata
PROVENANCE = evidence
RULES = inference
OPERATIONS = computation
PARSER = conversion from external information
DATABASE / GRAPH = persistent knowledge
ENGINE = reasoning + execution


The central design principle is:

SUBJECT → RELATION → OBJECT


Everything else should support, qualify, or provide provenance for that triplet without making the fundamental representation unnecessarily complicated.