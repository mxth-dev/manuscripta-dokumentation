# Tabellen

## manuscripts

| Feldname                  | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                      | TEI-msDesc                                                         |
| ------------------------- | ------------------------------ | ------- | ------------ | ---------------------------------------------- | ------------------------------------------------------------------ |
| id                        | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                | msDesc xml:id                                                      |
| title                     | VARCHAR(255)                   | INDEX   | –            | Kurztitel der Handschrift                      | head->title                                                        |
| shelfmark                 | VARCHAR(255)                   | INDEX   | –            | Signatur innerhalb der Sammlung                | msIdentifier->idno                                                 |
| code                      | VARCHAR(255)                   | INDEX   | –            | Handschriftencode                              | msIdentifier ->idno @sortKey                                       |
| format                    | ENUM                           | –       | –            | Format des Codex (z.B. Quart, Folio)           | physDesc->objectDesc->supportDesc->extent->dimensions->dim         |
| type                      | ENUM                           | –       | –            | Überlieferungsform (z.B. Codex, Fragment)      | physDesc->objectDesc @form                                         |
| codicological_description | TEXT                           | –       | –            | Kodikologische Beschreibung (z.B. Lagenformel) | physDesc->objectDesc->supportDesc->collation (formula)             |
| historical_description    | TEXT                           | –       | –            | Freitext zur Überlieferungsgeschichte          | history->summary AND/OR history->origin , provenance, acquistition |
| collection_id             | BIGINT UNSIGNED                | INDEX   | collections  | Zugehörige Sammlung                            | msIdentifier->repository @ref                                      |
| created_at                | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                       | TEI-Header->publicationStmt->date                                  |
| deleted_at                | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)           |                                                                    |

## parts

| Feldname      | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                              | TEI-msDesc                                                                    |
| ------------- | ------------------------------ | ------- | ------------ | ------------------------------------------------------ | ----------------------------------------------------------------------------- |
| id            | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                        | msPart @xml:id                                                                |
| manuscript_id | BIGINT UNSIGNED                | INDEX   | manuscripts  | Zugehörige Handschrift                                 | (msDesc @xml:id)                                                              |
| is_fragment   | BOOLEAN                        | –       | –            | Kennzeichnung als Fragment (true/false)                | msPart->physDesc->objectDesc @form="fragment"                                 |
| title         | VARCHAR(255)                   | INDEX   | –            | Kennzeichnung des Teils oder Fragments (ehemals: part) | msPart->msIdentifier->altIdentifier->idno                                     |
| folio         | VARCHAR(255)                   | –       | –            | Freitextfeld für Blatt- oder Seitenangaben             | msPart->physDesc->objectDesc->supportDesc->extent->measure @type="leavesCount |
| created_at    | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                               |                                                                               |
| deleted_at    | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                   |                                                                               |

## texts

| Feldname            | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                         | TEI-index-texts |
| ------------------- | ------------------------------ | ------- | ------------ | ------------------------------------------------- | --------------- |
| id                  | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                   | bibl xml:id     |
| title               | VARCHAR(255)                   | INDEX   | –            | Werktitel (Pflichtfeld)                           | bibl->title     |
| standard_identifier | VARCHAR(255)                   | INDEX   | –            | Normierter Identifier (z.B. BHL, BHG) (optional)  | bibl->ref       |
| comment             | TEXT                           | –       | –            | Freitextkommentar zu Inhalt, Fassung oder Edition | bibl->note      |
| created_at          | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                          |                 |
| deleted_at          | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)              |                 |

## text_witnesses

| Feldname          | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                            | TEI-msDesc(->msPart)                                    |
| ----------------- | ------------------------------ | ------- | ------------ | -------------------------------------------------------------------- | ------------------------------------------------------- |
| id                | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                      | x                                                       |
| text_id           | BIGINT UNSIGNED                | INDEX   | texts        | Verweis auf das überlieferte Werk                                    | x                                                       |
| part_id           | BIGINT UNSIGNED                | INDEX   | parts        | Verknüpfung zum entsprechenden Handschriftenteil                     | msContents->msItem->filiation->ref @type"altMs" @target |
| transmission_type | ENUM                           | –       | –            | Art der Überlieferung: Abschrift, Bearbeitung, Auszug oder Urfassung | msContents->msItem->filiation @type                     |
| comment           | TEXT                           | –       | –            | Freitextkommentar zur Überlieferung                                  | msContents->msItem->filiation->note                     |
| created_at        | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                             | x                                                       |
| deleted_at        | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                                 | x                                                       |

