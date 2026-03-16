# TEI-Mapping

Dieses Dokument beschreibt, wie die Felder der manuscripta.at-Datenbank auf Elemente und Attribute des TEI P5-Standards abgebildet werden. Es dient als statische Referenz für den TEI-Export und wird nicht automatisch aus dem Schema generiert.

Die Abbildungen folgen den Empfehlungen der TEI-Richtlinien für Handschriftenbeschreibungen (`msDesc`) sowie den Konventionen des ACDH-CH.

---

## manuscripts

| Feldname | TEI-Pfad |
|----------|----------|
| id | `msDesc @xml:id` |
| title | `msDesc/head/title` |
| shelfmark | `msDesc/msIdentifier/idno` |
| code | `msDesc/msIdentifier/idno @sortKey` |
| format | `msDesc/physDesc/objectDesc/supportDesc/extent/dimensions/dim` |
| type | `msDesc/physDesc/objectDesc @form` |
| codicological_description | `msDesc/physDesc/objectDesc/supportDesc/collation` |
| historical_description | `msDesc/history/summary` |
| collection_id | `msDesc/msIdentifier/repository @ref` |

## parts

| Feldname | TEI-Pfad |
|----------|----------|
| id | `msPart @xml:id` |
| manuscript_id | `msDesc @xml:id` |
| is_fragment | `msPart/physDesc/objectDesc @form="fragment"` |
| folio | `msPart/physDesc/objectDesc/supportDesc/extent/measure @type="leavesCount"` |

## bindings

| Feldname | TEI-Pfad |
|----------|----------|
| id | `bindingDesc/binding @xml:id` |

## texts

| Feldname | TEI-Pfad |
|----------|----------|
| id | `bibl @xml:id` |
| title | `bibl/title` |
| subtitle | `bibl/title @type="sub"` |
| standard_identifier | `bibl/ref` |
| bibliography | `bibl/listBibl` |
| comments | `bibl/note` |

## text_witnesses

| Feldname | TEI-Pfad |
|----------|----------|
| part_id | `msContents/msItem/filiation/ref @type="altMs" @target` |
| page_number_comment | `msContents/msItem/note` |

## initia

| Feldname | TEI-Pfad |
|----------|----------|
| id | `msItem @xml:id` |
| title | `msItem/title` |
| incipit | `msItem/incipit` |
| explicit | `msItem/explicit` |

## dates

| Feldname | TEI-Pfad |
|----------|----------|
| format | `date @type` |
| is_certain | `date @cert` |

## dimensions

| Feldname | TEI-Pfad |
|----------|----------|
| length | `physDesc/objectDesc/supportDesc/extent/dimensions/height` |
| width | `physDesc/objectDesc/supportDesc/extent/dimensions/width` |
| is_approximate | `physDesc/objectDesc/supportDesc/extent/dimensions @rend="circa"` |

## olims

| Feldname | TEI-Pfad |
|----------|----------|
| id | `altIdentifier @xml:id` |
| shelfmark | `altIdentifier/idno` |
| comment | `altIdentifier/note` |

## scripts

| Feldname | TEI-Pfad |
|----------|----------|
| id | `scriptDesc @xml:id` |
| script | `scriptDesc/scriptNote @script` |
| has_marginalia | `additions/term` |
| has_glosses | `additions/term` |
| has_secret_script | `additions/term` |

## book_decorations

| Feldname | TEI-Pfad |
|----------|----------|
| type | `decoDesc/decoNote @type` |

## binding_decorations

| Feldname | TEI-Pfad |
|----------|----------|
| id | `bindingDesc/binding/decoNote @xml:id` |
| type | `bindingDesc/binding/decoNote @type` |

## bindings (Einbandfragmente)

| Feldname | TEI-Pfad |
|----------|----------|
| has_fragments | `physDesc/accMat` |

## people

| Feldname | TEI-Pfad |
|----------|----------|
| id | `person @xml:id` |
| gnd_number | `person/ptr @type="gnd" @target` |
| name | `person/persName` |

## person_attributions

| Feldname | TEI-Pfad |
|----------|----------|
| id | `relation @xml:id` |
| person_id | `relation @active` |
| role | `relation @name` |
| is_certain | `relation @cert` |

## institutions

| Feldname | TEI-Pfad |
|----------|----------|
| id | `org @xml:id` |
| gnd_number | `org/ptr @type="gnd" @target` |
| name | `org/orgName` |

## collections

| Feldname | TEI-Pfad |
|----------|----------|
| id | `org @xml:id` |
| name | `org/orgName` |
| email | `org/desc @type="contact"/email` |
| website | `org/desc @type="contact"/ptr @type="website" @target` |
| description | `org/desc` |

## places

| Feldname | TEI-Pfad |
|----------|----------|
| id | `place @xml:id` |
| gnd_number | `place/ptr @type="gnd" @target` |
| name | `place/placeName` |

## provenance_attributions

| Feldname | TEI-Pfad |
|----------|----------|
| id | `history/provenance @xml:id` |
| comment | `history/provenance/p` |

## provenances

| Feldname | TEI-Pfad |
|----------|----------|
| subject_id | `placeName/orgName/persName @ref` |
| subject_type | `placeName/orgName/persName` |
| is_certain | `@cert` |
| comment | `history/provenance/p` |

## publications

| Feldname | TEI-Pfad |
|----------|----------|
| id | `biblStruct @xml:id` |
| title | `biblStruct/monogr/title` |
| short_title | `biblStruct/monogr/title @type="short"` |
| year | `biblStruct/monogr/imprint/date` |
| author | `biblStruct/monogr/author` |
| comment | `biblStruct/monogr/imprint/note` |

## references

| Feldname | TEI-Pfad |
|----------|----------|
| url | `ref @target` |
