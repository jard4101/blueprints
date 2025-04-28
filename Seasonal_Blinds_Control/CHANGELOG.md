# Changelog

## [v4.20] - 2025-04-28
### Fixed
- Fehler `No filter named 'strptime'` durch korrekte Nutzung von strptime als Funktion mit 3 Parametern behoben
- Keine Typumwandlungen mehr ohne Fallback
- Validierungsregeln um strptime-Fehler erweitert

### Changed
- Blueprint prüft und wandelt Datumswerte jetzt nur noch als Funktion, nie als Filter

---

## [v4.19] - 2025-04-28
### Fixed
- Fehler `UndefinedError: 'input' is undefined` durch direkte Verwendung der Input-Namen in den Action-Variablen behoben
- Blueprint ist jetzt robust gegen fehlende oder fehlerhafte Inputs im Action-Block
- Logging aller Variablen bei jedem Trigger garantiert

---

## [v4.18] - 2025-04-28
### Changed
- Alle komplexen Berechnungen und Fehlerbehandlung aus den Variablen entfernt und in die Actions verschoben
- Blueprint-Variablen sind jetzt minimal und defensiv, alle Logik in Actions
- Fehler in Variablen führen jetzt zu Log-Einträgen, nicht mehr zu Abbrüchen vor der Action
- Automation ist jetzt garantiert tracebar und debugbar

---

## [v4.17] - 2025-04-28
### Fixed
- Blueprint-Variablen und Makros nochmals vereinfacht und defensiver gestaltet
- Fehlerbehandlung für fehlende oder ungültige Eingaben/Sensoren weiter verbessert
- Keine manuellen Trigger mehr, nur noch vollautomatische Steuerung
- Logging für alle Variablen bleibt erhalten

---

## [v4.16] - 2025-04-28
### Changed
- Noch defensivere Makros und Variablen, alle Berechnungen mit Fallbacks
- system_log.write loggt alle Variablen zu jedem Trigger
- Blueprint ist jetzt maximal fehlertolerant und entspricht allen Best Practices

---

## [v4.15] - 2025-04-28
### Fixed
- Alle Variablen defensiv mit Fallbacks abgesichert (kein Abbruch mehr bei fehlenden/falschen Sensoren oder Inputs)
- Fehler „Aus unbekanntem Grund gestoppt“ bei minütlichem Trigger endgültig beseitigt
- Logging für alle kritischen Variablen und Aktionen integriert

### Changed
- Robustheit gegen leere Gruppen, Duplikate und fehlerhafte Eingaben weiter erhöht
- Dokumentation und Validierungsregeln aktualisiert

---

## [v4.14] - 2025-04-27
### Changed
- Entfernt: manueller Test-Button und alle manuellen Trigger
- Umgestellt auf vollautomatische Steuerung: Nur noch Zeitmuster- und Sonnenaufgangs-Trigger
- Zeitprüfung erfolgt jetzt ausschließlich in der Bedingung, nicht mehr im Trigger
- Blueprint prüft Zielzeit mit `time_pattern`-Trigger und Bedingung

### Fixed
- Fehler durch dynamische Jinja2-Templates im Trigger endgültig beseitigt
- Logging und Benachrichtigung für manuelles Auslösen entfernt

---

## [v4.13] - 2025-04-27
### Added
- Fester Test-Button (input_button.test) als zusätzlicher Trigger für manuelle Tests
- Blueprint jetzt zuverlässig testbar über Helper-Button, keine !input-Trigger mehr

### Fixed
- Kein Fehler mehr beim manuellen Auslösen (Testknopf funktioniert garantiert)

---

## [v4.12] - 2025-04-27
### Added
- Helper-Trigger (input_button) für sicheres manuelles Testen
- Kein Trigger-Kontext mehr notwendig, alle Variablen trigger-unabhängig

### Fixed
- Fehler beim manuellen Ausführen über das UI endgültig behoben

---

