blueprint_validation:
  metadata:
    version: "5.3"
    author: "jard4101"
    compatibility: "Home Assistant 2025.4+ (letzte 4 Versionen)"
    required_fields:
      - name
      - description
      - domain
      - source_url
      - author
      - triggers
      - mode
      - max_exceeded
    forbidden_fields:
      - last_modified
      - custom_fields
      - trigger
    allowed_modes:
      - single
      - queued
      - parallel
    max_exceeded_values:
      - silent
      - error

  validation_rules:
    syntax_checks:
      required_fields:
        global:
          - name
          - description
          - domain
          - mode
          - max_exceeded
        triggers:
          - platform
      key_spelling:
        mandatory_plurals:
          - triggers
          - actions
          - variables
      allowed_metadata:
        - name
        - description
        - domain
        - source_url
        - author
        - input
        - variables
        - triggers
        - action
      methods:
        - "YAML-Validierung (Einrückung, Tabs vs. Leerzeichen)"
        - "!input-Referenzprüfung"
        - "Selector-Typen gemäß HA-Dokumentation"
        - "Unerlaubte YAML-Tags Prüfung"
      trigger_structure:
        required_keys:
          - platform
        allowed_platforms:
          - time
          - state
          - event
          - template
          - time_pattern

    input_validation:
      description: "Eingabeparameter-Edge-Cases"
      checks:
        - "Regex-Validierung für TT-MM-Eingaben (^(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])$)"
        - "Entity-ID-Domain-Prüfung (cover.*)"
        - "Fallback-Werte bei Parsing-Fehlern"
        - "Domain-Mismatch-Erkennung mit Entity-ID-Validierung"

    logic_checks:
      input_references_and_variables:
        - "Inputs dürfen im Action-Block NICHT direkt als Jinja2-Variable ({{ input_name }}) verwendet werden, sondern müssen im Blueprint-Header unter variables: mit !input zugewiesen werden (z.B. var_name: !input input_name)."
        - "Im Action-Block dürfen ausschließlich diese Blueprint-Variablen (z.B. {{ var_name }}) verwendet werden."
        - "Keine Jinja2-Blockstatements ({% ... %}) oder komplexe Listenoperationen in Action-Variablen – nur reine Expressions ({{ ... }})."
        - "Alle Variablen und Makros müssen einen Fallback-Wert liefern und dürfen nie None/undefined zurückgeben."
        - "Keine Typumwandlungen ohne Fallback (z.B. strptime, as_timestamp)."
        - "Alle Sensor- und Entity-Attribute müssen auf Verfügbarkeit geprüft werden (is defined, is string, default)."
        - "strptime darf niemals als Filter (| strptime) verwendet werden, sondern nur als Funktion mit 3 Parametern (z.B. strptime(..., ..., now()))."
        - "Niemals Funktionsnamen wie str, now, strptime, strftime als Variablennamen verwenden (TypeError-Prävention)."
        - "Verwende states(), is_state(), state_attr() statt direktem Zugriff auf states.entity.state."
        - "Bei Zeit-/Datums-Inputs sollten Validierungen und ggf. Korrekturen/Abbrüche erfolgen, wenn Werte nicht plausibel (z.B. Endzeit vor Startzeit) sind."
        - "Alle Listen-Inputs müssen im Blueprint-Header als Variable mit !input zugewiesen und im Action-Block direkt verwendet werden."
      trigger_and_time_logic:
        - "Keine Templates im Trigger erlaubt (z.B. at: \"{{ ... }}\" ist verboten)."
        - "Dynamische Zeitberechnung (z.B. Sonnenaufgang, saisonale Zeiten) immer in Blueprint-Variablen oder Bedingungen prüfen."
        - "Für dynamische Zeitvergleiche time_pattern verwenden und Zielzeit in der Bedingung prüfen."
      robustness_resilience_and_error_handling:
        - "Blueprint muss bei fehlenden oder fehlerhaften Inputs/Sensoren stabil laufen (kein Abbruch, keine Endlosschleife)."
        - "Leere Gruppen immer mit default([]) behandeln."
        - "Duplikate zwischen Gruppen sollten idealerweise schon im UI entfernt werden."
        - "Keine Duplikat- oder Listenfilterlogik in Action-Variablen, sondern direkt die Inputs als Listen verwenden."
        - "Jede Action, die fehlschlagen könnte, sollte geloggt werden (system_log.write)."
        - "Fehlerbehandlung so einfach wie möglich halten und Fehler immer loggen."
        - "Fange nur spezifische Exceptions ab, die du wirklich behandeln willst (sofern im YAML/Jinja2-Kontext möglich)."
        - "Nutze Fallbacks oder Defaults, wo sinnvoll."
      testability_and_logging:
        - "Blueprint muss Logging für alle kritischen Variablen/Aktionen bieten."
        - "Blueprint muss ohne manuelle Trigger testbar sein (z.B. mit time_pattern oder echten Sensortriggern)."
        - "Im Fehlerfall müssen alle Variablen im Log erscheinen."
        - "Debugging sollte mit system_log.write und Trace-Funktion unterstützt werden."
      compatibility_and_yaml:
        - "Nur offiziell dokumentierte YAML-Keys und Selektoren verwenden."
        - "Blueprint muss mit Home Assistant 2025.4+ lauffähig sein."
        - "Keine !input-Referenzen im Trigger."
        - "Keine Makros im Blueprint-Header, nur in Actions."
        - "Blueprint muss nach jedem HA-Update (Release Notes!) getestet werden."
      blueprint_documentation:
        - "Changelog aktuell halten."
        - "Bekannte Einschränkungen und Testhinweise dokumentieren."
        - "Alle Inputs und Trigger im Blueprint klar beschreiben."
        - "Blueprint sollte Beispielkonfiguration und Hinweise zu Backups und Fehlerdiagnose enthalten."
      readability_and_maintainability:
        - "Automationen und Blueprints sollten so einfach und flach wie möglich gehalten werden."
        - "Keine unnötigen Verschachtelungen, keine überflüssigen choose/repeat-Blöcke."
        - "Kommentare und Struktur für spätere Anpassungen und Debugging bereitstellen."
      defensive_templates:
        - "Jede Variable mit Fallback-Wert absichern."
        - "Sensor-/Entity-Attribute immer auf Verfügbarkeit prüfen."
        - "Makros und Bedingungen gegen leere oder ungültige Werte absichern."
      trigger_time_validation:
        - "Keine dynamischen Jinja2-Templates im Zeit-Trigger (at: ...)."
        - "Dynamische Zeitberechnung nur in Bedingungen/Aktionen."
        - "time_pattern-Trigger für dynamische Zeitberechnung verwenden."
      testability_logging:
        - "Logging für alle kritischen Aktionen und Variablen aktivieren."
        - "Blueprint muss auch bei fehlenden oder fehlerhaften Inputs stabil laufen."
      group_duplicate_logic:
        - "Leere Gruppen mit default([]) behandeln."
        - "Duplikate zwischen Gruppen entfernen."
      compatibility:
        - "Blueprint muss mit Home Assistant 2025.4+ lauffähig sein."
        - "Nur offiziell dokumentierte YAML-Keys und Selektoren verwenden."
      parallel_execution:
        - "mode: parallel für Automatisierungen mit manuellen Triggern."
        - "max: 5-10 für Lastverteilung bei parallelem Modus."
      trigger_resilience:
        - "UTC-basierte Zeitstempel für saisonale Berechnungen."
        - "Fallback-Event für manuelle Steuerung."
      entity_handling:
        - "Case-sensitive Entity-ID-Prüfung."
        - "Domain-Filterung (z.B. cover.*)."
      mode_config:
        - "mode: queued/parallel für Zeit-basierte Automatisierungen."
        - "max_exceeded: silent/error."
      trigger_context:
        - "Jeder Zugriff auf trigger.* muss mit 'trigger is defined' abgesichert werden."
        - "Manuelle Auslösung muss ohne trigger-Kontext funktionieren."
      patterns:
        - "UTC/Lokalzeit-Konvertierung."
        - "Sommerzeit-Übergangsbehandlung."
        - "Throttle/Delay-Implementierung."

    performance_checks:
      description: "Laufzeitoptimierung"
      metrics:
        - "Komplexitätsanalyse (O(n) vs O(n²))"
        - "Batch-Verarbeitung bei Massenoperationen"
        - "Redundanzprüfung von Service Calls"

    documentation:
      requirements:
        - "Dokumentation bekannter Einschränkungen"
        - "Beispielkonfiguration mit Realwerten"
        - "Versionsabhängige Voraussetzungen"

  error_classification:
    - type: "Kritischer Fehler"
      examples: 
        - "Input direkt als Jinja2-Variable im Action-Block verwendet ({{ input_name }})"
        - "Variable im Action-Block nicht über !input im Header zugewiesen"
        - "Jinja2-Blockstatement ({% ... %}) oder Listenoperation in Action-Variable"
        - "input.group1_covers in Action oder Bedingung verwendet"
        - "Makros im Blueprint-Header"
        - "Keine Fallback-Werte für Variablen/Makros"
        - "None/undefined Rückgabe bei Variablen"
        - "Typumwandlung ohne Fallback"
        - "Sensor-/Entity-Attribute ungeprüft"
        - "strptime als Filter verwendet"
        - "Funktionsnamen (str, now, strptime, strftime) als Variablennamen"
        - "Direkter Zugriff auf states.entity.state"
        - "Unplausible Zeit/Datum-Inputs (z.B. Endzeit < Startzeit)"
        - "Listen-Input nicht als Variable mit !input im Header zugewiesen"
        - "Templates im Trigger (at: \"{{ ... }}\")"
        - "Dynamische Zeitberechnung im Trigger"
        - "Blueprint bricht bei fehlenden Inputs ab"
        - "Leere Gruppen nicht mit default([]) behandelt"
        - "Duplikate zwischen Gruppen"
        - "Fehlendes Logging bei fehleranfälligen Actions"
        - "Nicht dokumentierte YAML-Keys/Selektoren"
        - "!input-Referenz im Trigger"
        - "Automationen unnötig verschachtelt"
      priority: "high"
    - type: "Logikfehler"
      examples: 
        - "Komplexe Logik/Makros im Header"
        - "Unzureichende Logging-Ausgaben"
        - "Nicht testbare Blueprint-Logik"
        - "Fehlende Fehlerbehandlung bei Typumwandlungen"
        - "Domain-Mismatch bei Entity-Referenz"
        - "Fehlende Prüfung auf Duplikate"
        - "Fehlende Validierung/Korrektur bei Zeit-Inputs"
        - "Fehlende Kommentare/Struktur für Wartung"
      priority: "medium"
    - type: "Warnung"
      examples: 
        - "Unvollständige Dokumentation"
        - "Changelog nicht aktuell"
        - "Fehlende Testhinweise"
        - "Inputs/Trigger nicht klar beschrieben"
        - "Fehlende Hinweise zu Backups/Diagnose"
      priority: "low"

  test_methods:
    automated:
      - "Modus-Trigger-Konsistenzcheck"
      - "TT-MM-Formatvalidierung"
      - "UTC-Zeitstempel-Prüfung"
      - "Parallelitäts- und max-Wert-Prüfung"
      - "Case-sensitive Entity-ID-Validierung"
      - "Domain-Filter-Test"
      - "trigger.* Zugriff auf 'trigger is defined' prüfen"
      - "Simulation manueller Auslösung ohne trigger-Kontext"
      - "Fallback-Wert-Check für alle Variablen"
      - "Dynamisches Template in Zeit-Trigger-Check"
      - "Leere-Gruppen-Behandlung-Test"
      - "Logging-Aktivierung-Check"
      - "Prüfung auf input.group* Referenzen in Actions/Bedingungen"
      - "Makro-Prüfung im Header"
      - "Typumwandlung ohne Fallback"
      - "Sensor-/Entity-Attribute-Verfügbarkeitsprüfung"
      - "Keine !input-Referenzen im Trigger"
      - "Blueprint-Dokumentationsprüfung"
      - "strptime als Filter-Erkennung"
      - "Variablennamen-Konflikt mit Funktionsnamen"
      - "Direkter Zugriff auf states.entity.state"
      - "Plausibilitätsprüfung für Zeit-/Datums-Inputs"
      - "Verschachtelungstiefe und Block-Komplexität"
      - "Prüfung auf Jinja2-Blockstatements in Action-Variablen"
      - "Prüfung auf Listen-Inputs ohne !input-Zuweisung im Header"
    manual:
      - "HA-Neustart-Simulation"
      - "DST-Übergangstests (März/Oktober)"
      - "Parallele Trigger-Auslösung"
      - "Leere Eingabegruppen-Simulation"
      - "Fehlende/fehlerhafte Input-Simulation"
      - "Review der Blueprint-Dokumentation"
      - "Manuelle Prüfung auf Backups und Fehlerdiagnose-Hinweise"

  output_format:
    status: "pass/warn/error"
    report_fields:
      - "Fehlende Resilienzmechanismen"
      - "Domain-Spezifikationsfehler"
      - "Zeitformat-Inkonsistenzen"
      - "Fehler bei paralleler Ausführung"
      - "Case-Sensitivity-Probleme"
      - "trigger-Kontext-Fehler"
      - "Unbehandelte Variablen/Gruppen"
      - "Dynamische Zeit-Trigger"
      - "Fehlende Logging-Einträge"
      - "input.group*-Referenz in Action/Bedingung"
      - "Makros im Header"
      - "Typumwandlung ohne Fallback"
      - "Nicht dokumentierte YAML-Keys"
      - "Dokumentationslücken"
      - "strptime als Filter"
      - "Variablennamen-Konflikt"
      - "Direkter Zugriff auf states.entity.state"
      - "Unplausible Zeit/Datum-Inputs"
      - "Übermäßige Verschachtelung"
      - "Jinja2-Blockstatement in Action-Variable"
      - "Listen-Input ohne !input-Zuweisung"
    stats:
      - "critical_count"
      - "domain_errors"
      - "time_validation_issues"
      - "parallel_execution_issues"
      - "entity_case_issues"
      - "trigger_context_issues"
      - "defensive_programming_issues"
      - "logging_issues"
      - "documentation_issues"
      - "readability_issues"
