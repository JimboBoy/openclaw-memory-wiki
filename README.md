# OpenClaw Memory Wiki

**Ein "Second Brain" für OpenClaw Sessions** - inspiriert von Andrej Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

---

## 🎯 Problem

**Aktuelle Situation:**
- OpenClaw Sessions starten "frisch" ohne Kontext
- Memory ist begrenzt (Context Window)
- User muss Kontext immer wieder neu erklären
- Wertvolles Wissen aus früheren Sessions geht verloren

**Beispiel:**
> "Wir haben letzte Woche 3 Sessions über Chrome Extension Testing verbracht. Heute muss ich alles von vorne erklären."

---

## 💡 Lösung

**Zwei-Ebenen-Architektur:**

### 1. 📓 Logbuch (täglich, chronologisch)
Dokumentiert **was passiert ist** - wie ein Schiffstagebuch

```
logbook/
├── 2026-04-15.md
├── 2026-04-16.md
└── 2026-04-17.md
```

**Inhalt pro Tag:**
- Sessions die stattgefunden haben
- Entscheidungen die getroffen wurden
- Todos die entstanden sind
- Probleme die aufgetreten sind

---

### 2. 🧠 Memory (themenspezifisch, wachsend)
Dokumentiert **was wir gelernt haben** - wie ein Wissensarchiv

```
memory/
├── INDEX.md                 # Master-Übersicht aller Themen
├── projects/
│   ├── chrome-extension.md  # Alles zur Chrome Extension
│   ├── chatbot-cv.md        # Alles zum CV Chatbot
│   └── home-assistant.md    # Alles zu Home Assistant
├── topics/
│   ├── testing.md           # Test-Strategien (alle Projekte)
│   ├── github-workflows.md  # CI/CD Patterns
│   └── api-design.md        # API-Entscheidungen
└── decisions/
    ├── architecture-2026-04.md
    └── tools-selection.md
```

**Inhalt pro Thema:**
- Historie & Kontext
- Wichtige Entscheidungen
- Lessons Learned
- Current Status
- Open Questions

---

## 🔄 Der Indexierungs-Prozess

### Täglich (Cron-Job, z.B. 02:00 Uhr):

```
Neue Sessions (seit letztem Lauf)
         ↓
┌─────────────────────────────────┐
│  LLM analysiert & extrahiert:   │
│  - Topics                       │
│  - Projects                     │
│  - Decisions                    │
│  - Action Items                 │
└─────────────────────────────────┘
         ↓
    ┌────────────┬────────────┐
    │            │            │
    ↓            ↓            ↓
LOGBUCH      MEMORY       INDEX
(täglich)   (thematisch)  (aktualisieren)
```

### Beispiel-Extraktion:

**Session Input:**
```
User: "Wir haben Jest Tests für die Chrome Extension geschrieben. 
Die promptfoo Tests waren gut, aber zu statisch. 
Jetzt machen wir echte Multi-Turn Tests mit Simulated User."
```

**Extrahiert:**
```yaml
date: 2026-04-15
session_id: abc123
topics:
  - testing
  - chrome-extension
projects:
  - chrome-window-context-color
decisions:
  - "Simulated User statt statischer Tests"
  - "Jest statt nur promptfoo"
action_items:
  - "PR für Jest Tests erstellen"
learnings:
  - "promptfoo ist zu statisch für Multi-Turn"
```

---

## 📁 Dateistruktur

### Logbuch-Eintrag (`logbook/2026-04-15.md`):

```markdown
# 2026-04-15 - Daily Log

## Sessions Today
- **Session #421** (10:30) - Chrome Extension Testing
  - Topics: Testing, Jest, Promptfoo
  - Duration: 45 min
- **Session #422** (14:15) - Chatbot RAG Optimization  
  - Topics: RAG, Supabase, Embeddings
  - Duration: 30 min

## Decisions Made
- Jest Tests für Chrome Extension (statt nur promptfoo)
- Multi-Turn Testing mit Simulated User

## Action Items Created
- [ ] PR #22: Jest User Journey Tests
- [ ] RAG Performance optimieren

## Problems Solved
- Promptfoo Tests waren zu statisch → Jest Lösung gefunden

## Notes
- User war zufrieden mit Testing-Ansatz
- Nächste Session: PR Review
```

---

### Memory-Eintrag (`memory/projects/chrome-extension.md`):

```markdown
# Chrome Extension - Window Context Color

**Status:** In Development  
**Started:** 2026-04-08  
**Last Updated:** 2026-04-15  
**Repo:** https://github.com/GeraldWagner/chrome-window-context-color

---

## Overview
Chrome Extension die pro Fenster eine Farbe anzeigt. Hilft beim Kontext-Switching zwischen verschiedenen Tasks.

## Features
- ✅ Feature 1: Custom Colors (PR #1, gemerged)
- ✅ Feature 2: Window Manager (PR #3, offen)
- ✅ Feature 3: Preset System (PR #4, offen)
- 🔄 Feature 4: React Version (PR #5, in Review)

## Testing Strategy (Updated 2026-04-15)
**Decision:** Multi-Turn Tests mit Jest + Simulated User

**Warum:**
- Promptfoo war zu statisch
- Echte User Journeys wichtiger als Keyword-Tests
- LLM-Rubric für Qualitätsbewertung

**Implementation:**
- `__tests__/user-journeys.test.ts` - 12 Tests
- 4 Journeys: Skills, Experience, Language, Off-Topic
- LLM bewertet Antwortqualität (nicht nur Keywords)

## Open Questions
- Wie testen wir echte Browser-Interactions?
- Sollen wir Playwright für E2E hinzufügen?

## Lessons Learned
- Statische Tests (promptfoo) reichen nicht für UX
- LLM-Rubric ist besser als Keyword-Matching
- Multi-Turn Tests brauchen echten Chatbot-Server

## Related
- [[topics/testing.md]]
- [[topics/github-workflows.md]]
```

