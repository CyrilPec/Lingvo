Lingvo Triplet Rule v4.3
1. Purpose
Lingvo Triplets provide a compact, readable representation of semantic information for machine processing, knowledge extraction, and knowledge graph construction.
Lingvo preserves original wording while representing subjects, predicates, arguments, modifiers, contextual expressions, and record metadata in a consistent format.
The core principle is:
One triplet = one atomic assertion.
2. Basic Structure
A Lingvo record consists of a subject, a predicate, and one or more arguments or contextual expressions.
General Form
[article, adjectives, number, modifier]SUBJECT PREDICATE([adverb, modifier]ARGUMENT, ARGUMENT) CONTEXTS METADATA
The components are:
SUBJECT — the entity performing, experiencing, or participating in the assertion.
PREDICATE — the action, state, property, or relationship associated with the subject.
ARGUMENTS — objects, entities, values, or other semantic elements associated with the predicate.
CONTEXTS — additional expressions specifying circumstances, including location, time, manner, cause, source, or other semantic roles.
METADATA — information identifying or describing the record, such as its creation identifier, source, type, or confidence.
Examples:
Alice works for(Acme) id:1101026114120
Alice lived in(Paris) in(1999) id:101026115425
Alice is([a]doctor) id:101026114336
Police arrested([two]people) at(airport) in(England) id:101026094236
Each record must occupy exactly one physical line. Line breaks separate records.
The subject, predicate, and primary argument form the semantic core. Additional arguments, contexts, and metadata qualify the assertion or identify the record.
3. Subjects and Entities
An entity represents a person, organization, object, place, event, concept, measurement, or other identifiable subject of knowledge.
Examples:
Alice
Bank_of_England
financial_markets
energy_storage
Underscores may represent spaces within compound entity names.
Original entity wording must be preserved unless an explicit transformation is requested.
4. Entity Modifiers
Square brackets represent modifiers associated with an entity.
General Form
[MODIFIER]ENTITY
Modifiers may include articles, adjectives, numbers, adverbs where appropriate, and other descriptive expressions.
Examples:
[the]student
[British]researchers
[new]housing_policy
[high energy]prices
[new, battery]technology
[the, young, singular]student
Multiple modifiers are separated by commas. Modifiers may contain multiple words.
4.1. Modifier Scope
A bracketed modifier applies to the immediately following subject or argument unless its scope is explicitly specified otherwise.
Examples:
[British]researchers developed([new, battery]technology)
Police arrested([two]people)
[three]students read([interesting, old]books)
[the, young, singular]student uses([actively, English]language)
Interpretation:
[British] modifies researchers.
[new, battery] modifies technology.
[two] modifies people.
[three] modifies students.
[interesting, old] modifies books.
[actively, English] modifies language according to the declared notation.
Modifiers must not be transferred to another entity, the predicate, or a context without an explicit rule.
Modifier order and original wording must be preserved.
4.2. Number and Quantity
A number inside an entity's modifier brackets may represent quantity or another numerical descriptor.
Examples:
Police arrested([two]people)
[three]students arrived
If grammatical number and numerical quantity must be distinguished, implementations may use explicit descriptors such as grammatical_number:singular or quantity:3 in a supported extended notation.
5. Predicates
A predicate expresses an action, state, property, or relationship.
The predicate may be a single word or a multiword expression.
Examples:
Alice works for(Acme)
Alice is([a]doctor)
Government introduce([new]housing_policy)
Police arrested([two]people)
[high energy]prices increased(inflationary_pressure)
Alice depends on([the]team)
5.1. Preserve Original Wording
Lingvo does not automatically normalize predicates.
A conforming implementation must not automatically:
Change verb tense.
Convert verbs to dictionary forms.
Rename predicates.
Replace original wording with standardized relations.
Merge distinct words into a single normalized predicate.
Remove prepositions or grammatical particles.
For example:
Government introduce([new]housing_policy)
Police arrested([two]people)
Andrew_Bailey warned about(market_instability)
The original predicate wording must remain available for reconstruction.
5.2. Predicates Containing Prepositions
A preposition may form an integral part of a multiword predicate.
Examples:
Alice works for(Acme)
Alice depends on([the]team)
Andrew_Bailey warned about(market_instability)
Alice looked at(Bob)
In these examples, the expressions works for, depends on, warned about, and looked at may be interpreted as multiword predicates when the preposition belongs to the lexical or grammatical expression.
The parser must preserve the original wording. It must not automatically rewrite warned about as warned_about.
The distinction between a multiword predicate and a contextual preposition must be determined from the expression's grammatical and semantic function. If the distinction cannot be resolved reliably, the original wording must be preserved without inventing an interpretation.
6. Arguments
Arguments represent objects, entities, concepts, values, events, or other semantic elements directly associated with a predicate.
Arguments are enclosed in parentheses following the predicate.
General Form
PREDICATE(ARGUMENT)
Examples:
Alice uses(English)
Alice is([a]doctor)
Government introduce([new]housing_policy)
LOGINK faced([legal]problems)
An argument may have its own modifiers.
Examples:
Alice reads([interesting]books)
Police arrested([two]people)
[logistics]companies provide(warehousing, customs_clearance)
LOGINK faced([legal]problems, [financial]problems)
6.1. Multiple Arguments
A predicate may contain multiple comma-separated arguments.
Each argument may have independent modifiers.
Argument order must be preserved.
Examples:
[logistics]companies provide(warehousing, customs_clearance)
LOGINK faced([legal]problems, [financial]problems)
Alice gave([the]book, [the]student)
The final example preserves the original argument order; it does not, by itself, establish which argument is the recipient. Semantic roles may be added by an application when necessary.
7. Arguments Versus Contexts
Arguments and contexts must be distinguished by their syntactic structure and semantic function.
7.1. Arguments
Arguments represent the objects or entities directly associated with a predicate.
They normally appear inside the predicate's parentheses.
Example:
Alice uses(English)
Here, English is the argument of uses.
7.2. Contexts
Contexts describe the circumstances under which an assertion applies. They are commonly introduced by prepositions or conjunctions and appear outside the main predicate's argument parentheses.
Examples:
Alice lived in(Paris) in(1999)
Alice worked in(hospital) named(Opori)
Police arrested([two]people) at(airport) in(England)
[high energy]prices increased(inflationary_pressure) in(United_Kingdom)
In these examples, expressions such as in(Paris), in(1999), at(airport), and in(England) provide contextual information.
Contextual expressions may describe:
Location.
Time or period.
Manner.
Cause or reason.
Source or origin.
Purpose.
Conditions.
Other circumstances or semantic relations.
7.3. Prepositions and Multiword Predicates
The presence of a preposition does not automatically make an expression a context.
A preposition belongs to the predicate when it forms an integral part of a multiword predicate. Otherwise, a prepositional expression may introduce a contextual or relational element.
Compare:
Alice depends on([the]team)
Alice lived in(Paris) in(1999)
Alice arrived at(noon)
In depends on([the]team), depends on is the multiword predicate and team is its argument.
In Alice lived in(Paris) in(1999), the expressions in(Paris) and in(1999) represent location and time contexts, respectively.
In Alice arrived at(noon), at(noon) represents the time context.
The parser must preserve the original expression and distinguish predicate arguments from contextual expressions whenever the structure and semantic function permit reliable interpretation.
7.4. Contextual Expressions and Semantic Roles
A prepositional expression may express a relation that is important to the meaning of the assertion without being a direct object argument.
For example:
Alice works at(Opori)
Alice arrived from(London)
Alice acted with(caution)
These expressions provide contextual or relational information.
Where an application requires more explicit semantic roles, it may derive them in a separate representation without replacing the original Lingvo record.
8. Record Creation Identifier
The optional id: field identifies the record's creation time.
It is metadata, not a semantic argument and not the time of the event described by the record.
8.1. Format
id:DDMMYYHHmmss
Fields:
DD — day of the month.
MM — month.
YY — two-digit year.
HH — hour in 24-hour format.
mm — minute.
ss — second.
Example:
Alice lived in(Paris) in(1999) id:101026093101
The identifier represents a creation time of 09:31:01 on 10 October 2026.
8.2. Rules
The ID records when the triplet was created.
The ID must not alter the semantic meaning of the record.
Creation time must remain distinct from event time.
The ID must be preserved during parsing, serialization, export, and import.
An existing ID must not be silently replaced when a record is edited.
If a record is copied or split into new records, the application must define whether the original ID is retained as provenance or new IDs are assigned.
A timestamp with one-second precision does not guarantee uniqueness. If guaranteed uniqueness is required, an implementation must use an additional identifier or a more precise timestamp.
9. Metadata and Provenance
Metadata provides information about a record without changing its semantic core.
The id: field is reserved for the record creation identifier.
Additional metadata may include:
type:physical_property
domain:materials_science
source:document42
author:researcher
reference:source_identifier
confidence:0.95
status:asserted
condition:room_temperature
unit:kg
note:additional_information
Example:
Gold has density(19.3_g_cm3) type:physical_property domain:materials_science condition:room_temperature
Metadata keys and values must follow a consistent serialization convention.
Metadata must not be confused with arguments, contexts, or event-time expressions.
Provenance describes the source or origin of an assertion. It does not become an additional semantic element of the assertion.
10. Atomicity
A Lingvo record should express one primary assertion.
Unrelated assertions should be represented as separate records.
Instead of combining several independent facts in one record, use separate assertions:
Gold has_density(19.3_g_cm3) type:physical_property
Gold has_cultural_value(wealth) type:cultural_property
Gold is_used in(Jewelry) type:application
Gold is_used in(Electronics) type:application
Each line represents one primary assertion. Multiple records may describe the same entity.
11. Types, Time, and Measurements
11.1. Types
The optional type: metadata field describes the nature of an assertion.
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
Gold has_atomic_number(79) type:identity domain:chemistry
Types are descriptive metadata and do not replace the subject, predicate, or arguments.
11.2. Event Time
Event time describes when an event occurred, not when its record was created.
Examples:
Leonardo_da_Vinci created(Mona_Lisa) time:1503-1519
German_industrial_orders fell(10.6_percent) period:2026-08
Alice lived in(Paris) in(1999) id:101026093101
Approximate dates may use ~ where supported by the application.
Event-time conventions must remain distinct from the id: creation timestamp.
11.3. Measurements
Measurements should preserve numerical values and units.
Examples:
Gold has_density(19.3_g_cm3) type:measurement condition:room_temperature
Gold has_melting_point(1064.18_C) type:measurement pressure:1_atm
Values and units must remain interpretable during conversion.
12. Negation, Uncertainty, and Status
Negation must remain explicit and must not be silently removed during processing.
Examples:
Gold does_not_contain(Iron) type:claim
Gold is_not(Magnetic) type:physical_property
Company_X may_acquire(Company_Y) type:claim status:unconfirmed
Company_X will_acquire(Company_Y) type:claim status:unknown
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
Uncertainty must not be silently converted into a confirmed fact.
13. Events and Relationships Between Assertions
Events use the same subject–predicate–argument structure as other records.
Examples:
Leonardo_da_Vinci created(Mona_Lisa) type:event time:1503-1519
Mona_Lisa depicts(Lisa_Gherardini) type:representation
Mona_Lisa uses(Sfumato) type:technique
Events may have participants, locations, times, causes, and consequences.
Where an application requires explicit relationships between entire assertions, it should represent those relationships in a separate, well-defined structure. Such relationships must not be confused with ordinary entity arguments.
14. Knowledge Graph Construction
Lingvo records can be transformed into a knowledge graph.
Example:
Alice lived in(Paris) in(1999)
An application may derive an event-based graph:
Alice ──participates_in──> Living_Event
Living_Event ──location──> Paris
Living_Event ──time──> 1999
This representation distinguishes the event's location from its time.
A graph builder may derive normalized relations such as lives_in, works_at, or has_profession.
Derived relations must not silently replace the original Lingvo representation.
Entity resolution is a separate process: identical names do not necessarily identify the same real-world entity.
15. Parsing Requirements
A conforming Lingvo parser should:
Recognize the subject and predicate.
Recognize arguments enclosed in parentheses.
Recognize modifiers enclosed in square brackets.
Apply bracketed modifiers to the immediately following subject or argument.
Support multiple comma-separated arguments and modifiers.
Preserve original entity and predicate wording.
Preserve argument order and modifier order.
Recognize prepositional and conjunction-based contextual expressions.
Distinguish multiword predicates from contextual expressions where possible.
Recognize id: as record creation metadata.
Keep creation time distinct from event time.
Preserve supported metadata during processing.
Avoid automatic verb normalization or silent predicate renaming.
Preserve the original record whenever exact reconstruction is required.
Avoid inventing semantic interpretations when attachment is ambiguous.
The parser must distinguish a preposition that belongs to a multiword predicate from a preposition that introduces a context or relation.
When the distinction cannot be resolved reliably, the parser must preserve the original expression and avoid imposing an unsupported interpretation.
16. Serialization and Round-Trip Conversion
Lingvo may be converted to JSON, XML, SML, or other formats.
A lossless conversion must preserve:
Subject, predicate, and arguments.
Entity modifiers and their ordering.
Original predicate wording.
Argument order.
Prepositions and contextual expressions.
Record creation identifiers and supported metadata.
Event time and other semantic information.
Original text whenever required for exact reconstruction.
The central requirement is:
decode(encode(original)) == original
Equality means that all information covered by the serialization contract is preserved.
If a target format cannot represent a distinction, the converter must preserve it through an explicit extension or report that the conversion is lossy.
17. Canonical Examples
People and Places
Alice is([a]doctor)  id:101026093103
Alice lived in(Paris) in(1999)  id:101026093104
Alice worked in(hospital) named(Opori)  id:101026093105
Alice works for(Acme)  id:101026093106
Alice depends on([the]team)  id:101026093107
Alice arrived at(noon)  id:101026093108
Government and Policy
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
Creation Metadata
Alice lived in(Paris) in(1999) id:101026093101
The creation identifier describes the record, not the historical event.
18. Core Design Principles
Lingvo v4.3 follows these principles:
Atomicity: one record expresses one primary assertion.
Preservation: original predicate wording and entity modifiers are retained.
Explicit structure: subjects, arguments, and contextual expressions remain identifiable.
Modifier scope: modifiers apply to the immediately following subject or argument unless otherwise specified.
Argument–context distinction: arguments represent semantic elements associated with the predicate; contextual expressions describe circumstances or relations.
Multiword predicates: integral prepositions remain part of the predicate expression.
Flexible context: prepositions are preserved rather than assigned one universal semantic meaning.
Separate timestamps: event time and record creation time remain distinct.
Stable metadata: identifiers and supported metadata survive conversion.
Graph compatibility: normalized knowledge graphs may be derived without destroying the original representation.
Lossless conversion: serialization preserves all information covered by its contract.
Lingvo is the source representation of extracted semantic information. Knowledge graphs and other output formats are derived representations.
