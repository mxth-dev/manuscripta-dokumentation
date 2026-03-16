# Datenbankschema

Visuelle Übersicht der Datenbankstruktur, aufgeteilt nach thematischen Bereichen. Die vollständige Felddokumentation findet sich in [tabellen.md](tabellen.md).

---

## Handschriften und Sammlungen

Handschriften als zentrales Objekt mit ihrer institutionellen Einbettung, physischen Bänden und Maßangaben.

```mermaid
erDiagram
    manuscripts {
        int id PK
        text title
        text shelfmark
        text code
        text format
        text type
    }
    collections {
        int id PK
        text code
        text name
    }
    institutions {
        int id PK
        text gnd_number
        text name
        text type
    }
    places {
        int id PK
        text gnd_number
        text name
        text type
    }
    volumes {
        int id PK
        int position
    }
    extents {
        int id PK
        text count
        text unit
        bool is_approximate
    }
    dimensions {
        int id PK
        int length
        int width
        bool is_approximate
    }
    collection_complements {
        int id PK
        text original_shelfmark
    }
    manuscript_authors {
        int id PK
        text name
        text type
    }
    licenses {
        int id PK
        text short_title
        text url
    }

    collections ||--o{ manuscripts : "enthält"
    institutions ||--o{ collections : "führt"
    institutions }o--o{ places : "liegt in"
    manuscripts ||--o{ volumes : "hat"
    volumes ||--o{ extents : "hat"
    manuscripts ||--o{ dimensions : "hat"
    manuscripts ||--o{ collection_complements : "auch in"
    collections ||--o{ collection_complements : "ergänzt"
    manuscripts ||--o{ manuscript_authors : "verfasst von"
    collections }o--o| licenses : "Standardlizenz"
```

---

## Kodikologische Beschreibung

Physische und paläographische Eigenschaften der kodikologischen Teile.

```mermaid
erDiagram
    manuscripts {
        int id PK
        text title
        text shelfmark
    }
    parts {
        int id PK
        bool is_fragment
        text part_type
        text folio
        text collation
    }
    scripts {
        int id PK
        text script
        text notation
        bool has_marginalia
        bool has_glosses
        bool has_secret_script
    }
    book_decorations {
        int id PK
        text type
    }
    manuscript_material {
        int id PK
        text materialized_type
        text type
    }
    part_material {
        int id PK
        text materialized_type
        text type
    }
    dates {
        text format
        int value_1
        int value_2
        bool is_certain
        int range_from
        int range_to
        text datable_type
        int datable_id
    }
    languages {
        text iso_code PK
    }
    language_manuscript {
        int manuscript_id FK
        text iso_code FK
    }

    manuscripts ||--o{ parts : "hat"
    manuscripts ||--o{ scripts : "hat"
    manuscripts ||--o{ manuscript_material : "besteht aus"
    manuscripts ||--o{ dates : "datiert"
    parts ||--o{ scripts : "hat"
    parts ||--o{ book_decorations : "hat"
    parts ||--o{ part_material : "besteht aus"
    parts ||--o{ dates : "datiert"
    manuscripts ||--o{ language_manuscript : "hat Sprache"
    languages ||--o{ language_manuscript : "verwendet in"
```

---

## Texte und Überlieferung

Texte als abstrakte Werke und ihre Überlieferung in konkreten Handschriften, bis hin zu den einzelnen Textanfängen.

```mermaid
erDiagram
    texts {
        int id PK
        text title
        text subtitle
        text standard_identifier
    }
    text_witnesses {
        int id PK
        text subheading
        text page_numbers
        int position
    }
    initia {
        int id PK
        text incipit
        text explicit
        text title
        int position
    }
    manuscripts {
        int id PK
        text title
        text shelfmark
    }
    parts {
        int id PK
        text folio
        bool is_fragment
    }
    digital_copies {
        int id PK
        text manifest_url
        text label
    }
    digital_copy_pages {
        int id PK
        text label
        text canvas_id
        int position
    }

    texts ||--o{ text_witnesses : "überliefert in"
    manuscripts ||--o{ text_witnesses : "enthält"
    parts o|--o{ text_witnesses : "enthält"
    text_witnesses ||--o{ initia : "hat"
    manuscripts ||--o{ initia : "hat"
    parts o|--o{ initia : "hat"
    manuscripts ||--o{ digital_copies : "hat"
    digital_copies ||--o{ digital_copy_pages : "hat"
    initia }o--o| digital_copy_pages : "verlinkt auf"
    parts }o--o| digital_copy_pages : "verlinkt auf"
```