---

### Memory INDEX (`memory/INDEX.md`):

```markdown
# Memory Index

**Last Updated:** 2026-04-15 02:00

---

## Projects
| Project | Status | Last Updated | Topics |
|---------|--------|--------------|--------|
| [[projects/chrome-extension.md\|Chrome Extension]] | 🟡 In Dev | 2026-04-15 | Testing, React, GitHub |
| [[projects/chatbot-cv.md\|Chatbot CV]] | 🟢 Testing | 2026-04-14 | RAG, LangChain, Supabase |
| [[projects/home-assistant.md\|Home Assistant]] | 🔴 Blocked | 2026-04-10 | HA, Tuya, LEDVANCE |

## Topics
| Topic | Projects | Last Updated |
|-------|----------|--------------|
| [[topics/testing.md\|Testing]] | Chrome Extension, Chatbot | 2026-04-15 |
| [[topics/rag.md\|RAG]] | Chatbot | 2026-04-14 |
| [[topics/github-workflows.md\|GitHub Workflows]] | All | 2026-04-12 |

## Recent Decisions
- 2026-04-15: Jest statt nur promptfoo für Testing
- 2026-04-14: RAG mit Supabase statt Pinecone
- 2026-04-10: Home Assistant Token Problem

---

## Quick Queries
- "Was haben wir über Testing gelernt?" → [[topics/testing.md]]
- "Chrome Extension Status?" → [[projects/chrome-extension.md]]
- "Offene Entscheidungen?" → [[decisions/]]
```

---

## 🛠️ Implementation

### 1. Session → Markdown (automatisch)

Jede Session wird gespeichert als:
```yaml
---
session_id: abc123
date: 2026-04-15T10:30:00Z
duration_minutes: 45
topics: [testing, chrome-extension]
projects: [chrome-window-context-color]
decisions:
  - "Jest Tests statt nur promptfoo"
action_items:
  - "PR #22 erstellen"
---

# Session Transcript

[Full conversation...]
```

### 2. Daily Indexer (Cron-Job)

```bash
#每晚 02:00
openclaw-memory-index --since yesterday
```

**Macht:**
1. Liest alle neuen Sessions
2. Extrahiert Topics/Projects/Decisions
3. Schreibt `logbook/YYYY-MM-DD.md`
4. Updated relevante Memory-Dateien
5. Updated `memory/INDEX.md`

### 3. Session Start (Auto-Load)

Bei neuer Session:
```
User: "Lass uns mit der Chrome Extension weitermachen"
         ↓
System sucht Memory nach "chrome extension"
         ↓
Lädt: projects/chrome-extension.md
         ↓
Assistant hat automatisch Kontext! ✅
```

---

## 🎯 Vorteile

### Gegenüber normalem Memory:
- ✅ **Thematisch organisiert** (nicht nur chronologisch)
- ✅ **Wächst intelligent** (verdichtet Wissen)
- ✅ **Queryable** ("Was wissen wir über X?")
- ✅ **Nachvollziehbar** (Entscheidungen dokumentiert)

### Gegenüber Vector-DB:
- ✅ **Keine Embeddings** nötig
- ✅ **Keine Vector-DB** (FAISS, Chroma, etc.)
- ✅ **Nur Markdown + LLM**
- ✅ **Menschlich lesbar** (nicht nur Vektoren)

### Gegenüber Cloud-Lösungen:
- ✅ **100% lokal** (Privacy)
- ✅ **Du behältst Kontrolle**
- ✅ **Keine API-Calls** für Storage
- ✅ **Offline-fähig**

---

## 📊 Vergleich mit Karpathy's Original

| Feature | Karpathy's LLM Wiki | OpenClaw Memory Wiki |
|---------|---------------------|----------------------|
| **Input** | Markdown-Dateien | Session-Transcripts |
| **Index** | LLM erstellt | LLM extrahiert strukturiert |
| **Storage** | Eine große .md Datei | Logbuch + Memory (2-Ebenen) |
| **Query** | LLM sucht + antwortet | Memory laden + Kontext geben |
| **Update** | Manuell | Automatisch (Cron) |
| **Fokus** | Wissenserhalt | Session-Kontinuität |

---

## 🚀 Nächste Schritte

### Phase 1: Foundation (Week 1)
- [ ] Session → Markdown Export
- [ ] Logbuch-Struktur erstellen
- [ ] Memory-Ordner anlegen
- [ ] INDEX.md Template

### Phase 2: Indexer (Week 2)
- [ ] LLM Extraktion implementieren
- [ ] Cron-Job einrichten (täglich 02:00)
- [ ] Logbuch-Auto-Write
- [ ] Memory-Auto-Update

### Phase 3: Auto-Load (Week 3)
- [ ] Session-Start Memory-Load
- [ ] Topic-basierte Suche
- [ ] Kontext automatisch injizieren

### Phase 4: Polish (Week 4)
- [ ] Query-Interface ("Was wissen wir über X?")
- [ ] Memory-Visualisierung
- [ ] Export/Backup

---

## 📝 Inspiration

- **Andrej Karpathy's LLM Wiki:** https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- **MemMachine:** https://github.com/MemMachine/MemMachine
- **supermemory:** https://github.com/supermemoryai/supermemory

---

*Created: 2026-04-15*  
*Inspired by Andrej Karpathy's LLM Wiki concept*  
*Built for OpenClaw Session Continuity*