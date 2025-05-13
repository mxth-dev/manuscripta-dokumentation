# Tabellen

## manuscripts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|-|-|-|-|-|
| id | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | – | Primärschlüssel |
| title | VARCHAR(255) | INDEX | – | Kurztitel der Handschrift |
| shelfmark | VARCHAR(255) | INDEX | – | Signatur innerhalb der Sammlung |
| code | VARCHAR(255) | INDEX | – | Handschriftencode |
| format | ENUM | – | – | Format des Codex (z.B. Quart, Folio) |
| type | ENUM | – | – | Überlieferungsform (z.B. Codex, Fragment) |
| codicological_description | TEXT | – | – | Kodikologische Beschreibung (z.B. Lagenformel) |
| historical_description | TEXT | – | – | Freitext zur Überlieferungsgeschichte |
| collection_id | BIGINT UNSIGNED | INDEX | collections | Zugehörige Sammlung |
| created_at| DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## parts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|---------------|-------------------------------|---------|---------------|---------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| manuscript_id | BIGINT UNSIGNED | INDEX | manuscripts | Zugehörige Handschrift |
| is_fragment | BOOLEAN | – | – | Kennzeichnung als Fragment (true/false) |
| title | VARCHAR(255) | INDEX | – | Kennzeichnung des Teils oder Fragments (ehemals: part) |
| folio | VARCHAR(255) | – | – | Freitextfeld für Blatt- oder Seitenangaben |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## texts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------------|-------------------------------|---------|---------------|------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| title | VARCHAR(255) | INDEX | – | Werktitel (Pflichtfeld) |
| standard_identifier| VARCHAR(255) | INDEX | – | Normierter Identifier (z.B. BHL, BHG) (optional) |
| comment | TEXT | – | – | Freitextkommentar zu Inhalt, Fassung oder Edition |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## text_witnesses

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|------------------|-------------------------------|---------|---------------|-----------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| text_id | BIGINT UNSIGNED | INDEX | texts | Verweis auf das überlieferte Werk |
| part_id | BIGINT UNSIGNED | INDEX | parts | Verknüpfung zum entsprechenden Handschriftenteil |
| transmission_type| ENUM | – | – | Art der Überlieferung: Abschrift, Bearbeitung, Auszug oder Urfassung |
| comment | TEXT | – | – | Freitextkommentar zur Überlieferung |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## initia

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------|-------------------------------|---------|---------------|---------------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| manuscript_id | BIGINT UNSIGNED | INDEX | parts | Verknüpfung zur Handschrift |
| text_id | BIGINT UNSIGNED | INDEX | texts | Optionaler Verweis auf zugeordnetes Werk |
| position | INT | INDEX | - | Reihenfolge innerhalb der Handschrift |
| incipit | TEXT | – | – | Beginn des Textabschnitts |
| explicit | TEXT | – | – | Ende des Textabschnitts (optional) |
| title | VARCHAR(255) | – | – | Optionaler Titel oder Titelvorspann |
| folio | VARCHAR(255) | – | – | Seiten- oder Blattangabe (z.B. „24r–25v“) |
| comment | TEXT | – | – | Freitextkommentar zur Einordnung oder Unsicherheiten |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) 