## initia

| Feldname      | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                            | TEI-msDesc-msPart-msContents-msItem |
| ------------- | ------------------------------ | ------- | ------------ | ---------------------------------------------------- | ----------------------------------- |
| id            | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                      | msItem @xml:id                      |
| manuscript_id | BIGINT UNSIGNED                | INDEX   | parts        | Verknüpfung zur Handschrift                          | x                                   |
| text_id       | BIGINT UNSIGNED                | INDEX   | texts        | Optionaler Verweis auf zugeordnetes Werk             | msItem->title @ref                  |
| position      | INT                            | INDEX   | -            | Reihenfolge innerhalb der Handschrift                | msItem @n                           |
| incipit       | TEXT                           | –       | –            | Beginn des Textabschnitts                            | msItem->incipit                     |
| explicit      | TEXT                           | –       | –            | Ende des Textabschnitts (optional)                   | msItem-> explicit                   |
| title         | VARCHAR(255)                   | –       | –            | Optionaler Titel oder Titelvorspann                  | msItem->title                       |
| folio         | VARCHAR(255)                   | –       | –            | Seiten- oder Blattangabe (z.B. „24r–25v“)            | msItem->locus                       |
| comment       | TEXT                           | –       | –            | Freitextkommentar zur Einordnung oder Unsicherheiten | msItem->note                        |
| created_at    | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                             |                                     |
| deleted_at    | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                 |                                     |

## bindings

| Feldname         | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                   | TEI-msDesc-physDesc-bindingDesc                              |
| ---------------- | ------------------------------ | ------- | ------------ | ------------------------------------------- | ------------------------------------------------------------ |
| id               | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                             | @xml:id                                                      |
| manuscript_id    | BIGINT UNSIGNED                | INDEX   | manuscripts  | Verknüpfte Handschrift                      |                                                              |
| style            | ENUM                           | –       | –            | Stil des Einbands                           | bindingDesc->binding->p->term @style                         |
| special_features | ENUM                           | –       | –            | Besondere Merkmale des Einbands             | bindingDesc->binding->p->term @special_features              |
| is_restored      | BOOLEAN                        | –       | –            | Kennzeichnung als restaurierter Einband     | bindingDesc->binding->conditon->template text                |
| has_fragments    | BOOLEAN                        | –       | –            | Gibt an, ob Einbandfragmente enthalten sind | physDesc->accMat->template text                              |
| workshop_id      | BIGINT UNSIGNED                | INDEX   | bookbinders  | Verknüpfung zur Buchbinderwerkstatt         | bindingDesc->binding->p->ref @type="binding_workshop" target |
| created_at       | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                    |                                                              |
| deleted_at       | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)        |                                                              |

## digital_copies

| Feldname          | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                             | TEI-msDesc                                                                   |
| ----------------- | ------------------------------ | ------- | ------------ | ----------------------------------------------------- | ---------------------------------------------------------------------------- |
| id                | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                       |                                                                              |
| manuscript_id     | BIGINT UNSIGNED                | INDEX   | manuscripts  | Verknüpfung zur zugehörigen Handschrift               |                                                                              |
| iiif_manifest_url | TEXT                           | –       | –            | URL zum IIIF-Manifest                                 | additional->surrogates->listBibl->bibl @type="digital-facsimile"->ref target |
| thumbnail_url     | TEXT                           | –       | –            | Direktlink zur ersten Seite (für Vorschaubildanzeige) | additional->surrogates->listBibl->bibl @type="thumbnail"->ref target         |
| created_at        | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                              |                                                                              |
| deleted_at        | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                  |                                                                              |

## literature