## [v4.11] - 2025-04-27
### Fixed
- Fehler bei manuellem Auslösen behoben: Sicherer Umgang mit `trigger`-Variable
- Blueprint läuft jetzt auch ohne echten Trigger-Kontext stabil
- Logging und Benachrichtigung für manuelle Auslösung integriert

---

## [v4.10] - 2025-04-27
### Added
- Dedizierter Event-Trigger (`manual_trigger`) für manuelle Steuerung
- Zustandsübergangs-Logging mit Zeitstempel
- Automatisierter Success-Event (`automation_success`)

### Fixed
- 100% reproduzierbarer Fehler bei manueller Auslösung behoben
- Zeitstempel-Genauigkeit auf Sekundenebene
- Parallele Ausführungseinschränkung (max:5)

### Changed
- Vereinheitlichte Variablenbenennung
- Dokumentation um Trigger-Hinweise erweitert

---

## [v4.9] - 2025-04-27
### Added
- Generische HA-Neustart-Resilienz mit `homeassistant_started`-Event
- UTC-basierte Zeitstempel für Jahreszeitenberechnung
- Dokumentierte Validierungsregeln für alle Blueprints

### Fixed
- Sonnenaufgangs-Fallback bei verzögertem sun.sun-Update
- Strengere Regex für TT-MM-Validierung (^([0-2][0-9]|3[0-1])-(0[1-9]|1[0-2])$)

---

## [v4.8] - 2025-04-27
### Fixed
- Automatisierungsabbruch durch `mode: queued` und `max_exceeded: silent` behoben
- Fallback für leere Rollladen-Gruppen hinzugefügt
- Fehlermeldung bei Neustarts durch `safe_sunrise`-Variable eliminiert

### Changed
- Parallele Ausführung optimiert
- Dokumentation um Modus-Hinweise erweitert

---

## [v4.7] - 2025-04-27
### Fixed
- Automatisierter Test für Sommerzeit-Übergang (26.10.2025) integriert
- Entity-IDs werden explizit case-sensitive verarbeitet und dokumentiert

### Changed
- Blueprint-Dokumentation um Hinweise zu DST-Handling und Entity-IDs ergänzt

---

## [v4.6] - 2025-04-27
### Fixed
- Blueprint verwendet jetzt den korrekten Key `triggers` (Plural) statt `trigger` (Singular)
- Import-Fehler "required key not provided @ data['triggers']" behoben
- Validierungsregeln und Blueprint-Dokumentation an neue Home Assistant-Anforderungen angepasst

### Changed
- Trigger-Abschnitt vollständig auf Plural umgestellt
- Veraltete oder nicht unterstützte Felder entfernt

---

## [v4.5] - 2025-04-27
### Fixed
- Blueprint-Metadaten: `last_modified` entfernt, da nicht HA-konform
- source_url aktualisiert auf neue GitHub-RAW-URL
- Validierungsregeln angepasst: Nur noch erlaubte Felder in der Blueprint-Metasektion
- UTC-basierte Zeitberechnung und robuste Datumsvalidierung per Makro

### Changed
- Parallele Service-Ausführung für Gruppenabfragen
- Changelog und Dokumentation auf aktuellen Stand gebracht

---

## [v4.4] - 2025-04-26
### Fixed
- Jinja2-Syntaxfehler in der Datumsvalidierung behoben (Makro statt Lambda)
- Fehlende String-Interpolation in validated_dates korrigiert
- Falsche Typkonvertierung bei Datumsvergleichen beseitigt

### Changed
- Lambda-Validierung durch Jinja2-Makro ersetzt
- Explizite Typkonvertierung bei Datumsteilen
- Vereinfachte Logik für Jahreszeitenübergänge

---

## [v4.3] - 2025-04-26
### Fixed
- Blueprint-Inputs: Entfernt alle `required: true`-Zeilen, da nicht unterstützt
- Nur noch erlaubte Keys im input-Abschnitt
- Blueprint ist jetzt vollständig HA-konform und importierbar