## bindings

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------------|-------------------------------|---------|----------------------|--------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| manuscript_id | BIGINT UNSIGNED | INDEX | manuscripts | Verknüpfte Handschrift |
| style | ENUM | – | – | Stil des Einbands |
| special_features | ENUM | – | – | Besondere Merkmale des Einbands |
| is_restored | BOOLEAN | – | – | Kennzeichnung als restaurierter Einband |
| has_fragments | BOOLEAN | – | – | Gibt an, ob Einbandfragmente enthalten sind |
| workshop_id | BIGINT UNSIGNED | INDEX | bookbinders | Verknüpfung zur Buchbinderwerkstatt |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## digital_copies

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|-------------------|-------------------------------|---------|---------------|-------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| manuscript_id | BIGINT UNSIGNED | INDEX | manuscripts | Verknüpfung zur zugehörigen Handschrift |
| iiif_manifest_url | TEXT | – | – | URL zum IIIF-Manifest |
| thumbnail_url | TEXT | – | – | Direktlink zur ersten Seite (für Vorschaubildanzeige) |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## literature

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|-----------------|-------------------------------|---------|---------------|--------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| is_uniform_title | BOOLEAN | – | – | Gibt an, ob es sich um einen Sachtitel handelt |
| title | TEXT | – | – | Vollständiger Titel der Publikation |
| year | INT(4) | – | – | Erscheinungsjahr |
| short_title | VARCHAR(255) | INDEX | – | Kurzzitat zur internen Referenz (z.B. „Pächt 1969“) |
| type | ENUM | – | – | Publikationstyp als Enum (z.B. „Katalog“, „Monographie“, „Artikel“) |
| comment | TEXT | – | – | Anmerkungen zur Publikation |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## page_numbers

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|---------------|-------------------------------|---------|---------------|--------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| manuscript_id | BIGINT UNSIGNED | INDEX | manuscripts | Verknüpfte Handschrift |
| position | INT | INDEX | – | Reihenfolge der Einheit innerhalb der Zählfolge |
| count | VARCHAR(20) | – | – | Gezählte Einheit (z.B. „12“, „I“, „I*“) |
| unit | ENUM | – | – | Zähleinheit (Blatt oder Seite) |
| comment | TEXT | – | – | Optionaler Kommentar oder Erläuterung |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## dimensions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------------|-------------------------------|---------|---------------|---------------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| manuscript_id | BIGINT UNSIGNED | INDEX | manuscripts | Verknüpfte Handschrift |
| length | SMALLINT UNSIGNED | – | – | Länge (in mm) |
| width | SMALLINT UNSIGNED | – | – | Breite (in mm) |
| is_approximate | BOOLEAN | – | – | Gibt an, ob es sich um eine ungefähre Angabe handelt |
| is_fragment | BOOLEAN | – | – | Gibt an, ob sich die Maße auf ein Fragment beziehen |
| comment | TEXT | – | – | Optionaler Freitextkommentar |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## materials

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------------|-------------------------------|---------|---------------|-----------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| materialized_id | BIGINT UNSIGNED | INDEX | – | ID der verknüpften Entität (z.B. Handschrift oder Teil) |
| materialized_type | VARCHAR(255) | INDEX | – | Typ der verknüpften Entität (z.B. `manuscript`, `part`) |
| type | ENUM | – | – | Beschreibstoff als kontrolliertes Vokabular |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## scripts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------------|-------------------------------|---------|---------------|--------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| part_id | BIGINT UNSIGNED | INDEX | parts | Verknüpfung zum kodikologischen Teil |
| script_type | ENUM | – | – | Schrifttyp (Enum: z.B. Textualis, Bastarda, Capitalis) |
| notation_type | ENUM | – | – | Notationstyp (Enum: z.B. Neumen, Quadratnotation, Mensuralnotation) |
| has_marginalia | BOOLEAN | – | – | Gibt an, ob Marginalien vorhanden sind |
| has_glosses | BOOLEAN | – | – | Gibt an, ob Glossen vorhanden sind |
| has_secret_script | BOOLEAN | – | – | Gibt an, ob Geheimschrift verwendet wurde |
| comment | TEXT | – | – | Freitextkommentar zur Schriftbeschreibung |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## book_decorations

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------|-------------------------------|---------|---------------|--------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| part_id | BIGINT UNSIGNED | INDEX | parts | Verknüpfung zum kodikologischen Teil |
| type | ENUM | – | – | Ausstattungskategorie (Enum: z.B. Miniaturen, Fleuronnée) |
| comment | TEXT | – | – | Optionaler Freitextkommentar |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## text_transmissions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|-----------------|-------------------------------|---------|------------------|--------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| source_id | BIGINT UNSIGNED | INDEX | text_witnesses | Verweis auf die zugrundeliegende Textüberlieferung (Vorlage) |
| target_id | BIGINT UNSIGNED | INDEX | text_witnesses | Verweis auf die abschreibende oder abhängige Textüberlieferung |
| comment | TEXT | – | – | Kommentar zur Art der Abhängigkeit, Unsicherheiten oder Fragmentstatus |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung)

