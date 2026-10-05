Lingvo — News Triplets v2
Core format

Every news statement is represented as exactly three tokens:

SUBJECT RELATION OBJECT


Each of the three fields must be one token.

Multi-word concepts use _.

Example:

Japanese_families face higher_living_costs


Not:

Japanese families face higher living costs


Not:

Japanese_families face higher living costs


The first three tokens are always:

SUBJECT
RELATION
OBJECT


Additional information follows after |.

SUBJECT RELATION OBJECT | key: value | key: value


Example:

Japanese_families face higher_living_costs | time: 2026-10-05 | cause: inflation | source: Reuters

One-token rule
Subject

A subject must be one token.

Japan
Japanese_families
Microsoft
Ukrainian_workers
IMO
container_shipping
Tokyo_residents

Relation

A relation must be one token.

faces
acquires
launches
increases
decreases
affects
causes
plans
reports
experiences
investigates


If a relation would normally contain multiple words, combine them with _.

takes_over
plans_to_acquire
is_affected_by
calls_for
increases_pressure_on

Object

An object must be one token.

inflation
higher_living_costs
company_X
energy_prices
housing_shortage
maritime_security
electricity_disruptions

Metadata

Metadata is separated from the triplet using |.

SUBJECT RELATION OBJECT | key: value | key: value


Metadata values may contain multiple words if needed, but preferably use _ for consistency.

Example:

Ukrainian_families prepare difficult_winter | time: 2026-10-05 | concern: electricity_heating_security | source: Reuters


Recommended metadata:

time
date
period
location
country
number
amount
percentage
cause
reason
consequence
status
source
url
publication_date
extraction_date
confidence
topic
category
context
previous
next

News granularity

The system should mix atomic and story-level triplets.

The important requirement is that all three core fields remain single tokens.

Atomic
Microsoft acquires company_X

Event
Microsoft launches AI_product

Situation
Japanese_families face inflation_pressure

Human situation
Ukrainian_families prepare difficult_winter

Industry situation
Shipping_companies avoid dangerous_routes

Trend
Software_companies increase AI_spending


These are all valid because each has exactly:

SUBJECT RELATION OBJECT

Story-level triplets

A story-level triplet is allowed and encouraged when it provides more useful meaning.

Instead of:

Japanese_families face prices


prefer:

Japanese_families face higher_living_costs


Instead of:

Software_companies use AI


prefer:

Software_companies increase AI_adoption


Instead of:

Seafarers face danger


prefer:

Seafarers face maritime_security_risk


The goal is not maximum atomicity.

The goal is a useful semantic unit.

Avoid over-atomization

Do not split every meaningful concept into tiny fragments.

Bad:

Japan has inflation

Japan has prices

Japan has wages

Japan has consumers


Better:

Japanese_families face higher_living_costs | time: 2026-10-05 | cause: inflation | source: Reuters


Then add important supporting triplets:

Japanese_wages increase 1.9_percent | time: 2026-10-05 | source: Jiji

Japanese_consumers reduce spending | time: 2026-10-05 | cause: higher_living_costs | source: Reuters


The result captures both the story and its components.

Avoid oversized concepts

Do not make the object an entire sentence.

Bad:

Japan faces government_policy_that_may_reduce_food_taxes_for_two_years


Better:

Japan considers food_tax_reduction | duration: 2_years | status: proposed | source: Reuters


The object should represent a recognizable concept, event, or situation.

Semantic quality

The triplet should be understandable without requiring the original article.

Good:

Shipping_companies avoid Red_Sea_routes


Better than:

Shipping_companies change routes


Even better when supported by the source:

Shipping_companies avoid Red_Sea_routes | reason: security_risk | consequence: longer_voyages


The triplet provides the main meaning.

Metadata provides the detail.

Relations

Relations should describe the semantic connection between subject and object.

Common relations:

has
is
uses
owns
operates
launches
develops
acquires
sells
buys
increases
decreases
causes
affects
faces
experiences
supports
opposes
investigates
reports
announces
plans
expects
denies
confirms
cancels
replaces
expands
reduces
joins
leaves
attacks
protects
regulates


Multi-word relations use _.

Examples:

plans_to_acquire
calls_for
takes_over
is_affected_by
comes_under_pressure

Entity naming

Entities should normally be represented using a stable readable token.

Examples:

United_States
United_Kingdom
European_Union
Japan
South_Korea
Microsoft
OpenAI
Bank_of_Japan
International_Maritime_Organization


Avoid unnecessary punctuation.

Prefer:

Bank_of_Japan


over:

Bank-of-Japan


Prefer:

Strait_of_Hormuz


over:

Strait-of-Hormuz

Numbers

Numbers that are part of the semantic object can also be represented as one token.

Example:

Japan_records inflation_rate | percentage: 3.1_percent | time: 2026-10


If the number is supporting information, keep it in metadata.

Preferred:

Japanese_wages increase | percentage: 1.9_percent | year: 2026


rather than:

Japanese_wages increase 1.9_percent


This keeps the core triplet semantically clean.

Time

Time belongs in metadata unless time itself is the object.

