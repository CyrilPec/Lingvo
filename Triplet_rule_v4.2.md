Lingvo Triplet Rule v4
1. Purpose
Lingvo Triplets provide a compact, readable representation of semantic information for machine processing, knowledge extraction, and knowledge graph construction.
Lingvo preserves the original wording of predicates and represents entities, modifiers, arguments, contextual information, and record metadata in a consistent format.
The core principle is:
One triplet = one atomic assertion.
2. Basic Structure
A Lingvo record consists of a subject, a predicate, and one or more arguments or contextual expressions.
General form:
[article, adjectives, number]SUBJECT PREDICATE(ARGUMENTS) CONTEXT() METADATA:
Examples:
Alice works for(Acme) id:1101026114120
Alice lived in(Paris) in(1999) id:101026115425
Alice is a(doctor)id:101026114336
Police arrested([two]people) at(airport) in(England) id:101026094236
A triplet should occupy one physical line. Line breaks separate records.
The subject, predicate, and primary argument form the semantic core. Additional expressions qualify the assertion or identify the record.
3. Subjects and Entities
An entity represents a person, organization, object, place, event, concept, measurement, or other identifiable subject of knowledge.
Examples:
Alice
Bank_of_England
financial_markets
energy_storage
Underscores may represent spaces within compound entity names.
The original entity wording should be preserved unless an explicit transformation is requested.
4. Entity Modifiers
Square brackets represent modifiers attached to an entity.
General form:
[MODIFIER]ENTITY
Examples:
[British]researchers
[new]housing_policy
[high energy]prices
[new, battery]technology
Modifiers may contain multiple words. Multiple modifiers are separated by commas.
Modifiers may appear on subjects and arguments:
[British]researchers developed([new, battery]technology)
[new, battery]technology improves([energy]storage)
Police arrested([two]people)
The modifier belongs to the entity immediately following its closing bracket. Modifier order and original wording must be preserved.
5. Predicates
A predicate expresses an action, state, property, or relationship.
Examples:
Alice works_for(Acme)
Government introduce([new]housing_policy)
Police arrested([two]people)
[high energy]prices increased(inflationary_pressure)
5.1. Preserve Original Wording
Lingvo does not automatically normalize predicates.
Do not automatically:
Change verb tense.
Convert verbs to dictionary forms.
Rename predicates.
Replace original wording with standardized relations.
Merge distinct expressions into a single predicate.
For example:
Government introduce([new]housing_policy)
Police arrested([two]people)
The predicates remain introduce and arrested, respectively.
5.2. Predicates Containing Prepositions
Prepositions may form part of a multiword predicate.
Example:
Andrew_Bailey warned about(market_instability) in(financial_markets)
Here, warned about is preserved as the predicate expression. The expression in(financial_markets) supplies additional context.
The parser must not automatically rewrite warned about as warned_about.
The original predicate wording must remain available for reconstruction.
6. Arguments
Arguments are enclosed in parentheses and represent entities, concepts, values, events, or other semantic objects associated with a predicate.
Examples:
Alice works_for(Acme)
Alice is a(doctor)
Government introduce([new]housing_policy)
6.1. Multiple Arguments
A predicate may contain multiple comma-separated arguments.
Examples:
LOGINK faced([legal]problems, [financial]problems)
[logistics]companies provide(warehousing, customs_clearance)
Each argument may have its own modifiers.
Argument order must be preserved.
7. Prepositional Arguments and Context
Prepositional expressions preserve the original preposition and its associated argument.
General form:
PREPOSITION(ARGUMENT)
Examples:
in(Paris)
in(1999)
at(airport)
about(market_instability)
named(Opori)
Complete examples:
Alice lived in(Paris) in(1999)
Alice worked in(hospital) named(Opori)
Police arrested([two]people) at(airport) in(England)
[high energy]prices increased(inflationary_pressure) in(United_Kingdom)
Prepositional expressions may represent location, time, setting, cause, source, manner, or other contextual roles.
7.1. Context Depends on the Predicate
A preposition does not have one universal semantic meaning.
Examples:
Alice looked at(Bob)
Alice arrived at(noon)
Alice worked at(Opori)
The parser must preserve the original expression. Its semantic role depends on the predicate, argument, and context.
7.2. Predicate Arguments Versus Context
The parser should distinguish between:
The primary arguments of a predicate.
Prepositions that belong to a multiword predicate.
Additional contextual expressions.
Modifiers attached to entities.
When the syntax alone cannot resolve an attachment unambiguously, the parser should preserve the original text and avoid inventing an interpretation.
8. Triplet Creation ID
The optional id(...) field records the creation time of a triplet.
It is record metadata, not a semantic argument and not the time of the event described by the triplet.
8.1. Format
id(DDMMYYHHmmss)
Fields:
DD — day of the month.
MM — month.
YY — two-digit year.
HH — hour in 24-hour format.
mm — minute.
ss — second.
Example:
id:101026093101
This represents a creation time of 09:31:01 on 10 October 2026.
8.2. Rules
The ID records when the triplet was created.
The ID must not alter the semantic meaning of the triplet.
The creation time is distinct from event time, which may be represented by expressions such as in(1999).
The ID must be preserved during parsing, serialization, export, and import.
An existing ID must not be silently replaced when a triplet is edited.
If a triplet is copied or split into new records, the application must define whether the original ID is retained as provenance or new IDs are assigned.
If guaranteed uniqueness is required, a separate unique identifier or additional timestamp precision must be used. A timestamp accurate only to the minute cannot guarantee uniqueness.
8.3. Example
Alice lived in(Paris) in(1999) id:1010260931
Interpretation:
Subject: Alice
Predicate: lived
Location: Paris
Event time: 1999
Record creation time: 10 October 2026, 09:31:00
The ID describes the record, not the historical event.
10. Metadata and Provenance
Metadata provides information about an assertion without changing its semantic core.
In addition to the creation ID, an implementation may support named metadata fields such as:
type
domain
source
author
reference
confidence
status
condition
unit
note
For example:
Gold has density(19.3_g_cm3)  type:physical_property  domain:materials_science  condition:room_temperature
The exact metadata serialization must be defined consistently by the implementation. The id:... syntax is reserved for creation-time metadata and must not be confused with event-time expressions.
Provenance describes the source of an assertion. It does not become a fourth semantic element.
11. Atomicity
A triplet should express one primary assertion.
Avoid combining unrelated assertions in one record.
Instead of:
Gold is_dense and_valuable and_used_in(jewelry, electronics)
Use separate assertions:
Gold has density:19.3_g_cm3  type:physical_property
Gold has cultural_value:Wealth  type:cultural_property
Gold is_used in(Jewelry)  type:application
Gold is_used in(Electronics)  type:application
Each line represents one assertion. Multiple triplets can describe the same entity.
12. Types, Time, and Measurements
11.1. Types
The optional type metadata field describes the nature of an assertion.
Examples include:
identity
classification
physical_property
measurement
event
historical_fact
scientific_fact
economic_indicator
claim
observation
prediction
Example:
Gold has atomic_number:79  type:identity  domain:chemistry
Types are descriptive metadata and do not replace the subject, predicate, or argument.
11.2. Event Time
Event time describes when an event occurred, not when its triplet was created.
Examples:
Leonardo_da_Vinci created(Mona_Lisa)  time:1503-1519
German_industrial_orders fell(10.6_percent)  period:2026-08
Approximate dates may use ~ where supported by the application.
Prepositional time expressions may also be used:
Alice lived in(Paris) in(1999)
Event-time conventions must remain distinct from the id(...) creation timestamp.
11.3. Measurements
Measurements should preserve numerical values and units.
Examples:
Gold has density:19.3_g_cm3  type:measurement  condition:room_temperature
Gold has melting_point:1064.18_C  type:measurement  pressure:1_atm
The value and unit must remain interpretable during conversion.
13. Negation, Uncertainty, and Status
Negation must remain explicit.
Examples:
Gold does_not_contain(Iron)  type:claim
Gold is_not(Magnetic)  type:physical_property
Uncertainty must not be silently converted into a confirmed fact.
Examples:
Company_X may_acquire(Company_Y)  type:claim  status:unconfirmed
Company_X will_acquire(Company_Y)  type:claim  status:unknown
Possible status values include:
confirmed
probable
possible
unconfirmed
disputed
historical
estimated
predicted
These values are conventions, not an exhaustive vocabulary.
14. Events and Relationships Between Assertions
Events use the same basic subject–predicate–argument structure.
Examples:
Leonardo_da_Vinci created(Mona_Lisa)  type:event  time:1503-1519
Mona_Lisa depicts(Lisa_Gherardini)  type:representation
Mona_Lisa uses(Sfumato)  type:technique
Events may have participants, locations, times, causes, and consequences.
Where an application requires explicit relationships between entire assertions, it should represent those relationships in a separate, well-defined structure. They must not be confused with ordinary entity arguments.
15. Knowledge Graph Construction
Lingvo triplets can be transformed into a knowledge graph.
For example:
Alice lived in(Paris) in(1999)
An application may derive an event-based graph:
Alice ──lived──> Living_Event
Living_Event ──location──> Paris
Living_Event ──time──> 1999
This representation distinguishes the event's location from its time.
A graph builder may derive normalized relations such as lives_in, works_at, or has_profession. These derived relations must not silently replace the original Lingvo representation.
Entity resolution is a separate process: identical names do not necessarily identify the same real-world entity.
16. Parsing Requirements
A conforming parser should:
Recognize the subject and predicate.
Recognize arguments enclosed in parentheses.
Recognize modifiers enclosed in square brackets.
Support multiple comma-separated arguments and modifiers.
Preserve original entity and predicate wording.
Preserve argument order and modifier order.
Recognize prepositional expressions.
Distinguish predicate wording from additional contextual expressions where possible.
Recognize id:... as creation-time metadata.
Keep creation time distinct from event time.
Preserve supported metadata during processing.
Avoid automatic verb normalization.
Avoid silently renaming predicates.
Preserve the original record whenever exact reconstruction is required.
Avoid inventing semantic interpretations when attachment is ambiguous.
The exact grammar for ambiguous multiword predicates and contextual attachment should be defined by the parser implementation.
17. Serialization and Round-Trip Conversion
Lingvo may be converted to JSON, XML, SML, or other formats.
A lossless conversion must preserve:
Subject, predicate, arguments.
Entity modifiers and their ordering.
Original predicate wording.
Prepositions and contextual expressions.
Creation ID and supported metadata.
Event time and other semantic information.
Original text whenever required for exact reconstruction.
The central requirement is:
decode(encode(original)) == original
The equality means that all information covered by the serialization contract is preserved.
If a target format cannot represent a distinction, the converter must preserve it through an explicit extension or report that the conversion is lossy.
18. Canonical Examples
People and places
Alice is a(doctor)
Alice lived in(Paris) in(1999)
Alice worked in(hospital) named(Opori)
Government and policy
Government introduce([new]housing_policy)
[British]government announced(policy) in(England)
[new]policy aims to increase([available]housing_supply)
Technology
[British]researchers developed([new, battery]technology)
[new, battery]technology improves([energy]storage)
[cloud]applications support(team_collaboration)
Logistics
[logistics]companies provide(warehousing, customs_clearance)
LOGINK faced([legal]problems, [financial]problems)
[logistics]data supports(cargo_tracking) across(supply_chains)
Economy
[high energy]prices increased(inflationary_pressure) in(United_Kingdom)
Andrew_Bailey warned about(market_instability) in(financial_markets)
[private]landlords faced([higher]interest_rates, [increased]regulatory_costs)
Creation metadata
Alice lived in(Paris) in(1999) id:101026931
19. Core Design Principles
Lingvo follows these principles:
Atomicity: one triplet expresses one primary assertion.
Preservation: original predicate wording and entity modifiers are retained.
Explicit structure: subjects, arguments, and contextual expressions remain identifiable.
Flexible context: prepositions are preserved rather than treated as universal semantic relations.
Separate timestamps: event time and record creation time are distinct.
Stable metadata: record IDs and supported metadata survive conversion.
Graph compatibility: normalized knowledge graphs may be derived without destroying the original representation.
Lossless conversion: serialization should preserve all information covered by its contract.
