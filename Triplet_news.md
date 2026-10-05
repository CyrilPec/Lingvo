I am developing a system called Lingvo based on triplets.

Core representation

A triplet is:

SUBJECT → RELATION → OBJECT

Example:

Water → has_formula → H2O

Triplets are the basic units of structured information. They can represent facts, relationships, observations, events, operations, rules, and eventually program instructions.

Important concepts

Facts / axioms

A triplet can represent a known statement:

Earth → has_mass → 5.972e24_kg

I may call these axioms, especially when they are accepted as input knowledge.

Rules

Rules describe how new triplets can be derived from existing triplets.

Example:

X → is_a → Mammal
Mammal → is_a → Animal

Rule:

X → is_a → Mammal AND Mammal → is_a → Animal
→ X → is_a → Animal

The resulting triplet is a derived fact / theorem.

Operations

Triplets can also represent computation:

Operation1 → type → Addition
Operation1 → argument1 → 5
Operation1 → argument2 → 3
Operation1 → result → 8

Thus triplets can describe not only information but also operations that an interpreter can execute.

Parsing and reasoning

The intended architecture is:

Text → parser/LLM → triplets → validation → database → rules/inference → derived triplets

Parsing converts external information into structured triplets.

Inference uses rules to derive new information.

Parsing itself is not the theorem; the derived result is analogous to a theorem.

Books and web information

Books, web pages, and news can be converted into triplets using NLP/LLMs.

For example:

Book/web page → Gemini/NLP → triplets

Every extracted triplet should ideally retain provenance:

subject

relation

object

source

URL or book

page/paragraph when available

publication date

extraction date

evidence

confidence

status

An extracted news statement should normally be treated as a claim, not automatically as an unquestionable fact.

Time-dependent information

For changing information, preserve observations rather than overwriting facts.

Example:

Observation1 → subject → Bitcoin
Observation1 → relation → price
Observation1 → object → 100000_USD
Observation1 → timestamp → 2026-10-05T14:30

The system should be able to represent changes over time.

News knowledge graph

One intended application is automatically collecting current news:

Internet/news → text → Gemini/NLP → triplets → database → graph

Daily triplets should contain source and date, avoid duplicates, and distinguish new information from updates or contradictions.

Example:

CompanyA → plans_to_acquire → CompanyB

later becoming:

CompanyA → acquired → CompanyB

should be represented as a change over time rather than simply replacing the old information.

Database / graph

The initial implementation can use SQLite or PostgreSQL.

Triplets can be queried in any direction:

Water → ? → ?

? → has_formula → H2O

? → ? → Water

The long-term goal is a knowledge graph with inference, operations, provenance, temporal information, and natural-language querying.

Gemini integration

Gemini can be used as a semantic extractor/interpreter, not as the permanent knowledge database.

Possible architecture:

Book/Web page → Gemini → structured JSON triplets → Python → SQLite

The original source remains the evidence.

For a Debian/Linux CNC computer, Python can communicate with the Gemini REST API using HTTP requests without a graphical browser. Python 2.7 may require direct REST/HTTP rather than the current official Python SDK.

Conceptual goal

The long-term idea is:

TRIPLETS = structured information

RULES = inference

OPERATIONS = computation

PARSER = conversion from external information

DATABASE/GRAPH = persistent knowledge

ENGINE = reasoning + execution

The goal is to investigate whether a triplet-based representation can serve as a common foundation for knowledge, reasoning, computation, and eventually programming.