| Feldname         | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                           | TEI-listBibl-biblStruct                 |
| ---------------- | ------------------------------ | ------- | ------------ | ------------------------------------------------------------------- | --------------------------------------- |
| id               | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                     | biblStruct @xml:id                      |
| is_uniform_title | BOOLEAN                        | –       | –            | Gibt an, ob es sich um einen Sachtitel handelt                      | ?                                       |
| title            | TEXT                           | –       | –            | Vollständiger Titel der Publikation                                 | biblStruct->monogr->title               |
| year             | INT(4)                         | –       | –            | Erscheinungsjahr                                                    | biblStruct->monogr->imprint->date       |
| short_title      | VARCHAR(255)                   | INDEX   | –            | Kurzzitat zur internen Referenz (z.B. „Pächt 1969“)                 | biblStruct->monogr->title @type="short" |
| type             | ENUM                           | –       | –            | Publikationstyp als Enum (z.B. „Katalog“, „Monographie“, „Artikel“) | biblStruct @type                        |
| comment          | TEXT                           | –       | –            | Anmerkungen zur Publikation                                         | biblStruct->monogr->imprint->note       |
| created_at       | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                            |                                         |
| deleted_at       | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                                |                                         |

## page_numbers

| Feldname      | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                       |
| ------------- | ------------------------------ | ------- | ------------ | ----------------------------------------------- |
| id            | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                 |
| manuscript_id | BIGINT UNSIGNED                | INDEX   | manuscripts  | Verknüpfte Handschrift                          |
| position      | INT                            | INDEX   | –            | Reihenfolge der Einheit innerhalb der Zählfolge |
| count         | VARCHAR(20)                    | –       | –            | Gezählte Einheit (z.B. „12“, „I“, „I\*“)        |
| unit          | ENUM                           | –       | –            | Zähleinheit (Blatt oder Seite)                  |
| comment       | TEXT                           | –       | –            | Optionaler Kommentar oder Erläuterung           |
| created_at    | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                        |
| deleted_at    | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)            |

## dimensions

| Feldname       | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                            | TEI - msDesc-physDesc                                                  |
| -------------- | ------------------------------ | ------- | ------------ | ---------------------------------------------------- | ---------------------------------------------------------------------- |
| id             | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                      | x                                                                      |
| manuscript_id  | BIGINT UNSIGNED                | INDEX   | manuscripts  | Verknüpfte Handschrift                               | x                                                                      |
| length         | SMALLINT UNSIGNED              | –       | –            | Länge (in mm)                                        | physDesc->objectDesc->supportDesc->extent->dimensions->height          |
| width          | SMALLINT UNSIGNED              | –       | –            | Breite (in mm)                                       | physDesc->objectDesc->supportDesc->extent->dimensions->width           |
| is_approximate | BOOLEAN                        | –       | –            | Gibt an, ob es sich um eine ungefähre Angabe handelt | physDesc->objectDesc->supportDesc->extent->dimensions @rend="circa"    |
| is_fragment    | BOOLEAN                        | –       | –            | Gibt an, ob sich die Maße auf ein Fragment beziehen  | physDesc->objectDesc->supportDesc->extent->dimensions @type="fragment" |
| comment        | TEXT                           | –       | –            | Optionaler Freitextkommentar                         | physDesc->objectDesc->supportDesc->extent->note                        |
| created_at     | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                             |                                                                        |
| deleted_at     | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                 |                                                                        |

## materials

| Feldname          | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                               | TEI-msDesc-physDesc / msPart-physDesc                    |
| ----------------- | ------------------------------ | ------- | ------------ | ------------------------------------------------------- | -------------------------------------------------------- |
| id                | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                         |                                                          |
| materialized_id   | BIGINT UNSIGNED                | INDEX   | –            | ID der verknüpften Entität (z.B. Handschrift oder Teil) |                                                          |
| materialized_type | VARCHAR(255)                   | INDEX   | –            | Typ der verknüpften Entität (z.B. `manuscript`, `part`) |                                                          |
| type              | ENUM                           | –       | –            | Beschreibstoff als kontrolliertes Vokabular             | supportDesc @material and supportDesc->support->material |
| created_at        | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                |                                                          |
| deleted_at        | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                    |                                                          |

## scripts

