# Datenmodellierung

Die Struktur des Datenmodells orientiert sich an der bisherigen Nutzungspraxis und entwickelt sie weiter. Im Mittelpunkt steht dabei nicht ein theoretisch ideales Modell, sondern die konkreten Anforderungen, die sich aus den Eingabemöglichkeiten für Benutzer:innen ergeben. Die Modellierung geht also von dem aus, was tatsächlich gebraucht und genutzt wird.

Eine vollständige Auflistung aller bisher vorhandenen Felder wäre wenig zielführend, da viele von ihnen veraltet oder überflüssig sind und dennoch in der Datenbank verblieben sind. Das erschwert den Überblick und führt zu einer unnötigen Komplexität. Stattdessen wurde bei der Neugestaltung ein praktischer Zugang gewählt: Im Vordergrund stehen diejenigen Informationen und Strukturen, die in der Anwendung eine Rolle spielen. Dadurch lassen sich nicht mehr benötigte oder doppelt erfasste Daten auf natürliche Weise aussortieren.

Ausgangspunkt der Modellierung war die Handschrift als zentrales Objekt der Datenbank. Von dieser Einheit aus wurde nach außen gearbeitet: Es wurde analysiert, in welchen Beziehungen die Datensätze zur Handschrift stehen, und wie sich diese Beziehungen durch eine Umstrukturierung vereinfachen, standardisieren oder systematisieren lassen. Dabei wurde auch erhoben, welche Datenfelder sich für eine Normalisierung eignen, um die Wiederverwendbarkeit von Inhalten zu erhöhen und Redundanzen zu vermeiden.

## Zentrale Objekte

Hierbei handelt es sich um die Objekte, die im Zentrum des Erkenntnisinteresses der Benutzer:innen liegt. Sie bilden die strukturelle Basis der Datenbank, auf der die restlichen Tabellen aufbauen.

### Handschriften

#### Handschriften-Code und Signatur

Das bisher verwendete Feld `libCode` setzte sich aus dem Bibliothekscode der Sammlung und dem internen Handschriftencode zusammen (z.B. `AT5000-308` für Klosterneuburg, Cod. 308) und diente in älteren Versionen von manuscripta.at als primäre ID. In einigen Fällen wird dieser zusammengesetzte Code noch heute zur Adressierung verwendet, etwa in URLs wie `https://manuscripta.at/hs_detail.php?ms_code=AT5000-308`. Dabei ist der Parametername `ms_code` irreführend, da er sich tatsächlich auf den veralteten `libCode` bezieht.

Mit der Neustrukturierung verliert `libCode` seine Funktion als Identifikator und wird entfernt, da er eine redundante Information enthält, die sich aus bestehenden Feldern ableiten lässt. Das Feld `ms_code` bleibt jedoch erhalten, um die Kompatibilität mit externen Verlinkungen weiterhin sicherzustellen.

Das Feld `shelfmark`, das die originale Signatur innerhalb der Sammlung bezeichnet, bleibt in seiner bisherigen Form bestehen.

#### Titel

Der Titel der Handschrift wird künftig im Feld `title` gespeichert. Dieses ersetzt die bisherige Bezeichnung `heading`, um eine einheitliche Benennung namensgebender Felder im gesamten Datenmodell zu gewährleisten. Die Funktion bleibt dabei unverändert: Das Feld enthält eine kompakte Bezeichnung der Handschrift.

#### Material

Materialangaben wurden bislang direkt über einen Fremdschlüssel in der Handschriftentabelle gespeichert. Dadurch war es nur möglich, jeweils ein einzelnes Material pro Handschrift zu erfassen. Um künftig auch komplexere Fälle abbilden zu können – etwa Codices, die sowohl Papier- als auch Pergamentlagen enthalten –, wird eine eigene Tabelle eingeführt.

Die Zuordnung des Materials erfolgt polymorph und erlaubt somit sowohl Verbindungen zu Handschriften als auch zu einzelnen Teilen. Diese neue Struktur ermöglicht eine konsistente Materialerfassung unabhängig davon, auf welcher Ebene die Information verfügbar ist. Bestehende Einzelzuweisungen werden in die neue Tabelle überführt.

#### Blatt-/Seitenanzahl

Die bisherigen Freitexteingaben zur Blattanzahl werden in strukturierter Form in eine eigene Tabelle ausgelagert. Statt direkt in der Handschriften-Tabelle gespeichert zu werden, erfolgt die Erfassung künftig als eigenständiger Datentyp mit eigener Struktur. Die Handschrift ist über eine n:m-Beziehung mit diesen Angaben verknüpft.

Jeder Eintrag dieser Tabelle enthält Angaben zur Anzahl, zur Einheit (Blatt oder Seite, definiert über ein Enum-Feld) und zur Position (z.B. Vorsatz, Hauptteil, Nachsatz). Diese Normalisierung erlaubt künftig eine verlässliche Auswertung und automatisierte Umfangsberechnung.

#### Abmessungen

Die Angaben zu Höhe und Breite einer Handschrift werden künftig nicht mehr direkt in der Handschriften-Tabelle geführt, sondern als eigenständiger Datentyp in einer eigenen Tabelle gespeichert. Dadurch wird es möglich, mehrere Maßangaben pro Handschrift zu erfassen – etwa bei fragmentarischen Überlieferungen oder unsicheren Größenangaben. Weitere Informationen zur Struktur dieses Datentyps finden sich im eigenen Kapitel „Abmessungen“.

#### Datierung

Die bisherige Struktur der Datierungsangaben wird im Wesentlichen beibehalten, allerdings in eine eigene, zentral verwaltete Tabelle überführt, die unter dem Namen `dates` geführt wird. Diese Tabelle ist polymorph aufgebaut und kann flexibel mit unterschiedlichen Entitäten wie Handschriften, Handschriftenteilen, Einbänden oder Provenienzattributionen verknüpft werden. Sie erlaubt sowohl die Angabe von exakten Daten als auch von Zeitspannen (z.B. von–bis) und unterstützt verschiedene Darstellungsformen wie „um“, „nach“ oder „zwischen“. Zusätzlich kann über ein separates Boolean-Feld dokumentiert werden, ob eine Datierung explizit über eine Quelle (z.B. Kolophon) belegt ist. Jene Quellen können zudem über ein Kommentarfeld ausgewiesen werden.