---

## Provenienzen

Herkunfts- und Besitzgeschichte von Handschriften und Einbänden, mit Verknüpfung zu Orten, Institutionen und Personen.

```mermaid
erDiagram
    manuscripts {
        int id PK
        text title
        text shelfmark
    }
    provenance_attributions {
        int id PK
        text attributable_type
        text type
        bool is_unknown
        text comment
        int position
    }
    provenances {
        int id PK
        text subject_type
        bool is_certain
        bool has_exlibris
        text logic
        text comment
        int position
    }
    institutions {
        int id PK
        text gnd_number
        text name
    }
    places {
        int id PK
        text gnd_number
        text name
    }
    people {
        int id PK
        text gnd_number
        text name
    }
    collections {
        int id PK
        text name
    }
    olims {
        int id PK
        text shelfmark
        text comment
    }
    dates {
        text format
        int range_from
        int range_to
        bool is_certain
        text datable_type
        int datable_id
    }

    manuscripts ||--o{ provenance_attributions : "hat"
    provenance_attributions ||--o{ provenances : "enthält"
    provenances }o--o| institutions : "Körperschaft"
    provenances }o--o| places : "Ort"
    provenances }o--o| people : "Person"
    provenances }o--o| collections : "Sammlung"
    manuscripts ||--o{ olims : "hat"
    provenance_attributions ||--o{ dates : "datiert"
```

---

## Einband

Einbände mit Schmuck, Merkmalen und Werkstattzuordnung.

```mermaid
erDiagram
    manuscripts {
        int id PK
        text title
        text shelfmark
    }
    bindings {
        int id PK
        text style
        bool has_fragments
        bool is_restored
    }
    binding_decorations {
        int id PK
        text type
    }
    binding_features {
        int id PK
        text type
    }
    binding_workshops {
        int id PK
        text name
        text ebdb_id
    }
    binding_workshop_institution {
        int id PK
        bool is_certain
        text logic
        int position
    }
    binding_workshop_place {
        int id PK
        bool is_certain
        text logic
        int position
    }
    institutions {
        int id PK
        text name
    }
    places {
        int id PK
        text name
    }
    dates {
        text format
        int range_from
        int range_to
        text datable_type
        int datable_id
    }

    manuscripts ||--o{ bindings : "hat"
    bindings ||--o{ binding_decorations : "hat"
    bindings ||--o{ binding_features : "hat"
    bindings ||--o{ dates : "datiert"
    binding_workshops ||--o{ binding_workshop_institution : "hat"
    binding_workshops ||--o{ binding_workshop_place : "hat"
    binding_workshop_institution }o--|| institutions : "Institution"
    binding_workshop_place }o--|| places : "Ort"
```

---

## Normadaten, Bibliographie und Zuordnungen

Personen, Publikationen, externe Ressourcen sowie Listen und Agenten.

```mermaid
erDiagram
    people {
        int id PK
        text gnd_number
        text name
    }
    person_attributions {
        int id PK
        text attributable_type
        text role
        bool is_certain
    }
    publications {
        int id PK
        text short_title
        text title
        int year
        text author
    }
    references {
        int id PK
        text referencable_type
        text volume
        text page_start
        text page_end
    }
    reference_links {
        int id PK
        text url
        text label
    }
    resources {
        int id PK
        text referencable_type
        text type
        text external_id
        text url
    }
    agents {
        int id PK
        text kind
        text name
        text uri
    }
    agent_collection {
        int id PK
        text role
        int sort_index
    }
    collections {
        int id PK
        text name
    }
    lists {
        int id PK
        text name
        text visibility
    }
    list_items {
        int id PK
        text entity_type
        int entity_id
        int position
    }

    people ||--o{ person_attributions : "zugeordnet"
    publications ||--o{ references : "zitiert in"
    references ||--o{ reference_links : "hat"
    agents ||--o{ agent_collection : "beteiligt an"
    collections ||--o{ agent_collection : "hat"
    lists ||--o{ list_items : "enthält"
```