| Feldname          | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                           | TEI msDesc->mPart->physDesc->                           |
| ----------------- | ------------------------------ | ------- | ------------ | ------------------------------------------------------------------- | ------------------------------------------------------- |
| id                | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                     | scriptDesc @xml:id                                      |
| part_id           | BIGINT UNSIGNED                | INDEX   | parts        | Verknüpfung zum kodikologischen Teil                                |                                                         |
| script_type       | ENUM                           | –       | –            | Schrifttyp (Enum: z.B. Textualis, Bastarda, Capitalis)              | scritDesc->scriptNote @script and ScriptNote->term @ref |
| notation_type     | ENUM                           | –       | –            | Notationstyp (Enum: z.B. Neumen, Quadratnotation, Mensuralnotation) | musicNotation                                           |
| has_marginalia    | BOOLEAN                        | –       | –            | Gibt an, ob Marginalien vorhanden sind                              | additions - template text for true values element term  |
| has_glosses       | BOOLEAN                        | –       | –            | Gibt an, ob Glossen vorhanden sind                                  | additions - template text for true values element term  |
| has_secret_script | BOOLEAN                        | –       | –            | Gibt an, ob Geheimschrift verwendet wurde                           | additions - template text for true values element term  |
| comment           | TEXT                           | –       | –            | Freitextkommentar zur Schriftbeschreibung                           | scriptDesc->scriptNote->note                            |
| created_at        | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                            |                                                         |
| deleted_at        | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                                |                                                         |

## book_decorations

| Feldname   | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                 | TEI decoDesc             |
| ---------- | ------------------------------ | ------- | ------------ | --------------------------------------------------------- | ------------------------ |
| id         | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                           |                          |
| part_id    | BIGINT UNSIGNED                | INDEX   | parts        | Verknüpfung zum kodikologischen Teil                      |                          |
| type       | ENUM                           | –       | –            | Ausstattungskategorie (Enum: z.B. Miniaturen, Fleuronnée) | decoDesc->decoNote @type |
| comment    | TEXT                           | –       | –            | Optionaler Freitextkommentar                              | decoNote                 |
| created_at | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                  |                          |
| deleted_at | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                      |                          |

## text_transmissions

| Feldname   | SQL-Datentyp                   | Index   | Referenziert   | Kommentar                                                              | TEI->msContents->msItem   |
| ---------- | ------------------------------ | ------- | -------------- | ---------------------------------------------------------------------- | ------------------------- |
| id         | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –              | Primärschlüssel                                                        | msItem->filiation @xml:id |
| source_id  | BIGINT UNSIGNED                | INDEX   | text_witnesses | Verweis auf die zugrundeliegende Textüberlieferung (Vorlage)           |                           |
| target_id  | BIGINT UNSIGNED                | INDEX   | text_witnesses | Verweis auf die abschreibende oder abhängige Textüberlieferung         |                           |
| comment    | TEXT                           | –       | –              | Kommentar zur Art der Abhängigkeit, Unsicherheiten oder Fragmentstatus | msItem->filiation->note   |
| created_at | DATETIME                       | –       | –              | Zeitpunkt der Erstellung                                               |                           |
| deleted_at | DATETIME                       | INDEX   | –              | Soft delete (Zeitpunkt der Löschung)                                   |                           |

## bookbinders

| Feldname       | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                                  | TEI - listOrg @type="bookbindersr"                                                    |
| -------------- | ------------------------------ | ------- | ------------ | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| id             | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                            | org @xml:id                                                                           |
| title          | VARCHAR(255)                   | INDEX   | –            | Bezeichnung der Buchbinderwerkstatt                                        | org->orgName                                                                          |
| institution_id | BIGINT UNSIGNED                | INDEX   | institutions | Verknüpfung zu einer übergeordneten Institution (z.B. Kloster, Bibliothek) | org->desc @type="relation"->listRelation->relation @name="parent_institution" @active |
| external_link  | TEXT                           | –       | –            | Link zur Einbanddatenbank oder anderen externen Ressourcen                 | org->bibl->ref @target                                                                |
| comment        | TEXT                           | –       | –            | Freitextkommentar zu Werkstatt, Quellenlage oder Unsicherheiten            | org->desc @type="comment"                                                             |
| created_at     | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                                   |                                                                                       |
| deleted_at     | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                                       |                                                                                       |

## binding_decorations