#### Provenienzzuordnungen

Provenienzangaben werden künftig nicht mehr direkt innerhalb des Handschriftendatensatzes gepflegt, sondern über eine eigene Entität – die Provenienzzuordnung – verwaltet. Diese strukturelle Auslagerung ermöglicht eine deutlich flexiblere, differenziertere und wiederverwendbare Modellierung komplexer Besitzverhältnisse und Herkunftsangaben. Nähere Informationen zur Funktionsweise der Provenienzzuordnungen finden sich im entsprechenden Unterkapitel.

#### Entstehungsort

Angaben zum mutmaßlichen Entstehungsort einer Handschrift werden künftig nicht mehr direkt in der Handschriftentabelle gespeichert. Stattdessen erfolgt die Modellierung über einen Eintrag in der Tabelle der Provenienzen, der durch eine spezifische Provenienzzuordnung mit der Handschrift verbunden ist. Diese Zuordnung kennzeichnet den Provenienzeintrag ausdrücklich als Entstehungsort.

Diese Auslagerung ermöglicht es, auch beim Entstehungsort Unsicherheiten zu dokumentieren und alternative Orte anzugeben – etwa zur Unterscheidung zwischen plausibler Herkunft und belegtem Produktionsort. Die Struktur entspricht damit jener der Besitz- oder Aufbewahrungsprovenienzen und fügt sich nahtlos in das bestehende Provenienzmodell ein.

Für Bearbeiter:innen und Benutzer:innen ergibt sich daraus keine Änderung im Umgang mit diesen Angaben – abgesehen von einer konsequent normalisierten Datenstruktur.

#### Sprache

Sprachangaben werden künftig nicht mehr in Form von abgekürzten Einzelbuchstaben gespeichert, sondern über eine eigene Zwischentabelle verwaltet. Diese Tabelle bildet eine n:m-Beziehung zwischen Handschriften und standardisierten Sprachangaben ab. Grundlage hierfür ist eine eigene Tabelle `languages`, die ISO-konforme Sprachcodes sowie Klartextbezeichnungen enthält.

Die Zwischentabelle ermöglicht die Verknüpfung einer Handschrift mit mehreren Spracheinträgen. Dadurch wird die bisherigen Zeichenkettenstruktur abgelöst.

Für Benutzer:innen und Bearbeiter:innen ändert sich durch diese Normalisierung nichts im praktischen Umgang: Die Eingabe bleibt gewohnt einfach, die Anzeige klar strukturiert – die technische Umstellung dient primär der Transparenz, Nachvollziehbarkeit und Systematisierung im Hintergrund.

#### Überlieferungsform

Die Überlieferungsform einer Handschrift wird über ein kontrolliertes Vokabular gepflegt. Dieses Vokabular bleibt gegenüber der bisherigen Implementierung unverändert, wird jedoch technisch als Enum-Feld innerhalb der Datenbank umgesetzt.

#### Format

Das physische Format einer Handschrift – etwa Quart, Oktav oder Folio – beschreibt die Faltung und das ursprüngliche Maß der Lagen und gehört zu den grundlegenden formalen Eigenschaften eines Codex. Diese Angabe wurde bereits bislang in strukturierter Form erfasst, jedoch über eine ID referenziert, die programmatisch aufgelöst und nicht in einer separaten Tabelle gepflegt wurde.

In der neuen Struktur wird das Format als kontrolliertes Vokabular direkt innerhalb der Handschriftentabelle über ein Enum-Feld abgebildet.

#### Olim-Signaturen

Olim-Signaturen – also frühere Signaturen einer Handschrift – stellen eine wertvolle Quelle für die Rekonstruktion ihrer Sammlungsgeschichte dar. Sie dokumentieren frühere Ordnungszusammenhänge, Besitzvermerke oder institutionelle Zuordnungen und erlauben Rückschlüsse auf die historische Struktur eines Bestandes.

Um diese Informationen systematisch zu erfassen, wird für jede Olim-Signatur ein Eintrag in einer eigenen Tabelle erstellt, der der Handschrift direkt zugeordnet ist. Dieser Eintrag kann um weitere Informationen, etwa die Datierung und institutionelle Zuordnung angereichert werden.

#### Historische Beschreibung

Für die Beschreibung der Geschichte einer Handschrift steht weiterhin ein Freitextfeld zur Verfügung. Inhaltlich bleibt die Funktion unverändert, jedoch wird die bisher verwendete HTML-Kodierung in Markdown überführt.

#### Kodikologische Beschreibung

Das derzeitige Feld `gesbeschreibung` enthält Informationen, die der Sigle B der traditionellen Handschriftenbeschreibung entsprechen. Darunter finden sich typischerweise Angaben zur Foliierung und die Lagenformel.

Eine Herauslösung der rund 700 bereits erfassten Lagenformeln in ein eigenes Feld erscheint sinnvoll. Es handelt sich dabei um eine wichtige Angabe zur inneren Struktur einer Handschrift, die eine bedeutende Zusatzinformation zum Digitalisat bietet. Durch die Auslagerung kann eine Eingabehilfe für die komplexe Formatierung der Lagenformeln bereitgestellt werden.

#### Literatur und Kataloge

Literaturverweise und Katalogeinträge werden – wie bisher – über eine Zwischentabelle mit den jeweiligen Handschriften verbunden. Diese Struktur erlaubt die Auswahl eines Titels aus der zentralen Publikationstabelle sowie die Ergänzung weiterer Angaben wie Seitenzahlen oder eines Direktlinks zum Volltext oder Digitalisat.

#### Teile

Teile bleiben auch im neuen Datenmodell ein eigenständiger Datentyp und sind jeweils einer Handschrift eindeutig zugeordnet. Die Beziehung zwischen Handschrift und Teil ist weiterhin als 1:n-Struktur modelliert, wodurch eine Handschrift aus einem oder mehreren kodikologischen Teilen bestehen kann.

#### Initien/Texte

