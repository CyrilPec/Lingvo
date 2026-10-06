Lingvo Triplet v4
1. Purpose

Lingvo Triplet v4 is a universal representation for converting language and knowledge into atomic semantic statements.

The same format can represent:

facts

events

properties

relationships

scientific knowledge

news

history

art

economics

people

places

objects

concepts

measurements

claims

The core principle is:

SUBJECT RELATION OBJECT


Everything beyond the three semantic elements is optional qualification.

2. Fundamental Rule

ONE TRIPLET = ONE LINE

A triplet MUST NOT span multiple lines.

Canonical form:

SUBJECT RELATION OBJECT | key:value | key:value | key:value


Example:

Gold has_density 19.3_g_cm3 | type:physical_property | domain:materials_science | condition:room_temperature


Each line represents one atomic semantic assertion.

3. Semantic Core

Every triplet contains exactly three semantic elements:

SUBJECT RELATION OBJECT


Formally:

𝑇
𝑐
𝑜
𝑟
𝑒
=
(
𝑆
,
𝑅
,
𝑂
)

Where:

S = Subject
R = Relation
O = Object


Example:

Gold has_atomic_number 79

S = Gold
R = has_atomic_number
O = 79


The semantic core MUST remain identifiable independently of its metadata.

4. Metadata

Optional metadata follows the semantic core using |.

SUBJECT RELATION OBJECT | key:value | key:value


Recommended metadata fields:

type:
domain:
context:
time:
location:
condition:
unit:
value:
confidence:
status:
source:
author:
reference:
note:


Not every field is required.

Example:

Gold has_density 19.3_g_cm3 | type:physical_property | domain:materials_science | condition:room_temperature

5. Structured Core + Flexible Extension

Lingvo v4 deliberately combines structured and flexible representation.

STRUCTURED SEMANTIC CORE
+
STRUCTURED METADATA
+
OPTIONAL FREE-FORM NOTE


The first three elements are machine-readable.

Metadata should normally be structured:

time:2026-10-06
domain:politics
confidence:high


Free-form information is permitted through note::

Gold symbolizes Wealth | type:cultural_meaning | domain:art | note:Associated_with_divinity_and_royal_status_in_ancient_Egypt


Free-form text MUST NOT replace the semantic core when the information can be represented as a triplet.

6. Atomicity

A triplet should express one primary assertion.

Bad:

Gold is dense and valuable and used in jewelry and electronics


Good:

Gold has_density 19.3_g_cm3 | type:physical_property
Gold has_cultural_value Wealth | type:cultural_property
Gold is_used_in Jewelry | type:application
Gold is_used_in Electronics | type:application


One line = one assertion.

7. Relation Vocabulary

Relations should preferably use normalized names.

Examples:

has
has_property
has_symbol
has_value
has_mass
has_density
has_color
has_structure

is
is_a
is_type_of
is_part_of
is_instance_of
belongs_to

causes
produces
results_in
depends_on
affects
influences

creates
created
develops
developed
produces

uses
used_in
used_for
applied_in

contains
located_in
occurs_in

depicts
represents
symbolizes
references
interprets

precedes
follows
occurs_during

supports
contradicts
claims
denies


The vocabulary is extensible.

Unknown or domain-specific relations are permitted when necessary.

8. Types

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

Gold has_atomic_number 79 | type:identity | domain:chemistry

9. Context

Context qualifies the assertion without changing the semantic core.

Gold has_density 19.3_g_cm3 | type:physical_property | condition:room_temperature

Gold symbolizes Wealth | type:cultural_meaning | context:ancient_Egypt

German_industrial_orders fell 10.6_percent | type:economic_indicator | period:2026-08 | comparison:month_on_month

10. Time

Time should be represented explicitly whenever relevant.

Examples:

Lydia minted_gold_coins ~640_BCE | type:historical_fact | domain:numismatics

German_industrial_orders fell 10.6_percent | type:economic_indicator | period:2026-08

Leonardo_da_Vinci created Mona_Lisa | type:artwork | time:1503-1519


Approximate dates may use ~.

11. Measurements

Measurements should preserve the numerical value and unit.

Preferred:

Gold has_density 19.3_g_cm3 | type:measurement | condition:room_temperature

Gold has_melting_point 1064.18_C | type:measurement | pressure:1_atm


The unit should be part of the object when it is intrinsic to the measurement.

12. Provenance

Knowledge can optionally carry provenance.

German_industrial_orders fell 10.6_percent | type:economic_indicator | period:2026-08 | source:Reuters


Recommended provenance fields:

source:
author:
reference:
retrieved:


Provenance describes where the assertion came from.

It does not become a fourth semantic element.

13. Confidence and Status

Assertions may have confidence and status.

Gold occurs_in Quartz_Veins | type:geological_occurrence | confidence:high

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

14. Negation

Negation should remain explicit.

Preferred:

Gold does_not_contain Iron | type:claim


or, where appropriate:

Gold is_not Magnetic | type:physical_property


The relation should express the semantic meaning clearly.

15. Questions and Uncertainty

Questions can be represented when needed.

Company_X will_acquire Company_Y | type:claim | status:unknown


A question should not be silently converted into a fact.

16. Events

Events use the same three-element structure.

Leonardo_da_Vinci created Mona_Lisa | type:event | domain:art | time:1503-1519

Germany cancelled F126_frigate_program | type:event | domain:defence | time:2026


Events can then be connected to participants, locations, dates, causes, and consequences through additional triplets.