| Feldname   | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                | TEI - physDesc->bindingDesc->binding |
| ---------- | ------------------------------ | ------- | ------------ | -------------------------------------------------------- | ------------------------------------ |
| id         | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                          | binding->decoNote xml:id             |
| binding_id | BIGINT UNSIGNED                | INDEX   | bindings     | Verknüpfung zum Einband                                  |                                      |
| type       | ENUM                           | –       | –            | Art des Schmucks (kontrolliertes Vokabular)              | decoNote @type                       |
| comment    | TEXT                           | –       | –            | Optionaler Freitextkommentar zur Ausprägung oder Technik | decoNote                             |
| created_at | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                 |                                      |
| deleted_at | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                     |                                      |

## dates

| Feldname     | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                          | TEI -date     |
| ------------ | ------------------------------ | ------- | ------------ | ------------------------------------------------------------------ | ------------- |
| id           | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                    | @xml:id       |
| datable_id   | BIGINT UNSIGNED                | INDEX   | –            | ID der verknüpften Entität (z.B. Handschrift, Teil, Einband)       |               |
| datable_type | VARCHAR(255)                   | INDEX   | –            | Typ der verknüpften Entität (z.B. `manuscript`, `part`, `binding`) |               |
| format       | ENUM                           | –       | –            | Formatcode der Datierung (siehe unten)                             | @type         |
| range_from   | INT                            | –       | –            | Berechneter Datumsbeginn (Jahr)                                    | @from ?       |
| range_to     | INT                            | –       | –            | Berechnetes Datumsende (Jahr)                                      | @to ?         |
| value_1      | INT                            | –       | –            | Erster Wert zur Datierung (z.. Jahrhundert oder Jahr)              | @not_before ? |
| value_2      | INT                            | –       | –            | Zweiter Wert (z.B. Jahrhundert, Viertel etc.), ggf. NULL           | @not_after ?  |
| is_certain   | BOOLEAN                        | –       | –            | Gibt an, ob die Datierung als gesichert gilt                       | @cert         |
| source       | TEXT                           | –       | –            | Freitext zur Quelle der Datierung (z.B. „Hs. datiert (36v)“)       | @resp         |
| created_at   | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                           |               |
| deleted_at   | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                               |               |

## provenances

| Feldname       | SQL-Datentyp                   | Index   | Referenziert            | Kommentar                                                               | TEI-msDesc->history             |
| -------------- | ------------------------------ | ------- | ----------------------- | ----------------------------------------------------------------------- | ------------------------------- |
| id             | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –                       | Primärschlüssel                                                         |                                 |
| attribution_id | BIGINT UNSIGNED                | INDEX   | provenance_attributions | Verweis auf die zugehörige Provenienzzuordnung                          |                                 |
| subject_id     | BIGINT UNSIGNED                | INDEX   | –                       | ID der referenzierten Entität (z.B. Ort, Institution, Sammlung, Person) | placeName/orgName/persName @ref |
| subject_type   | VARCHAR(255)                   | INDEX   | –                       | Typ der referenzierten Entität (z.B. `institution`, `place`, `person`)  | placeName/orgName/persName      |
| is_certain     | BOOLEAN                        | –       | –                       | Gibt an, ob es sich um eine gesicherte Zuordnung handelt                | @cert (low/high)                |
| comment        | TEXT                           | –       | –                       | Optionaler Freitextkommentar zur Einordnung oder zur Quellenlage        | provenance->p                   |
| created_at     | DATETIME                       | –       | –                       | Zeitpunkt der Erstellung                                                |                                 |
| deleted_at     | DATETIME                       | INDEX   | –                       | Soft delete (Zeitpunkt der Löschung)                                    |                                 |

## provenance_attributions

| Feldname          | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                               | TEI-msDesc->history->provenance |
| ----------------- | ------------------------------ | ------- | ------------ | ----------------------------------------------------------------------- | ------------------------------- |
| id                | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                         | provenance @xml:id              |
| attributable_id   | BIGINT UNSIGNED                | INDEX   | –            | ID des verknüpften Objekts (z.B. Handschrift, Teil)                     |                                 |
| attributable_type | VARCHAR(255)                   | INDEX   | –            | Typ des verknüpften Objekts (z.B. `manuscript`, `part`)                 |                                 |
| logic             | ENUM                           | –       | –            | Verknüpfung mehrerer Provenienzstellen als kombinierend oder alternativ | ?                               |
| comment           | TEXT                           | –       | –            | Freitextkommentar oder Beleg zur Zuordnung                              | provenance->p                   |
| date_id           | BIGINT UNSIGNED                | –       | dates        | Optionale Verknüpfung zu einer zeitlichen Einordnung                    | provenance->date                |
| created_at        | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                                |                                 |
| deleted_at        | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                                    |                                 |

