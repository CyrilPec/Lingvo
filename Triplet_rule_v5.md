Lingvo Triplet Rule v5
1. Purpose

Lingvo Triplet v5 extends v4 with semantic normalization of prepositions, conjunctions, and other linguistic connectors.

The goal is to separate:

SURFACE LANGUAGE
        ↓
LINGUISTIC CONNECTOR
        ↓
SEMANTIC TYPE
        ↓
NORMALIZED TRIPLET


A natural-language preposition such as to, from, in, or with is treated as a linguistic signal.

The final knowledge representation should preferably contain the normalized semantic relation, not the original surface word.

2. Core Principle

The v4 semantic core remains:

SUBJECT RELATION OBJECT


One triplet MUST occupy one line.

Example:

Germany exports_to China | type:trade


v5 adds a normalization process:

Germany exports cars to China.

to → destination/recipient

Germany exports_to China | type:trade


The surface preposition is therefore converted into semantic information.

3. Surface Layer and Semantic Layer

Lingvo v5 distinguishes two representations.

Surface representation

The original language:

Germany exports cars to China.

Semantic representation

The normalized knowledge:

Germany exports_to China | type:trade


Formally:

𝑆
𝑢
𝑟
𝑓
𝑎
𝑐
𝑒
→
𝑁
𝑜
𝑟
𝑚
𝑎
𝑙
𝑖
𝑧
𝑎
𝑡
𝑖
𝑜
𝑛
𝑆
𝑒
𝑚
𝑎
𝑛
𝑡
𝑖
𝑐

The original surface form MAY be retained as provenance or linguistic metadata.

4. Preposition Normalization

Prepositions SHOULD be mapped to semantic types whenever their meaning is sufficiently clear.

Recommended initial mappings:

to       → destination
to       → recipient

from     → source
from     → origin

in       → location
in       → containment
in       → context

at       → location
at       → point
at       → time_point

on       → surface
on       → temporal_context

with     → instrument
with     → association
with     → accompaniment

for      → purpose
for      → beneficiary
for      → duration

by       → agent
by       → means
by       → deadline

through  → pathway
through  → mechanism

into     → destination
into     → transformation

onto     → surface_destination

under    → spatial_condition
under    → authority
under    → threshold

over     → spatial_relation
over     → temporal_period
over     → quantity

before   → temporal_before
after    → temporal_after
during   → temporal_context

between  → relation_between

about    → topic

against  → opposition

without  → absence


The mapping is contextual.

The same preposition can have different semantic types.

5. Context Determines Meaning

A preposition MUST NOT be normalized mechanically without considering context.

Example:

Germany exports cars to China.


Here:

to → destination/recipient


Normalized:

Germany exports_to China | type:trade


But:

Germany moved to Berlin.


Here:

to → destination


Normalized:

Germany moved_to Berlin | type:movement


And:

Germany spoke to China.


Here:

to → recipient


Normalized:

Germany communicated_with China | type:diplomacy


Therefore:

𝑆
𝑒
𝑚
𝑎
𝑛
𝑡
𝑖
𝑐
𝑇
𝑦
𝑝
𝑒
=
𝑓
(
𝑃
𝑟
𝑒
𝑝
𝑜
𝑠
𝑖
𝑡
𝑖
𝑜
𝑛
,
𝑉
𝑒
𝑟
𝑏
,
𝐶
𝑜
𝑛
𝑡
𝑒
𝑥
𝑡
)

6. Do Not Preserve Redundant Prepositions

The normalized triplet should not unnecessarily contain both the original preposition and its semantic meaning.

Prefer:

Germany exports_to China | type:trade


over:

Germany exports Cars | prep:to | target:China


when the destination relation can be incorporated directly into the normalized relation.

The surface form MAY be retained separately:

Germany exports_to China | type:trade | surface_prep:to


but surface_prep is optional.

7. Preposition Conversion Examples
Destination

Surface:

Germany exports cars to China.


Normalized:

Germany exports_to China | type:trade

Origin

Surface:

Gold comes from South_Africa.


Normalized:

Gold originates_from South_Africa | type:origin

Location

Surface:

Mona_Lisa is displayed in Louvre.


Normalized:

Mona_Lisa displayed_in Louvre | type:location

Instrument

Surface:

Artist painted with Brush.


Normalized:

Artist painted_with Brush | type:instrument

Purpose

Surface:

Gold is used for Electronics.


Normalized:

Gold used_for Electronics | type:application

Agent

Surface:

Mona_Lisa was painted by Leonardo_da_Vinci.


Normalized:

