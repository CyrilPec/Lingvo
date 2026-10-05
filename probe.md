```mermaid
graph LR
    Force -->|causes| Acceleration
    Force -->|has_unit| Newton
    Force -->|depends_on| Mass
    Acceleration -->|depends_on| Time
```
```mermaid
graph LR
    Force -->|is_a| PhysicalQuantity
    Force -->|is_a| Vector
    Force -->|has_unit| Newton
    Force -->|causes| Acceleration
    Force -->|depends_on| Mass

    Acceleration -->|has_unit| "m/s²"
    Acceleration -->|depends_on| Time

    NewtonSecondLaw -->|represented_by| "$F = ma$"
    NewtonSecondLaw -->|depends_on| Mass
    NewtonSecondLaw -->|depends_on| Acceleration
```