## olims

| Feldname         | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                             | TEI-msDesc->msIdentifier->altIdentifier type="former" |
| ---------------- | ------------------------------ | ------- | ------------ | --------------------------------------------------------------------- | ----------------------------------------------------- |
| id               | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                       | @xml:id                                               |
| manuscript_id    | BIGINT UNSIGNED                | INDEX   | manuscripts  | Verknüpfte Handschrift                                                |                                                       |
| signature        | VARCHAR(255)                   | –       | –            | Die überlieferte (alte) Signatur der Handschrift                      | altIdentifier->idno                                   |
| comment          | TEXT                           | –       | –            | Optionaler Kommentar zur Einordnung, Quelle oder historischen Kontext | altIdentifier->note                                   |
| attribution_id   | BIGINT UNSIGNED                | INDEX   | –            | ID der verknüpften Institution oder Sammlung                          | altIdentifier->repository                             |
| attribution_type | VARCHAR(255)                   | INDEX   | –            | Typ der verknüpften Quelle (z.B. `institution`, `collection`)         |                                                       |
| created_at       | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                              |                                                       |
| deleted_at       | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                                  |                                                       |

## people

| Feldname                 | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                   | TEI - index listPerson |
| ------------------------ | ------------------------------ | ------- | ------------ | ----------------------------------------------------------- | ---------------------- |
| id                       | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                             | person @xml:id         |
| name                     | VARCHAR(255)                   | INDEX   | –            | Gebräuchlicher oder bibliographischer Name der Person       | person->persName       |
| gnd_number               | VARCHAR(50)                    | INDEX   | –            | GND-Nummer für Normdatenverknüpfung                         | person->ptr @target    |
| biographical_description | TEXT                           | –       | –            | Optional: biographische Angaben, Lebensdaten, Herkunft etc. | person->note           |
| created_at               | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                    |                        |
| deleted_at               | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                        |                        |

## languages

| Feldname   | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                           | TEI-msDesc                     |
| ---------- | ------------------------------ | ------- | ------------ | --------------------------------------------------- | ------------------------------ |
| id         | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                     |                                |
| iso_code   | VARCHAR(10)                    | UNIQUE  | –            | Normierter ISO-Sprachcode (z.B. `la`, `de`)         | msContents->textLang @mainLang |
| name       | VARCHAR(255)                   | INDEX   | –            | Klartextname der Sprache (z.B. „Latein“, „Deutsch“) | msContents->textLang           |
| created_at | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                            |                                |
| deleted_at | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                |                                |

## places

| Feldname   | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                              | TEI-listPlace                    |
| ---------- | ------------------------------ | ------- | ------------ | ------------------------------------------------------ | -------------------------------- |
| id         | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                        | place xml:id                     |
| name       | VARCHAR(255)                   | INDEX   | –            | Normierter Ortsname (z.B. „Wien“, „Salzburg“, „Cluny“) | place->placeName                 |
| gnd_number | VARCHAR(50)                    | INDEX   | –            | GND-Identifier des Ortsdatensatzes                     | place -> ptr @type="gnd" @target |
| comment    | TEXT                           | –       | –            | Optionaler Freitextkommentar                           | place->note                      |
| created_at | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                               |                                  |
| deleted_at | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                   |                                  |

## institutions

| Feldname   | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                     | TEI-listOrg                  |
| ---------- | ------------------------------ | ------- | ------------ | ------------------------------------------------------------- | ---------------------------- |
| id         | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                               | org @xml:id                  |
| name       | VARCHAR(255)                   | INDEX   | –            | Normierter Name der Institution (z.B. „Stift Klosterneuburg“) | org->orgName                 |
| gnd_number | VARCHAR(50)                    | INDEX   | –            | GND-Identifier der Körperschaft                               | org->ptr @type="gnd" @target |
| place_id   | BIGINT UNSIGNED                | INDEX   | places       | Verknüpfung zum Standort                                      | org->placeName @ref          |
| comment    | TEXT                           | –       | –            | Optionaler Freitextkommentar                                  | org->note                    |
| created_at | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                      |                              |
| deleted_at | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                          |                              |

