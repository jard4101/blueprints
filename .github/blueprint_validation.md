blueprint_validation:
  metadata:
    version: "4.8"
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
      trigger_resilience:
        - "Fallback für sun.sun-Abhängigkeiten"
        - "UTC-Zeitstempel für Jahresberechnungen"
      mode_config:
        - "mode: queued/parallel für Zeit-basierte Automatisierungen"
        - "max_exceeded: silent/error"
      patterns:
        - "UTC/Lokalzeit-Konvertierung"
        - "Sommerzeit-Übergangsbehandlung"
        - "Case-sensitive Entity-Prüfung"
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
        - "Nicht-UTC-Zeitstempel in Jahresberechnungen"
        - "Fehlendes sun.sun-Fallback"
        - "Ungültiges TT-MM-Format"
      priority: "high"
    - type: "Logikfehler"
      examples: 
        - "Falsche Moduswahl für Zeitautomation"
        - "Cover-Domain-Mismatch"
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
    stats:
      - "critical_count"
      - "domain_errors"
      - "time_validation_issues"
