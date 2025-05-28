# Dokumentation: Entwicklungsprozess manuscripta.at

Diese Dokumentation fasst die laufende Neugestaltung des Datenmodells von *manuscripta.at* zusammen. Sie dient der Beschreibung der grundlegenden Struktur, der modellierten Entitäten sowie der zugrunde liegenden konzeptionellen Überlegungen.

Die Dokumentation gliedert sich derzeit in drei Bereiche:

- [**Technische Konventionen**](./technische-konventionen.md): Hinweise zu Modellierungsprinzipien und Namenskonventionen
- [**Modellbeschreibung**](./datenmodellierung.md): Überblick über Datentypen, Beziehungen und funktionale Konzepte  
- [**Tabellenübersicht**](./tabellen.md): Technische Darstellung der Tabellenstruktur mit Felddefinitionen und Datentypen

Die Dokumente werden fortlaufend ergänzt und überarbeitet. Ziel ist es, den Entwicklungsstand transparent zu dokumentieren und die interne wie externe Zusammenarbeit zu erleichtern.

Die Arbeiten erfolgen im Auftrag der [Österreichischen Akademie der Wissenschaften, Institut für Mittelalterforschung](https://www.oeaw.ac.at/imafo/home/).

## Informationen zum geplanten Relaunch von manuscripta.at

*manuscripta.at* ist eine Plattform zur Erschließung mittelalterlicher Handschriften aus österreichischen Sammlungen. Seit ihrer Entstehung im Jahr 2009 wird sie kontinuierlich mit hochwertigen Katalogisaten befüllt und bildet damit eine zentrale Grundlage für die mediävistische Forschung. Nach dem umfassenden Relaunch im Jahr 2014 steht nun ein weiterer grundlegender Entwicklungsschritt bevor.

Ziel ist es, die Plattform zukunftsfähig zu gestalten, ohne dabei Bewährtes aufzugeben. Die technische Basis soll nachhaltig modernisiert, die langfristige Wartbarkeit gesichert und neue Schnittstellen geschaffen werden, um den Austausch mit anderen Datenbanken zu erleichtern. Standards wie TEI werden dabei ebenso berücksichtigt wie eine transparente Ausweisung von Autor*innenschaft.

Ein zentrales Anliegen ist die Sicherung der Inhalte:
- Die Digitalisate werden künftig in ein digitales Archivsystem (ARCHE) überführt, um eine langfristige Speicherung und Sicherung gemäß aktuellen Standards zu gewährleisten.
- Änderungen an den Handschriftenbeschreibungen werden automatisch im Hintergrund versioniert, sodass Bearbeitungsverläufe nachvollziehbar bleiben.
- Die Bildlizenzierung wird gemeinsam mit den sammlungsführenden Institutionen geklärt – mit dem Ziel transparenter und konsistenter Nutzungsbedingungen.

Gleichzeitig bleibt das oberste Prinzip: Der gewachsene Datenbestand soll in seiner Qualität und Aussagekraft vollständig erhalten bleiben.

### Häufig gestellte Fragen

**Gehen beim Relaunch Daten verloren?**  
Nein. Die über Jahre hinweg sorgfältig erhobenen Daten bilden das Herzstück von manuscripta.at. Ihr Erhalt hat im gesamten Entwicklungsprozess höchste Priorität. Änderungen an Struktur oder Darstellung erfolgen ausschließlich dort, wo sie zur besseren Verständlichkeit oder langfristigen Nutzbarkeit beitragen. Im Zuge der Arbeiten werden einzelne Datenfelder in ein standardisiertes Format überführt – eine Praxis, die bereits in den vergangenen Jahren erfolgreich erprobt wurde, etwa bei der Vereinheitlichung von Datierungen.

**Bleiben bestehende Permalinks gültig?**  
Ja. Manuscripta.at wird regelmäßig in wissenschaftlichen Publikationen zitiert – häufig über Permalinks. Um die langfristige Zitierfähigkeit der Plattform zu gewährleisten, wird bei der technischen Umstellung besonders darauf geachtet, dass bestehende Permalinks auch künftig zuverlässig zum Ziel führen. Geplante Änderungen an URL-Strukturen werden entsprechend abgefangen.

**Ist während der Entwicklungsphase mit Einschränkungen zu rechnen?**  
Nein. Die laufende Katalogisierung wird auch während der Entwicklungsphase uneingeschränkt fortgeführt. Die bestehende Plattform bleibt bis zur vollständigen Umstellung zugänglich. Lediglich in einer kurzen Übergangsphase – voraussichtlich in der ersten Jahreshälfte 2026 – ist ein temporäres Einfrieren notwendig, um den sicheren und vollständigen Datentransfer durchzuführen. 

**Gibt es Möglichkeiten zur Mitwirkung?**  
Ja, das Team der Abteilung für Schrift- und Buchwesen freut sich über Anregungen, Rückmeldungen und Austausch mit der Community. Auch eine aktive Beteiligung – etwa im Rahmen des Stakeholderprozesses oder von Tests – ist willkommen. Interessierte können jederzeit Kontakt aufnehmen, um mögliche Formen der Zusammenarbeit zu besprechen.

### Geplanter Zeitrahmen

**Frühjahr 2025**  
Beginn der Entwicklungsarbeiten: Analyse und Überarbeitung des bestehenden Datenmodells sowie erste Schritte zur Normalisierung des Datenbestands.

**Sommer 2025**  
Übertragung der bestehenden Daten in das neue System sowie Beginn der Archivierung der Digitalisate in der ARCHE. Erste Tests mit ausgewählten Teilbeständen unterstützen die Weiterentwicklung des Datenmodells.

**Herbst 2025**  
Entwicklung eines ersten Prototyps der neuen Plattform. Interne Tests der Benutzeroberfläche und Suchfunktionen beginnen. Gleichzeitig wird die Anbindung an die ARCHE weiter ausgebaut. 

**Frühjahr 2026**  
Externe Tests unter Einbindung von Partnerinstitutionen und Fachcommunity. Rückmeldungen fließen gezielt in die Weiterentwicklung ein.

**Sommer 2026**  
Abschluss der technischen Umsetzung und Vorbereitung des Plattformwechsels. In einer kurzen Übergangsphase wird die bestehende Plattform eingefroren, um einen sicheren und vollständigen Datentransfer zu gewährleisten.