## bookbinders

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------|-------------------------------|---------|----------------|--------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| title | VARCHAR(255) | INDEX | – | Bezeichnung der Buchbinderwerkstatt |
| institution_id | BIGINT UNSIGNED | INDEX | institutions | Verknüpfung zu einer übergeordneten Institution (z.B. Kloster, Bibliothek) |
| external_link | TEXT | – | – | Link zur Einbanddatenbank oder anderen externen Ressourcen |
| comment | TEXT | – | – | Freitextkommentar zu Werkstatt, Quellenlage oder Unsicherheiten |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## binding_decorations

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------|-------------------------------|---------|---------------|--------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| binding_id | BIGINT UNSIGNED | INDEX | bindings | Verknüpfung zum Einband |
| type | ENUM | – | – | Art des Schmucks (kontrolliertes Vokabular) |
| comment | TEXT | – | – | Optionaler Freitextkommentar zur Ausprägung oder Technik |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## dates

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|-----------------|-------------------------------|---------|---------------|--------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| datable_id | BIGINT UNSIGNED | INDEX | – | ID der verknüpften Entität (z.B. Handschrift, Teil, Einband) |
| datable_type | VARCHAR(255) | INDEX | – | Typ der verknüpften Entität (z.B. `manuscript`, `part`, `binding`) |
| format | ENUM | – | – | Formatcode der Datierung (siehe unten) |
| range_from | INT | – | – | Berechneter Datumsbeginn (Jahr) |
| range_to | INT | – | – | Berechnetes Datumsende (Jahr) |
| value_1 | INT | – | – | Erster Wert zur Datierung (z.. Jahrhundert oder Jahr) |
| value_2 | INT | – | – | Zweiter Wert (z.B. Jahrhundert, Viertel etc.), ggf. NULL |
| is_certain | BOOLEAN | – | – | Gibt an, ob die Datierung als gesichert gilt |
| source | TEXT | – | – | Freitext zur Quelle der Datierung (z.B. „Hs. datiert (36v)“) |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## provenances

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------------------|-------------------------------|---------|---------------------------|---------------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| attribution_id | BIGINT UNSIGNED | INDEX | provenance_attributions | Verweis auf die zugehörige Provenienzzuordnung |
| subject_id | BIGINT UNSIGNED | INDEX | – | ID der referenzierten Entität (z.B. Ort, Institution, Sammlung, Person) |
| subject_type | VARCHAR(255) | INDEX | – | Typ der referenzierten Entität (z.B. `institution`, `place`, `person`) |
| is_certain | BOOLEAN | – | – | Gibt an, ob es sich um eine gesicherte Zuordnung handelt |
| comment | TEXT | – | – | Optionaler Freitextkommentar zur Einordnung oder zur Quellenlage |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## provenance_attributions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------------|-------------------------------|---------|---------------|--------------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| attributable_id | BIGINT UNSIGNED | INDEX | – | ID des verknüpften Objekts (z.B. Handschrift, Teil) |
| attributable_type | VARCHAR(255) | INDEX | – | Typ des verknüpften Objekts (z.B. `manuscript`, `part`) |
| logic | ENUM | – | – | Verknüpfung mehrerer Provenienzstellen als kombinierend oder alternativ |
| comment | TEXT | – | – | Freitextkommentar oder Beleg zur Zuordnung |
| date_id | BIGINT UNSIGNED | – | dates | Optionale Verknüpfung zu einer zeitlichen Einordnung |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## olims

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|------------------|-------------------------------|---------|---------------|----------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| manuscript_id | BIGINT UNSIGNED | INDEX | manuscripts | Verknüpfte Handschrift |
| signature | VARCHAR(255) | – | – | Die überlieferte (alte) Signatur der Handschrift |
| comment | TEXT | – | – | Optionaler Kommentar zur Einordnung, Quelle oder historischen Kontext |
| attribution_id | BIGINT UNSIGNED | INDEX | – | ID der verknüpften Institution oder Sammlung |
| attribution_type | VARCHAR(255) | INDEX | – | Typ der verknüpften Quelle (z.B. `institution`, `collection`) |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## people

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------|-------------------------------|---------|---------------|-----------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| name | VARCHAR(255) | INDEX | – | Gebräuchlicher oder bibliographischer Name der Person |
| gnd_number | VARCHAR(50) | INDEX | – | GND-Nummer für Normdatenverknüpfung |
| biographical_description | TEXT | – | – | Optional: biographische Angaben, Lebensdaten, Herkunft etc. |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## languages

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------|-------------------------------|---------|---------------|------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| iso_code | VARCHAR(10) | UNIQUE | – | Normierter ISO-Sprachcode (z.B. `la`, `de`) |
| name | VARCHAR(255) | INDEX | – | Klartextname der Sprache (z.B. „Latein“, „Deutsch“) |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## places

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------|-------------------------------|---------|---------------|-----------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| name | VARCHAR(255) | INDEX | – | Normierter Ortsname (z.B. „Wien“, „Salzburg“, „Cluny“) |
| gnd_number | VARCHAR(50) | INDEX | – | GND-Identifier des Ortsdatensatzes |
| comment | TEXT | – | – | Optionaler Freitextkommentar |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## institutions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------|-------------------------------|---------|---------------|------------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| name | VARCHAR(255) | INDEX | – | Normierter Name der Institution (z.B. „Stift Klosterneuburg“) |
| gnd_number | VARCHAR(50) | INDEX | – | GND-Identifier der Körperschaft |
| place_id | BIGINT UNSIGNED | INDEX | places | Verknüpfung zum Standort |
| comment | TEXT | – | – | Optionaler Freitextkommentar |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## iconclasses

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|------------------|-------------------------------|---------|---------------|--------------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| code | VARCHAR(50) | UNIQUE | – | Iconclass-Code (z.B. „11H“) |
| title | VARCHAR(255) | INDEX | – | Bezeichnung der Bildkategorie (z.B. „Die Heilige Familie“) |
| parent_id | BIGINT UNSIGNED | INDEX | iconclasses | Optional: Verweis auf übergeordnete Kategorie |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## collections

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------|-------------------------------|---------|---------------|-----------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| institution_id| BIGINT UNSIGNED | INDEX | institutions | Verknüpfte Institution (z.B. Stift, Universitätsbibliothek) |
| name | VARCHAR(255) | INDEX | – | Name der Sammlung (z.B. „Stiftsbibliothek“, „Diözesanarchiv“) |
| email | VARCHAR(255) | – | – | Zentrale oder bestandsbezogene Kontaktadresse (optional) |
| website | TEXT | – | – | Link zur offiziellen Webseite oder digitalen Präsenz |
| description | TEXT | – | – | Freitext zur Sammlungsgeschichte, inhaltlichen Ausrichtung oder Besonderheiten |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## groups

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|--------------|-------------------------------|---------|---------------|------------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| title | VARCHAR(255) | INDEX | – | Titel der Sachgruppe (z.B. „Missale“, „Kirchenreformen im 15. Jahrhundert“) |
| description | TEXT | – | – | Optionaler Freitext zur thematischen oder funktionalen Einordnung |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## person_attributions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|------------------|-------------------------------|---------|---------------|---------------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| attributable_id | BIGINT UNSIGNED | INDEX | – | ID der verknüpften Entität (z.B. Teil, Schrift, Ausstattung) |
| attributable_type| VARCHAR(255) | INDEX | – | Typ der verknüpften Entität (z.B. `part`, `script`, `book_decorations`) |
| person_id | BIGINT UNSIGNED | INDEX | people | Verknüpfte Person aus der zentralen Personentabelle |
| role | ENUM | – | – | Funktionsbezeichnung der Person im jeweiligen Kontext |
| is_certain | BOOLEAN | – | – | Gibt an, ob die Zuschreibung als gesichert gilt |
| comment | TEXT | – | – | Freitextkommentar zur Attribution oder Quellenangabe |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## language_manuscripts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------------|-------------------------------|---------|---------------|--------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| manuscript_id | BIGINT UNSIGNED | INDEX | manuscripts | Verknüpfte Handschrift |
| language_id | BIGINT UNSIGNED | INDEX | languages | Zugeordnete Sprache aus der zentralen Sprachentabelle |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## language_literature

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|---------------|-------------------------------|---------|---------------|--------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| literature_id | BIGINT UNSIGNED | INDEX | literatures | Verknüpfte Publikation (Literatur- oder Katalogeintrag) |
| language_id | BIGINT UNSIGNED | INDEX | languages | Zugeordnete Sprache aus der zentralen Sprachentabelle |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## literature_manuscripts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------------|-------------------------------|---------|---------------|-------------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| manuscript_id | BIGINT UNSIGNED | INDEX | manuscripts | Verknüpfte Handschrift |
| literature_id | BIGINT UNSIGNED | INDEX | literatures | Verknüpfte Publikation |
| pages | VARCHAR(50) | – | – | Seitenzahl oder Seitenbereich des Bezugs |
| external_link | TEXT | – | – | Optionaler Direktlink zur digitalen Quelle (z.B. PDF, Online-Edition) |
| comment | TEXT | – | – | Optionaler Freitextkommentar zur Art des Bezugs oder spezifischen Stellen |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## group_manuscripts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|------------------|-------------------------------|---------|----------------|---------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| manuscript_id | BIGINT UNSIGNED | INDEX | manuscripts | Verknüpfte Handschrift |
| group_id | BIGINT UNSIGNED | INDEX | groups | Verknüpfte Sachgruppe |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |

## iconclass_parts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------------|-------------------------------|---------|---------------|-----------------------------------------------------------------------------|
| id | BIGINT UNSIGNED AUTO_INCREMENT| PRIMARY | – | Primärschlüssel |
| part_id | BIGINT UNSIGNED | INDEX | parts | Verknüpfter Handschriftenteil |
| iconclass_id | BIGINT UNSIGNED | INDEX | iconclasses | Zugeordneter Iconclass-Eintrag |
| folio | VARCHAR(50) | – | – | Angabe eines Folios oder Seitenbereichs (z.B. „28r“, „3v–4r“) |
| comment | TEXT | – | – | Optionaler Freitextkommentar zur Darstellung oder Interpretation |
| created_at | DATETIME | – | – | Zeitpunkt der Erstellung |
| deleted_at | DATETIME | INDEX | – | Soft delete (Zeitpunkt der Löschung) |