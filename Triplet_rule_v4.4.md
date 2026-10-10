#Lingvo Triplet Rule v4.4
1. Purpose
Lingvo Triplets provide a compact, readable representation of semantic information for machine processing, knowledge extraction, and knowledge graph construction.
Lingvo preserves original wording while representing subjects, predicates, arguments, modifiers, subordinate expressions, contextual relations, and metadata.
Core principle: One triplet represents one assertion.
2. Basic Structure
A Lingvo record consists of a subject, a predicate, and one or more arguments or contextual expressions.
General form
[article, adjectives, number, modifier]SUBJECT PREDICATE([modifier]ARGUMENT, ARGUMENT) CONTEXTS METADATA
The components are:
SUBJECT — the entity performing, experiencing, or participating in the assertion.
PREDICATE — the action, state, property, or relationship associated with the subject.
ARGUMENTS — objects, entities, values, or other semantic elements associated with the predicate.
STRUCTURAL ATTACHMENTS — subordinate expressions enclosed in curly braces and attached to the immediately preceding expression.
CONTEXTS — additional expressions describing circumstances, such as location, time, manner, cause, or source.
METADATA — information identifying or describing the record, such as its identifier, source, type, or confidence.
Examples:
Alice works for(Acme) id:1101026114120
Alice lived in(Paris) in(1999) id:101026115425
Alice is([a]doctor) id:101026114336
Hakan_Fidan raised(concerns{about(escalating Russian and Ukrainian attacks in the Black Sea)}) time:2026-10-08 source:Reuters id:101026125810
[Russia, Ukraine]who are_increasing(military operations) in(the Black Sea) time:2026-10 source:Reuters id:101026125812
Police arrested([two]people) at(airport) in(England) id:101026094236
[Black_Sea]attacks threaten(the safety of commercial vessels) time:2026-10 source:Reuters id:101026094238
Alice was(afraid{of(the_dark)}) id:101026120003
Each record must occupy exactly one physical line. Line breaks separate records.
The subject, predicate, and primary argument form the semantic core. Structural attachments, additional arguments, contexts, and metadata provide further information.
4. Subjects and Entities
An entity represents a person, organization, object, place, event, concept, measurement, or other identifiable subject of knowledge.
Examples:
Alice
Bank_of_England
financial_markets
energy_storage
Underscores may represent spaces within compound entity names.
Original entity wording must be preserved unless an explicit transformation is requested.
5. Entity Modifiers
Square brackets represent modifiers associated with an entity.
General form
[MODIFIER]ENTITY
Modifiers may include articles, adjectives, numbers, and other descriptive expressions.
Examples:
[the]student
[British]researchers
[new]housing_policy
[high energy]prices
[new, battery]technology
[the, young, singular]student
Multiple modifiers are separated by commas. Modifiers may contain multiple words.
4.1. Modifier scope
A bracketed modifier applies to the immediately following subject or argument unless explicitly specified otherwise.
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
Modifiers must not be transferred to another entity, predicate, or context without an explicit rule.
Modifier order and original wording must be preserved.
4.2. Number and quantity
Numbers inside modifier brackets may represent quantity or another numerical descriptor.
Examples:
Police arrested([two]people)
[three]students arrived
When grammatical number and numerical quantity must be distinguished, implementations may use explicit descriptors such as grammatical_number:singular and quantity:3.
6. Predicates
A predicate expresses an action, state, property, or relationship. It may consist of one word or a multiword expression.
Examples:
Alice works for(Acme)
Alice is([a]doctor)
Alice depends on([the]team)
Andrew_Bailey warned about(market_instability)
Police arrested([two]people)
5.1. Preserve original wording
Lingvo does not automatically normalize predicates.
A conforming implementation must not automatically:
Change verb tense.
Convert verbs to dictionary forms.
Rename predicates.
Replace original wording with standardized relations.
Merge distinct words into a normalized predicate.
Remove prepositions or grammatical particles.
Original predicate wording must remain available for reconstruction.
5.2. Predicates containing prepositions
A preposition may form an integral part of a multiword predicate.
Examples:
Alice works for(Acme)
Alice depends on([the]team)
Andrew_Bailey warned about(market_instability)
Alice looked at(Bob)
The expressions works for, depends on, warned about, and looked at may be interpreted as multiword predicates when the preposition is integral to the expression.
The parser must preserve the original wording. It must not automatically rewrite warned about as warned_about.
When the distinction between a multiword predicate and a contextual preposition cannot be resolved reliably, the original expression must be preserved without imposing an unsupported interpretation.
7. Arguments
Arguments represent objects, entities, concepts, values, events, or other semantic elements directly associated with a predicate.
General form
PREDICATE(ARGUMENT)
Examples:
Alice uses(English)
Alice is([a]doctor)
Government introduce([new]housing_policy)
LOGINK faced([legal]problems)
Arguments may have modifiers.
Examples:
Alice reads([interesting]books)
Police arrested([two]people)
[logistics]companies provide(warehousing, customs_clearance)
LOGINK faced([legal]problems, [financial]problems)
6.1. Multiple arguments
A predicate may contain multiple comma-separated arguments. Each argument may have independent modifiers.
Argument order must be preserved.
Examples:
[logistics]companies provide(warehousing, customs_clearance)
LOGINK faced([legal]problems, [financial]problems)
Alice gave([the]book, [the]student)
The final example preserves argument order but does not independently specify semantic roles such as recipient. Applications may add those roles in a separate representation.
8. Arguments versus contexts
Arguments and contexts must be distinguished by their syntactic structure and semantic function.
7.1. Arguments
Arguments represent objects or entities directly associated with a predicate and normally appear inside its parentheses.
Example:
Alice uses(English)
Here, English is the argument of uses.
7.2. Contexts
Contexts describe the circumstances under which an assertion applies. They are commonly introduced by prepositions or conjunctions and appear outside the main predicate's argument parentheses.
Examples:
Alice lived in(Paris) in(1999)
Alice worked in(hospital) named(Opori)
Police arrested([two]people) at(airport) in(England)
Contexts may express location, time, manner, cause, reason, source, origin, purpose, condition, or other relations.
7.3. Prepositions and multiword predicates
The presence of a preposition does not automatically make an expression a context.
Compare:
Alice depends on([the]team)
Alice lived in(Paris) in(1999)
Alice arrived at(noon)
In depends on([the]team), depends on is the multiword predicate and team is its argument.
In Alice lived in(Paris) in(1999), the expressions in(Paris) and in(1999) represent location and time contexts.
In Alice arrived at(noon), at(noon) represents the time context.
The parser must preserve the original expression and distinguish predicate arguments from contextual expressions whenever the structure and semantic function permit reliable interpretation.
9. Structural Attachments
Curly braces {...} indicate a subordinate expression structurally attached to the immediately preceding expression.
Structural attachment makes the relationship between an expression and its complement explicit, rather than leaving the complement as a separate context.
8.1. General form
EXPRESSION{RELATION(ARGUMENT)}
The expression inside curly braces belongs to the immediately preceding expression. It must not automatically be interpreted as an independent context of the main predicate.
8.2. Example: emotional state
Alice was(afraid{of(the_dark)}) id:101026120003
Interpretation:
Alice — subject.
was — predicate.
afraid — primary state argument.
{of(the_dark)} — subordinate expression attached to afraid.
the_dark — argument of the relation of.
id:101026120003 — record metadata.
The expression of(the_dark) specifies what Alice was afraid of. It is structurally attached to afraid, not treated as an independent context of was.
8.3. Additional examples
Alice became(tired{of(sitting)}) id:101026120001
Alice was(proud{of(her_work)}) id:101026120002
Alice was(afraid{of(the_dark)}) id:101026120003
Alice was(interested{in(science)}) id:101026120004
Alice was(dependent{on([the]team)}) id:101026120005
In each example, the subordinate expression specifies the complement associated with the preceding state or expression.
8.4. Nested attachments
Curly braces may be nested to represent multiple levels of structural dependency.
Example:
Alice became(tired{of(sitting{by([her]sister)}{on([the]bank)})}) id:101026120006
Interpretation:
tired is the state associated with Alice.
{of(...)} attaches the activity sitting to tired.
{by([her]sister)} attaches a relational expression to sitting.
{on([the]bank)} attaches a location expression to sitting.
This structure represents the intended dependency between the state, activity, and subordinate expressions.
The precise attachment must reflect the intended meaning of the source text. Braces must not be used to imply a semantic relationship that the source does not support.
8.5. Structural attachment rules
Curly braces mark structural attachment, not literal quotation.
A braced expression attaches to the immediately preceding expression.
Parentheses inside braces retain their ordinary argument or relation function.
Square brackets retain their modifier function.
Braced expressions may contain further braced expressions.
Each attachment must preserve the original wording of the expression.
Structural attachments must not be silently converted into independent contexts.
An implementation must preserve attachment scope during parsing, serialization, and reconstruction.
If attachment scope is ambiguous, the parser must preserve the original expression and avoid inventing a relationship.
10. Record Creation Identifier
The optional id: field identifies the record's creation time. It is metadata, not a semantic argument or the time of the event described.
9.1. Format
id:DDMMYYHHmmss
Fields:
DD — day.
MM — month.
YY — two-digit year.
HH — hour in 24-hour format.
mm — minute.
ss — second.
Example:
Alice lived in(Paris) in(1999) id:101026093101
The identifier represents a creation time of 09:31:01 on 10 October 2026.
9.2. Rules
The ID records when the triplet was created.
Creation time must remain distinct from event time.
The ID must be preserved during parsing, serialization, export, and import.
An existing ID must not be silently replaced when a record is edited.
Applications must define how IDs are handled when records are copied or split.
A timestamp with one-second precision does not guarantee uniqueness. An additional identifier or greater precision is required when guaranteed uniqueness is needed.
11. Metadata and Provenance
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
Gold has_density(19.3_g_cm3) type:physical_property domain:materials_science condition:room_temperature
Metadata keys and values must follow a consistent serialization convention.
Provenance describes the source or origin of an assertion. It does not become an additional semantic element of the assertion.
12. Atomicity
A Lingvo record should express one primary assertion. Independent assertions should normally be represented as separate records.
Examples:
Gold has_cultural_value(wealth) type:cultural_property
Gold is_used in(Jewelry) type:application
Gold is_used in(Electronics) type:application
Each line represents one primary assertion. Multiple records may describe the same entity.
Structural attachments may express subordinate relationships within one assertion without requiring each subordinate expression to become an independent record.
13. Types, Time, and Measurements
12.1. Types
The optional type: metadata field describes the nature of an assertion.
Examples include identity, classification, physical_property, measurement, event, historical_fact, scientific_fact, economic_indicator, claim, observation, and prediction.
Example:
Gold has_atomic_number(79) type:identity domain:chemistry
Types are descriptive metadata and do not replace the subject, predicate, or arguments.
12.2. Event time
Event time describes when an event occurred, not when its record was created.
Examples:
Leonardo_da_Vinci created(Mona_Lisa) time:1503-1519
German_industrial_orders fell(10.6_percent) period:2026-08
Alice lived in(Paris) in(1999) id:101026093101
Approximate dates may use ~ where supported by the application.
Event-time conventions must remain distinct from the id: creation timestamp.
12.3. Measurements
Measurements should preserve numerical values and units.
Examples:
Gold has_density(19.3_g_cm3) type:measurement condition:room_temperature
Gold has_melting_point(1064.18_C) type:measurement pressure:1_atm
Values and units must remain interpretable during conversion.
14. Negation, Uncertainty, and Status
Negation must remain explicit and must not be silently removed during processing.
Examples:
Gold does_not_contain(Iron) type:claim
Gold is_not(Magnetic) type:physical_property
Company_X may_acquire(Company_Y) type:claim status:unconfirmed
Company_X will_acquire(Company_Y) type:claim status:unknown
Possible status values include confirmed, probable, possible, unconfirmed, disputed, historical, estimated, and predicted.
Uncertainty must not be silently converted into a confirmed fact.
15. Events and Relationships Between Assertions
Events use the same subject–predicate–argument structure as other records.
Examples:
Leonardo_da_Vinci created(Mona_Lisa) type:event time:1503-1519
Mona_Lisa depicts(Lisa_Gherardini) type:representation
Mona_Lisa uses(Sfumato) type:technique
Events may have participants, locations, times, causes, and consequences.
Structural attachments represent dependencies within an assertion. Relationships between independent assertions should be represented separately when an application requires them.
16. Knowledge Graph Construction
Lingvo records can be transformed into knowledge graphs.
Example:
Alice was(afraid{of(the_dark)}) id:101026120003
A derived graph may represent:
Alice ──state──> afraid
afraid ──object_of_fear──> the_dark
This graph is a derived semantic interpretation. It does not replace the original Lingvo record.
A graph builder may derive normalized relations, but must preserve the original wording and attachment structure when lossless reconstruction is required.
Entity resolution remains a separate process: identical names do not necessarily identify the same real-world entity.
17. Parsing Requirements
A conforming Lingvo parser should:
Recognize the subject and predicate.
Recognize arguments enclosed in parentheses.
Recognize modifiers enclosed in square brackets.
Apply bracketed modifiers to the immediately following subject or argument.
Recognize structural attachments enclosed in curly braces.
Attach each braced expression to the immediately preceding expression.
Support nested structural attachments.
Preserve argument order and modifier order.
Recognize contextual expressions introduced by prepositions or conjunctions.
Distinguish multiword predicates from contextual expressions where possible.
Recognize id: as record creation metadata.
Keep creation time distinct from event time.
Preserve original wording and supported metadata.
Avoid automatic verb normalization or silent predicate renaming.
Preserve attachment scope during conversion and reconstruction.
Avoid inventing semantic interpretations when attachment is ambiguous.
Curly braces must be parsed as structural operators, not as literal quotation marks or decorative punctuation.
18. Serialization and Round-Trip Conversion
Lingvo may be converted to JSON, XML, SML, or other formats.
A lossless conversion must preserve:
Subject, predicate, and arguments.
Entity modifiers and their ordering.
Original predicate wording.
Argument order.
Prepositions and contextual expressions.
Structural attachments and their nesting.
Record creation identifiers and supported metadata.
Event time and other semantic information.
Original text whenever required for exact reconstruction.
The central requirement is:
decode(encode(original)) == original
Equality means that all information covered by the serialization contract is preserved.
If a target format cannot represent a distinction, the converter must preserve it through an explicit extension or report that the conversion is lossy.
19. Canonical Examples
People and places
Alice is([a]doctor) id:101026120101
Alice lived in(Paris) in(1999) id:101026120102
Alice works for(Acme) id:101026120103
Alice depends on([the]team) id:101026120104
Alice was(afraid{of(the_dark)}) id:101026120105
Alice was(proud{of(her_work)}) id:101026120106
Government and policy
Government introduced([new]housing_policy)
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
Nested structural attachments
Alice became(tired{of(sitting)}) id:101026120201
Alice was(afraid{of(the_dark)}) id:101026120202
Alice was(interested{in(science)}) id:101026120203
Alice became(tired{of(sitting{by([her]sister)}{on([the]bank)})}) id:101026120204
20. Core Design Principles
Lingvo v4.4 follows these principles:
Atomicity: one record expresses one primary assertion.
Preservation: original wording and entity modifiers are retained.
Explicit structure: subjects, arguments, and contexts remain identifiable.
Modifier scope: square-bracketed modifiers apply to the immediately following subject or argument.
Argument–context distinction: arguments represent semantic elements associated with predicates; contexts describe circumstances or relations.
Multiword predicates: integral prepositions remain part of the predicate expression.
Structural attachment: curly braces explicitly attach subordinate expressions to the immediately preceding expression.
Nested dependencies: structural attachments may contain further attachments.
Flexible context: prepositions are preserved rather than assigned one universal semantic meaning.
Separate timestamps: event time and record creation time remain distinct.
Stable metadata: identifiers and supported metadata survive conversion.
Graph compatibility: normalized knowledge graphs may be derived without destroying the original representation.
Lossless conversion: serialization preserves all information covered by its contract.
Lingvo is the source representation of extracted semantic information. Knowledge graphs and other output formats are derived representations.
