blueprint_validation:
  metadata:
    version: "4.6"
    author: "jard4101"
    compatibility: "Home Assistant 2025.4+ (letzte 4 Versionen)"
    required_fields:
      - name
      - description
      - domain
      - source_url
      - author
      - triggers
    forbidden_fields:
      - last_modified
      - custom_fields
      - trigger

  validation_rules:
    syntax_checks:
      required_fields:
        global:
          - name
          - description
          - domain
        triggers:
          - platform
        variables:
          - input
      key_spelling:
        mandatory_plurals:
          - triggers
          - actions
          - variables
        forbidden_singulars:
          - trigger
          - action
          - variable
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
        - "Regex-Validierung für Text-Inputs"
        - "Fallback-Werte bei Parsing-Fehlern"
        - "Domain-Mismatch-Erkennung mit Entity-ID-Validierung"

    logic_checks:
      description: "Logische und zeitliche Konsistenz"
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
        - "Fehlendes Pflichtfeld: triggers"
        - "Falscher Singular-Key: trigger"
        - "Ungültige Plattform: mqtt"
      priority: "high"
    - type: "Logikfehler"
      examples: 
        - "Zeitstempel-Fehler"
        - "Race Conditions"
      priority: "medium"
    - type: "Warnung"
      examples: 
        - "Undokumentierte Features"
        - "Nicht optimierte Batch-Verarbeitung"
      priority: "low"

  test_methods:
    automated:
      - "Global Required Fields Check"
      - "Plural/Singular Key Validation"
      - "Platform Whitelist Checker"
    manual:
      - "Visuelle Key-Syntax-Prüfung"
      - "Kontextabhängige Pluralverwendung"

  output_format:
    status: "pass/warn/error"
    report_fields:
      - "Fehlende Pflichtfelder"
      - "Falsche Key-Schreibweise"
      - "Platform-Vorgaben"
    stats:
      - "missing_required_fields"
      - "spelling_errors"
      - "invalid_platforms"
