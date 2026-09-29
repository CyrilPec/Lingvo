First Universal Object model.xml
        = ontology / capabilities

LINGVO OBJECT INSTANCE MODEL.xml
        = schema / construction rules

Cat.xml
        = actual object

                    UNIVERSAL OBJECT MODEL
                             │
              defines Model / Instance / Variation
                             │
          ┌──────────────────┼──────────────────┐
          ▼                  ▼                  ▼
      Word Model        Syllable Model      Sound Model
          │                  │                  │
          │                  │                  │
          ▼                  ▼                  ▼
     Word Instances     Syllable Instances   Sound Instances
          │                  │                  │
          └──── contains ────►│                  │
                             └──── contains ────►│

MODEL LEVEL

Word Model
   │
   └── allows → Syllable Model


INSTANCE LEVEL

Word Instance
   │
   └── contains → Syllable Instance


instances contain instances, while models define what containment is allowed and how the contained object can vary.
