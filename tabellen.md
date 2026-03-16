# Tabellen

Dieses Dokument listet alle Domänentabellen der Datenbank mit ihren Feldern, Datentypen und Verknüpfungen. Informationen zur Abbildung auf TEI P5 sind in der separaten Datei [tei-mapping.md](tei-mapping.md) zu finden.

## Index

- [agent_collection](#agent_collection)
- [agents](#agents)
- [binding_decorations](#binding_decorations)
- [binding_features](#binding_features)
- [binding_workshop_institution](#binding_workshop_institution)
- [binding_workshop_place](#binding_workshop_place)
- [binding_workshops](#binding_workshops)
- [bindings](#bindings)
- [book_decorations](#book_decorations)
- [catalogs](#catalogs)
- [collection_complements](#collection_complements)
- [collections](#collections)
- [dates](#dates)
- [digital_copies](#digital_copies)
- [digital_copy_pages](#digital_copy_pages)
- [dimensions](#dimensions)
- [extents](#extents)
- [glossary_terms](#glossary_terms)
- [iconclass_terms](#iconclass_terms)
- [initia](#initia)
- [institution_place](#institution_place)
- [institutions](#institutions)
- [language_manuscript](#language_manuscript)
- [language_publications](#language_publications)
- [languages](#languages)
- [licenses](#licenses)
- [list_items](#list_items)
- [lists](#lists)
- [manuscript_authors](#manuscript_authors)
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
- [publications](#publications)
- [reference_links](#reference_links)
- [references](#references)
- [resources](#resources)
- [scripts](#scripts)
- [text_witnesses](#text_witnesses)
- [texts](#texts)
- [volumes](#volumes)

---

## agent_collection

Verknüpft Agenten mit Sammlungen und hält fest, in welcher Rolle sie an einer Sammlung beteiligt sind.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| collection_id | INTEGER | INDEX | [collections](#collections) | Zugehörige Sammlung |
| agent_id | INTEGER | INDEX | [agents](#agents) | Zugehöriger Agent |
| role | TEXT | – | – | Rolle des Agenten innerhalb der Sammlung |
| sort_index | INTEGER | – | – | Reihenfolge in der Auflistung |
| note | TEXT | – | – | Freitextnotiz |

## agents

Körperschaften oder Personen, die an einer Sammlung institutionell beteiligt sind, etwa als Herausgeber oder Trägerinstitution.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| kind | TEXT | – | – | Art des Agenten: `person` oder `organization` |
| name | TEXT | – | – | Name des Agenten |
| uri | TEXT | UNIQUE | – | Eindeutiger Bezeichner (z. B. GND-URI) |
| email | TEXT | – | – | Kontaktadresse (optional) |
| website | TEXT | – | – | Website (optional) |
| is_active | INTEGER | – | – | Gibt an, ob der Agent aktiv ist |

## binding_decorations

Erfasst Zierelemente eines Einbands. Ein Einband kann mehrere Einträge haben.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| binding_id | INTEGER | INDEX | [bindings](#bindings) | Zugehöriger Einband |
| type | TEXT | – | – | Art des Schmucks (kontrolliertes Vokabular) |

## binding_features

Erfasst funktionale oder dekorative Merkmale eines Einbands wie Schließen, Beschläge oder Eckverstärkungen.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| binding_id | INTEGER | INDEX | [bindings](#bindings) | Zugehöriger Einband |
| type | TEXT | – | – | Art des Merkmals (kontrolliertes Vokabular) |

## binding_workshop_institution

Verknüpft eine Buchbinderwerkstatt mit einer Institution.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| binding_workshop_id | INTEGER | INDEX | [binding_workshops](#binding_workshops) | Zugehörige Werkstatt |
| institution_id | INTEGER | INDEX | [institutions](#institutions) | Zugehörige Institution |
| comments | TEXT | – | – | Freitextkommentar |
| is_certain | INTEGER | – | – | Gibt an, ob die Zuordnung als gesichert gilt |
| logic | TEXT | – | – | Verknüpfungslogik mit dem vorherigen Eintrag (`and`/`or`) |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung |

## binding_workshop_place

Verknüpft eine Buchbinderwerkstatt mit einem Ort.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| binding_workshop_id | INTEGER | INDEX | [binding_workshops](#binding_workshops) | Zugehörige Werkstatt |
| place_id | INTEGER | INDEX | [places](#places) | Zugehöriger Ort |
| comments | TEXT | – | – | Freitextkommentar |
| is_certain | INTEGER | – | – | Gibt an, ob die Zuordnung als gesichert gilt |
| logic | TEXT | – | – | Verknüpfungslogik mit dem vorherigen Eintrag (`and`/`or`) |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung |

## binding_workshops

Buchbinderwerkstätten als eigenständiger Datentyp, mit optionaler Verlinkung zur Einbanddatenbank der Staatsbibliothek zu Berlin.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| name | TEXT | – | – | Bezeichnung der Werkstatt |
| ebdb_id | TEXT | – | – | ID in der Einbanddatenbank (EBDB) |

## bindings

Einbände als eigenständiger Datentyp. Obwohl die Beziehung zur Handschrift in der Regel 1:1 ist, wird der Einband separat erfasst, da er eigene Datierungen, Provenienzen und Ausstattungsmerkmale aufweisen kann.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| has_fragments | INTEGER | – | – | Gibt an, ob Einbandfragmente oder Makulatur enthalten sind |
| is_restored | INTEGER | – | – | Gibt an, ob der Einband restauriert wurde |
| style | TEXT | – | – | Stilbezeichnung (kontrolliertes Vokabular) |
| comments | TEXT | – | – | Freitextkommentar |

## book_decorations

Erfasst Ausstattungskategorien eines kodikologischen Teils (z. B. Miniaturen, Fleuronnée). Ein Teil kann mehrere Einträge haben.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| part_id | INTEGER | INDEX | [parts](#parts) | Zugehöriger kodikologischer Teil |
| type | TEXT | – | – | Ausstattungskategorie (kontrolliertes Vokabular) |

## catalogs

Verknüpft Handschriftenkataloge (Publikationen) mit den Sammlungen, die sie beschreiben.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| publication_id | INTEGER | INDEX | [publications](#publications) | Zugehörige Publikation |
| collection_id | INTEGER | INDEX | [collections](#collections) | Beschriebene Sammlung |

## collection_complements

Handschriften können in mehreren Sammlungen geführt werden, etwa wenn ein Codex nach einem Sammlungswechsel unter einer neuen Signatur erfasst wurde. Diese Tabelle hält die ergänzenden Zuordnungen fest.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| original_collection_id | INTEGER | INDEX | [collections](#collections) | Ergänzende Sammlung |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| original_shelfmark | TEXT | – | – | Signatur in der ergänzenden Sammlung |
| comments | TEXT | – | – | Freitextkommentar |
| date | TEXT | – | – | Zeitangabe zur Zuordnung |
| position | INTEGER | – | – | Reihenfolge in der Auflistung |

## collections

Sammlungen innerhalb einer Institution. Eine Institution kann mehrere Sammlungen führen, etwa eine Stiftsbibliothek und ein Stiftsarchiv.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| code | TEXT | INDEX | – | Sammlungscode (z. B. `AT5000`) |
| name | TEXT | INDEX | – | Name der Sammlung |
| email | TEXT | – | – | Kontaktadresse (optional) |
| website | TEXT | – | – | Website (optional) |
| description | TEXT | – | – | Freitext zur Sammlungsgeschichte und inhaltlichen Ausrichtung |
| institution_id | INTEGER | INDEX | [institutions](#institutions) | Übergeordnete Institution |
| default_license_id | INTEGER | – | [licenses](#licenses) | Standardlizenz für Digitalisate der Sammlung |

## dates

Datierungsangaben als eigener Datentyp. Die Tabelle ist polymorph aufgebaut und kann mit verschiedenen Entitäten verknüpft werden (Handschriften, Teile, Einbände usw.).

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| format | TEXT | – | – | Art der Datierung (kontrolliertes Vokabular, z. B. `century`, `year`, `between`) |
| value_1 | INTEGER | – | – | Erster Datierungswert (z. B. Jahrhundert oder Jahr) |
| value_2 | INTEGER | – | – | Zweiter Datierungswert, falls vorhanden (z. B. bei Zeitspannen) |
| is_certain | INTEGER | – | – | Gibt an, ob die Datierung als gesichert gilt |
| range_from | INTEGER | – | – | Berechneter Beginn des Zeitraums (Jahr) |
| range_to | INTEGER | – | – | Berechnetes Ende des Zeitraums (Jahr) |
| datable_type | TEXT | INDEX | – | Typ der verknüpften Entität (z. B. `manuscript`, `part`, `binding`) |
| datable_id | INTEGER | INDEX | – | ID der verknüpften Entität |
| comment | TEXT | – | – | Freitextkommentar zur Datierung |

## digital_copies

IIIF-Digitalisate, die einer Handschrift zugeordnet sind. Jede Handschrift kann mehrere Digitalisate haben.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| label | TEXT | – | – | Bezeichnung des Digitalisats |
| manifest_url | TEXT | – | – | URL des IIIF-Manifests |
| cover_canvas_id | TEXT | – | – | Canvas-ID der Titelseite für die Vorschau |
| license | TEXT | – | – | Lizenzangabe |
| attribution | TEXT | – | – | Bildnachweis |
| logo_url | TEXT | – | – | Logo der bereitstellenden Institution |

## digital_copy_pages

Einzelne Seiten (Canvases) eines Digitalisats. Diese werden aus dem IIIF-Manifest importiert und lokal gespeichert, um direkte Verlinkungen von Teilen und Initien auf bestimmte Seiten zu ermöglichen.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| digital_copy_id | INTEGER | INDEX | [digital_copies](#digital_copies) | Zugehöriges Digitalisat |
| label | TEXT | – | – | Bezeichnung der Seite (z. B. `001r`) |
| canvas_id | TEXT | – | – | IIIF Canvas-URI |
| image_service | TEXT | – | – | URL des IIIF Image Service |
| width | INTEGER | – | – | Bildbreite in Pixeln |
| height | INTEGER | – | – | Bildhöhe in Pixeln |
| position | INTEGER | – | – | Reihenfolge innerhalb des Digitalisats |

## dimensions

Abmessungen einer Handschrift in Millimetern. Mehrere Einträge pro Handschrift sind möglich, etwa bei unsicheren Angaben oder unterschiedlichen Messungen.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| length | INTEGER | – | – | Höhe in Millimetern |
| width | INTEGER | – | – | Breite in Millimetern |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung |
| is_approximate | INTEGER | – | – | Gibt an, ob es sich um eine ungefähre Angabe handelt |
| is_length_extrapolated | INTEGER | – | – | Gibt an, ob die Höhe extrapoliert wurde |
| is_width_extrapolated | INTEGER | – | – | Gibt an, ob die Breite extrapoliert wurde |
| comments | TEXT | – | – | Freitextkommentar |

## extents

Blatt- oder Seitenanzahl eines Bandes. Die Angabe ist strukturiert und ermöglicht eine genaue Rekonstruktion des Umfangs (z. B. `I, 436, I* Bl.`).

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| volume_id | INTEGER | INDEX | [volumes](#volumes) | Zugehöriger Band |
| count | TEXT | – | – | Gezählte Einheit (z. B. `436`, `I`, `I*`) |
| unit | TEXT | – | – | Zähleinheit (kontrolliertes Vokabular: Blatt, Seite usw.) |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung |
| comments | TEXT | – | – | Freitextkommentar |
| is_approximate | INTEGER | – | – | Gibt an, ob die Angabe als ungefähr gilt |

## iconclass_terms

Referenzvokabular des Iconclass-Systems zur ikonographischen Erschließung. Die Tabelle bildet die Hierarchie der Iconclass-Kategorien ab.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| code | TEXT | PRIMARY | – | Iconclass-Code (z. B. `11H`) |
| label_en | TEXT | – | – | Bezeichnung auf Englisch |
| label_de | TEXT | – | – | Bezeichnung auf Deutsch |
| parent_code | TEXT | INDEX | [iconclass_terms](#iconclass_terms) | Übergeordnete Kategorie (optional) |

## initia

Initien (Textanfänge) und Explizite innerhalb einer Handschrift. Sie können einem Textüberlieferungseintrag zugeordnet werden, sofern der Text identifizierbar ist.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| part_id | INTEGER | INDEX | [parts](#parts) | Zugehöriger kodikologischer Teil |
| text_witness_id | INTEGER | INDEX | [text_witnesses](#text_witnesses) | Zugehörige Textüberlieferung (optional) |
| digital_copy_page_id | INTEGER | INDEX | [digital_copy_pages](#digital_copy_pages) | Verknüpfte Seite im Digitalisat (optional) |
| title | TEXT | – | – | Optionaler Titelvorspann |
| incipit | TEXT | – | – | Textanfang |
| explicit | TEXT | – | – | Textende (optional) |
| bibliography | TEXT | – | – | Literaturhinweise |
| comments | TEXT | – | – | Freitextkommentar |
| source | TEXT | – | – | Quelle oder Nachweis des Eintrags |
| position | INTEGER | INDEX | – | Reihenfolge innerhalb der Handschrift |

## institution_place

Verknüpft Institutionen mit ihrem geographischen Standort.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| institution_id | INTEGER | INDEX | [institutions](#institutions) | Zugehörige Institution |
| place_id | INTEGER | INDEX | [places](#places) | Zugehöriger Ort |

## institutions

Körperschaften wie Bibliotheken, Archive oder Klöster, bevorzugt auf Basis der Gemeinsamen Normdatei (GND).

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| gnd_number | TEXT | INDEX | – | GND-Identifier |
| name | TEXT | INDEX | – | Normierter Name der Institution |
| lat | TEXT | – | – | Geographische Breite (optional) |
| lon | TEXT | – | – | Geographische Länge (optional) |
| type | TEXT | – | – | Kategorisierung (z. B. Archiv, Bibliothek, Kloster) |

## language_manuscript

Verknüpft Handschriften mit den in ihnen verwendeten Sprachen (n:m).

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| iso_code | TEXT | INDEX | [languages](#languages) | Sprachcode nach ISO 639 |

## language_publications

Verknüpft Publikationen mit ihrer Sprache (n:m).

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| publication_id | INTEGER | INDEX | [publications](#publications) | Zugehörige Publikation |
| iso_code | TEXT | INDEX | [languages](#languages) | Sprachcode nach ISO 639 |

## languages

Kontrolliertes Sprachvokabular auf Basis von ISO 639.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| iso_code | TEXT | PRIMARY | – | ISO-639-Sprachcode (z. B. `la`, `de`) |

## licenses

Lizenzen für Digitalisate und andere Inhalte.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| short_title | TEXT | – | – | Kurzbezeichnung der Lizenz (z. B. `CC BY 4.0`) |
| long_title | TEXT | – | – | Vollständige Bezeichnung |
| url | TEXT | – | – | Link zur Lizenztextseite |
| description | TEXT | – | – | Freitextbeschreibung |

## list_items

Einzelne Einträge in einer Benutzerliste.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| list_id | INTEGER | INDEX | [lists](#lists) | Zugehörige Liste |
| entity_type | TEXT | INDEX | – | Typ des Eintrags (`manuscript`, `digital_copy`, `digital_copy_page`) |
| entity_id | INTEGER | INDEX | – | ID des Eintrags |
| position | INTEGER | – | – | Reihenfolge innerhalb der Liste |

## lists

Von Benutzerinnen und Benutzern angelegte Listen, in denen Handschriften, Digitalisate oder Seiten gesammelt werden können. Listen können öffentlich oder privat sein.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| name | TEXT | – | – | Bezeichnung der Liste |
| is_default | INTEGER | – | – | Gibt an, ob dies die Standardliste des Benutzers ist |
| visibility | TEXT | INDEX | – | Sichtbarkeit: `private` oder `public` |
| public_key | TEXT | UNIQUE | – | Öffentlicher Zugangsschlüssel für geteilte Listen |
| published_at | TEXT | – | – | Zeitpunkt der Veröffentlichung |

## manuscript_authors

Redaktionelle Zuständigkeiten für Handschriftenbeschreibungen. Ein Eintrag gibt an, wer eine Beschreibung verfasst oder bearbeitet hat.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| name | TEXT | INDEX | – | Name der zuständigen Person |
| type | TEXT | – | – | Art der Beteiligung (z. B. Bearbeitung, Ersterfassung) |
| description | TEXT | – | – | Freitextbeschreibung |
| position | INTEGER | – | – | Reihenfolge in der Auflistung |

## manuscript_material

Beschreibstoffe einer Handschrift. Über eine polymorphe Beziehung können sowohl Handschriften als auch Teile mehrere Materialien zugeordnet bekommen.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| materialized_type | TEXT | INDEX | – | Typ der verknüpften Entität (`manuscript` oder `part`) |
| materialized_id | TEXT | INDEX | – | ID der verknüpften Entität |
| type | TEXT | – | – | Art des Materials (kontrolliertes Vokabular, z. B. `parchment`, `paper`) |

## manuscripts

Handschriften als zentrales Objekt der Datenbank.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| title | TEXT | INDEX | – | Kurztitel der Handschrift |
| shelfmark | TEXT | INDEX | – | Signatur innerhalb der Sammlung |
| code | TEXT | INDEX | – | Historischer Handschriftencode (z. B. `AT5000-308`) |
| format | TEXT | – | – | Format des Codex (z. B. Quart, Folio) |
| type | TEXT | – | – | Überlieferungsform (z. B. Codex, Fragment) |
| codicological_description | TEXT | – | – | Kodikologische Beschreibung (z. B. Lagenformel, Foliierung) |
| historical_description | TEXT | – | – | Überlieferungsgeschichte |
| collection_id | INTEGER | INDEX | [collections](#collections) | Zugehörige Sammlung |
| comments | TEXT | – | – | Freitextkommentar |

## olims

Frühere Signaturen einer Handschrift. Sie dokumentieren ältere Ordnungszusammenhänge und können zur Provenienzgeschichte beitragen.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| shelfmark | TEXT | – | – | Überlieferte frühere Signatur |
| comment | TEXT | – | – | Kommentar zur Einordnung oder Quelle |

## page_numbers

Seiten- und Folioangaben, die polymorphe Verweise auf bestimmte Stellen innerhalb einer Handschrift ermöglichen. Wird u. a. für die genaue Lokalisierung von Textüberlieferungen verwendet.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| locatable_type | TEXT | INDEX | – | Typ der verknüpften Entität |
| locatable_id | TEXT | INDEX | – | ID der verknüpften Entität |
| from | TEXT | – | – | Startseite oder -folio |
| to | TEXT | – | – | Endseite oder -folio (optional) |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung |
| from_key | TEXT | – | – | Normierter Sortierschlüssel für den Startwert |
| to_key | TEXT | – | – | Normierter Sortierschlüssel für den Endwert |

## part_material

Beschreibstoffe eines kodikologischen Teils. Identische Struktur wie [manuscript_material](#manuscript_material).

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| materialized_type | TEXT | INDEX | – | Typ der verknüpften Entität (`manuscript` oder `part`) |
| materialized_id | TEXT | INDEX | – | ID der verknüpften Entität |
| type | TEXT | – | – | Art des Materials (kontrolliertes Vokabular) |

## parts

Kodikologische Teile und Fragmente einer Handschrift.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| is_fragment | INTEGER | – | – | Gibt an, ob es sich um ein Fragment handelt |
| part_type | TEXT | – | – | Typ des Teils (kontrolliertes Vokabular) |
| folio | TEXT | – | – | Blatt- oder Seitenangaben (Freitext) |
| decoration_description | TEXT | – | – | Beschreibung der Buchausstattung |
| iconographic_program | TEXT | – | – | Beschreibung des Bildprogramms |
| collation | TEXT | – | – | Lagenformel |
| collation_comments | TEXT | – | – | Kommentar zur Lagenformel |
| digital_copy_page_id | INTEGER | – | [digital_copy_pages](#digital_copy_pages) | Verknüpfte Seite im Digitalisat (optional) |

## people

Personen, die im Zusammenhang mit Handschriften auftreten, etwa als Autoren, Schreiber, Buchmaler oder Vorbesitzer. Die Tabelle fasst frühere getrennte Tabellen für Autoren und Personen zusammen.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| gnd_number | TEXT | INDEX | – | GND-Identifier (optional) |
| name | TEXT | INDEX | – | Gebräuchlicher oder bibliographischer Name |

## person_attributions

Verknüpft Personen mit verschiedenen Entitäten und hält die jeweilige Funktion fest. Die Tabelle ist polymorph aufgebaut.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| attributable_id | INTEGER | INDEX | – | ID der verknüpften Entität |
| attributable_type | TEXT | INDEX | – | Typ der verknüpften Entität (z. B. `part`, `script`, `text`) |
| person_id | INTEGER | INDEX | [people](#people) | Zugehörige Person |
| role | TEXT | – | – | Funktion der Person (z. B. Schreiber, Buchmaler, Autor) |
| is_certain | INTEGER | – | – | Gibt an, ob die Zuordnung als gesichert gilt |
| comments | TEXT | – | – | Freitextkommentar |

## places

Geographische Orte auf Basis von Normdaten, bevorzugt aus der Gemeinsamen Normdatei (GND).

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| gnd_number | TEXT | INDEX | – | GND-Identifier (optional) |
| name | TEXT | INDEX | – | Normierter Ortsname |
| lat | TEXT | – | – | Geographische Breite (optional) |
| lon | TEXT | – | – | Geographische Länge (optional) |
| type | TEXT | – | – | Kategorisierung (z. B. `place`, `settlement`, `monastery`) |

## provenance_attributions

Provenienzzuordnungen verbinden eine Entität (Handschrift, Einband) mit einer oder mehreren Provenienzstellen und beschreiben Art und Kontext der Zuordnung.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| attributable_type | TEXT | INDEX | – | Typ der verknüpften Entität (z. B. `manuscript`, `binding`) |
| attributable_id | INTEGER | INDEX | – | ID der verknüpften Entität |
| type | TEXT | – | – | Art der Provenienz (kontrolliertes Vokabular) |
| is_unknown | INTEGER | – | – | Gibt an, ob die Provenienz unbekannt ist |
| comment | TEXT | – | – | Freitextkommentar oder Quellennachweis |
| position | INTEGER | – | – | Reihenfolge in der Auflistung |
| former_shelfmark | TEXT | – | – | Frühere Signatur innerhalb dieser Provenienzangabe |

## provenances

Einzelne Provenienzstellen innerhalb einer Provenienzzuordnung. Ein Eintrag verweist auf eine Institution, einen Ort, eine Sammlung oder eine Person.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| attribution_id | INTEGER | INDEX | [provenance_attributions](#provenance_attributions) | Zugehörige Provenienzzuordnung |
| subject_id | INTEGER | INDEX | – | ID der referenzierten Entität |
| subject_type | TEXT | INDEX | – | Typ der referenzierten Entität (`institution`, `place`, `person`, `collection`) |
| logic | TEXT | – | – | Verknüpfungslogik mit dem vorherigen Eintrag (`and`/`or`) |
| has_exlibris | INTEGER | – | – | Gibt an, ob ein Exlibris belegt ist |
| is_certain | INTEGER | – | – | Gibt an, ob die Zuordnung als gesichert gilt |
| comment | TEXT | – | – | Freitextkommentar |
| comments | TEXT | – | – | Ergänzende Freitextnotiz (häufig aus dem Datenimport) |
| position | INTEGER | – | – | Reihenfolge in der Auflistung |

## publications

Bibliographische Einträge: Forschungsliteratur, Handschriftenkataloge, Editionen und digitale Veröffentlichungen.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| title | TEXT | – | – | Vollständiger Titel |
| is_uniform_title | INTEGER | – | – | Gibt an, ob es sich um einen Sachtitel handelt |
| short_title | TEXT | INDEX | – | Kurzzitat zur internen Referenz (z. B. `Pächt 1969`) |
| year | INTEGER | – | – | Erscheinungsjahr |
| author | TEXT | – | – | Autor- oder Herausgeberangabe |
| comment | TEXT | – | – | Freitextanmerkung |

## reference_links

Zusätzliche URLs zu einem Literaturverweis, etwa für Digitalisate oder Volltexte.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| reference_id | INTEGER | INDEX | [references](#references) | Zugehöriger Literaturverweis |
| url | TEXT | – | – | URL der externen Ressource |
| label | TEXT | – | – | Bezeichnung des Links |
| position | INTEGER | – | – | Reihenfolge in der Auflistung |

## references

Literaturverweise, die verschiedene Entitäten (Handschriften, Initien usw.) mit Publikationen verknüpfen. Die Tabelle ist polymorph aufgebaut.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| referencable_id | TEXT | INDEX | – | ID der verknüpften Entität |
| referencable_type | TEXT | INDEX | – | Typ der verknüpften Entität |
| publication_id | INTEGER | INDEX | [publications](#publications) | Zugehörige Publikation |
| url | TEXT | – | – | Direktlink zur Onlineressource |
| volume | TEXT | – | – | Bandzahl (bei mehrbändigen Werken) |
| page_start | TEXT | – | – | Erste Seite |
| page_end | TEXT | – | – | Letzte Seite |
| locator | TEXT | – | – | Weitere Angabe zur genauen Fundstelle (z. B. Nr., Sp.) |
| comment | TEXT | – | – | Freitextkommentar |

## resources

Externe Ressourcen und Identifier, die mit einer Entität verknüpft sind, etwa Normdaten-URIs oder Links zu anderen Datenbanken. Die Tabelle ist polymorph aufgebaut.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| referencable_id | TEXT | INDEX | – | ID der verknüpften Entität |
| referencable_type | TEXT | INDEX | – | Typ der verknüpften Entität |
| type | TEXT | – | – | Art der Ressource (z. B. `gnd`, `viaf`, `orcid`) |
| data_provider | TEXT | – | – | Anbieter der Ressource |
| external_id | TEXT | – | – | Identifier in der externen Ressource |
| url | TEXT | – | – | URL der externen Ressource |

## scripts

Paläographische Beschreibungen auf Ebene der kodikologischen Teile. Ein Teil kann mehrere Einträge haben, etwa bei unterschiedlichen Schreiberhänden.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| part_id | INTEGER | INDEX | [parts](#parts) | Zugehöriger kodikologischer Teil |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| script | TEXT | – | – | Schrifttyp (kontrolliertes Vokabular) |
| notation | TEXT | – | – | Notationstyp (kontrolliertes Vokabular) |
| has_marginalia | INTEGER | – | – | Gibt an, ob Marginalien vorhanden sind (nullable) |
| has_glosses | INTEGER | – | – | Gibt an, ob Glossen vorhanden sind (nullable) |
| has_secret_script | INTEGER | – | – | Gibt an, ob Geheimschrift verwendet wurde (nullable) |
| columns | TEXT | – | – | Spaltenzahl |
| rows | TEXT | – | – | Zeilenzahl |
| width | TEXT | – | – | Schriftspiegelbreite |
| height | TEXT | – | – | Schriftspiegelhöhe |
| comments | TEXT | – | – | Freitextkommentar |

## text_witnesses

Überlieferungen eines Textes in einer konkreten Handschrift. Ein Eintrag verbindet einen Text mit einer Handschrift und ggf. einem kodikologischen Teil.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| text_id | INTEGER | INDEX | [texts](#texts) | Zugehöriger Text |
| part_id | INTEGER | INDEX | [parts](#parts) | Zugehöriger kodikologischer Teil (optional) |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| subheading | TEXT | – | – | Zwischentitel oder Rubrik |
| bibliography | TEXT | – | – | Literaturhinweise zur Überlieferung |
| comments | TEXT | – | – | Freitextkommentar |
| source | TEXT | – | – | Quelle oder Nachweis des Eintrags |
| position | INTEGER | INDEX | – | Reihenfolge innerhalb der Handschrift |
| page_numbers | TEXT | – | – | Folioangaben (serialisiert) |
| page_number_comment | TEXT | – | – | Kommentar zu mehrdeutigen Folioangaben |
| is_empty | INTEGER | – | – | Gibt an, ob die Überlieferung leer ist (kein Inhalt erfasst) |

## texts

Werke, die in Handschriften überliefert sind. Diese Tabelle erfasst den Text als abstrakte Einheit, unabhängig von einer konkreten Handschrift.

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| title | TEXT | INDEX | – | Werktitel |
| subtitle | TEXT | – | – | Alternativer oder ergänzender Titel |
| standard_identifier | TEXT | INDEX | – | Normierter Identifier (z. B. BHL) |
| bibliography | TEXT | – | – | Literaturhinweise |
| comments | TEXT | – | – | Freitextkommentar |

## volumes

Bände einer Handschrift. Eine Handschrift kann aus mehreren physischen Bänden bestehen, jeweils mit einem eigenen Umfangseintrag in [extents](#extents).

| Feldname | SQL-Datentyp | Index | Referenziert | Kommentar |
|----------|--------------|-------|--------------|-----------|
| id | INTEGER | PRIMARY | – | Primärschlüssel |
| manuscript_id | INTEGER | INDEX | [manuscripts](#manuscripts) | Zugehörige Handschrift |
| position | INTEGER | INDEX | – | Reihenfolge in der Auflistung |
