# Lingvo Triplet v4
1. Purpose
Lingvo Triplet v4 is a universal representation for converting language and knowledge into atomic semantic statements.
The same format can represent: (facts, events, properties, relationships, scientific knowledge, news, history, art, economics, people, places, objects, concepts, measurements, claims.
The core principle is: SUBJECT RELATION OBJECT
Everything beyond the three semantic elements is optional qualification.
2. Fundamental Rule
ONE TRIPLET = ONE LINE
A triplet MUST NOT span multiple lines.
Canonical form: SUBJECT RELATION OBJECT | key:value | key:value | key:value
Example: Gold has density:19.3_g_cm3 | type:physical_property | domain:materials_science | condition:room_temperature
3. Semantic Core
Every triplet contains exactly three semantic elements: SUBJECT RELATION OBJECT
Formally:𝑇𝑐𝑜𝑟𝑒=(𝑆,𝑅,𝑂)
Where: S = Subject, R = Relation, O = Object
Example: Gold has atomic_number:79
S = Gold
R = has 
O = atomic_number:79
The semantic core MUST remain identifiable independently of its metadata.
5. Metadata
Optional metadata follows the semantic core using |.
SUBJECT RELATION OBJECT | key:value | key:value
Recommended metadata fields: type: domain: context: time: location: condition: unit: value: confidence: status: source: author: reference: note:
Not every field is required.
Example: Gold has density:19.3_g_cm3 | type:physical_property | domain:materials_science | condition:room_temperature
7. Structured Core + Flexible Extension
Lingvo v4 deliberately combines structured and flexible representation.
STRUCTURED SEMANTIC CORE + STRUCTURED METADATA + OPTIONAL FREE-FORM NOTE
The first three elements are machine-readable.
Metadata should normally be structured: |time:2026-10-06|domain:politics|confidence:high
Free-form information is permitted through note:: Gold symbolizes Wealth | type:cultural_meaning | domain:art | note:Associated_with_divinity_and_royal_status_in_ancient_Egypt
Free-form text MUST NOT replace the semantic core when the information can be represented as a triplet.
8. Atomicity
A triplet should express one primary assertion.
Bad: Gold is dense and valuable and used in jewelry and electronics
Good:
Gold has density:19.3_g_cm3 | type:physical_property
Gold has cultural_value:Wealth | type:cultural_property
Gold is_used in:Jewelry | type:application
Gold is_used in:Electronics | type:application
One line = one assertion.
9. Relation Vocabulary
Relations should preferably use normalized names.
Examples:
has
is
belongs
causes
produces
results
depends
affects
influences
creates
created
develops
developed
produces
uses
used
used
applied
contains
located
occurs
depicts
represents
symbolizes
references
interprets
precedes
follows
occurs
supports
contradicts
claims
denies
The vocabulary is extensible.
Unknown or domain-specific relations are permitted when necessary.
10. Types
The type: field describes the nature of the assertion.
Recommended types include:
identity
classification
property
physical_property
chemical_property
material_property
measurement
structure
relationship
causal
event
state
process
historical_fact
political_fact
economic_indicator
scientific_fact
application
representation
cultural_meaning
symbolism
claim
definition
observation
prediction
Example:
Gold has atomic_number:79 | type:identity | domain:chemistry
11. Context
Context qualifies the assertion without changing the semantic core.
Gold has density:19.3_g_cm3 | type:physical_property | condition:room_temperature
Gold symbolizes Wealth | type:cultural_meaning | context:ancient_Egypt
German_industrial_orders fell:10.6_percent | type:economic_indicator | period:2026-08 | comparison:month_on_month
12. Time
Time should be represented explicitly whenever relevant.
Examples:
Lydia minted gold_coins | in_~640_BCE | type:historical_fact | domain:numismatics
German_industrial_orders fell 10.6_percent | type:economic_indicator | period:2026-08
Leonardo_da_Vinci created Mona_Lisa | type:artwork | time:1503-1519
Approximate dates may use ~.
13. Measurements
Measurements should preserve the numerical value and unit.
Preferred:
Gold has density:19.3_g_cm3 | type:measurement | condition:room_temperature
Gold has melting_point:1064.18_C | type:measurement | pressure:1_atm
The unit should be part of the object when it is intrinsic to the measurement.
14. Provenance
Knowledge can optionally carry provenance.
German_industrial_orders fell 10.6_percent | type:economic_indicator | period:2026-08 | source:Reuters
Recommended provenance fields:
source:
author:
reference:
retrieved:
Provenance describes where the assertion came from.
It does not become a fourth semantic element.
15. Confidence and Status
Assertions may have confidence and status.
Gold occurs in_Quartz_Veins | type:geological_occurrence | confidence:high
Company_X may_acquire Company_Y | type:claim | status:unconfirmed
Recommended statuses:
confirmed
probable
possible
unconfirmed
disputed
historical
estimated
predicted
16. Negation
Negation should remain explicit.
Preferred:
Gold does_not_contain Iron | type:claim
or, where appropriate:
Gold is_not Magnetic | type:physical_property
The relation should express the semantic meaning clearly.
17. Questions and Uncertainty
Questions can be represented when needed.
Company_X will_acquire Company_Y | type:claim | status:unknown
A question should not be silently converted into a fact.
18. Events
Events use the same three-element structure.
Leonardo_da_Vinci created Mona_Lisa | type:event | domain:art | time:1503-1519
Germany cancelled F126_frigate_program | type:event | domain:defence | time:2026
Events can then be connected to participants, locations, dates, causes, and consequences through additional triplets.
19. Art Example
Leonardo_da_Vinci created Mona_Lisa | type:artwork | domain:art | time:1503-1519
Mona_Lisa depicts Lisa_Gherardini | type:representation | domain:art
Mona_Lisa uses Sfumato | type:technique | domain:painting
Leonardo_da_Vinci developed Sfumato | type:technique | domain:painting
Mona_Lisa belongs to_High_Renaissance | type:classification | domain:art
20. Physics Example
Force causes Acceleration | type:law | domain:physics
Acceleration depends on_Force | type:relationship | domain:physics
Acceleration depends on_Mass | type:relationship | domain:physics
Force has unit:Newton | type:measurement | domain:physics
Mass has unit:Kilogram | type:measurement | domain:physics
21. News Example
German_industrial_orders fell 10.6_percent | type:economic_indicator | domain:industry | period:2026-08 | comparison:month_on_month | source:Reuters
Large_scale_transport_orders fell 61.5_percent | type:economic_indicator | domain:industry | period:2026-08 | source:Reuters
Germany seeks stronger_EU_trade_protection in_China | type:trade_policy | domain:economy | context:Chinese_imports | source:Financial_Times
Germany_intelligence_services warn Germany of_Russian_activities | type:security_warning | domain:national_security | date:2026-10-06 | source:tagesschau
22. Triplet Graph
Triplets form a graph.
Leonardo_da_Vinci
        │
      created
        ↓
    Mona_Lisa
      │     │
   depicts  uses
      ↓     ↓
    Lisa   Sfumato
Formally: 𝐺=(𝑉,𝐸)
Where:
𝑉 = semantic entities
𝐸 = triplets
A triplet is an edge:𝑆→𝑅𝑂
24. Sentence Representation
A sentence can contain multiple atomic triplets:
Gold is dense and resists corrosion.
becomes:
Gold has density:19.3_g_cm3 | type:physical_property
Gold resists corrosion | type:chemical_property
Therefore:𝑆𝑒𝑛𝑡𝑒𝑛𝑐𝑒={𝑇1,𝑇2,…,𝑇𝑛}
Triplets preserve the meaning while the original sentence provides linguistic context.
25. Paragraph Representation
A paragraph is a collection of sentences and their triplets:𝑃𝑎𝑟𝑎𝑔𝑟𝑎𝑝ℎ={𝑆1,𝑆2,…,𝑆𝑛}𝑆𝑖={𝑇1,𝑇2,…,𝑇𝑚}
Cross-sentence relationships can be represented as additional triplets.
Example:
Boy is_running Home
Boy is_carrying Red_Bag
Mother is_waiting_for Boy
26. Document Representation
A document consists of paragraphs, sentences, and triplets:𝐷𝑜𝑐𝑢𝑚𝑒𝑛𝑡→𝑃𝑎𝑟𝑎𝑔𝑟𝑎𝑝ℎ𝑠→𝑆𝑒𝑛𝑡𝑒𝑛𝑐𝑒𝑠→𝑇𝑟𝑖𝑝𝑙𝑒𝑡𝑠
The triplet is the semantic unit.
27. Vector Representation
The textual triplet is not itself required to be a vector.
Instead:𝑇=(𝑆,𝑅,𝑂,𝑀)
can be encoded into:𝑉𝑇=𝐸𝑛𝑐𝑜𝑑𝑒(𝑇)
Each semantic element can have its own representation:𝑉𝑆, 𝑉𝑅, 𝑉𝑂
Therefore:𝑉𝑇=𝑓(𝑉𝑆,𝑉𝑅,𝑉𝑂,𝑉𝑀)
This allows Lingvo to maintain both:
INTERPRETABLE REPRESENTATION
        ↓
SUBJECT RELATION OBJECT
        ↓
GRAPH
and:
NUMERICAL REPRESENTATION
        ↓
VECTOR
The vector is derived from the structured knowledge rather than replacing it.
29. Hierarchical Representation
Lingvo can represent information at multiple levels:
CHARACTER
    ↓
MORPHEME
    ↓
WORD
    ↓
PHRASE
    ↓
TRIPLET
    ↓
SENTENCE
    ↓
PARAGRAPH
    ↓
DOCUMENT
The triplet provides the bridge between linguistic structure and knowledge structure.
30. Validation Rules
A valid v4 triplet MUST:
Contain exactly one semantic core.
Contain a Subject.
Contain a Relation.
Contain an Object.
Occupy exactly one physical line.
Use | to separate metadata from the semantic core.
Avoid embedding multiple independent assertions in the core.
Keep provenance separate from the semantic core.
Valid:
Gold has density:19.3_g_cm3 | type:measurement | domain:physics
Invalid:
Gold is dense and valuable and used in jewelry
Invalid:
Gold has_density 19.3_g_cm3
| type:measurement
| domain:physics
The second example violates the one triplet = one line rule.
31. Minimal Form
The smallest valid triplet is:
SUBJECT RELATION OBJECT
Example:
Gold is metal
32. Full Form
The most expressive standard form is:
SUBJECT RELATION OBJECT | type:value | domain:value | context:value | time:value | location:value | condition:value | confidence:value | status:value | source:value | note:value
Not every field is required.
33. Core Principle
Lingvo v4 follows one fundamental architecture:
Three Semantic Elements + Optional Qualification
Therefore:𝑇𝑣4=(𝑆,𝑅,𝑂)+𝑀𝑒𝑡𝑎𝑑𝑎𝑡𝑎+𝑂𝑝𝑡𝑖𝑜𝑛𝑎𝑙 𝐹𝑟𝑒𝑒𝑇𝑒𝑥𝑡
And at system level:𝐿𝑎𝑛𝑔𝑢𝑎𝑔𝑒→𝑇𝑟𝑖𝑝𝑙𝑒𝑡𝑠→𝐺𝑟𝑎𝑝ℎ→𝑉𝑒𝑐𝑡𝑜𝑟𝑠
The triplet remains human-readable and machine-parseable, while the graph and vector layers provide increasingly powerful representations for computation and AI.