Texte und Initien bilden zusammen eine mehrstufige Beschreibungseinheit innerhalb einer Handschrift. Zwischen Handschriften und Texten besteht eine 1:n-Beziehung: Eine Handschrift kann über Textzuordnungen mehrere Texte enthalten. Zwischen Textzuordnungen und Initien liegt ebenfalls eine 1:n-Beziehung vor, da ein einzelner Text in einer Handschrift durch mehrere Initien vertreten sein kann – etwa bei Auszügen, Wiederholungen oder strukturell getrennten Abschnitten desselben Werkes.

Da das Modul „Texte“ erst nachträglich in manuscripta.at eingeführt wurde, besteht für viele bereits erfasste Initien noch keine explizite Verknüpfung mit einem Text. In etwa der Hälfte der Fälle fehlt derzeit diese Verbindung vollständig. Die intendierte Struktur – Handschrift → Textzuordnungen (Texte) → Initien – lässt sich daher in der gegenwärtigen Datenlage noch nicht konsistent umsetzen.

Die bestehende Struktur bleibt zunächst erhalten, wobei eine schrittweise Nachverknüpfung der Initien mit den entsprechenden Texten vorgesehen ist.

#### Einband

Obwohl zwischen einer Handschrift und ihrem Einband in der Regel eine 1:1-Beziehung besteht, wird der Einband im neuen Datenmodell weiterhin als eigenständiger Datentyp geführt. Die Einbandinformationen werden nicht direkt in der Handschriftentabelle gespeichert, sondern in einer separaten Tabelle verwaltet. Diese Entscheidung trägt dem Umstand Rechnung, dass Einbände eine eigene Datierung, eigenständige Merkmale sowie potenzielle Provenienzen aufweisen können, die unabhängig von der Handschrift selbst modelliert und ausgewertet werden sollen.

### Teile/Fragmente

#### Typ

Teile und Fragmente werden im Datenmodell gemeinsam in einer einzigen Tabelle erfasst, da sich ihre Struktur und die erforderlichen Eingabefelder weitgehend decken. Zur Unterscheidung diente bislang ein Textfeld mit den Werten „HT“ (für Handschriftenteile) und „F“ (für Fragmente). Diese Kennzeichnung wird künftig durch ein klar benanntes Boolean-Feld ersetzt, das angibt, ob es sich bei dem betreffenden Eintrag um ein Fragment handelt (`is_fragment`).

#### Kennzeichnung

Das Feld `part` erlaubt die Angabe einer Kennzeichnung eines Teils oder Fragments. Dieses wird in `title` umbenannt.

#### Beschreibstoff

Ein kodikologischer Teil einer Handschrift kann aus mehreren unterschiedlichen Materialien bestehen – etwa aus Papier und Pergament innerhalb eines einzigen Blocks. Um dieser Realität gerecht zu werden, wird die bisherige 1:n-Beziehung zwischen Teilen und Material auf eine n:m-Beziehung erweitert. Die Umsetzung erfolgt über eine eigene polymorphe Beziehung der Material-Tabelle, die es ermöglicht, jedem Teil mehrere Materialien zuzuordnen.

#### Blatt-/Seitenangaben

Die Blatt- und Seitenangaben eines Handschriftenteils werden derzeit in einem Freitextfeld erfasst. Eine Normalisierung dieser Angaben – etwa zur präzisen Verlinkung einzelner Blätter mit digitalen Reproduktionen – wäre grundsätzlich sinnvoll, würde jedoch einen erheblichen manuellen Aufwand erfordern. Da der Mehrwert in Relation zum Aufwand derzeit als zu gering eingeschätzt wird, bleibt das Feld vorerst in seiner bisherigen Freitextform bestehen.

#### Datierung

Die Datierung wird aus Gründen der Wiederverwendbarkeit und Flexibilität in eine eigene Tabelle ausgelagert. Dadurch kann sie unabhängig mit verschiedenen Entitäten wie Handschriften, Teilen oder Einbänden verknüpft werden. Ein ergänzendes Boolean-Feld erlaubt es, Einträge als ausdrücklich datiert zu kennzeichnen.

#### Ort/Region

Die bislang als Freitextfeld erfassten Angaben zu Ort und Region sollen künftig normalisiert und strukturiert werden. Da sich ein kodikologischer Teil auf mehrere Orte gleichzeitig beziehen kann – etwa im Fall unsicherer Zuschreibungen – wird eine flexible Mehrfachzuordnung ermöglicht. Zusätzlich sollen Unsicherheiten explizit erfasst werden können. Für die Umsetzung wird die bereits bestehende Tabelle für Provenienzstellen genutzt, wodurch auch diese Angaben in einheitlicher Weise mit anderen ortsbezogenen Informationen verknüpft werden können.

#### Schrift

Jedem kodikologischen Teil können mehrere paläographische Beschreibungen zugeordnet werden. Diese sind in einer eigenen Tabelle erfasst und stehen in einer 1:n-Beziehung zum jeweiligen Teil. Auf diese Weise lassen sich unterschiedliche Schreiberhände, Schriftarten oder Entwicklungen innerhalb eines Teils differenziert dokumentieren.

#### Ausstattung

Die Kategorie „Ausstattung“ erfasst die kunsthistorischen Merkmale eines kodikologischen Teils. Dazu gehört unter anderem die Angabe, ob der Teil Illuminationen enthält. Zusätzlich kann aus mehreren Ausstattungskategorien ausgewählt werden (siehe Datentyp „Ausstattung“), die über eine 1:n-Beziehung mit dem jeweiligen Teil verknüpft sind. Auch Zuschreibungen an einen oder mehrere Buchmaler sind möglich.

Eine Erweiterung dieser Struktur stellt die Möglichkeit dar, einzelne Teile mit Iconclass-IDs zu versehen. Hierfür wird eine eigene Zuordnungstabelle eingeführt, die eine n:m-Beziehung zwischen Handschriftenteilen und Iconclass-Einträgen abbildet. Die Verknüpfung kann zusätzlich durch Seiten- bzw. Folioangaben präzisiert werden, um einzelne Darstellungen innerhalb des Digitalisats gezielt zu referenzieren. Dieses kontrollierte Vokabular erlaubt eine systematische ikonographische Erschließung und unterstützt die gezielte Auswertung nach dargestellten Motiven und Themenfeldern.

