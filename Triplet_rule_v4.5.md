#Lingvo Triplet Rule v4.4
1. Purpose
Lingvo Triplets provide a compact, readable representation of semantic information for machine processing, knowledge extraction, and knowledge graph construction.
Lingvo preserves original wording while representing subjects, predicates, arguments, modifiers, subordinate expressions, contextual relations, and metadata.
Core principle: One triplet represents one assertion.
1. Basic Structure
A Lingvo record consists of a subject, a predicate, arguments,  contextual expressions, id.
General form
[article, adjectives, number, modifier]SUBJECT PREDICATE([adverb, modifier]ARGUMENT, ARGUMENT) CONTEXTS() METADATA:
The components are:
SUBJECT — the entity performing, experiencing, or participating in the assertion.
PREDICATE — the action, state, property, or relationship associated with the subject.
ARGUMENTS — objects, entities, values, or other semantic elements associated with the predicate.
STRUCTURAL ATTACHMENTS — subordinate expressions enclosed in curly braces and attached to the immediately preceding expression.
CONTEXTS — additional expressions describing circumstances, such as location, time, manner, cause, or source.
METADATA — information identifying or describing the record, such as its identifier, source, type, or confidence.
Each record must occupy exactly one physical line. Line breaks separate records.
The subject, predicate, and primary argument form the semantic core. Structural attachments, additional arguments, contexts, and metadata provide further information.
Examples:
Alice works in(company) id:1101026114120
Alice lived in(Paris) in(1999) id:101026115425
Alice is([a]doctor) id:101026114336
1. Record Creation Identifier
The id: field identifies the record's creation time. It is metadata, not a semantic argument or the time of the event described.
1.1. Format
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
1.1. Rules
The ID records when the triplet was created.
Creation time must remain distinct from event time.
The ID must be preserved during parsing, serialization, export, and import.
An existing ID must not be silently replaced when a record is edited.
Applications must define how IDs are handled when records are copied or split.
A timestamp with one-second precision does not guarantee uniqueness. An additional identifier or greater precision is required when guaranteed uniqueness is needed.
1. Subjects and Entities
An entity represents a person, organization, object, place, event, concept, measurement, or other identifiable subject of knowledge.
Underscores may represent spaces within compound entity names.
Original entity wording must be preserved unless an explicit transformation is requested.
Examples:
Alice
Bank
markets
storage
1. Entity Modifiers
Square brackets represent modifiers associated with an entity.
General form
[MODIFIER]ENTITY
Modifiers may include articles, adjectives, numbers, and other descriptive expressions.
Multiple modifiers are separated by commas. Modifiers may contain multiple words.
Examples:
[the]student
[British]researchers
[new,housing]policy
[high,energy]prices
[new,battery]technology
[the,young, singular]student
1.1. Modifier scope
A bracketed modifier applies to the immediately following subject or argument unless explicitly specified otherwise.
Modifiers must not be transferred to another entity, predicate, or context without an explicit rule.
Modifier order and original wording must be preserved.
Examples:
[British]researchers developed([new, battery]technology)
Police arrested([two]people)
[three]students read([interesting, old]books)
[the, young, singular]student uses([actively,English]language)
Interpretation:
[British] modifies researchers.
[new, battery] modifies technology.
[two] modifies people.
[three] modifies students.
[interesting, old] modifies books.
1.2. Number and quantity
Numbers inside modifier brackets may represent quantity or another numerical descriptor.
When grammatical number and numerical quantity must be distinguished, implementations may use explicit descriptors such as grammatical_number:singular and quantity:3.
Examples:
Police arrested([two]people)
[three]students arrived
1. Predicates
A predicate expresses an action, state, property, or relationship. It may consist of one word or a multiword expression.
Examples:
Alice works for(company)
Alice is([a]doctor)
Alice depends on([the]team)
Andrew_Bailey warned about(market_instability)
Police arrested([two]people)
1.1. Preserve original wording
Lingvo does not automatically normalize predicates.
A conforming implementation must not automatically:
Change verb tense.
Convert verbs to dictionary forms.
Rename predicates.
Replace original wording with standardized relations.
Merge distinct words into a normalized predicate.
Remove prepositions or grammatical particles.
Original predicate wording must remain available for reconstruction.
1.2. Predicates containing prepositions
A preposition may form an integral part of a multiword predicate.
The expressions works for, depends on, warned about, and looked at may be interpreted as multiword predicates when the preposition is integral to the expression.
The parser must preserve the original wording. It must not automatically rewrite warned about as warned_about.
When the distinction between a multiword predicate and a contextual preposition cannot be resolved reliably, the original expression must be preserved without imposing an unsupported interpretation.
Examples:
Alice works for(Company)
Alice depends on([the]team)
Andrew_Bailey warned about(market_instability)
Alice looked at(Bob)
1. Arguments
Arguments represent objects, entities, concepts, values, events, or other semantic elements directly associated with a predicate.
Arguments may have modifiers.
General form PREDICATE(ARGUMENT)
Examples:
Alice uses(English)
Alice is([a]doctor)
Government introduce([new,housing] policy)
Company faced([legal]problems)
Alice reads([interesting]books)
Police arrested([two]people)
[logistics]companies provide(warehousing, [customs]clearance)
Company faced([legal]problems, [financial]problems)
1.1. Multiple arguments
A predicate may contain multiple comma-separated arguments. Each argument may have independent modifiers.
Argument order must be preserved.
Examples:
[logistics]companies provide(warehousing, customs_clearance)
Company faced([legal]problems, [financial]problems)
Alice gave([the]book, [the]pen) id:101026173024
1. Arguments versus contexts
Arguments and contexts must be distinguished by their syntactic structure and semantic function.
1.1. Arguments
Arguments represent objects or entities directly associated with a predicate and normally appear inside its parentheses.
Example:
Alice uses(English)
Here, English is the argument of uses.
1.1. Contexts
Contexts describe the circumstances under which an assertion applies. They are commonly introduced by prepositions or conjunctions and appear outside the main predicate's argument parentheses.
Examples:
Alice lived in(Paris) in(1999)
Alice worked in(hospital) named(Opori)
Police arrested([two]people) at(airport) in(England)
Contexts may express location, time, manner, cause, reason, source, origin, purpose, condition, or other relations.
1.1. Prepositions and multiword predicates
The presence of a preposition does not automatically make an expression a context.
Compare:
Alice depends on([the]team)
Alice lived in(Paris) in(1999)
Alice arrived at(noon)
In depends on([the]team), depends on is the multiword predicate and team is its argument.
In Alice lived in(Paris) in(1999), the expressions in(Paris) and in(1999) represent location and time contexts.
In Alice arrived at(noon), at(noon) represents the time context.
The parser must preserve the original expression and distinguish predicate arguments from contextual expressions whenever the structure and semantic function permit reliable interpretation.
1. Structural Attachments
Curly braces {...} indicate a subordinate expression structurally attached to the immediately preceding expression.
Structural attachment makes the relationship between an expression and its complement explicit, rather than leaving the complement as a separate context.
1.1. General form
EXPRESSION{RELATION(ARGUMENT)}
The expression inside curly braces belongs to the immediately preceding expression. It must not automatically be interpreted as an independent context of the main predicate.
1.1. Example: emotional state
Alice was(afraid{of([the]dark)}) id:101026120003
Interpretation:
Alice — subject.
was — predicate.
afraid — primary state argument.
{of([the]dark)} — subordinate expression attached to afraid.
the_dark — argument of the relation of.
id:101026120003 — record metadata.
The expression of(the_dark) specifies what Alice was afraid of. It is structurally attached to afraid, not treated as an independent context of was.
In each example, the subordinate expression specifies the complement associated with the preceding state or expression.
1.1. Additional examples
Alice became(tired{of(sitting)}) id:101026120001
Alice was(proud{of(her_work)}) id:101026120002
Alice was(afraid{of([the]dark)}) id:101026120003
Alice was(interested{in(science)}) id:101026120004
Alice was(dependent{on([the]team)}) id:101026120005
1.1. Nested attachments
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
1.1. Structural attachment rules
Curly braces mark structural attachment, not literal quotation.
A braced expression attaches to the immediately preceding expression.
Parentheses inside braces retain their ordinary argument or relation function.
Square brackets retain their modifier function.
Braced expressions may contain further braced expressions.
Each attachment must preserve the original wording of the expression.
Structural attachments must not be silently converted into independent contexts.
An implementation must preserve attachment scope during parsing, serialization, and reconstruction.
If attachment scope is ambiguous, the parser must preserve the original expression and avoid inventing a relationship.
1. Metadata and Provenance
Metadata provides information about a record without changing its semantic core.
The id: field is reserved for the record creation identifier.
Additional metadata may include:
type:[physical]property
domain:materials_science
source:document42
author:researcher
reference:source_identifier
confidence:0.95
status:asserted
condition:[room]temperature
unit:kg
note:[additional]information
Example:
Gold has(density:19.3_g_cm3) type:physical_property domain:materials_science condition:[room]temperature
Metadata keys and values must follow a consistent serialization convention.
Provenance describes the source or origin of an assertion. It does not become an additional semantic element of the assertion.
1. Atomicity
A triplet record should express one primary assertion. Independent assertions should normally be represented as separate records.
Each line represents one primary assertion. Multiple records may describe the same entity.
Structural attachments may express subordinate relationships within one assertion without requiring each subordinate expression to become an independent record.
Examples:
Gold has([cultural]value:wealth) type:cultural_property
Gold is_used in(Jewelry) type:application
Gold is_used in(Electronics) type:application
1. Types, Time, and Measurements
1.1. Types
The optional type: metadata field describes the nature of an assertion.
Examples include identity, classification, physical_property, measurement, event, historical_fact, scientific_fact, economic_indicator, claim, observation, and prediction.
Example:
Gold has([atomic]number:79) type:identity domain:chemistry
Types are descriptive metadata and do not replace the subject, predicate, or arguments.
1.1. Event time
Event time describes when an event occurred, not when its record was created.
Approximate dates may use ~ where supported by the application.
Event-time conventions must remain distinct from the id: creation timestamp.
Examples:
Leonardo_da_Vinci created(Mona_Lisa) in(1503-1519)
German_industrial_orders fell(10.6_percent) period:2026-08
Alice lived in(Paris) in(1999) id:101026093101
1.1. Measurements
Measurements should preserve numerical values and units.
Examples:
Gold has_density([19.3]g_cm3) type:measurement condition:room_temperature
Gold has_melting_point([1064.18]C) type:measurement pressure:1_atm
Values and units must remain interpretable during conversion.
1. Negation, Uncertainty, and Status
Negation must remain explicit and must not be silently removed during processing.
Examples:
Gold does_not_contain(Iron) type:claim
Gold is_not(Magnetic) type:physical_property
Company_X [may]acquire(Company_Y) type:claim status:unconfirmed
Company_X [will]acquire(Company_Y) type:claim status:unknown
Possible status values include confirmed, probable, possible, unconfirmed, disputed, historical, estimated, and predicted.
Uncertainty must not be silently converted into a confirmed fact.
1. Events and Relationships Between Assertions
Events use the same subject–predicate–argument structure as other records.
Examples:
Leonardo_da_Vinci created(Mona_Lisa) type:event time:1503-1519
Mona_Lisa depicts(Lisa_Gherardini) type:representation
Mona_Lisa uses(Sfumato) type:technique
Events may have participants, locations, times, causes, and consequences.
Structural attachments represent dependencies within an assertion. Relationships between independent assertions should be represented separately when an application requires them.
1. Core Design Principles
Triplet follows these principles:
Atomicity: one record expresses one primary assertion.
Preservation: original wording and entity modifiers are retained.
Explicit structure:id, subject, argument, and contexts remain identifiable.
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