17. Art Example
Leonardo_da_Vinci created Mona_Lisa | type:artwork | domain:art | time:1503-1519
Mona_Lisa depicts Lisa_Gherardini | type:representation | domain:art
Mona_Lisa uses Sfumato | type:technique | domain:painting
Leonardo_da_Vinci developed Sfumato | type:technique | domain:painting
Mona_Lisa belongs_to High_Renaissance | type:classification | domain:art

18. Physics Example
Force causes Acceleration | type:law | domain:physics
Acceleration depends_on Force | type:relationship | domain:physics
Acceleration depends_on Mass | type:relationship | domain:physics
Force has_unit Newton | type:measurement | domain:physics
Mass has_unit Kilogram | type:measurement | domain:physics

19. News Example
German_industrial_orders fell 10.6_percent | type:economic_indicator | domain:industry | period:2026-08 | comparison:month_on_month | source:Reuters
Large_scale_transport_orders fell 61.5_percent | type:economic_indicator | domain:industry | period:2026-08 | source:Reuters
Germany seeks_stronger_EU_trade_protection China | type:trade_policy | domain:economy | context:Chinese_imports | source:Financial_Times
Germany intelligence_services warn Germany Russian_activities | type:security_warning | domain:national_security | date:2026-10-06 | source:tagesschau

20. Triplet Graph

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


Formally:

𝐺
=
(
𝑉
,
𝐸
)

where:

𝑉
 = semantic entities

𝐸
 = triplets

A triplet is an edge:

𝑆
→
𝑅
𝑂

21. Sentence Representation

A sentence can contain multiple atomic triplets:

Gold is dense and resists corrosion.


becomes:

Gold has_density 19.3_g_cm3 | type:physical_property
Gold resists_corrosion | type:chemical_property


Therefore:

𝑆
𝑒
𝑛
𝑡
𝑒
𝑛
𝑐
𝑒
=
{
𝑇
1
,
𝑇
2
,
…
,
𝑇
𝑛
}

Triplets preserve the atomic meaning while the original sentence provides linguistic context.

22. Paragraph Representation

A paragraph is a collection of sentences and their triplets:

𝑃
𝑎
𝑟
𝑎
𝑔
𝑟
𝑎
𝑝
ℎ
=
{
𝑆
1
,
𝑆
2
,
…
,
𝑆
𝑛
}

𝑆
𝑖
=
{
𝑇
1
,
𝑇
2
,
…
,
𝑇
𝑚
}

Cross-sentence relationships can be represented as additional triplets.

Example:

Boy is_running Home
Boy is_carrying Red_Bag
Mother is_waiting_for Boy

23. Document Representation

A document consists of paragraphs, sentences, and triplets:

𝐷
𝑜
𝑐
𝑢
𝑚
𝑒
𝑛
𝑡
→
𝑃
𝑎
𝑟
𝑎
𝑔
𝑟
𝑎
𝑝
ℎ
𝑠
→
𝑆
𝑒
𝑛
𝑡
𝑒
𝑛
𝑐
𝑒
𝑠
→
𝑇
𝑟
𝑖
𝑝
𝑙
𝑒
𝑡
𝑠

The triplet is the atomic semantic unit.

24. Vector Representation

The textual triplet is not itself required to be a vector.

Instead:

𝑇
=
(
𝑆
,
𝑅
,
𝑂
,
𝑀
)

can be encoded into:

𝑉
𝑇
=
𝐸
𝑛
𝑐
𝑜
𝑑
𝑒
(
𝑇
)

Each semantic element can have its own representation:

𝑉
𝑆
,
 
𝑉
𝑅
,
 
𝑉
𝑂

Therefore:

𝑉
𝑇
=
𝑓
(
𝑉
𝑆
,
𝑉
𝑅
,
𝑉
𝑂
,
𝑉
𝑀
)

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

25. Hierarchical Representation

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

26. Validation Rules

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

Gold has_density 19.3_g_cm3 | type:measurement | domain:physics


Invalid:

Gold is dense and valuable and used in jewelry


Invalid:

Gold has_density 19.3_g_cm3
| type:measurement
| domain:physics


The second example violates the one triplet = one line rule.

27. Minimal Form

The smallest valid triplet is:

SUBJECT RELATION OBJECT


Example:

Gold is metal

28. Full Form

The most expressive standard form is:

SUBJECT RELATION OBJECT | type:value | domain:value | context:value | time:value | location:value | condition:value | confidence:value | status:value | source:value | note:value


Not every field is required.

29. Core Principle

Lingvo v4 follows one fundamental architecture:

Three Semantic Elements
+
Optional Qualification

Therefore:

𝑇
𝑣
4
=
(
𝑆
,
𝑅
,
𝑂
)
+
𝑀
𝑒
𝑡
𝑎
𝑑
𝑎
𝑡
𝑎
+
𝑂
𝑝
𝑡
𝑖
𝑜
𝑛
𝑎
𝑙
 
𝐹
𝑟
𝑒
𝑒
𝑇
𝑒
𝑥
𝑡

And at system level:

𝐿
𝑎
𝑛
𝑔
𝑢
𝑎
𝑔
𝑒
→
𝑇
𝑟
𝑖
𝑝
𝑙
𝑒
𝑡
𝑠
→
𝐺
𝑟
𝑎
𝑝
ℎ
→
𝑉
𝑒
𝑐
𝑡
𝑜
𝑟
𝑠

The triplet remains human-readable and machine-parseable, while the graph and vector layers provide increasingly powerful representations for computation and AI.