Die bestehenden textbasierten Felder zur Beschreibung – `freitext`, `bildprogramm` und `kunsthist_kommentar` – bleiben erhalten, werden jedoch künftig unter angepassten Feldnamen fortgeführt.

### Texte

Die Tabelle der Texte dient der strukturierten Erfassung von Werken, die in Handschriften überliefert sind. Sie ersetzt die bisher redundante und teils uneinheitliche Speicherung von Textinformationen in verschiedenen Tabellen.
Ziel der neuen Tabelle ist es, diese Redundanzen zu beseitigen, indem Texte anhand ihrer Werktitel sowie optionaler Autorenzuordnungen zentral erfasst werden. Bestehende Einträge werden analysiert und anhand von Titelgleichheit und übereinstimmender Personenzuordnung zusammengeführt.
Ein Textdatensatz enthält mindestens einen Werktitel. Optional können außerdem folgende Angaben erfasst werden:

- eine Verknüpfung zu einer Person mit entsprechender Funktionsbezeichnung
- eine Datierung, sofern das Werk historisch eingeordnet werden kann.

### Textüberlieferungen

Die Tabelle der Textüberlieferungen bildet die Verbindung zwischen einem Text, einer konkreten Handschrift und darin enthaltener Initien. In der Regel handelt es sich bei diesen Einträgen nicht um originale Werke, sondern um Abschriften. Um diesen Unterschied explizit zu erfassen, steht ein Boolean-Feld zur Verfügung, über das vermerkt werden kann, ob es sich bei der überlieferten Fassung um eine Erstfassung handelt.

Zusätzlich besteht die Möglichkeit, eine bekannte Vorlage der Abschrift zu verknüpfen – sofern diese ebenfalls in der Datenbank erfasst ist. Diese Verknüpfung erfolgt über eine eigene Abschriften-Tabelle, die zwei Textüberlieferungen miteinander in Beziehung setzt: die spätere Abschrift und ihre angenommene oder nachgewiesene Vorlage. Auf diese Weise lassen sich Überlieferungsnetzwerke und textgeschichtliche Abhängigkeiten systematisch abbilden, die bislang in der Struktur nicht darstellbar waren.

Ein Kommentarfeld erlaubt die Hinterlegung weiterer Informationen zur jeweiligen Textfassung – etwa zur Vollständigkeit oder zur sprachlichen Fassung.

### Initien

Die Tabelle Initien dient der Erfassung von Anfängen und Abschlüssen von Textabschnitten in Handschriften und stellt damit ein zentrales Werkzeug zur inhaltlichen Erschließung dar.

Jeder Eintrag umfasst folgende Felder:
- Initium: Der Anfang des Textes (in der überlieferten Form).
- Explicit: Das Textende, sofern vorhanden.
- Folioangabe: Die Position innerhalb der Handschrift (z.B. „24r–25v“ oder „132r“).
- Verknüpfung zur Handschrift: Die Initien sind jeweils einem konkreten Codex zugeordnet.

Verknüpfung zur Textüberlieferung: Sofern der Text identifizierbar ist, kann das Initium einem Eintrag in der Textüberlieferungen-Tabelle zugeordnet werden. Diese Beziehung ist optional.

Zusätzlich besteht die Möglichkeit, eine oder mehrere Sprachen über eine eigene Zwischentabelle mit der Sprachentabelle zu verknüpfen. Dies ist insbesondere bei mehrsprachigen Abschnitten oder unklaren Sprachlagen relevant.

### Einbände

#### Stil

Die stilistische Einordnung eines Einbands erfolgt über ein standardisiertes Enum-Feld, das an die Stelle des bisherigen numerischen Codes tritt.

#### Schmuck

Einbandschmuck kann vielfältig sein und wird daher in einer separaten 1:n-Beziehung zum Einband abgebildet. Dies erlaubt die Zuweisung mehrerer Schmuckarten zu einem Einband und bietet zugleich die Möglichkeit, auf ein kontrolliertes Vokabular zurückzugreifen.

#### Besonderheiten

Besondere Merkmale des Einbands werden ebenfalls über ein Enum-Feld erfasst. So lassen sich auch seltene oder ungewöhnliche Eigenschaften systematisch dokumentieren und auswerten.

#### Restauriert

Ob ein Einband restauriert wurde, wird über ein Boolean-Feld vermerkt.

#### Ort/Region

Die Angabe zum Entstehungsort eines Einbands erfolgt über eine polymorphe Verknüpfung mit der Tabelle der Provenienzstellen.

#### Werkstatt/Buchbinder

Die Zuweisung zu einer Buchbinderwerkstatt erfolgt über einen eigenen Datentyp, der zusätzlich mit einer Institution verknüpft sein kann. Eine Verlinkung mit der Einbanddatenbank der Staatsbibliothek zu Bayern wird ermöglicht.

#### Datierung

Die Datierung eines Einbands wird – wie bei anderen datierbaren Entitäten – über die zentrale Tabelle für Datierungen abgebildet. Dies ermöglicht eine einheitliche Struktur und erleichtert die Auswertung über unterschiedliche Objekttypen hinweg. Die Angabe kann exakt oder unsicher sein und umfasst bei Bedarf auch ein Datumsspektrum.

#### Einbandfragmente/Makulatur

Die Verwendung von Makulatur oder eingebundenen Fragmenten wird über ein Boolean-Feld erfasst, das angibt, ob im Einband entsprechendes Material nachgewiesen wurde.

### Digitalisate

Im neuen Datenmodell werden Digitalisate nicht mehr systemintern gespeichert oder verwaltet. Stattdessen erfolgt die Einbindung vollständig über externe IIIF-Schnittstellen (International Image Interoperability Framework). Diese Architektur erlaubt es, Digitalisate unabhängig von ihrer physischen Speicherung dynamisch in die Oberfläche einzubinden.

Aus diesem Grund entfällt die Notwendigkeit einer eigenen komplexen Datenstruktur zur Abbildung von Bildpfaden oder Metadaten. Stattdessen gibt es eine reduzierte, strukturierte Tabelle, die lediglich der Verknüpfung von IIIF-Manifests mit Handschriften dient.

