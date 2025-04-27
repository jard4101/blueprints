blueprint_validation:
  metadata:
    version: "4.7"
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
      # ... (restliche bestehende Syntax-Checks)

    logic_checks:
      mode_validation:
        - condition: "mode in ['single','queued','parallel']"
          error_message: "Ungültiger Modus: {mode}"
        - condition: "triggers|length > 1 → mode != 'single'"
          error_message: "Mehrfach-Trigger benötigen queued/parallel-Modus"
      
      error_handling:
        - required: true
          test: "Fallback für leere Gruppen vorhanden"
          error_message: "Fehlende Fehlerbehandlung bei leeren Entity-Gruppen"
      
      # ... (bestehende Logik-Checks)

  error_classification:
    - type: "Kritischer Fehler"
      examples: 
        - "Ungültiger Modus: batch"
        - "Fehlendes max_exceeded Feld"
        - "Queued-Modus mit Einzeltrigger"
      priority: "high"
    # ... (bestehende Fehlerklassen)

  test_methods:
    automated:
      - "Modus-Trigger-Konsistenzcheck"
      - "Error-Fallback-Validierung"
      - "Wertebereichsprüfung max_exceeded"
    # ... (bestehende Tests)

# ... (restliche unveränderte Abschnitte)
