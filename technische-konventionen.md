# Technische Grundlagen und Prinzipien

## Naming Conventions

Um eine konsistente, nachvollziehbare und wartbare Entwicklung zu gewährleisten, gelten für Datenmodellierung und Programmierung einheitliche Benennungsrichtlinien. Diese betreffen Tabellen und Felder ebenso wie Variablennamen, Funktionen und Schnittstellen.

Tabellennamen in der Datenbank sind durchgehend in englischer Sprache, kleingeschrieben und verwenden Unterstriche zur Trennung mehrerer Wörter (`snake_case`). Sie stehen im Plural, da sie in der Regel Mengen von Objekten repräsentieren – so etwa `manuscripts`, `persons` oder `institutions`. Zwischentabellen zur Realisierung von n:m-Beziehungen kombinieren die Namen beider verknüpften Tabellen in alphabetischer Reihenfolge, verbunden durch einen Unterstrich, wie etwa `manuscript_materials` oder `part_decorations`.

Auch Feldnamen folgen dem Muster `snake_case`. Primärschlüssel werden als id bezeichnet, während Fremdschlüssel das Schema `name_id` nutzen, etwa `person_id` oder `institution_id`. Wahrheitswerte (Booleans) erhalten präzise sprechende Präfixe wie `is_` oder `has_`, z.B. `is_fragment`, `has_glosses`. Zeitstempel folgen dem Konventionspaar `created_at` und `updated_at`.

Enumerationen (z.B. Schrift- oder Ausstattungstypen) werden als Enums geführt. Die möglichen Werte stehen in Kleinschreibung und verwenden ebenfalls Unterstriche, wie z.B. `textualis_formata`, `miniature` oder `penwork`. Die zugehörigen Feldnamen in der Datenbank verweisen explizit auf ihren Zweck, etwa `script_type` oder `binding_style`.

Auch für die Programmierung gelten klare Regeln. Die Projektsprache ist Englisch, die Zeichencodierung UTF-8. Magische Zahlen oder kryptische Kürzel sollen vermieden werden; Konstanten und Variablen erhalten selbstredende Namen, die ihren Verwendungszweck eindeutig beschreiben, sodass ihre Bedeutung aus dem Kontext ohne zusätzliche Kommentare ersichtlich ist.

Im Programmcode – unabhängig von der konkret verwendeten Sprache – orientieren sich Variablennamen an den Konventionen stark typisierter Umgebungen: sie sind in `snake_case` gehalten und spiegeln klar ihre Bedeutung wider. Konstanten hingegen stehen in `SCREAMING_SNAKE_CASE`, etwa `DEFAULT_LOCALE` oder `MAX_LENGTH`. Funktionsnamen beginnen mit einem Verb und folgen ebenfalls dem `snake_case`-Muster – z.B. `load_manuscript_data` oder `update_record`. Methoden zur Prüfung von Eigenschaften oder Zuständen nutzen die üblichen Präfixe wie `is_`, `has_` oder `can_`.

Strukturen und Aufzählungen (Structs, Enums) verwenden die `PascalCase`-Schreibweise: `Manuscript`, `TextWitness`. Die Felder innerhalb dieser Strukturen bleiben in `snake_case`. Gleiches gilt für Traits, die als Typbezeichner idealerweise in verbalem Stil benannt sind, wie `Renderable` oder `Loadable`.

Dateinamen und Modulbezeichner folgen ebenfalls `snake_case`. Abkürzungen sollen vermieden werden, mit Ausnahme gängiger technischer Kürzel wie `id`, `url` oder `http`.

REST-APIs sind pluralisch und kleingeschrieben aufgebaut. Routen lauten z.B. `/manuscripts`, Detailseiten `/manuscripts/{id}`, Unterobjekte entsprechend `/manuscripts/{id}/texts`. JSON-Schlüssel innerhalb der API entsprechen den Datenbankfeldnamen und bleiben in `snake_case`.

## Redundanzvermeidung und polymorphe Strukturen

Die bisherige Datenbank enthielt eine Vielzahl an redundanten Feldern, die vor allem der besseren Lesbarkeit der Datensätze für einen Anwendungsfall vorgesehen waren, bei dem Datensätze direkt in der Datenbank editiert wurden – ein Szenario, das mit der aktuellen Architektur nicht mehr vorgesehen ist. In der neugestalteten Struktur wird konsequent auf Redundanz verzichtet. Wiederholte Informationen werden nicht mehr mehrfach in verschiedenen Tabellen gespeichert, sondern systematisch ausgelagert und verknüpft. Dies verbessert nicht nur die Konsistenz der Daten, sondern reduziert auch Pflegeaufwand und Fehleranfälligkeit.

Um gleichzeitig eine größtmögliche Wiederverwendbarkeit zentraler Informationseinheiten zu ermöglichen, werden – wo sinnvoll – polymorphe Datentypen eingeführt. Diese erlauben die flexible Verknüpfung einer Entität mit unterschiedlichen anderen Objekten im Modell. Ein besonders markantes Beispiel ist die Datierung: Da Datierungsangaben sich auf unterschiedliche Ebenen beziehen können – etwa auf eine Handschrift insgesamt, einen Teil oder einen Einband – wird der Datierungseintrag als eigenständige Entität geführt, die über polymorphe Schlüssel mit den jeweiligen Objekten verbunden werden kann.