Jeder Eintrag in der Digitalisat-Tabelle enthält:
- den IIIF-Manifest-Link, über den das vollständige Digitalisat eingebunden wird,
- eine Verknüpfung zur jeweiligen Handschrift,
- einen statischen Direktlink zur ersten Seite (z.B. 001r), der zur Erzeugung eines Vorschaubildes (Thumbnail) verwendet wird.

### Literatur/Kataloge

Die zentrale Tabelle für Literatur und Kataloge erfasst alle bibliographischen Ressourcen, die im Rahmen der Datenbank zitiert oder verknüpft werden – darunter Forschungsliteratur, gedruckte Handschriftenkataloge, Editionen, Nachträge und digitale Veröffentlichungen.

Ein Eintrag umfasst folgende Informationen:
- Autor / Herausgeber: Verantwortliche Person(en) für das Werk. Eine Normalisierung der Autorendaten ist aufgrund des hohen Arbeitsaufwandes zu diesem Zeitpunkt wenig zielführend.
- Sachtitel-Kennzeichnung: Ein Boolean-Feld gibt an, ob der Titel ein Sachtitel (anstelle eines regulären Werktitels) ist.
- Titel: Vollständiger Titel der Publikation.
- Jahr: Erscheinungsjahr, sofern bekannt.
- Kurzzitat: Standardisierte Kurzform für Referenzen innerhalb der Datenbank.
- Anmerkungen: Freitextfeld für zusätzliche Angaben.
- Publikationstyp: Ein Enum-Feld, das die Publikation einer Kategorie zuordnet, wie etwa “Katalog”, “Monographie” oder “Artikel”.

Die bisherige Freitexterfassung der Sprachen wird durch eine normalisierte Lösung ersetzt: Die Sprachen werden künftig über eine Zwischentabelle mit der zentralen Sprachentabelle `languages` verknüpft.

## Inhaltliche Merkmale und Erschließung

### Blatt-/Seitenzahlen

Die bisherigen Angaben zur Blatt- oder Seitenanzahl einer Handschrift wurden als unstrukturierte Zeichenketten in einem Freitextfeld gespeichert. Diese Lösung war fehleranfällig, da sie falsche Eingaben ermöglichte und machte eine systematische Auswertungen unmöglich. Künftig wird die Blatt-/Seitenanzahl als eigener Datentyp modelliert und in einer separaten Tabelle gespeichert.

Die neue Tabelle steht in einer n:m-Beziehung zu Handschriften. Ein einzelner Eintrag enthält die gezählte Einheit (z.B. „12“, „I“, „I*“), die verwendete Zähleinheit (Blatt oder Seite) sowie bei Bedarf einen erläuternden Kommentar. Die Reihenfolge der Einträge ist rekonstruierbar, sodass die gewohnte Darstellung wie „I, 12, 436, I* Bl.“ weiterhin möglich bleibt.

Diese Normalisierung verbessert nicht nur die Konsistenz, sondern erlaubt auch automatische Umfangsberechnungen und erweiterte Filteroptionen in der Benutzeroberfläche. Bestehende Freitextangaben können zu einem hohen Prozentsatz in das neue Format überführt werden.

### Abmessungen

Die Abmessungen einer Handschrift werden künftig als eigener Datentyp behandelt und in einer separaten Tabelle gespeichert. Diese Struktur trägt dem Umstand Rechnung, dass die Buchblöcke mittelalterlicher Codices nicht immer einheitlich zugeschnitten sind. Es können mehrere Einträge pro Handschrift bestehen, die unterschiedliche Messungen oder Angaben zur Schätzung enthalten.

Jeder Eintrag enthält:
- Länge und Breite in Millimetern, jeweils als Minimal- und Maximalwert
- eine Markierung, ob es sich um eine ungefähre Angabe handelt
- einen Hinweis, ob sich die Maße auf ein Fragment beziehen
- einen optionalen Kommentar zur Erläuterung

So kann beispielsweise der Eintrag „ca. 410/415x290/295“ aufgeschlüsselt werden in zwei Einträge (“410x290” und “415x295”), die beide als „ungefähr“ markiert sind. Diese Normalisierung erlaubt künftig die Filterung nach physischer Größe oder die statistische Auswertung innerhalb eines Korpus. Bestehende Maßangaben wurden geprüft und können zu einem hohen Prozentsatz automatisch transformiert werden.

### Material

Die Tabelle `materials` erfasst Materialzuweisungen zu verschiedenen Entitäten (z.B. Handschriften oder deren Teile) über eine polymorphe Beziehung. Anstelle einer separaten Referenz auf eine eigenständige Materialdefinition wird die Materialart direkt in einem Enum-Feld type gespeichert, das gängige Beschreibstoffe wie Pergament oder Papier.

### Schrift

Die Schriftbeschreibungen erfassen paläographische Merkmale auf Ebene der kodikologischen Teile. Sie stehen in einer 1:n-Beziehung zu diesen Teilen, sodass ein Teil mehrere unterschiedliche Schriftbeschreibungen aufweisen kann – etwa bei einem Wechsel der Schreiber oder bei unterschiedlichen Schriftarten innerhalb desselben Abschnitts.

Die bestehende Feldstruktur wird grundsätzlich beibehalten, jedoch technisch konsolidiert: Die bislang als numerische ID gespeicherten Angaben zu Schrifttyp und Notation werden künftig als kontrollierte Vokabulare (Enum-Felder) umgesetzt, wodurch sie klarer interpretierbar und direkt validierbar sind.

Bisher als Zahlen codierte Eigenschaften wie das Vorhandensein von Marginalien, Glossen oder Geheimschrift werden künftig als Boolean-Felder mit optionalem Zustand geführt. Die bisher verwendete Codierung „9“ für „nicht erfasst“ entfällt und wird durch null ersetzt, was dem Standard eines `nullable` Boolean-Felds entspricht. In der Benutzeroberfläche muss dies durch eine dreiwertige Auswahlmöglichkeit oder entsprechende Kennzeichnung berücksichtigt werden.

Zusätzlich ist eine optionale Verknüpfung mit einem Personendatensatz möglich – etwa zur Angabe eines bekannten oder vermuteten Schreibers. Unsicherheiten in der Zuschreibung können über ein entsprechendes Feld kenntlich gemacht werden.

### Ausstattung

