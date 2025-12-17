# Changelog

Alle wichtigen Änderungen am Self-Healing System werden hier dokumentiert.

## [1.0.0] - 2025-12-17

### 🎉 Initial Release

#### Added
- **Core System**
  - Docker-basierte Container-Orchestrierung
  - Load Balancer mit nginx (Round-Robin)
  - 4 Webserver Backend-Instanzen
  - MariaDB für Logging und Persistenz

- **Self-Healing Logic**
  - Automatische Container-Überwachung (30s Intervall)
  - Gestufte Reparatur-Aktionen (Restart → Rebuild → Escalate)
  - Exit Code Analyse (OOM, SIGSEGV Detection)
  - Repair-History basierte Entscheidungen

- **Claude AI Integration**
  - MCP Gateway für AI-Kommunikation
  - Kontextbasierte Entscheidungsfindung
  - Confidence Scoring
  - Transparente Begründungen

- **n8n Workflow**
  - Schedule Trigger für kontinuierliches Monitoring
  - Automatische Fehler-Erkennung
  - Multi-Branch Entscheidungslogik
  - Discord Webhook Integration

- **Dashboard**
  - Echtzeit System-Status
  - Server Health Monitoring
  - Load Balancing Metriken
  - Incident History
  - Claude AI Decision Logs
  - Simulate Buttons für Testing

- **Discord Integration**
  - Farbcodierte Benachrichtigungen
  - @Admin Mention bei Escalation
  - Detaillierte Incident-Informationen

- **Chaos Monkey**
  - Zufällige Failure-Simulation
  - Konfigurierbare Wahrscheinlichkeiten
  - Cooldown-Mechanismus
  - Verschiedene Failure-Typen

- **Status API**
  - REST Endpoints für Container-Status
  - Repair History Abfrage
  - Incident Management
  - Claude Logs

---

## Versionsformat

Dieses Projekt folgt [Semantic Versioning](https://semver.org/):

- **MAJOR**: Inkompatible API-Änderungen
- **MINOR**: Neue Features (abwärtskompatibel)
- **PATCH**: Bug Fixes (abwärtskompatibel)

---


## Links

- [GitHub Repository](https://github.com/overlex2/self-healing-system)
- [Issue Tracker](https://github.com/overlex2/self-healing-system/issues)
- [Dokumentation](https://github.com/overlex2/self-healing-system#readme)