Leonardo_da_Vinci painted Mona_Lisa | type:artwork


The passive construction and preposition by are normalized into the active semantic relation.

8. Conjunction Normalization

Conjunctions can connect semantic assertions.

Examples:

and      → conjunction
or       → alternative
but      → contrast
because  → cause
therefore→ consequence
although → concession
while    → temporal_overlap
if       → condition
unless   → exception


Example:

Surface:

Germany increased tariffs because imports increased.


Semantic representation:

Germany increased_tariffs | type:economic_event
Imports increased | type:economic_event
T1 caused_by T2 | type:causal

9. Conjunctions Connect Triplets

A conjunction can represent a relationship between two triplets.

T1 = Germany increased_tariffs
T2 = Imports increased

T1 caused_by T2 | type:causal


For and:

T1 = Germany increased_exports
T2 = China increased_imports

T1 associated_with T2 | type:conjunction


For but:

T1 = Germany increased_exports
T2 = Domestic_demand fell

T1 contrasts_with T2 | type:contrast

10. Linguistic Connectors Are Evidence

The original linguistic connector can be retained as evidence of how the semantic relation was derived.

Example:

Germany exports cars to China.


Normalized:

Germany exports_to China | type:trade | surface_prep:to


This creates two layers:

surface_prep:to
        ↓
semantic_type:destination
        ↓
exports_to


The surface form is linguistic evidence.

The normalized relation is the knowledge representation.

11. Multiple Languages

Normalization should ideally map different linguistic forms to the same semantic relation.

For example:

English: Germany exports cars to China.
French:  L'Allemagne exporte des voitures vers la Chine.
German:  Deutschland exportiert Autos nach China.


All can normalize toward:

Germany exports_to China | type:trade


Therefore:

𝐷
𝑖
𝑓
𝑓
𝑒
𝑟
𝑒
𝑛
𝑡
 
𝐿
𝑎
𝑛
𝑔
𝑢
𝑎
𝑔
𝑒
𝑠
→
𝐶
𝑜
𝑚
𝑚
𝑜
𝑛
 
𝑆
𝑒
𝑚
𝑎
𝑛
𝑡
𝑖
𝑐
 
𝑅
𝑒
𝑝
𝑟
𝑒
𝑠
𝑒
𝑛
𝑡
𝑎
𝑡
𝑖
𝑜
𝑛

This is a major purpose of normalization.

12. Synonymous Connectors

Different surface forms may express the same semantic relationship.

Example:

to
toward
towards
into


may express destination depending on context.

They can normalize toward:

destination


Likewise:

from
out_of
originating_from


may normalize toward:

origin


Normalization should prioritize semantic meaning over exact wording.

13. Relation Normalization

Prepositions may modify or determine the final relation.

Surface:

Germany is dependent on China.


Instead of preserving:

Germany dependent China | prep:on


normalize:

Germany depends_on China | type:dependency


Surface:

Germany is interested in China.


normalize:

Germany interested_in China | type:interest


Surface:

Germany is opposed to China.


normalize:

Germany opposed_to China | type:opposition


The final relation should express the semantic meaning naturally.

14. Semantic Relation Families

Normalized relations should preferably belong to semantic families.

LOCATION
    located_in
    moved_to
    moved_from
    originates_from

DIRECTION
    goes_to
    points_to
    directed_toward

TIME
    occurs_before
    occurs_after
    occurs_during
    lasts_for

CAUSE
    causes
    caused_by
    results_in
    results_from

PURPOSE
    used_for
    intended_for
    created_for

AGENT
    created_by
    performed_by
    controlled_by

INSTRUMENT
    made_with
    measured_with
    operated_with

TOPIC
    about
    discusses
    concerns

OPPOSITION
    opposed_to
    competes_with
    conflicts_with

ASSOCIATION
    associated_with
    connected_with
    accompanied_by

15. Normalization Pipeline

A v5 parser should follow:

1. Tokenize
2. Identify entities
3. Identify verbs / predicates
4. Identify prepositions
5. Identify conjunctions
6. Determine grammatical roles
7. Determine semantic type
8. Normalize relation
9. Create triplet
10. Attach metadata
11. Validate
12. Store


Formally:

𝑇
𝑒
𝑥
𝑡
→
𝑇
𝑜
𝑘
𝑒
𝑛
𝑠
→
𝑆
𝑦
𝑛
𝑡
𝑎
𝑥
→
𝑆
𝑒
𝑚
𝑎
𝑛
𝑡
𝑖
𝑐
𝑅
𝑜
𝑙
𝑒
𝑠
→
𝑁
𝑜
𝑟
𝑚
𝑎
𝑙
𝑖
𝑧
𝑎
𝑡
𝑖
𝑜
𝑛
→
𝑇
𝑟
𝑖
𝑝
𝑙
𝑒
𝑡
𝑠