Die Tabelle zur Ausstattungskategorisierung dokumentiert, welche kunsthistorisch oder funktional relevanten Ausstattungselemente einem bestimmten Handschriftenteil zugeordnet sind. Die Einträge stehen jeweils in einer 1:n-Beziehung zu den kodikologischen Teilen. Die möglichen Kategorien werden über ein kontrolliertes Vokabular (Enum-Feld) definiert. So wird sichergestellt, dass die Erfassung einheitlich, auswertbar und erweiterbar bleibt.

### Abschriften

Die Tabelle Abschriften dient der modellhaften Darstellung textlicher Abhängigkeiten innerhalb des Bestands. Sie erlaubt es, eine Verbindung zwischen zwei Textüberlieferungen zu dokumentieren – typischerweise zwischen einer späteren Abschrift und ihrer angenommenen oder nachgewiesenen Vorlage. Beide Enden dieser Beziehung verweisen auf Einträge in der Tabelle der Textzuordnungen, also auf konkrete Texte in spezifischen Handschriften.

Ziel dieser Struktur ist es, Überlieferungszusammenhänge sichtbar zu machen, die über herkömmliche Kataloginformationen hinausgehen. Durch die systematische Erfassung solcher Verknüpfungen lassen sich textgeschichtliche Netzwerke rekonstruieren und erstmals auf Datenbankebene abbilden und analysieren.

Jeder Eintrag in der Abschriften-Tabelle enthält:
- eine Referenz zur Vorlage (Textzuordnung)
- eine Referenz zur Abschrift (ebenfalls eine Textzuordnung)
- ein Kommentarfeld für zusätzliche Hinweise, etwa zur Art der Abhängigkeit, zur Unsicherheit der Zuschreibung oder zur Art der Vorlage (z.B. vollständig vs. fragmentarisch).

Die Zuordnung ist nicht zwangsläufig symmetrisch und kann auch mehrere Vorlagen für eine Abschrift oder umgekehrt mehrere Abschriften zu einer Vorlage erfassen.
Diese Tabelle eröffnet neue Möglichkeiten zur Analyse von Werküberlieferungen, Schreibstuben, klösterlichen Netzwerken und Abschriftentraditionen innerhalb des Korpus.

### Buchbinderwerkstatt 

Die `bookbinders` Tabelle bildet Buchbinderwerkstätten als eigenständigen Datentyp ab. Dies ermöglicht eine zweifache Anbindung: Zum einen kann eine Werkstatt mit einer übergeordneten Institution verknüpft werden, zum anderen wird durch die eigenständige Modellierung die Verlinkung mit externen Ressourcen, etwa der Einbanddatenbank der Staatsbibliothek zu Berlin, erleichtert. Neben jenen Verlinkungen umfasst die Tabelle die Bezeichnung der Werkstatt. Eine Datierung, etwa für den Tätigkeitszeitraum, kann optional verknüpft werden. 

### Einbandschmuck

Der Einbandschmuck wird in einer eigenen Tabelle erfasst, die in einer 1:n-Beziehung zum jeweiligen Einband steht. Damit kann ein Einband mit einer beliebigen Anzahl an Schmuckmerkmalen versehen werden. Die jeweilige Ausprägung des Schmucks – etwa Blindstempel, Streicheisenlinien, Supralibros oder Golddruck – wird über ein Enum-Feld festgelegt.

## Zeitliche und räumliche Verortung

### Datierungen

Die Tabelle `dates` dient der flexiblen Erfassung zeitlicher Angaben, die sich auf unterschiedliche Objekte im System beziehen können – etwa auf Handschriften, Teile, Einbände oder Provenienzzuordnungen. Die bisherige Systematik der Datierung bleibt vollständig erhalten; es erfolgt keine inhaltliche Änderung, sondern lediglich eine Auslagerung der Datumsangaben in eine eigene Tabelle.

Die bisherigen Felder für Anfang und Ende des Zeitraums sowie das zugehörige Datierungsformat werden übernommen. Die bisher verwendete ID für das Datierungsformat wird durch ein Enum-Feld ersetzt.

### Provenienzstellen

Die Tabelle provenances dient der Erfassung einzelner Herkunftsangaben und steht in einer n:m-Beziehung zu Provenienzzuordnungen. Sie verknüpft eine konkrete Provenienzstelle – etwa eine Institution, Sammlung, Person oder ein Ort – über eine polymorphe Beziehung mit einer bestimmten Provenienzzuordnung.

Dadurch lassen sich mehrere mögliche oder kombinierte Herkunftsangaben einer Handschrift abbilden, wie etwa bei unsicheren oder alternativen Zuschreibungen.

### Provenienzzuordnungen

Die Provenienz einer Entität wird über eine Tabelle verwaltet, die hier als „Provenienzzuordnung“ bezeichnet wird. Diese Tabelle verbindet eine Entität (z.B. Handschrift, Einband) mit einer oder mehreren potenziellen Provenienzstellen und erlaubt es, Veränderungen der Besitzverhältnisse einer Handschrift im Laufe der Zeit abzubilden.

Zu den gespeicherten Informationen gehören unter anderem:
- die Art der Herkunft (z.B. Institution, Sammlung, Ort oder Person),
- der Bezug zur Handschrift (z.B. Entstehungsort, Vorbesitzer),
- die zeitliche Einordnung (sofern vorhanden),
- ein Freitextkommentar zur Erläuterung oder Belegstelle,
- sowie eine Markierung, falls es sich um eine unsichere Angabe handelt.

Zudem kann angegeben werden, ob es sich bei mehreren Herkunftsangaben um alternative Möglichkeiten handelt, die sich gegenseitig ausschließen – beispielsweise durch die Kennzeichnung mit einem logischen „oder“.

Beispielhafte Umsetzung: Zwei mögliche Vorbesitzer einer Handschrift – beispielsweise Schnals, Kartause Allerengelberg oder Neustift, Augustiner-Chorherrenstift – würden jeweils als Provenienzen erfasst. Diese Provenienzen werden über polymorphe Verknüpfungen mit einer Provenienzzuordnung verbunden, die diese wiederum mit der Handschrift in Beziehung bringt. Die Provenienzzuordnung erfasst zudem, in welcher Beziehung die einzelnen Stellen zueinander stehen (z.B. als Alternativen durch "oder") und enthält gegebenenfalls eine zeitliche Einordnung sowie einen erläuternden Kommentar. Im beschriebenen Fall wären beide Angaben als unsicher markiert. Eine spätere, gesicherte Angabe wie der Übergang in die Österreichische Nationalbibliothek ab 1786 würde ebenfalls über eine eigene Provenienzzuordnung gespeichert, ergänzt durch ein Datum und erläuternde Anmerkungen.