## iconclasses

| Feldname   | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                  | TEI-term @ref=SKOS URI |
| ---------- | ------------------------------ | ------- | ------------ | ---------------------------------------------------------- | ---------------------- |
| id         | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                            |                        |
| code       | VARCHAR(50)                    | UNIQUE  | –            | Iconclass-Code (z.B. „11H“)                                |                        |
| title      | VARCHAR(255)                   | INDEX   | –            | Bezeichnung der Bildkategorie (z.B. „Die Heilige Familie“) |                        |
| parent_id  | BIGINT UNSIGNED                | INDEX   | iconclasses  | Optional: Verweis auf übergeordnete Kategorie              |                        |
| created_at | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                   |                        |
| deleted_at | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                       |                        |

## collections

| Feldname       | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                                      | TEI-listOrg @type="collections"                                                       |
| -------------- | ------------------------------ | ------- | ------------ | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| id             | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                                | org @xml:id                                                                           |
| institution_id | BIGINT UNSIGNED                | INDEX   | institutions | Verknüpfte Institution (z.B. Stift, Universitätsbibliothek)                    | org->desc @type="relation"->listRelation->relation @name="parent_institution" @active |
| name           | VARCHAR(255)                   | INDEX   | –            | Name der Sammlung (z.B. „Stiftsbibliothek“, „Diözesanarchiv“)                  | org->orgName                                                                          |
| email          | VARCHAR(255)                   | –       | –            | Zentrale oder bestandsbezogene Kontaktadresse (optional)                       | org->desc @type="contact"->email                                                      |
| website        | TEXT                           | –       | –            | Link zur offiziellen Webseite oder digitalen Präsenz                           | org->desc @type="contact"->ptr @type="website" @target                                |
| description    | TEXT                           | –       | –            | Freitext zur Sammlungsgeschichte, inhaltlichen Ausrichtung oder Besonderheiten | org->desc                                                                             |
| created_at     | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                                       |                                                                                       |
| deleted_at     | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                                           |                                                                                       |

## groups

| Feldname    | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                                   | TEI->history->summary->listRelations->relation |
| ----------- | ------------------------------ | ------- | ------------ | --------------------------------------------------------------------------- | ---------------------------------------------- |
| id          | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                             | relation->desc->name @type="group" @xml:id     |
| title       | VARCHAR(255)                   | INDEX   | –            | Titel der Sachgruppe (z.B. „Missale“, „Kirchenreformen im 15. Jahrhundert“) | relation->desc->name @type="group"             |
| description | TEXT                           | –       | –            | Optionaler Freitext zur thematischen oder funktionalen Einordnung           | relation->desc-                                |
| created_at  | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                                    |                                                |
| deleted_at  | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                                        |                                                |

## person_attributions

| Feldname          | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                               | TEI->jeweiliges Element->listRelations->relation |
| ----------------- | ------------------------------ | ------- | ------------ | ----------------------------------------------------------------------- | ------------------------------------------------ |
| id                | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                         | relation @xml:id                                 |
| attributable_id   | BIGINT UNSIGNED                | INDEX   | –            | ID der verknüpften Entität (z.B. Teil, Schrift, Ausstattung)            |                                                  |
| attributable_type | VARCHAR(255)                   | INDEX   | –            | Typ der verknüpften Entität (z.B. `part`, `script`, `book_decorations`) |                                                  |
| person_id         | BIGINT UNSIGNED                | INDEX   | people       | Verknüpfte Person aus der zentralen Personentabelle                     | relation @active                                 |
| role              | ENUM                           | –       | –            | Funktionsbezeichnung der Person im jeweiligen Kontext                   | relation @name                                   |
| is_certain        | BOOLEAN                        | –       | –            | Gibt an, ob die Zuschreibung als gesichert gilt                         | relation @cert (high/low)                        |
| comment           | TEXT                           | –       | –            | Freitextkommentar zur Attribution oder Quellenangabe                    | relation->desc                                   |
| created_at        | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                                |                                                  |
| deleted_at        | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                                    |                                                  |

