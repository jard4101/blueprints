blueprint_validation:
  metadata:
    version: "4.10"
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

    input_validation:
      description: "Eingabeparameter-Edge-Cases"
      checks:
        - "Regex-Validierung für TT-MM-Eingaben (^(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])$)"
        - "Entity-ID-Domain-Prüfung (cover.*)"
        - "Fallback-Werte bei Parsing-Fehlern"
        - "Domain-Mismatch-Erkennung mit Entity-ID-Validierung"

    logic_checks:
      parallel_execution:
        - "mode: parallel für Automatisierungen mit manuellen Triggern"
        - "max: 5-10 für Lastverteilung bei parallelem Modus"
      trigger_resilience:
        - "UTC-basierte Zeitstempel für saisonale Berechnungen"
        - "Fallback-Event für manuelle Steuerung"
      entity_handling:
        - "Case-sensitive Entity-ID-Prüfung"
        - "Domain-Filterung (z.B. cover.*)"
      mode_config:
        - "mode: queued/parallel für Zeit-basierte Automatisierungen"
        - "max_exceeded: silent/error"
      trigger_context:
        - "Jeder Zugriff auf trigger.* muss mit 'trigger is defined' abgesichert werden"
        - "Manuelle Auslösung muss ohne trigger-Kontext funktionieren"
      patterns:
        - "UTC/Lokalzeit-Konvertierung"
        - "Sommerzeit-Übergangsbehandlung"
        - "Throttle/Delay-Implementierung"

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
        - "Nicht-UTC-Zeitstempel in saisonaler Berechnung"
        - "Fehlender Fallback-Event für manuelle Steuerung"
        - "Ungültiger max-Wert für parallelen Modus"
        - "trigger.* Zugriff ohne 'trigger is defined'"
        - "Manuelle Auslösung schlägt fehl wegen trigger-Abhängigkeit"
      priority: "high"
    - type: "Logikfehler"
      examples: 
        - "Falsche Moduswahl für manuelle Trigger"
        - "Case-insensitive Entity-ID"
        - "Domain-Mismatch bei Entity-Referenz"
      priority: "medium"
    - type: "Warnung"
      examples: 
        - "Manuelle DST-Übergangsbehandlung"
        - "Fehlende parallele Trigger-Absicherung"
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
    manual:
      - "HA-Neustart-Simulation"
      - "DST-Übergangstests (März/Oktober)"
      - "Parallele Trigger-Auslösung"
      - "Leere Eingabegruppen-Simulation"

  output_format:
    status: "pass/warn/error"
    report_fields:
      - "Fehlende Resilienzmechanismen"
      - "Domain-Spezifikationsfehler"
      - "Zeitformat-Inkonsistenzen"
      - "Fehler bei paralleler Ausführung"
      - "Case-Sensitivity-Probleme"
      - "trigger-Kontext-Fehler"
    stats:
      - "critical_count"
      - "domain_errors"
      - "time_validation_issues"
      - "parallel_execution_issues"
      - "entity_case_issues"
      - "trigger_context_issues"