### Olim-Signaturen

Die Tabelle `olims` dient der systematischen Erfassung alter Signaturen von Handschriften. Solche „Olim-Signaturen“ sind wichtige Hinweise zur Sammlungsgeschichte und Provenienz einer Handschrift.

Ein Eintrag in der Tabelle `olims` enthält die eigentliche Signatur als Freitext sowie ein optionales Kommentarfeld zur Kontextualisierung oder für einen Quellennachweis. Zusätzlich kann eine Datierung verknüpft werden, die den Zeitraum der Gültigkeit oder des Auftretens dieser Signatur angibt.

Die Verknüpfung zu einer Institution oder Sammlung erfolgt über eine polymorphe Beziehung. So kann nachvollzogen werden, aus welchem institutionellen Zusammenhang die frühere Signatur stammt.

## Normadatenbasierte Entitäten

### Personen

Die neue Personen-Tabelle führt zwei bislang getrennt geführte Tabellen zusammen: Zum einen die Tabelle `aut`, die bisher für die Textzuordnung verwendet wurde und primär Autorennamen enthielt, zum anderen die Tabelle `personen`, die insbesondere für Buchmaler:innen und andere Rollen genutzt wurde. Beide Tabellen enthielten bereits GND-Nummern, wodurch in einem Großteil der Fälle eine automatisierte Zusammenführung der Datensätze möglich ist.

Ziel dieser Vereinheitlichung ist es, alle natürlichen Personen, die im Zusammenhang mit einer Handschrift auftreten – sei es als Autor, Buchmaler, Schreiber, Vorbesitzer oder in einer anderen Funktion – in einer einzigen, strukturierten Tabelle zu erfassen.

Ein Eintrag in der Personen-Tabelle umfasst folgende Felder:
- Name: Der gebräuchliche oder bibliographisch relevante Name der Person.
- GND-Nummer: Eine Verknüpfung zu einem Normdatensatz (falls vorhanden), die eine eindeutige Identifikation ermöglicht.
- Biographische Angaben: Ein optionales Freitextfeld für Lebensdaten, Funktionen, Herkunft oder weitere kontextuelle Informationen.

### Sprachen

Die Tabelle `languages` enthält die standardisierten Sprachangaben, auf die sich Zuordnungen zu Handschriften, Teilen oder anderen Entitäten beziehen. Ziel ist es, alle sprachbezogenen Angaben zentral, einheitlich und auswertbar zu speichern.

Jeder Eintrag in dieser Tabelle umfasst:
- einen normierten ISO-Sprachcode (z.B. la, de)
- einen Klartextnamen der Sprache (z.B. "Latein", "Deutsch")

Diese zentral gepflegte Tabelle gewährleistet Konsistenz bei der Verwendung von Sprachangaben im System.

### Orte

Die Tabelle Orte dient der Erfassung geographischer Bezeichnungen auf Grundlage von Normdaten. Sie bildet die Grundlage für die strukturierte Verknüpfung aller ortsbezogenen Angaben im System – etwa bei Entstehungsangaben oder Provenienzen.

Die Einträge in der Ortstabelle werden möglichst aus der Gemeinsamen Normdatei (GND) übernommen und enthalten mindestens:
- den GND-Identifier,
- den normierten Ortsnamen,
- bei Bedarf geographische Koordinaten (zur Visualisierung oder Analyse).

### Institutionen

Die Tabelle `institutions` erfasst Körperschaften, die im Zusammenhang mit Handschriften eine Rolle spielen. Auch diese Einträge werden bevorzugt aus der GND übernommen.

Jede Institution ist mit mindestens folgenden Informationen hinterlegt:
- Name der Institution (normiert nach GND),
- GND-Identifier,
- optional: Ort (als Verknüpfung zur Ortstabelle),

Ein Teil der Institutionen ist bisher in der alten `personen`-Tabelle als juristische Person mitgeführt worden – etwa bei Buchmalern mit Angabe einer Klosterwerkstatt. Diese Einträge werden im Zuge der Neustrukturierung aus der bisherigen Tabelle herausgelöst und als eigenständige Institutionen erfasst. Damit wird eine klare Trennung zwischen natürlichen und juristischen Personen etabliert.

Institutionen können mit verschiedenen Datentypen in Beziehung gesetzt werden – z.B. als Provenienzstellen, Aufbewahrungsorte, Herausgeber, Werkstätten oder Träger bestimmter Funktionen in der Überlieferungsgeschichte.

### Iconclass 

Die Tabelle `iconclasses` enthält die von Iconclass definierten Bildkategorien, die der ikonographischen Erschließung dienen. Jeder Eintrag besteht mindestens aus dem Iconclass-Code und der zugehörigen Bezeichnung. Optional kann eine Verknüpfung zu einer übergeordneten Kategorie hergestellt werden, um die hierarchische Struktur des Iconclass-Systems abzubilden. Die Verbindung zu konkreten Handschriftenteilen erfolgt über eine Zwischentabelle, sodass einzelne Bildthemen präzise und mehrfach zugeordnet werden können. Dadurch wird eine systematische Auswertung ikonographischer Inhalte innerhalb der Datenbank möglich.

## Strukturelle Gruppierung und Kontext

### Handschriftensammlungen

Die Tabelle der Sammlungen dient der strukturierten Erfassung einzelner Bestände innerhalb einer Institution, insbesondere dann, wenn diese Institution mehrere getrennt verwaltete oder historisch gewachsene Einheiten umfasst – etwa eine Stiftsbibliothek und ein Stiftsarchiv.

