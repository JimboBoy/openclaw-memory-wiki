# OpenClaw Memory Wiki

**Ein "Second Brain" für OpenClaw Sessions** - inspiriert von Andrej Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

---

## 🎯 Problem

OpenClaw Sessions starten "frisch" ohne Kontext. Wertvolles Wissen geht verloren.

---

## 💡 Lösung (Karpathy-Style)

**Super simpel - nur 3 Ordner + 1 Datei:**

```
workspace/
├── AGENTS.md          # Schema (dem LLM sagen WAS zu tun ist)
├── raw/               # Session transcripts (append-only)
├── wiki/              # LLM schreibt Knowledge Articles
│   └── INDEX.md       # One-line Übersicht aller Articles
└── outputs/           # Query-Antworten
```

---

## 🔄 So funktioniert's

### 1. Session speichern
```bash
cp session.md raw/2026-04-15-session-abc.md
```

### 2. LLM sagen zu prozessieren
```
User: "Process new sessions"
```

### 3. LLM macht alles automatisch:
- Liest Session
- Extrahiert Topics, Projects, Decisions
- Schreibt wiki-Artikel
- Updated INDEX.md
- Fügt Backlinks hinzu

---

## 📋 AGENTS.md (Schema)

Das Herzstück. Sagt dem LLM:

```markdown
## On ingesting new sessions:
1. Read wiki/INDEX.md
2. Identify new concepts
3. Create/update wiki articles
4. Add backlinks
5. Update INDEX.md

## On query:
1. Read INDEX.md
2. Read relevant articles
3. Write answer to outputs/
```

---

## 🎯 User Commands

- `"Process new sessions"` → Indexiere alle neuen Sessions in raw/
- `"Query: [frage]"` → Beantworte aus Wiki-Wissen
- `"Run linting pass"` → Health check (widersprüche, lücken)
- `"What do we know about [topic]?"` → Zusammenfassung

---

## 📊 Beispiel

**User speichert Session:**
```
raw/2026-04-15-chrome-testing.md
```

**User sagt:**
```
"Process new sessions"
```

**LLM antwortet:**
```
✅ 1 session processed
   - Updated: wiki/projects/chrome-extension.md
   - Updated: wiki/topics/testing.md
   - Created: wiki/decisions/jest-over-promptfoo.md
   - Updated: wiki/INDEX.md
```

---

## 🚀 Installation

### 1. Clone
```bash
cd /Users/openclawassistent/.openclaw/workspace
git clone https://github.com/JimboBoy/openclaw-memory-wiki.git
```

### 2. Struktur kopieren
```bash
cp openclaw-memory-wiki/AGENTS.md ./AGENTS.md
mkdir -p raw wiki outputs
echo "# Wiki Index" > wiki/INDEX.md
```

### 3. Sessions hinzufügen
```bash
cp ~/.openclaw/sessions/*.md raw/
```

### 4. LLM sagen
```
"Process new sessions according to AGENTS.md"
```

---

## 🎯 Vorteile

| Feature | Karpathy-Style | Vector-DB |
|---------|----------------|-----------|
| **Setup** | ⭐⭐⭐⭐⭐ 3 Ordner | ⭐⭐ Complex |
| **Lesbar** | ⭐⭐⭐⭐⭐ Markdown | ⭐⭐ Vektoren |
| **Privacy** | ⭐⭐⭐⭐⭐ Lokal | ⭐⭐⭐ Cloud? |
| **Kosten** | ⭐⭐⭐⭐⭐ Nur LLM | ⭐⭐⭐ DB + LLM |
| **Flexibel** | ⭐⭐⭐⭐⭐ LLM entscheidet | ⭐⭐ Schema |

---

## 📝 Inspiration

- **Karpathy's Gist:** https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- **Codersera:** https://codersera.com/blog/karpathy-llm-knowledge-base-second-brain

**Key Insight:** LLMs sind Knowledge Compiler, nicht nur Query-Responder.

---

*Created: 2026-04-15*  
*Version: 1.0 (Karpathy-style)*