Example:

Japanese_families face inflation_pressure | time: 2026-10-05


Historical development:

Company_A announces acquisition | time: 2026-01-10 | status: planned


Later:

Company_A completes acquisition | time: 2026-06-15 | status: completed


Both statements should remain.

Claims and status

A news triplet represents information reported by a source.

It should not automatically be treated as an absolute fact.

Example:

Company_A plans_to_acquire Company_B | status: reported | source: Reuters


Later:

Company_A cancels acquisition | status: confirmed | source: Bloomberg


Useful status values:

reported
announced
planned
proposed
confirmed
completed
denied
disputed
estimated
expected
ongoing
cancelled

Provenance

Every news triplet should preserve its source when possible.

Example:

IMO warns seafarer_risk | time: 2026-10-05 | source: IMO | publication_date: 2026-10-05


Recommended provenance:

source
url
publication_date
extraction_date
confidence
status


The triplet is the structured representation.

The source is the evidence.

Human perspective

Country news should not consist only of government and institutional actions.

Include meaningful situations involving:

families
workers
students
consumers
residents
farmers
patients
migrants
children
elderly_people
sailors
small_businesses
local_communities


Examples:

Israeli_families face higher_living_costs

Ukrainian_students experience school_disruption

Japanese_workers receive higher_wages

Seafarers face maritime_security_risk


These give the knowledge graph a broader representation of real life.

Industry perspective

Industry news should combine different levels.

For maritime news:

IMO issues safety_guidance
Shipping_companies avoid dangerous_routes
Seafarers face security_risk
Ports experience congestion
Tanker_rates increase
Shipowners order alternative_fuel_vessels


These can all describe different aspects of the same story.

Multiple triplets from one story

A single article can generate multiple related triplets.

Example:

Shipping_companies avoid Red_Sea_routes | reason: security_risk | source: Reuters

Ships take longer_routes | route: Cape_of_Good_Hope | source: Reuters

Longer_routes increase fuel_consumption | source: Reuters

Longer_routes increase shipping_costs | source: Reuters


Each triplet has exactly three tokens in the semantic core.

Together they describe the story.

Cause and consequence

When supported by the source, related triplets can describe a chain.

Example:

Russia attacks Ukrainian_infrastructure

Ukrainian_infrastructure suffers damage

Ukrainian_families face electricity_disruptions

Ukrainian_families prepare difficult_winter


The graph can therefore represent:

event
  ↓
damage
  ↓
consequence
  ↓
human_situation

Source diversity

Use multiple sources where possible.

Examples:

Reuters
AP
AFP
BBC
Bloomberg
Financial_Times
Guardian
local_news
national_news_agencies
government_sources
company_sources
scientific_sources
IMO
specialist_industry_sources


Different sources may describe different aspects of the same event.

Do not discard a source merely because another source covers the same event.

Duplicate and related reports

Two sources can describe the same underlying event.

Example:

Microsoft launches AI_product | source: Reuters

Microsoft launches AI_product | source: Bloomberg


These may later be linked to the same event.

The individual provenance should remain.

Contradictions

Contradictory reports should remain visible.

Example:

Government reports inflation_decline | source: Source_A

Union reports inflation_increase | source: Source_B


Do not silently choose one.

Store both claims and their provenance.

Mermaid

The three-token core can be converted directly into a graph.

Input:

Microsoft acquires company_X | time: 2026-10-05 | value: 2_billion_USD


Semantic core:

Microsoft
acquires
company_X


Possible Mermaid:

Microsoft -->|acquires| company_X


Metadata can remain outside the graph or be added to the visualization when useful.

Parsing

A parser can conceptually perform:

split line at "|"


Then:

first section = triplet

remaining sections = metadata


The first section contains exactly three tokens:

SUBJECT
RELATION
OBJECT


Example:

Japanese_families face higher_living_costs | time: 2026-10-05 | source: Reuters


Parser result:

subject  = Japanese_families
relation = face
object   = higher_living_costs


Metadata:

time   = 2026-10-05
source = Reuters


This makes the format simple to parse while preserving rich information.

Quality rule

A good news triplet should satisfy all of the following:

exactly three semantic tokens

subject is one token

relation is one token

object is one token

multi-word concepts use `_`

metadata follows `|`

important context is preserved

the triplet remains understandable

provenance is preserved

time is preserved

claims are not confused with confirmed facts


The goal is:

one-token structure

meaningful semantics

rich context

simple parsing

useful graph representation

Final format

The canonical format is:

SUBJECT RELATION OBJECT | key: value | key: value | key: value


Example:

Japanese_families face higher_living_costs | time: 2026-10-05 | cause: inflation_energy_prices | source: Reuters


Another example:

Shipping_companies avoid Red_Sea_routes | time: 2026-10-05 | reason: maritime_security_risk | consequence: longer_voyages | source: Reuters


Another:

Ukrainian_families prepare difficult_winter | time: 2026-10-05 | concerns: electricity_heating_security | source: Reuters


The semantic core is always:

SUBJECT RELATION OBJECT


Everything after | enriches the triplet without changing its fundamental structure.