Im neuen Datenmodell sind Sammlungen einer Institution untergeordnet, d.h. jede Sammlung verweist auf genau eine übergeordnete Institution. Der bisher zur Sammlung gehörende Bibliothekscode (z.B. AT5000) wird stattdessen der Institution zugewiesen. Diese Trennung erlaubt es, inhaltlich oder verwaltungstechnisch getrennte Bestände innerhalb derselben Institution differenziert zu erfassen, ohne neue Codes einführen zu müssen. Dadurch lassen sich etwa die Stiftsbibliothek und das Stiftsarchiv von Klosterneuburg getrennt darstellen, obwohl sie unter demselben Institutionscode zusammengeführt sind.

Jede Sammlung umfasst die folgenden Informationen:
- Titel der Sammlung.
- Kontaktdaten: eine zentrale oder bestandsbezogene E-Mail-Adresse.
- Webseite: Link zur offiziellen Seite der Sammlung oder eines entsprechenden Informationsangebots.
- Beschreibung: Freitextfeld für weiterführende Informationen zur Sammlungsgeschichte, inhaltlichen Ausrichtung oder Besonderheiten.

### Sachgruppen

Zur inhaltlichen Strukturierung und thematischen Erschließung des Gesamtbestands können Handschriften künftig sammlungsübergreifend Gruppen zugewiesen werden. Diese sogenannten Sachgruppen dienen der Klassifikation nach thematischen, funktionalen oder typologischen Kriterien – etwa „Missale“, „Kirchenreformen im 15. Jahrhundert“ oder „Textzeugen des Thomas von Aquin“.

Technisch erfolgt die Umsetzung über eine eigene Tabelle mit frei definierbaren Titeln und optionalen Beschreibungen. Die Verknüpfung zwischen Handschrift und Gruppe wird über eine separate Zwischentabelle abgebildet, die eine n:m-Beziehung ermöglicht: Eine Handschrift kann mehreren Gruppen zugeordnet sein, und jede Gruppe kann beliebig viele Handschriften enthalten.

Diese Struktur erlaubt es, Sammlungsgrenzen zu überschreiten, spezifische Forschungsperspektiven abzubilden und gezielte Such- oder Filteroptionen bereitzustellen.

## Beziehungs- und Zuordnungstabellen

### Personenzuordnungen

Die Tabelle der Personenzuordnungen ermöglicht die strukturierte Verknüpfung von Personen mit verschiedenen Entitäten im System. Sie bildet damit das zentrale Bindeglied zwischen der Personentabelle und allen Rollen, die Personen im handschriftlichen Kontext einnehmen können.

Zuordnungen sind durch folgende Elemente definiert:
- die verknüpfte Entität (z.B. Teil, Schrift),
- der verknüpfte Personeneintrag (aus der zentralen Personen-Tabelle),
- eine Funktionsbezeichnung (z.B. Schreiber, Buchmaler) – geführt als Enum,
- optional: ein Unsicherheitsindikator (z.B. Boolean-Feld „fraglich“),
- optional: ein Kommentarfeld für weitere Informationen.

Die Personenzuordnungen sind polymorph aufgebaut: Eine Person kann mit beliebig vielen Objekten verknüpft sein, und jede Entität kann mit mehreren Personen in unterschiedlichen Rollen assoziiert werden.

### Materialzuordnungen

Die Tabelle der Materialzuordnungen dient der flexiblen Erfassung von Beschreibstoffen in der Datenbankstruktur. Sie erlaubt die Verbindung von Materialeinträgen mit unterschiedlichen Entitätstypen – insbesondere mit Handschriften und deren Teilen – über eine polymorphe n:m-Beziehung.

### Handschrift-Sprachen-Zuordnungen

Die Zuordnung von Sprachen zu Handschriften erfolgt über eine eigene Zwischentabelle. Diese bildet eine n:m-Beziehung zwischen den Entitäten und der zentralen Sprachentabelle `languages` ab und ersetzt damit die bisherige Praxis, Sprachangaben als verdichteten Buchstabencode in einem Textfeld der Handschriftentabelle zu speichern (z.B. „LD“ für Latein und Deutsch).

Jede Sprachenzuordnung dokumentiert:
- die referenzierte Handschrift
- die zugeordnete Sprache aus der Sprachentabelle

### Literatur–Sprachen Zwischentabelle

Die Zuordnung von Sprachen zu Handschriften erfolgt über eine eigene Zwischentabelle. Diese bildet eine n:m-Beziehung zwischen den Entitäten und der zentralen Sprachentabelle `languages` ab und ersetzt damit die bisherige Praxis, Sprachangaben als verdichteten Buchstabencode in einem Textfeld der Handschriftentabelle zu speichern (z.B. „LD“ für Latein und Deutsch).

Jede Sprachenzuordnung dokumentiert:
- die referenzierte Handschrift
- die zugeordnete Sprache aus der Sprachentabelle

### Handschrift-Literatur-Zuordnungen

Die Zwischentabelle der Literaturzuordnungen bildet eine n:m-Verknüpfung zwischen Handschriften und Einträgen in der zentralen Publikationstabelle. Sie ermöglicht die präzise Angabe, welche Literatur sich auf welche Handschrift bezieht.

Neben der reinen Verknüpfung können zusätzliche Informationen erfasst werden, darunter:
- die Seitenzahl oder der Seitenbereich, auf den sich der Verweis bezieht,
- ein Direktlink zur digitalen Version der Quelle (sofern vorhanden).

### Handschrift–Sachgruppen Zwischentabelle

Die Zuordnung von Handschriften zu Sachgruppen erfolgt über eine eigene Zwischentabelle, die eine n:m-Beziehung zwischen den beiden Entitäten abbildet. Jede Zeile der Tabelle verbindet eine Handschrift mit einer Sachgruppe und ermöglicht so eine flexible, sammlungsübergreifende Gruppierung.

### Iconclass-Zuordnung 

Die Zwischentabelle `iconclass_parts` bildet eine n:m-Beziehung zwischen Handschriftenteilen und Einträgen in der Iconclass-Tabelle. Sie ermöglicht es, einem Teil mehrere ikonographische Kategorien zuzuweisen und gleichzeitig eine präzise Lokalisierung innerhalb des Objekts vorzunehmen – etwa durch die Angabe eines Folios oder Seitenbereichs. Zusätzlich kann ein Kommentarfeld genutzt werden, um nähere Informationen zur Darstellung oder zur Interpretation des Motivs zu hinterlegen. 