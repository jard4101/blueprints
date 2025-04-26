{
  "blueprint_validation": {
    "metadata": {
      "version": "3.0",
      "author": "Home Assistant Community",
      "last_modified": "2025-04-27",
      "compatibility": "Home Assistant 2025.4+ (letzte 4 Versionen)"
    },
    "validation_rules": {
      "syntax_checks": {
        "description": "YAML-Struktur und Blueprint-Metadaten",
        "methods": [
          "yamllint Validierung",
          "Selector-Typenprüfung gemäß HA-Dokumentation",
          "Verbotene YAML-Tags-Erkennung (z.B. !!timestamp)",
          "!input-Referenzprüfung mit Dependency-Graphen"
        ]
      },
      "input_validation": {
        "description": "Erweiterte Eingabeprüfung",
        "testcases": [
          "Regex-Validierung für Text-Inputs",
          "Automatische Fallback-Wertgenerierung",
          "Domain-spezifische Entity-ID-Validierung",
          "Numerische Grenzwertsimulation (Min/Max)"
        ]
      },
      "logic_checks": {
        "time_handling": [
          "UTC/Lokalzeit-Konvertierung",
          "Sommerzeit-Übergangsszenarien",
          "Jahreswechsel-Behandlung"
        ],
        "concurrency": [
          "Race-Condition-Erkennung",
          "Throttle/Delay-Mechanismen",
          "Parallele Service-Call-Orchestrierung"
        ],
        "entity_handling": [
          "Case-sensitive Entity-Prüfung",
          "Unavailable-Status-Fallback",
          "Massenoperationen-Batchverarbeitung"
        ]
      },
      "performance_checks": {
        "analysis": [
          "Algorithmische Komplexitätsbewertung (O-Notation)",
          "Redundanzprüfung bei Service Calls",
          "Speicherverbrauchsprofilierung"
        ]
      },
      "documentation": {
        "requirements": [
          "Beispielkonfiguration mit Realwerten",
          "Versionsabhängigkeiten dokumentieren",
          "Bekannte Einschränkungen auflisten",
          "Änderungshistorie pflegen"
        ]
      },
      "error_handling": {
        "classification": [
          {"type": "Kritisch", "examples": ["Syntaxfehler", "Ungültige Selectors"]},
          {"type": "Logik", "examples": ["Zeitstempelfehler", "Race Conditions"]},
          {"type": "Warnung", "examples": ["Undokumentierte Parameter"]}
        ]
      }
    },
    "validation_workflow": {
      "steps": [
        "Statische Codeanalyse",
        "Dynamische HA-Simulation",
        "Lasttest (High-Frequency-Trigger)",
        "Netzwerkausfall-Szenario"
      ],
      "output": {
        "format": "Erweitertes JSON-Report",
        "elements": [
          "Zeilenbasierte Fehlerlokalisierung",
          "Kontextsensitive Lösungsvorschläge",
          "Performance-Kennzahlen (Laufzeit/Speicher)"
        ]
      }
    },
    "integration": {
      "testing": {
        "automated": [
          "YAML-Linter-Pipeline",
          "Unit/Integration-Test-Suite",
          "HA-Neustart-Simulation"
        ],
        "manual": [
          "Grenzwert-Testmatrix",
          "Multi-User-Stresstest",
          "Geräte-Fallback-Szenarien"
        ]
      }
    }
  }
}