## language_manuscripts

| Feldname      | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                             | TEI-msDesc-msContents->textLang |
| ------------- | ------------------------------ | ------- | ------------ | ----------------------------------------------------- | ------------------------------- |
| id            | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                       |
| manuscript_id | BIGINT UNSIGNED                | INDEX   | manuscripts  | Verknüpfte Handschrift                                |
| language_id   | BIGINT UNSIGNED                | INDEX   | languages    | Zugeordnete Sprache aus der zentralen Sprachentabelle |
| created_at    | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                              |
| deleted_at    | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                  |

## language_literature

| Feldname      | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                               |
| ------------- | ------------------------------ | ------- | ------------ | ------------------------------------------------------- |
| id            | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                         |
| literature_id | BIGINT UNSIGNED                | INDEX   | literatures  | Verknüpfte Publikation (Literatur- oder Katalogeintrag) |
| language_id   | BIGINT UNSIGNED                | INDEX   | languages    | Zugeordnete Sprache aus der zentralen Sprachentabelle   |
| created_at    | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                |
| deleted_at    | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                    |

## literature_manuscripts

| Feldname      | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                                 | TEI-msDesc->additional->listBibl |
| ------------- | ------------------------------ | ------- | ------------ | ------------------------------------------------------------------------- | -------------------------------- |
| id            | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                           |
| manuscript_id | BIGINT UNSIGNED                | INDEX   | manuscripts  | Verknüpfte Handschrift                                                    |
| literature_id | BIGINT UNSIGNED                | INDEX   | literatures  | Verknüpfte Publikation                                                    |
| pages         | VARCHAR(50)                    | –       | –            | Seitenzahl oder Seitenbereich des Bezugs                                  |
| external_link | TEXT                           | –       | –            | Optionaler Direktlink zur digitalen Quelle (z.B. PDF, Online-Edition)     |
| comment       | TEXT                           | –       | –            | Optionaler Freitextkommentar zur Art des Bezugs oder spezifischen Stellen |
| created_at    | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                                  |
| deleted_at    | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                                      |

## group_manuscripts

| Feldname      | SQL-Datentyp                   | Index   | Referenziert | Kommentar                            | TEI->history->summary->listRelations |
| ------------- | ------------------------------ | ------- | ------------ | ------------------------------------ | ------------------------------------ |
| id            | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                      | relation @xml:id                     |
| manuscript_id | BIGINT UNSIGNED                | INDEX   | manuscripts  | Verknüpfte Handschrift               | relation @type="ms_group"            |
| group_id      | BIGINT UNSIGNED                | INDEX   | groups       | Verknüpfte Sachgruppe                | relation @name                       |
| created_at    | DATETIME                       | –       | –            | Zeitpunkt der Erstellung             |                                      |
| deleted_at    | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung) |                                      |

## iconclass_parts

| Feldname     | SQL-Datentyp                   | Index   | Referenziert | Kommentar                                                        | TEI-term @ref=SKOS URI |
| ------------ | ------------------------------ | ------- | ------------ | ---------------------------------------------------------------- | ---------------------- |
| id           | BIGINT UNSIGNED AUTO_INCREMENT | PRIMARY | –            | Primärschlüssel                                                  | -                      |
| part_id      | BIGINT UNSIGNED                | INDEX   | parts        | Verknüpfter Handschriftenteil                                    | -                      |
| iconclass_id | BIGINT UNSIGNED                | INDEX   | iconclasses  | Zugeordneter Iconclass-Eintrag                                   | -                      |
| folio        | VARCHAR(50)                    | –       | –            | Angabe eines Folios oder Seitenbereichs (z.B. „28r“, „3v–4r“)    | -                      |
| comment      | TEXT                           | –       | –            | Optionaler Freitextkommentar zur Darstellung oder Interpretation | -                      |
| created_at   | DATETIME                       | –       | –            | Zeitpunkt der Erstellung                                         | -                      |
| deleted_at   | DATETIME                       | INDEX   | –            | Soft delete (Zeitpunkt der Löschung)                             | -                      |