16. Example: Full Sentence

Surface:

Germany increased exports to China because Chinese demand for electric vehicles increased in 2026.


Normalized triplets:

Germany increased_exports China | type:economic_event | context:trade
China demand_for Electric_Vehicles | type:market_relation | time:2026
Demand increased | type:economic_event | context:electric_vehicles | time:2026
Germany increased_exports China | type:causal_consequence | cause:T3


The exact relation names may evolve as the vocabulary becomes more mature.

The important principle is that the semantic graph should preserve the relationships expressed by the original connectors.

17. Triplet-to-Triplet Connections

v5 introduces an explicit statement-level connection:

T1 RELATION T2


Example:

T1 caused_by T2 | type:causal


This creates two graph levels:

ENTITY GRAPH

Entity ──Relation──> Entity


and:

ASSERTION GRAPH

Triplet ──Relation──> Triplet


Together:

𝐸
𝑛
𝑡
𝑖
𝑡
𝑦
𝐺
𝑟
𝑎
𝑝
ℎ
+
𝐴
𝑠
𝑠
𝑒
𝑟
𝑡
𝑖
𝑜
𝑛
𝐺
𝑟
𝑎
𝑝
ℎ

18. Human Readability

The canonical human-readable form remains:

SUBJECT RELATION OBJECT | key:value


v5 does not replace v4.

It adds semantic normalization.

Example:

Germany exports_to China | type:trade


remains readable by humans while also being suitable for machine processing.

19. Machine Representation

A normalized v5 triplet can be represented as:

{
  "s": "Germany",
  "r": "exports_to",
  "o": "China",
  "m": {
    "type": "trade"
  }
}


If linguistic provenance is retained:

{
  "s": "Germany",
  "r": "exports_to",
  "o": "China",
  "m": {
    "type": "trade",
    "surface_prep": "to"
  }
}


The semantic relation is exports_to.

The word to is linguistic provenance.

20. Normalization Principle

The fundamental v5 rule is:

𝑆
𝑢
𝑟
𝑓
𝑎
𝑐
𝑒
 
𝐶
𝑜
𝑛
𝑛
𝑒
𝑐
𝑡
𝑜
𝑟
→
𝑆
𝑒
𝑚
𝑎
𝑛
𝑡
𝑖
𝑐
 
𝑇
𝑦
𝑝
𝑒
→
𝑁
𝑜
𝑟
𝑚
𝑎
𝑙
𝑖
𝑧
𝑒
𝑑
 
𝑅
𝑒
𝑙
𝑎
𝑡
𝑖
𝑜
𝑛

Therefore:

to
    ↓
destination
    ↓
exports_to

from
    ↓
origin
    ↓
originates_from

with
    ↓
instrument
    ↓
painted_with

because
    ↓
causal
    ↓
caused_by

21. Final Architecture

Lingvo now has:

                NATURAL LANGUAGE
                       │
                       ↓
                LINGUISTIC LAYER
                       │
              words / grammar /
           prepositions / conjunctions
                       │
                       ↓
              NORMALIZATION LAYER
                       │
                       ↓
                TRIPLET v5
                       │
                S R O + metadata
                       │
              ┌────────┴────────┐
              ↓                 ↓
        ENTITY GRAPH       ASSERTION GRAPH
              │                 │
              └────────┬────────┘
                       ↓
                 KNOWLEDGE CLOUD
                       │
                 ┌─────┴─────┐
                 ↓           ↓
             NAVIGATION    VECTORS

22. Core v5 Principle

Language expresses relationships through words. Lingvo normalizes those linguistic relationships into explicit semantic connections.

The surface language can change.

The semantic representation remains stable.

𝐸
𝑛
𝑔
𝑙
𝑖
𝑠
ℎ
≈
𝐹
𝑟
𝑒
𝑛
𝑐
ℎ
≈
𝐺
𝑒
𝑟
𝑚
𝑎
𝑛
→
𝐶
𝑜
𝑚
𝑚
𝑜
𝑛
 
𝑆
𝑒
𝑚
𝑎
𝑛
𝑡
𝑖
𝑐
 
𝐺
𝑟
𝑎
𝑝
ℎ

This makes preposition and conjunction normalization a fundamental part of the Lingvo semantic layer rather than merely a formatting feature.