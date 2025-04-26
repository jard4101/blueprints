blueprint_validation:
  metadata:
    version: "3.0"
    author: "Home Assistant Community"
    last_modified: "2025-04-27"
    compatibility: "Home Assistant 2025.4+ (4 letzte Versionen)"
  
  validation_rules:
    syntax_checks:
      description: "YAML-Struktur und Blueprint-Metadaten"
      methods:
        - "YAML-Validierung (Einrückung, Tabs vs. Leerzeichen)"
        - "!input-Referenzprüfung"
        - "Selector-Typen gemäß HA-Dokumentation"
        - "Unerlaubte YAML-Tags Prüfung"

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
      examples: ["Syntaxfehler", "Ungültige Selectors"]
      priority: "high"
    - type: "Logikfehler" 
      examples: ["Zeitstempel-Fehler", "Race Conditions"]
      priority: "medium"
    - type: "Warnung"
      examples: ["Undokumentierte Features", "Performance-Warnungen"]
      priority: "low"

  test_methods:
    automated:
      - "YAML-Linter Integration"
      - "Unit/Integrationstests mit Testdaten"
      - "HA Config Check Simulation"
    manual:
      - "HA-Neustart-Szenarien"
      - "Netzwerkausfall-Simulation"
      - "Grenzwert-Tests für numerische Inputs"

  output_format:
    status: "pass/warn/error"
    report_fields:
      - "Fehlerklassifizierung"
      - "Zeilennummern"
      - "Lösungsvorschläge"
    stats:
      - "error_count"
      - "warning_count"
      - "performance_issues"