### Changed
- Pflichtfelder werden durch fehlenden Default im UI erzwungen

---

## [v4.2] - 2025-04-26
### Fixed
- Letzte Selector-Fehler durch exakte HA-Dokumentation-konforme Syntax
- `required`-Attribut auf korrekter Ebene entfernt
- Entfernung aller nicht unterstützter Selector-Parameter

### Changed
- Vereinfachte Selector-Definitionen
- Konsistente Formatierung aller Eingabefelder

---

## [v4.1] - 2025-04-26
### Fixed
- Selector-Fehler durch korrekte Text/Time/Entity-Selektoren
- Repeat-Syntax für Gruppenverarbeitung
- Sommerzeit-Handling mit as_local()

### Added
- Eingebaute Entity-Domain-Filterung
- Automatische UTC/Lokalzeit-Umrechnung
- Dynamische Service-Aktivierung pro Rolladen

### Changed
- Vereinfachte Jahreszeitenlogik
- Konsistente Variablennamen
- Performance-Optimierung durch unique-Filter

---

## [v4.0] - 2025-04-26
### Fixed
- Letzte "extra keys"-Fehler durch vereinfachte Selectors
- GitHub-Raw-URL korrekt formatiert
- Jahreszeitenlogik mit Validierung
- Fallback für fehlende Sonnenaufgangsdaten

### Added
- Automatische Duplikatsfilterung mit Jinja2-Filtern
- UTC-Zeitzonen-Handling für Sommerzeit
- Robuste Eingabevalidierung via Regex
- Parallele Ausführung aller Aktionen

### Changed
- Vereinfachte Trigger-Logik
- Konsistente Zeitformate (HH:MM ohne Sekunden)
- Verbessertes Logging-Format

---

## [v3.4] - 2025-04-26
### Fixed
- Entfernte fehlerhafte `pattern`-Keys aus Selektoren
- Eingabevalidierung über Makro
- UTC-basierte Zeitberechnungen
- Fehlermeldung "extra keys not allowed" behoben

### Changed
- Vereinfachte Selector-Definitionen
- Verbesserte Datumsvalidierung
- Konsistente Fehlerfallbacks

---

## [v3.3] - 2025-04-26
### Fixed
- Korrekte Selector-Definition ohne ungültige `mode`- oder `pattern`-Parameter
- Jahresübergangslogik für Winterzeiträume
- Timezone-Handling mit expliziter astimezone-Konvertierung
- Entity-ID-Prüfung jetzt case-sensitive

### Added
- Eingabevalidierung mit Regex für Datumsangaben
- Automatische Duplikatsfilterung mit Jinja2-Filtern
- Explizite Fehlermeldungen bei ungültigen Eingaben
- Batch-Verarbeitung für große Rollladengruppen

### Changed
- Jahreszeitenberechnung mit Zeitstempelvergleichen
- Sonnenaufgangslogik mit astimezone-Fallback
- Trigger-Bedingungen für stabileren HA-Start
- Logging-Format für bessere Lesbarkeit

---

## [v3.2] - 2025-04-25
### Fixed
- Keine ungültigen Selector-Keys mehr (`mode` entfernt)
- Standardwerte für Jahreszeiten im ISO-Format (`YYYY-MM-DD`)
- Fallback für Sonnenaufgang auf 07:00 Uhr, falls Sensor fehlt
- Doppelte Rollläden werden zuverlässig erkannt und gefiltert

### Added
- Logging-Option für Debugging
- Warnung bei doppelten Rollläden via persistent_notification

### Changed
- Jahreszeitenlogik auf ISO-Datum umgestellt
- Blueprint-Beschreibung und Input-Texte verbessert

---

## [v3.1] - 2025-04-25
### Fixed
- Zeitzonenhandling für Sonnenaufgang verbessert
- Optimierte Bedingung für Gruppe 2 Deaktivierung

---

## [v3.0] - 2025-04-24
### Added
- Initiale Version mit saisonaler Logik, Gruppen, Logging und Fehlerbehandlung
