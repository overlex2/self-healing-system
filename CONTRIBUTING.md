# Contributing to Self-Healing System

Danke für dein Interesse am Projekt! 🎉

## 🚀 Wie kann ich beitragen?

### Bug Reports

1. Prüfe ob der Bug bereits gemeldet wurde (Issues durchsuchen)
2. Erstelle ein neues Issue mit:
   - Klare Beschreibung des Problems
   - Schritte zur Reproduktion
   - Erwartetes vs. tatsächliches Verhalten
   - Screenshots falls relevant
   - System-Informationen (OS, Docker Version, etc.)

### Feature Requests

1. Erstelle ein Issue mit dem Label `enhancement`
2. Beschreibe das Feature und den Nutzen
3. Mögliche Implementierungsideen sind willkommen

### Pull Requests

1. Fork das Repository
2. Erstelle einen Feature-Branch (`git checkout -b feature/amazing-feature`)
3. Committe deine Änderungen (`git commit -m 'Add amazing feature'`)
4. Push zum Branch (`git push origin feature/amazing-feature`)
5. Öffne einen Pull Request

## 📋 Coding Standards

### JavaScript/Node.js

```javascript
// Verwende ES6+ Syntax
const myFunction = (param) => {
  // Code hier
};

// Async/Await bevorzugen
async function fetchData() {
  const result = await api.get('/data');
  return result;
}

// Konsistente Namensgebung
const containerName = 'webserver1';  // camelCase für Variablen
const MAX_RETRIES = 3;               // UPPER_CASE für Konstanten
```

### Docker

- Ein Service pro Container
- Alpine Images wenn möglich
- Multi-stage Builds für kleinere Images
- Health Checks definieren

### Commits

```
feat: Add new feature
fix: Fix bug in component
docs: Update documentation
style: Format code
refactor: Refactor code
test: Add tests
chore: Update dependencies
```

## 🧪 Testing

```bash
# Vor dem PR ausführen
docker-compose up -d
curl http://localhost:3001/status
# Alle Container sollten "up" sein
```

## 📁 Projektstruktur

Bitte halte dich an die bestehende Struktur:

```
self-healing-system/
├── nginx/           # Load Balancer Configs
├── webserver/       # Webserver Dateien
├── status-api/      # Node.js API
├── chaos-monkey/    # Test-Tool
└── n8n/            # Workflow Definitionen
```

## ❓ Fragen?

Erstelle ein Issue mit dem Label `question` oder kontaktiere den Maintainer.

---

Vielen Dank für deinen Beitrag! 🙏
