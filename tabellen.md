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
- [iconclasses](#iconclasses)
- [iconclass_parts](#iconclass_parts)
- [initia](#initia)
- [institutions](#institutions)
- [languages](#languages)
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
| id | INTEGER | PRIMARY | – | Primärschlüssel | binding->decoNote xml:id |
| binding_id | INTEGER | INDEX | [bindings](#bindings) | Verknüpfung zum Einband | – |
| type | TEXT | – | – | Art des Schmucks (kontrolliertes Vokabular) | decoNote @type |

## binding_features

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | bindingDesc->binding->p->term @special_features |
| binding_id | INTEGER | INDEX | [bindings](#bindings) | Verknüpfung zum Einband | – |
| type | TEXT | – | – | Typ des Merkmals (z. B. Schließen, Beschläge) | – |

## binding_workshop_institution

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | INDEX | – | Primärschlüssel | – |
| binding_workshop_id | INTEGER | INDEX | [binding_workshops](#binding_workshops) | Verknüpfung zur Tabelle [binding_workshops](#binding_workshops) | – |
| institution_id | INTEGER | INDEX | [institutions](#institutions) | Verknüpfung zur Tabelle [institutions](#institutions) | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| is_certain | BOOLEAN | – | – | – | – |
| logic | TEXT | – | – | Art der logischen Verknüpfung mit der vorherigen Angabe (and/or) | – |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung | – |

## binding_workshop_place

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | INDEX | – | Primärschlüssel | – |
| binding_workshop_id | INTEGER | INDEX | [binding_workshops](#binding_workshops) | Verknüpfung zur Tabelle [binding_workshops](#binding_workshops) | – |
| place_id | INTEGER | INDEX | [places](#places) | Verknüpfung zur Tabelle [places](#places) | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| is_certain | BOOLEAN | – | – | – | – |
| logic | TEXT | – | – | Art der logischen Verknüpfung mit der vorherigen Angabe (and/or) | – |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung | – |

## binding_workshops

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | INDEX | – | Primärschlüssel | – |
| name | TEXT | – | – | Bezeichnung der Buchbinderwerkstatt | – |
| ebdb_id | TEXT | – | – | Verknüpfung zur Einbanddatenbank | – |

## bindings

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | @xml:id |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Verknüpfte Handschrift | – |
| has_fragments | BOOLEAN | – | – | Gibt an, ob Einbandfragmente enthalten sind | physDesc->accMat->template text |
| is_restored | BOOLEAN | – | – | Kennzeichnung als restaurierter Einband | bindingDesc->binding->conditon->template text |
| style | TEXT | – | – | Stil des Einbands (kontrolliertes Vokabular) | bindingDesc->binding->p->term @style |

## book_decorations

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | – |
| part_id | INTEGER | INDEX | [parts](#parts) | Verknüpfung zum kodikologischen Teil | – |
| type | TEXT | – | – | Ausstattungskategorie (kontrolliertes Vokabular, z. B. Miniaturen, Fleuronnée) | decoDesc->decoNote @type |

## catalogs

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | INDEX | – | Primärschlüssel | – |
| literature_id | INTEGER | INDEX | [literature](#literature) | Verknüpfung zur Tabelle [literature](#literature) | – |
| collection_id | INTEGER | INDEX | [collections](#collections) | Verknüpfung zur Tabelle [collections](#collections) | – |

## collections

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | org @xml:id |
| code | TEXT | – | – | Sammlungscode aus der historischen Datenbank | – |
| name | TEXT | INDEX | – | Name der Sammlung (z.B. „Stiftsbibliothek“, „Diözesanarchiv“) | org->orgName |
| email | TEXT | – | – | Zentrale oder bestandsbezogene Kontaktadresse (optional) | org->desc @type="contact"->email |
| website | TEXT | – | – | Link zur offiziellen Webseite oder digitalen Präsenz | org->desc @type="contact"->ptr @type="website" @target |
| description | TEXT | – | – | Freitext zur Sammlungsgeschichte, inhaltlichen Ausrichtung oder Besonderheiten | org->desc |

## dates

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| format | TEXT | – | – | Formatcode der Datierung (siehe unten) | @type |
| value_1 | INTEGER | – | – | Erster Wert zur Datierung (z. B. Jahrhundert oder Jahr) | @not_before ? |
| value_2 | INTEGER | – | – | Zweiter Wert (z.B. Jahrhundert, Viertel etc.), ggf. NULL | @not_after ? |
| is_certain | BOOLEAN | – | – | Gibt an, ob die Datierung als gesichert gilt | @cert |
| range_from | TEXT | – | – | Berechneter Datumsbeginn als ISO-Datumsstring (Jahr/Monat/Tag) | @from ? |
| range_to | TEXT | – | – | Berechnetes Datumsende als ISO-Datumsstring (Jahr/Monat/Tag) | @to ? |
| datable_type | TEXT | INDEX | – | Typ der verknüpften Entität (z.B. `manuscript`, `part`, `binding`) | – |
| datable_id | INTEGER | INDEX | – | ID der verknüpften Entität (z.B. Handschrift, Teil, Einband) | – |

## dimensions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | – |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Verknüpfte Handschrift | – |
| length | INTEGER | – | – | Höhe der Handschrift bzw. des Bandes (in mm) | physDesc->objectDesc->supportDesc->extent->dimensions->height |
| width | INTEGER | – | – | Breite der Handschrift bzw. des Bandes (in mm) | physDesc->objectDesc->supportDesc->extent->dimensions->width |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung | – |
| is_approximate | BOOLEAN | – | – | Gibt an, ob es sich um eine ungefähre Angabe handelt | physDesc->objectDesc->supportDesc->extent->dimensions @rend="circa" |
| is_length_extrapolated | BOOLEAN | – | – | Markiert extrapolierte Höhenangaben | – |
| is_width_extrapolated | BOOLEAN | – | – | Markiert extrapolierte Breitenangaben | – |
| comments | TEXT | – | – | Freitextkommentar | – |

## extents

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | INDEX | – | Primärschlüssel | – |
| volume_id | INTEGER | INDEX | [volumes](#volumes) | Verknüpfung zur Tabelle [volumes](#volumes) | – |
| count | TEXT | – | – | Anzahl der Blätter/Seiten etc. | – |
| unit | TEXT | – | – | Einheit (kontrolliertes Vokabular: Blatt, Seite, Lage usw.) | – |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| is_approximate | BOOLEAN | – | – | Kennzeichnung unsicherer Angaben | – |

## iconclasses

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | classDecl->taxonomy @xml:id |
| code | TEXT | INDEX | – | Iconclass-Code (z.B. `11H`) | classDecl->taxonomy->category @n |
| label | TEXT | – | – | Bezeichnung des Motivs | classDecl->taxonomy->category->term |
| parent_id | INTEGER | INDEX | [iconclasses](#iconclasses) | Verweis auf übergeordnetes Motiv (optional) | classDecl->taxonomy->category @corresp |

## iconclass_parts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | – |
| part_id | INTEGER | INDEX | [parts](#parts) | Verknüpfter Handschriftenteil | msContents->msItem->filiation->ref @target |
| iconclass_id | INTEGER | INDEX | [iconclasses](#iconclasses) | Verknüpfter Iconclass-Eintrag | classDecl->taxonomy->category @xml:id |
| folio | TEXT | – | – | Blätter-/Seitenangabe zur Lokalisierung | msContents->msItem->filiation->ref @cRef |
| comments | TEXT | – | – | Freitextkommentar | note |
| position | INTEGER | – | – | Reihenfolge innerhalb des Teils | – |

## initia

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | msItem @xml:id |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Verknüpfung zur Handschrift | – |
| part_id | INTEGER | INDEX | [parts](#parts) | Verknüpfung zum Handschriftenteil | – |
| text_witness_id | INTEGER | INDEX | [text_witnesses](#text_witnesses) | Verknüpfung zur Tabelle [text_witnesses](#text_witnesses) | – |
| title | TEXT | – | – | Optionaler Titel oder Titelvorspann | msItem->title |
| incipit | TEXT | – | – | Beginn des Textabschnitts | msItem->incipit |
| explicit | TEXT | – | – | Ende des Textabschnitts (optional) | msItem->explicit |
| bibliography | TEXT | – | – | Literaturangaben zum Initium | – |
| comments | TEXT | – | – | Freitextkommentar | – |

## institutions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | org @xml:id |
| gnd_number | TEXT | INDEX | – | GND-Identifier der Körperschaft | org->ptr @type="gnd" @target |
| name | TEXT | INDEX | – | Normierter Name der Institution (z.B. „Stift Klosterneuburg“) | org->orgName |
| lat | REAL | – | – | Geokoordinate (Breite) | org->location->geo @latitude |
| lon | REAL | – | – | Geokoordinate (Länge) | org->location->geo @longitude |
| type | TEXT | – | – | Kategorisierung der Institution (z. B. Archiv, Bibliothek) | org->desc @type |

## languages

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | language @xml:id |
| code | TEXT | INDEX | – | Normierter Sprachcode (ISO 639) | language @ident |
| name | TEXT | – | – | Klartextbezeichnung der Sprache | language->term |

## language_literature

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| literature_id | INTEGER | INDEX | [literature](#literature) | Verknüpfte Publikation (Literatur- oder Katalogeintrag) | – |
| language_id | INTEGER | INDEX | [languages](#languages) | ID innerhalb des kontrollierten Sprachvokabulars | – |

## language_manuscript

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Verknüpfte Handschrift | – |
| language_id | INTEGER | INDEX | [languages](#languages) | ID innerhalb des kontrollierten Sprachvokabulars | – |

## literature

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | biblStruct @xml:id |
| title | TEXT | – | – | Vollständiger Titel der Publikation | biblStruct->monogr->title |
| is_uniform_title | BOOLEAN | – | – | Kennzeichnung eines normierten Sachtitels | – |
| short_title | TEXT | INDEX | – | Kurzzitat zur internen Referenz (z.B. „Pächt 1969“) | biblStruct->monogr->title @type="short" |
| year | INTEGER | – | – | Erscheinungsjahr | biblStruct->monogr->imprint->date |
| author | TEXT | – | – | Autor:innenangabe, soweit verfügbar | biblStruct->monogr->author |
| comment | TEXT | – | – | Anmerkungen zur Publikation | biblStruct->monogr->imprint->note |

## manuscript_material

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | INDEX | – | Primärschlüssel | – |
| materialized_type | TEXT | INDEX | – | Typ polymorpher Beziehung (`manuscript` oder `part`) | – |
| materialized_id | INTEGER | INDEX | – | Verknüpfte Entität innerhalb des polymorphen Modells | – |
| type | TEXT | – | – | Art des Materials (kontrolliertes Vokabular) | – |

## manuscripts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | msDesc xml:id |
| title | TEXT | INDEX | – | Kurztitel der Handschrift | head->title |
| shelfmark | TEXT | INDEX | – | Signatur innerhalb der Sammlung | msIdentifier->idno |
| code | TEXT | INDEX | – | Historischer Handschriftencode | msIdentifier->idno @sortKey |
| format | TEXT | – | – | Format des Codex (z.B. Quart, Folio) | physDesc->objectDesc->supportDesc->extent->dimensions->dim |
| type | TEXT | – | – | Überlieferungsform (z.B. Codex, Fragment) | physDesc->objectDesc @form |
| codicological_description | TEXT | – | – | Kodikologische Beschreibung (z.B. Lagenformel) | physDesc->objectDesc->supportDesc->collation |
| historical_description | TEXT | – | – | Freitext zur Überlieferungsgeschichte | history->summary / history->origin |
| collection_id | INTEGER | INDEX | [collections](#collections) | Zugehörige Sammlung | msIdentifier->repository @ref |
| comments | TEXT | – | – | Freitextkommentar | – |

## olims

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | @xml:id |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Verknüpfte Handschrift | – |
| shelfmark | TEXT | – | – | Überlieferte (alte) Signatur der Handschrift | altIdentifier->idno |
| comment | TEXT | – | – | Optionaler Kommentar zur Einordnung, Quelle oder historischen Kontext | altIdentifier->note |

## page_numbers

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | – |
| locatable_type | TEXT | INDEX | – | Typ polymorpher Beziehung | – |
| locatable_id | INTEGER | INDEX | – | ID der polymorphen Entität | – |
| from | TEXT | – | – | Von-Angabe | – |
| to | TEXT | – | – | Bis-Angabe | – |
| position | INTEGER | INDEX | – | Reihenfolge der Einheit innerhalb der Zählfolge | – |
| from_key | TEXT | – | – | Schlüssel für Vergleich mit anderen Seiten-/Folioangaben und Sortierung | – |
| to_key | TEXT | – | – | Schlüssel für Vergleich mit anderen Seiten-/Folioangaben und Sortierung | – |

## part_material

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | INDEX | – | Primärschlüssel | – |
| materialized_type | TEXT | INDEX | – | Typ polymorpher Beziehung (`manuscript` oder `part`) | – |
| materialized_id | INTEGER | INDEX | – | Verknüpfte Entität innerhalb des polymorphen Modells | – |
| type | TEXT | – | – | Art des Materials (kontrolliertes Vokabular) | – |

## parts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | msPart @xml:id |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift | msDesc @xml:id |
| is_fragment | BOOLEAN | – | – | Kennzeichnung als Fragment (true/false) | msPart->physDesc->objectDesc @form="fragment" |
| folio | TEXT | – | – | Freitextfeld für Blatt- oder Seitenangaben | msPart->physDesc->objectDesc->supportDesc->extent->measure @type="leavesCount" |

## people

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | person @xml:id |
| gnd_number | TEXT | INDEX | – | GND-Nummer für Normdatenverknüpfung | person->ptr @target |
| name | TEXT | INDEX | – | Gebräuchlicher oder bibliographischer Name der Person | person->persName |

## person_attributions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | relation @xml:id |
| attributable_id | INTEGER | INDEX | – | ID der verknüpften Entität (z.B. Teil, Schrift, Ausstattung) | – |
| attributable_type | TEXT | INDEX | – | Typ der verknüpften Entität (z.B. `part`, `script`, `book_decorations`, `text`) | – |
| person_id | INTEGER | INDEX | [people](#people) | Verknüpfte Person aus der zentralen Personentabelle | relation @active |
| role | TEXT | – | – | Funktionsbezeichnung der Person im jeweiligen Kontext | relation @name |
| is_certain | BOOLEAN | – | – | Gibt an, ob die Zuschreibung als gesichert gilt | relation @cert (high/low) |
| comments | TEXT | – | – | Freitextkommentar | – |

## places

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | place xml:id |
| gnd_number | TEXT | INDEX | – | GND-Identifier des Ortsdatensatzes | place->ptr @type="gnd" @target |
| name | TEXT | INDEX | – | Normierter Ortsname (z.B. „Wien“, „Salzburg“, „Cluny“) | place->placeName |
| lat | REAL | – | – | Breitengrad | – |
| lon | REAL | – | – | Längengrad | – |
| type | TEXT | – | – | Kategorisierung des Orts (z. B. Stadt, Kloster) | place->note @type |

## provenance_attributions

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | provenance @xml:id |
| attributable_type | TEXT | INDEX | – | Typ des verknüpften Objekts (z.B. `manuscript`, `part`) | – |
| attributable_id | INTEGER | INDEX | – | ID des verknüpften Objekts (z.B. Handschrift, Teil) | – |
| type | TEXT | – | – | Art der Provenienzzuschreibung (kontrolliertes Vokabular) | – |
| is_unknown | BOOLEAN | – | – | Kennzeichnung für unbekannte Provenienzangaben | – |
| comment | TEXT | – | – | Freitextkommentar oder Beleg zur Zuordnung | provenance->p |
| position | INTEGER | – | – | Reihenfolge der Angabe | – |
| has_exlibris | BOOLEAN | – | – | Vermerkt, ob ein Exlibris belegt ist | – |
| former_shelfmark | TEXT | – | – | Überlieferte Signatur innerhalb der Provenienzangabe | note |

## provenances

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | – |
| attribution_id | INTEGER | INDEX | [provenance_attributions](#provenance_attributions) | Verweis auf die zugehörige Provenienzzuordnung | – |
| subject_id | INTEGER | INDEX | – | ID der referenzierten Entität (z.B. Ort, Institution, Sammlung, Person) | placeName/orgName/persName @ref |
| subject_type | TEXT | INDEX | – | Typ der referenzierten Entität (z.B. `institution`, `place`, `person`) | placeName/orgName/persName |
| logic | TEXT | – | – | Verknüpfungslogik zwischen mehreren Angaben (z.B. `and`, `or`) | – |
| is_certain | BOOLEAN | – | – | Gibt an, ob es sich um eine gesicherte Zuordnung handelt | @cert (low/high) |
| comment | TEXT | – | – | Freitextkommentar zur Einordnung oder zur Quellenlage | provenance->p |
| comments | TEXT | – | – | Zusätzliche, häufig importierte Freitextnotiz | provenance->p |
| position | INTEGER | – | – | Reihenfolge innerhalb der Provenienzliste | – |

## references

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | INDEX | – | Primärschlüssel | – |
| referencable_id | INTEGER | INDEX | – | Verknüpfte Entität (polymorph) | – |
| referencable_type | TEXT | INDEX | – | Typ polymorpher Beziehung (z.B. `manuscript`, `text`) | – |
| literature_id | INTEGER | INDEX | [literature](#literature) | Verknüpfte Literaturstelle | – |
| url | TEXT | – | – | Optionaler Link zur Online-Ressource | ref @target |

## scripts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | scriptDesc @xml:id |
| part_id | INTEGER | INDEX | [parts](#parts) | Verknüpfung zum kodikologischen Teil | – |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Verknüpfung zur Tabelle [manuscripts](#manuscripts) | – |
| script | TEXT | – | – | Schrifttyp (kontrolliertes Vokabular) | scriptDesc->scriptNote @script / scriptNote->term @ref |
| notation | TEXT | – | – | Notationstyp (kontrolliertes Vokabular) | musicNotation |
| has_marginalia | BOOLEAN | – | – | Gibt an, ob Marginalien vorhanden sind | additions – term |
| has_secret_script | BOOLEAN | – | – | Gibt an, ob Geheimschrift verwendet wurde | additions – term |
| has_glosses | BOOLEAN | – | – | Gibt an, ob Glossen vorhanden sind | additions – term |
| columns | TEXT | – | – | Spaltenzahl | layoutDesc->layout->num @type="columns" |
| rows | TEXT | – | – | Zeilenzahl | layoutDesc->layout->num @type="rows" |
| width | TEXT | – | – | Schriftspiegelbreite (sofern angegeben) | layoutDesc->layout->measure @type="width" |
| height | TEXT | – | – | Schriftspiegelhöhe (sofern angegeben) | layoutDesc->layout->measure @type="height" |
| comments | TEXT | – | – | Freitextkommentar | scriptDesc->scriptNote |

## text_witnesses

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | – |
| text_id | INTEGER | INDEX | [texts](#texts) | Verweis auf das überlieferte Werk | – |
| part_id | INTEGER | INDEX | [parts](#parts) | Verknüpfung zum entsprechenden Handschriftenteil | msContents->msItem->filiation->ref @type="altMs" @target |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Verknüpfung zur Tabelle [manuscripts](#manuscripts) | – |
| comments | TEXT | – | – | Freitextkommentar | – |
| page_number_comment | TEXT | – | – | Kommentar zu nicht eindeutigen Folioangaben | note |
| position | INTEGER | INDEX | – | Reihenfolge innerhalb der Handschrift | – |

## texts

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | PRIMARY | – | Primärschlüssel | bibl xml:id |
| title | TEXT | INDEX | – | Werktitel (Pflichtfeld) | bibl->title |
| subtitle | TEXT | – | – | Alternativer oder ergänzender Titel | bibl->title @type="sub" |
| standard_identifier | TEXT | INDEX | – | Normierter Identifier (z.B. BHL) | bibl->ref |
| bibliography | TEXT | – | – | Literaturhinweise zum Werk | listBibl |
| comments | TEXT | – | – | Freitextkommentar | note |

## volumes

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar | TEI-mapping |
|----------|--------------|-------|--------------|-----------|-------------|
| id | INTEGER | INDEX | – | Primärschlüssel | – |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Verknüpfung zur Tabelle [manuscripts](#manuscripts) | – |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung | – |
