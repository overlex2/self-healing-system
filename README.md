# 🤖 Self-Healing System

> Autonome Infrastruktur mit Claude AI für automatische Server-Reparatur

![Status](https://img.shields.io/badge/Status-Active-00ff88?style=flat-square)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=flat-square&logo=docker)
![n8n](https://img.shields.io/badge/n8n-Workflow-FF6D5A?style=flat-square)
![Claude AI](https://img.shields.io/badge/Claude-AI-8B5CF6?style=flat-square)

---

## 📋 Übersicht

Das **Self-Healing System** ist eine autonome Infrastruktur-Lösung, die Server-Ausfälle automatisch erkennt, analysiert und repariert - komplett ohne manuelles Eingreifen.

### ✨ Features

- 🔍 **Automatische Fehlererkennung** - Kontinuierliches Monitoring aller Container
- 🧠 **KI-gestützte Analyse** - Claude AI trifft intelligente Entscheidungen
- 🔧 **Gestufte Reparatur** - Restart → Rebuild → Escalate
- 📊 **Echtzeit-Dashboard** - Live-Monitoring mit Metriken
- 💬 **Discord Integration** - Sofortige Benachrichtigungen
- 🐒 **Chaos Monkey** - Integrierte Test-Funktionalität

---

## 🏗️ Architektur

```
┌─────────────────────────────────────────────────────────┐
│                    WINDOWS HOST                          │
│                 MCP Gateway (Port 3000)                  │
└─────────────────────────┬───────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────┐
│              UBUNTU VM (10.0.0.10)                       │
│                    Docker Host                           │
│  ┌─────────────────────────────────────────────────┐    │
│  │              🌐 Load Balancer                    │    │
│  │                 (nginx:alpine)                   │    │
│  └──────────────────────┬──────────────────────────┘    │
│                         │                                │
│    ┌────────┬───────────┼───────────┬────────┐          │
│    ▼        ▼           ▼           ▼        ▼          │
│  ┌────┐  ┌────┐      ┌────┐      ┌────┐  ┌────┐        │
│  │WS1 │  │WS2 │      │WS3 │      │WS4 │  │    │        │
│  └────┘  └────┘      └────┘      └────┘  └────┘        │
│                                                          │
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌───────┐ │
│  │  n8n   │ │ Claude │ │ Chaos  │ │MariaDB │ │Status │ │
│  │        │ │   AI   │ │ Monkey │ │        │ │  API  │ │
│  └────────┘ └────────┘ └────────┘ └────────┘ └───────┘ │
└─────────────────────────────────────────────────────────┘
```

---

## ⚡ Self-Healing Logik

### Entscheidungsbaum

| Repairs | Aktion | Beschreibung |
|---------|--------|--------------|
| 0-2 | 🟢 **RESTART** | Container neu starten |
| 3 | 🟡 **REBUILD** | Container neu bauen |
| 4+ | 🔴 **ESCALATE** | Admin via Discord benachrichtigen |

### Exit Code Analyse

| Exit Code | Bedeutung | Aktion |
|-----------|-----------|--------|
| 0, 1 | Normal beendet | Restart |
| 137 | OOM (Out of Memory) | Rebuild |
| 139 | SIGSEGV (Segfault) | Escalate |

---

## 🚀 Installation

### Voraussetzungen

- Docker & Docker Compose
- Node.js 18+
- Ubuntu 22.04+ (VM oder Server)
- Discord Webhook URL (optional)
- Claude API Key (für MCP)

### Quick Start

```bash
# Repository klonen
git clone https://github.com/username/self-healing-system.git
cd self-healing-system

# Umgebungsvariablen konfigurieren
cp .env.example .env
nano .env

# System starten
docker-compose up -d

# Status prüfen
docker ps
```

### Umgebungsvariablen

```env
# .env Datei
MARIADB_ROOT_PASSWORD=your_secure_password
MARIADB_DATABASE=selfhealing
MARIADB_USER=admin
MARIADB_PASSWORD=your_password

# Discord Webhook (optional)
DISCORD_WEBHOOK_URL=https://discord.com/api/webhooks/...

# n8n
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=admin123
```

---

## 📁 Projektstruktur

```
self-healing-system/
├── docker-compose.yml      # Container-Orchestrierung
├── .env                    # Umgebungsvariablen
├── nginx/
│   ├── nginx.conf          # Load Balancer Config
│   └── html/
│       └── index.html      # Dashboard
├── webserver/
│   └── html/
│       └── index.html      # Webserver Response
├── status-api/
│   ├── package.json
│   └── status-api.js       # Status API Server
├── chaos-monkey/
│   ├── package.json
│   └── chaos.js            # Chaos Monkey Script
├── n8n/
│   └── workflows/
│       └── self-healing.json
└── README.md
```

---

## 🐳 Docker Container

| Container | Image | Port | Beschreibung |
|-----------|-------|------|--------------|
| loadbalancer | nginx:alpine | 80 | Reverse Proxy & Load Balancer |
| webserver1-4 | nginx:alpine | - | Backend Webserver |
| n8n | n8n/n8n:latest | 5678 | Workflow Automation |
| status-api | node:20-alpine | 3001 | REST API für Status |
| mariadb | mariadb:10.11 | 3306 | Datenbank |
| chaos-monkey | node:20-alpine | - | Test-Tool |

---

## 📊 Dashboard

Das Dashboard ist erreichbar unter: `http://<VM-IP>:80`

### Features

- **System Status** - Online/Degraded/Offline Anzeige
- **Server Health** - Status aller Container
- **Load Balancing** - Traffic-Verteilung
- **Live Metriken** - Requests, Response Time, Uptime
- **Incident History** - Vergangene Vorfälle
- **Claude AI Logs** - KI-Entscheidungen
- **Simulate Buttons** - Test-Funktionen

### Simulate Buttons

| Button | Funktion |
|--------|----------|
| 🟢 Restart | Simuliert 1 Repair → Container Restart |
| 🟡 Rebuild | Simuliert 3 Repairs → Container Rebuild |
| 🔴 Escalate | Simuliert 5 Repairs → Discord Alert |

---

## ⚙️ n8n Workflow

### Import

1. n8n öffnen: `http://<VM-IP>:5678`
2. Login: admin / admin123
3. Workflows → Import from File
4. `n8n/workflows/self-healing.json` auswählen
5. Workflow aktivieren (Toggle)

### Workflow-Nodes

```
Schedule Trigger (30s)
    ↓
Execute Command (docker ps)
    ↓
Code in JavaScript (Filter down services)
    ↓
If (hasDownServices)
    ├─ true → HTTP Request (Status API)
    │            ↓
    │         Get Logs (Repair History)
    │            ↓
    │         HTTP Request (Claude AI)
    │            ↓
    │         Decide Action
    │            ↓
    │         Check Action
    │            ├─ escalate → Discord2 (@Admin)
    │            ├─ rebuild → Execute Command → Discord
    │            └─ restart → Execute Command → Discord
    │
    └─ false → (nichts tun)
```

---

## 🤖 Claude AI Integration

### MCP Gateway Setup (Windows)

```bash
# MCP Server installieren
npm install -g @anthropic/mcp-server

# Gateway starten
mcp-gateway --port 3000
```

### API Endpoint

```
POST http://10.0.0.1:3000/analyze
Content-Type: application/json

{
  "service": "webserver1",
  "status": "DOWN",
  "raw": "Exited (0) 5 minutes ago",
  "repairCount": 2,
  "logs": "Container logs..."
}
```

### Response Format

```json
{
  "success": true,
  "decision": {
    "action": "restart",
    "reasoning": "Container beendet ohne Fehler, erste Reparatur",
    "confidence": 0.8,
    "commands": ["docker restart webserver1"]
  },
  "response_time_ms": 3495,
  "tokens": 431
}
```

---

## 💬 Discord Integration

### Webhook erstellen

1. Discord Server → Server Settings → Integrations
2. Webhooks → New Webhook
3. Name: "Self-Healing Bot"
4. URL kopieren
5. In `.env` eintragen

### Nachrichtentypen

| Typ | Farbe | Beschreibung |
|-----|-------|--------------|
| Restart | 🟢 Grün | Informativ, kein Handlungsbedarf |
| Rebuild | 🟡 Gelb | Warnung, beobachten |
| Escalate | 🔴 Rot | @Admin Mention, Aktion erforderlich |

---

## 🐒 Chaos Monkey

Der Chaos Monkey simuliert zufällige Ausfälle zum Testen.

### Konfiguration

```javascript
// chaos.js
const CONFIG = {
  interval: 60000,        // Alle 60 Sekunden
  probability: 0.3,       // 30% Wahrscheinlichkeit
  maxAttacks: 2,          // Max 2 Angriffe pro Container
  cooldown: 480000,       // 8 Min Cooldown
  distribution: {
    restart: 0.375,       // 37.5%
    rebuild: 0.375,       // 37.5%
    escalate: 0.25        // 25%
  }
};
```

### Aktivieren/Deaktivieren

```bash
# Starten
docker start chaos-monkey

# Stoppen
docker stop chaos-monkey

# Logs anzeigen
docker logs -f chaos-monkey
```

---

## 🗄️ Datenbank Schema

### Tabelle: `incidents`

```sql
CREATE TABLE incidents (
  id INT AUTO_INCREMENT PRIMARY KEY,
  server_name VARCHAR(50),
  incident_type VARCHAR(20),
  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
  resolved BOOLEAN DEFAULT FALSE,
  resolution_time INT
);
```

### Tabelle: `auto_repairs`

```sql
CREATE TABLE auto_repairs (
  id INT AUTO_INCREMENT PRIMARY KEY,
  server_name VARCHAR(50),
  repair_action VARCHAR(20),
  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
  success BOOLEAN,
  incident_id INT,
  claude_decision TEXT,
  execution_time_ms INT,
  error_log TEXT
);
```

### Tabelle: `claude_logs`

```sql
CREATE TABLE claude_logs (
  id INT AUTO_INCREMENT PRIMARY KEY,
  prompt TEXT,
  response TEXT,
  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
  response_time_ms INT,
  tokens_used INT,
  decision_made VARCHAR(20),
  confidence FLOAT
);
```

---

## 🔧 Troubleshooting

### Container startet nicht

```bash
# Logs prüfen
docker logs <container_name>

# Container neu starten
docker-compose restart <service>

# Kompletter Reset
docker-compose down -v
docker-compose up -d
```

### n8n Workflow funktioniert nicht

1. Workflow aktiviert? (Toggle muss grün sein)
2. Credentials korrekt?
3. URLs erreichbar? (Status API, MCP Gateway)
4. Logs prüfen: `docker logs n8n`

### Discord Alerts kommen nicht

1. Webhook URL korrekt?
2. n8n HTTP Request Node prüfen
3. Discord Server-Berechtigungen prüfen

### Dashboard zeigt keine Daten

1. Status-API läuft? `curl http://localhost:3001/status`
2. CORS aktiviert?
3. Browser Console auf Fehler prüfen

---

## 📈 Performance

| Metrik | Wert |
|--------|------|
| Erkennungszeit | ~30 Sekunden |
| Claude AI Response | ~3-4 Sekunden |
| Restart Zeit | ~5-10 Sekunden |
| Rebuild Zeit | ~15-30 Sekunden |
| End-to-End Recovery | <1 Minute |

---

## 🛠️ Entwicklung

### Lokales Setup

```bash
# Dependencies installieren
cd status-api && npm install
cd ../chaos-monkey && npm install

# Status API lokal starten
node status-api.js

# Chaos Monkey lokal starten
node chaos.js
```

### Tests ausführen

```bash
# Container Status testen
curl http://localhost:3001/status

# Simulate Restart
curl -X POST http://localhost:3001/simulate/restart \
  -H "Content-Type: application/json" \
  -d '{"service": "webserver1"}'
```

---

## 📄 Lizenz

MIT License - siehe [LICENSE](LICENSE)


---
