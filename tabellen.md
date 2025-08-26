# Tabellen

## Index
- [binding_decorations](#binding_decorations)
- [binding_features](#binding_features)
- [binding_workshop_institution](#binding_workshop_institution)
- [binding_workshop_place](#binding_workshop_place)
- [binding_workshops](#binding_workshops)
- [bindings](#bindings)
- [book_decorations](#book_decorations)
- [catalogs](#catalogs)
- [collections](#collections)
- [dates](#dates)
- [dimensions](#dimensions)
- [extents](#extents)
- [initia](#initia)
- [institutions](#institutions)
- [language_literature](#language_literature)
- [language_manuscript](#language_manuscript)
- [literature](#literature)
- [manuscript_material](#manuscript_material)
- [manuscripts](#manuscripts)
- [olims](#olims)
- [page_numbers](#page_numbers)
- [part_material](#part_material)
- [parts](#parts)
- [people](#people)
- [person_attributions](#person_attributions)
- [places](#places)
- [provenance_attributions](#provenance_attributions)
- [provenances](#provenances)
- [references](#references)
- [scripts](#scripts)
- [text_witnesses](#text_witnesses)
- [texts](#texts)
- [volumes](#volumes)
## binding_decorations

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | binding->decoNote xml:id |
| binding_id | BIGINT UNSIGNED | INDEX | [bindings](#bindings) | Verknüpfung zum Einband | – |
| type | ENUM | – | – | Art des Schmucks (kontrolliertes Vokabular) | decoNote @type |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## binding_features

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | bindingDesc->binding->p->term @special_features |
| binding_id | BIGINT UNSIGNED | INDEX | bindings | Verknüpfung zum Einband | – |
| type | ENUM | – | – | Typ des Merkmals (z. B. Schließen, Beschläge) | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## binding_workshop_institution

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| binding_workshop_id | BIGINT UNSIGNED | INDEX | [binding_workshops](#binding_workshops) | Verknüpfung zur Tabelle binding_workshops | – |
| institution_id | BIGINT UNSIGNED | INDEX | [institutions](#institutions) | Verknüpfung zur Tabelle institutions | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| is_certain | BOOLEAN | – | – | – | – |
| logic | TEXT | – | – | Art der logischen Verknüpfung mit der vorherigen Angabe (and/or) | – |
| position | INT | INDEX | – | Reihenfolge in der Auflistung | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## binding_workshop_place

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| binding_workshop_id | BIGINT UNSIGNED | INDEX | [binding_workshops](#binding_workshops) | Verknüpfung zur Tabelle binding_workshops | – |
| place_id | BIGINT UNSIGNED | INDEX | [places](#places) | Verknüpfung zur Tabelle places | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| is_certain | BOOLEAN | – | – | – | – |
| logic | TEXT | – | – | Art der logischen Verknüpfung mit der vorherigen Angabe (and/or) | – |
| position | INT | INDEX | – | Reihenfolge in der Auflistung | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## binding_workshops

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| name | TEXT | – | – | Bezeichnung der Buchbinderwerkstatt | – |
| ebdb_id | TEXT | – | – | Verknüpfung zur Einbanddatenbank | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## bindings

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | @xml:id |
| manuscript_id | BIGINT UNSIGNED | INDEX | [manuscripts](#manuscripts) | Verknüpfte Handschrift | – |
| has_fragments | BOOLEAN | – | – | Gibt an, ob Einbandfragmente enthalten sind | physDesc->accMat->template text |
| is_restored | BOOLEAN | – | – | Kennzeichnung als restaurierter Einband | bindingDesc->binding->conditon->template text |
| style | ENUM | – | – | Stil des Einbands | bindingDesc->binding->p->term @style |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## book_decorations

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | – |
| part_id | BIGINT UNSIGNED | INDEX | [parts](#parts) | Verknüpfung zum kodikologischen Teil | – |
| type | ENUM | – | – | Ausstattungskategorie (Enum: z.B. Miniaturen, Fleuronnée) | decoDesc->decoNote @type |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## catalogs

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| literature_id | BIGINT UNSIGNED | INDEX | [literature](#literature) | Verknüpfung zur Tabelle literatures | – |
| collection_id | BIGINT UNSIGNED | INDEX | [collections](#collections) | Verknüpfung zur Tabelle collections | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## collections

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | org @xml:id |
| code | TEXT | – | – | – | – |
| name | VARCHAR(255) | INDEX | – | Name der Sammlung (z.B. „Stiftsbibliothek“, „Diözesanarchiv“) | org->orgName |
| email | VARCHAR(255) | – | – | Zentrale oder bestandsbezogene Kontaktadresse (optional) | org->desc @type="contact"->email |
| url | TEXT | – | – | Link zur offiziellen Webseite oder digitalen Präsenz | org->desc @type="contact"->ptr @type="website" @target |
| description | TEXT | – | – | Freitext zur Sammlungsgeschichte, inhaltlichen Ausrichtung oder Besonderheiten | org->desc |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## dates

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| format | ENUM | – | – | Formatcode der Datierung (siehe unten) | @type |
| value_1 | INT | – | – | Erster Wert zur Datierung (z.. Jahrhundert oder Jahr) | @not_before ? |
| value_2 | INT | – | – | Zweiter Wert (z.B. Jahrhundert, Viertel etc.), ggf. NULL | @not_after ? |
| is_certain | BOOLEAN | – | – | Gibt an, ob die Datierung als gesichert gilt | @cert |
| range_from | INT | – | – | Berechneter Datumsbeginn (Jahr) | @from ? |
| range_to | INT | – | – | Berechnetes Datumsende (Jahr) | @to ? |
| datable_type | VARCHAR(255) | INDEX | – | Typ der verknüpften Entität (z.B. `manuscript`, `part`, `binding`) | – |
| datable_id | BIGINT UNSIGNED | INDEX | – | ID der verknüpften Entität (z.B. Handschrift, Teil, Einband) | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## dimensions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | x |
| manuscript_id | BIGINT UNSIGNED | INDEX | [manuscripts](#manuscripts) | Verknüpfte Handschrift | x |
| length | SMALLINT UNSIGNED | – | – | Länge (in mm) | physDesc->objectDesc->supportDesc->extent->dimensions->height |
| width | SMALLINT UNSIGNED | – | – | Breite (in mm) | physDesc->objectDesc->supportDesc->extent->dimensions->width |
| position | INT | INDEX | - | Reihenfolge in der Auflistung | – |
| is_approximate | BOOLEAN | – | – | Gibt an, ob es sich um eine ungefähre Angabe handelt | physDesc->objectDesc->supportDesc->extent->dimensions @rend="circa" |
| is_length_extrapolated | BOOLEAN | – | – | – | – |
| is_width_extrapolated | BOOLEAN | – | – | – | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## extents

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| volume_id | BIGINT UNSIGNED | INDEX | [volumes](#volumes) | Verknüpfung zur Tabelle volumes | – |
| count | TEXT | – | – | Anzahl der Blätter/Seiten etc. | – |
| unit | ENUM | – | – | Einheit (Blatt, Folio, etc.; kontrolliertes Vokabular) | – |
| position | INT | INDEX | – | Reihenfolge in der Auflistung | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| is_approximate | BOOLEAN | – | – | – | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## initia

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | msItem @xml:id |
| manuscript_id | BIGINT UNSIGNED | INDEX | [manuscripts](#manuscripts) | Verknüpfung zur Handschrift | x |
| part_id | BIGINT UNSIGNED | INDEX | [parts](#parts) | Verknüpfung zum Handschriftenteil | x |
| text_witness_id | BIGINT UNSIGNED | INDEX | [text_witnesses](#text_witnesses) | Verknüpfung zur Tabelle text_witnesss | – |
| title | VARCHAR(255) | – | – | Optionaler Titel oder Titelvorspann | msItem->title |
| incipit | TEXT | – | – | Beginn des Textabschnitts | msItem->incipit |
| explicit | TEXT | – | – | Ende des Textabschnitts (optional) | msItem-> explicit |
| bibliography | TEXT | – | – | – | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## institutions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | org @xml:id |
| gnd_number | VARCHAR(50) | INDEX | – | GND-Identifier der Körperschaft | org->ptr @type="gnd" @target |
| name | VARCHAR(255) | INDEX | – | Normierter Name der Institution (z.B. „Stift Klosterneuburg“) | org->orgName |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## language_literature

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| literature_id | BIGINT UNSIGNED | INDEX | [literature](#literature) | Verknüpfte Publikation (Literatur- oder Katalogeintrag) | – |
| language_id | BIGINT UNSIGNED | INDEX | [languages](#languages) | Zugeordnete Sprache aus der zentralen Sprachentabelle | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## language_manuscript

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| manuscript_id | BIGINT UNSIGNED | INDEX | [manuscripts](#manuscripts) | Verknüpfung zur Tabelle manuscripts | – |
| language_id | BIGINT UNSIGNED | INDEX | [languages](#languages) | Verknüpfung zur Tabelle languages | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## literature

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | biblStruct @xml:id |
| title | TEXT | – | – | Vollständiger Titel der Publikation | biblStruct->monogr->title |
| is_uniform_title | BOOLEAN | – | – | Gibt an, ob es sich um einen Sachtitel handelt | – |
| short_title | VARCHAR(255) | INDEX | – | Kurzzitat zur internen Referenz (z.B. „Pächt 1969“) | biblStruct->monogr->title @type="short" |
| year | INT(4) | – | – | Erscheinungsjahr | biblStruct->monogr->imprint->date |
| comment | TEXT | – | – | Anmerkungen zur Publikation | biblStruct->monogr->imprint->note |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## manuscript_material

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| materialized_type | TEXT | INDEX | – | Typ polymorpher Beziehung | – |
| materialized_id | BIGINT UNSIGNED | INDEX | – | Verknüpfung zur Tabelle materializeds | – |
| type | ENUM | – | – | Art des Materials (kontrolliertes Vokabular) | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## manuscripts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | msDesc xml:id |
| title | VARCHAR(255) | INDEX | – | Kurztitel der Handschrift | head->title |
| shelfmark | VARCHAR(255) | INDEX | – | Signatur innerhalb der Sammlung | msIdentifier->idno |
| code | VARCHAR(255) | INDEX | – | Handschriftencode | msIdentifier ->idno @sortKey |
| format | ENUM | – | – | Format des Codex (z.B. Quart, Folio) | physDesc->objectDesc->supportDesc->extent->dimensions->dim |
| type | ENUM | – | – | Überlieferungsform (z.B. Codex, Fragment) | physDesc->objectDesc @form |
| codicological_description | TEXT | – | – | Kodikologische Beschreibung (z.B. Lagenformel) | physDesc->objectDesc->supportDesc->collation (formula) |
| historical_description | TEXT | – | – | Freitext zur Überlieferungsgeschichte | history->summary AND/OR history->origin , provenance, acquistition |
| collection_id | BIGINT UNSIGNED | INDEX | [collections](#collections) | Zugehörige Sammlung | msIdentifier->repository @ref |
| comments | TEXT | – | – | Freitextkommentar | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | TEI-Header->publicationStmt->date |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## olims

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | @xml:id |
| manuscript_id | BIGINT UNSIGNED | INDEX | [manuscripts](#manuscripts) | Verknüpfte Handschrift | – |
| shelfmark | VARCHAR(255) | – | – | Die überlieferte (alte) Signatur der Handschrift | altIdentifier->idno |
| comment | TEXT | – | – | Optionaler Kommentar zur Einordnung, Quelle oder historischen Kontext | altIdentifier->note |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## page_numbers

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | – |
| locatable_type | TEXT | INDEX | – | Typ polymorpher Beziehung | – |
| locatable_id | BIGINT UNSIGNED | INDEX | – | ID der polymorphen Entität | – |
| from | TEXT | – | – | Von-Angabe | – |
| to | TEXT | – | – | Bis-Angabe | – |
| position | INT | INDEX | – | Reihenfolge der Einheit innerhalb der Zählfolge | – |
| from_key | TEXT | – | – | Schlüssel fūr Vergleich mit anderen Seiten-/Folioangaben und Sortierung | – |
| to_key | TEXT | – | – | Schlüssel fūr Vergleich mit anderen Seiten-/Folioangaben und Sortierung | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## part_material

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| materialized_type | TEXT | INDEX | – | Typ polymorpher Beziehung | – |
| materialized_id | BIGINT UNSIGNED | INDEX | – | Verknüpfung zur Tabelle materializeds | – |
| type | ENUM | – | – | Art des Materials (Kontrolliertes Vokabular) | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## parts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | msPart @xml:id |
| manuscript_id | BIGINT UNSIGNED | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift | (msDesc @xml:id) |
| is_fragment | BOOLEAN | – | – | Kennzeichnung als Fragment (true/false) | msPart->physDesc->objectDesc @form="fragment" |
| folio | VARCHAR(255) | – | – | Freitextfeld für Blatt- oder Seitenangaben | msPart->physDesc->objectDesc->supportDesc->extent->measure @type="leavesCount |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## people

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | person @xml:id |
| gnd_number | VARCHAR(50) | INDEX | – | GND-Nummer für Normdatenverknüpfung | person->ptr @target |
| name | VARCHAR(255) | INDEX | – | Gebräuchlicher oder bibliographischer Name der Person | person->persName |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## person_attributions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | relation @xml:id |
| attributable_id | BIGINT UNSIGNED | INDEX | – | ID der verknüpften Entität (z.B. Teil, Schrift, Ausstattung) | – |
| attributable_type | VARCHAR(255) | INDEX | – | Typ der verknüpften Entität (z.B. `part`, `script`, `book_decorations`) | – |
| person_id | BIGINT UNSIGNED | INDEX | [people](#people) | Verknüpfte Person aus der zentralen Personentabelle | relation @active |
| role | ENUM | – | – | Funktionsbezeichnung der Person im jeweiligen Kontext | relation @name |
| is_certain | BOOLEAN | – | – | Gibt an, ob die Zuschreibung als gesichert gilt | relation @cert (high/low) |
| comments | TEXT | – | – | Freitextkommentar | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## places

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | place xml:id |
| gnd_number | VARCHAR(50) | INDEX | – | GND-Identifier des Ortsdatensatzes | place -> ptr @type="gnd" @target |
| name | VARCHAR(255) | INDEX | – | Normierter Ortsname (z.B. „Wien“, „Salzburg“, „Cluny“) | place->placeName |
| lat | FLOAT | – | – | Breitengrad | – |
| lon | FLOAT | – | – | Längengrad | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## provenance_attributions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | provenance @xml:id |
| attributable_type | VARCHAR(255) | INDEX | – | Typ des verknüpften Objekts (z.B. `manuscript`, `part`) | – |
| attributable_id | BIGINT UNSIGNED | INDEX | – | ID des verknüpften Objekts (z.B. Handschrift, Teil) | – |
| type | ENUM | – | – | Art der Provenienzzuschreibung (z.B. Entstehungsort; kontrolliertes Vokabular) | – |
| is_unknown | BOOLEAN | – | – | Für den Fall einer "unbekannt"-Angabe | – |
| comment | TEXT | – | – | Freitextkommentar oder Beleg zur Zuordnung | provenance->p |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## provenances

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | – |
| attribution_id | BIGINT UNSIGNED | INDEX | [provenance_attributions](#provenance_attributions) | Verweis auf die zugehörige Provenienzzuordnung | – |
| subject_id | BIGINT UNSIGNED | INDEX | – | ID der referenzierten Entität (z.B. Ort, Institution, Sammlung, Person) | placeName/orgName/persName @ref |
| subject_type | VARCHAR(255) | INDEX | – | Typ der referenzierten Entität (z.B. `institution`, `place`, `person`) | placeName/orgName/persName |
| logic | TEXT | – | – | – | – |
| is_certain | BOOLEAN | – | – | Gibt an, ob es sich um eine gesicherte Zuordnung handelt | @cert (low/high) |
| comment | TEXT | – | – | Optionaler Freitextkommentar zur Einordnung oder zur Quellenlage | provenance->p |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## references

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| referencable_id | BIGINT UNSIGNED | INDEX | – | Verknüpfung zur Tabelle referencables | – |
| referencable_type | TEXT | INDEX | – | Typ polymorpher Beziehung | – |
| literature_id | BIGINT UNSIGNED | INDEX | [literature](#literature) | Verknüpfung zur Tabelle literatures | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## scripts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | scriptDesc @xml:id |
| part_id | BIGINT UNSIGNED | INDEX | [parts](#parts) | Verknüpfung zum kodikologischen Teil | – |
| manuscript_id | BIGINT UNSIGNED | INDEX | [manuscripts](#manuscripts) | Verknüpfung zur Tabelle manuscripts | – |
| script | ENUM | – | – | Schrifttyp (Enum: z.B. Textualis, Bastarda, Capitalis) | scritDesc->scriptNote @script and ScriptNote->term @ref |
| notation | ENUM | – | – | Notationstyp (Enum: z.B. Neumen, Quadratnotation, Mensuralnotation) | musicNotation |
| has_marginalia | BOOLEAN | – | – | Gibt an, ob Marginalien vorhanden sind | additions - template text for true values element term |
| has_secret_script | BOOLEAN | – | – | Gibt an, ob Geheimschrift verwendet wurde | additions - template text for true values element term |
| columns | TEXT | – | – | – | – |
| rows | TEXT | – | – | – | – |
| width | TEXT | – | – | – | – |
| height | TEXT | – | – | – | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## text_witnesses

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | x |
| text_id | BIGINT UNSIGNED | INDEX | [texts](#texts) | Verweis auf das überlieferte Werk | x |
| part_id | BIGINT UNSIGNED | INDEX | [parts](#parts) | Verknüpfung zum entsprechenden Handschriftenteil | msContents->msItem->filiation->ref @type"altMs" @target |
| manuscript_id | BIGINT UNSIGNED | INDEX | [manuscripts](#manuscripts) | Verknüpfung zur Tabelle manuscripts | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| position | INT | INDEX | - | Reihenfolge innerhalb der Handschrift | x |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | x |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | x |

## texts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel | bibl xml:id |
| title | VARCHAR(255) | INDEX | – | Werktitel (Pflichtfeld) | bibl->title |
| subtitle | TEXT | – | – | – | – |
| standard_identifier | VARCHAR(255) | INDEX | – | Normierter Identifier (z.B. BHL) (optional) | bibl->ref |
| bibliography | TEXT | – | – | Bibliographie zu einem Text | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |

## volumes

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | BIGINT UNSIGNED | INDEX | – | Primärschlüssel | – |
| manuscript_id | BIGINT UNSIGNED | INDEX | [manuscripts](#manuscripts) | Verknüpfung zur Tabelle manuscripts | – |
| position | INT | INDEX | – | Reihenfolge in der Auflistung | – |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung | – |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) | – |
