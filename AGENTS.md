# Memory Wiki - AGENTS.md

**Inspired by:** [Andrej Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

---

## 📁 Directories

```
workspace/
├── AGENTS.md          # This file - schema for LLM
├── raw/               # Session transcripts (append-only, never edit)
├── wiki/              # LLM-authored knowledge articles
│   └── INDEX.md       # One-line summary of all articles
└── outputs/           # Query responses and reports
```

---

## 🔄 On Ingesting New Sessions (raw/)

When new session files appear in `raw/`:

1. **Read** `wiki/INDEX.md` to understand existing knowledge
2. **Identify** new concepts, topics, projects, decisions in the session
3. **Create or update** wiki articles for each concept:
   - `wiki/projects/[project-name].md` - Project-specific knowledge
   - `wiki/topics/[topic-name].md` - Cross-project knowledge
   - `wiki/decisions/[decision-name].md` - Important decisions
4. **Add backlinks** between related articles using `[[link]]` syntax
5. **Update** `wiki/INDEX.md` with new/updated entries

### Example Session Processing

**Input:** `raw/2026-04-15-session-abc.md`
```
User: "We implemented Jest tests for the Chrome Extension. 
The promptfoo tests were too static, so we switched to multi-turn 
testing with simulated users."
```

**LLM extracts:**
- Projects: `chrome-extension`
- Topics: `testing`, `jest`, `multi-turn-tests`
- Decisions: `jest-over-promptfoo`

**Creates/updates:**
- `wiki/projects/chrome-extension.md`
- `wiki/topics/testing.md`
- `wiki/decisions/jest-over-promptfoo.md`
- `wiki/INDEX.md`

---

## 📋 INDEX.md Format

One line per article:

```markdown
# Wiki Index

**Last Updated:** YYYY-MM-DD

## Projects
- [[chrome-extension]] - Window color coding for context switching
- [[chatbot-cv]] - RAG-powered CV chatbot with LangChain
- [[home-assistant]] - Smart home automation with LEDVANCE

## Topics
- [[testing]] - Multi-turn testing strategies with Jest + LLM-Rubric
- [[rag]] - Retrieval-augmented generation with Supabase
- [[github-workflows]] - CI/CD patterns and PR automation

## Decisions
- [[jest-over-promptfoo]] - Jest for multi-turn tests instead of static promptfoo
- [[memory-wiki-architecture]] - Two-level logbook + memory system
```

---

## ❓ On Answering Queries

When user asks a question:

1. **Read** `wiki/INDEX.md` to get overview
2. **Identify** relevant wiki articles
3. **Read** those articles in full
4. **Write** answer to `outputs/YYYY-MM-DD-query-slug.md`
5. **Include** backlinks to source articles

### Example Query

**User:** "What testing strategies did we decide on for the Chrome Extension?"

**LLM:**
1. Reads INDEX.md
2. Finds: `[[testing]]`, `[[chrome-extension]]`, `[[jest-over-promptfoo]]`
3. Reads those articles
4. Writes answer to `outputs/2026-04-15-testing-strategy.md`

---

## 🧹 On Linting Pass (Periodic Health Check)

Run weekly or after many sessions:

1. **Read** all wiki articles
2. **Flag** contradictions between articles
3. **Identify** concepts mentioned but lacking articles
4. **Create stubs** for missing articles
5. **Fix** stale backlinks
6. **Report** gaps to user

---

## ✍️ Writing Style

Wiki articles should be:

- **Encyclopedia-style** - Objective, factual tone
- **Self-contained** - Each article makes sense alone
- **Interlinked** - Heavy use of `[[backlinks]]`
- **Dated** - Include "Last Updated" timestamp
- **Attributed** - Note which sessions informed this article

### Article Template

```markdown
# [Article Title]

**Created:** YYYY-MM-DD  
**Last Updated:** YYYY-MM-DD  
**Related:** [[related-article-1]], [[related-article-2]]

---

## Overview

[Brief description of what this is]

---

## Key Points

- Point 1
- Point 2
- Point 3

---

## History

[Chronological development]

---

## Decisions

[Important decisions related to this topic]

---

## Open Questions

[Unresolved issues]

---

*Source sessions: [[raw/YYYY-MM-DD-session-abc]]*
```

---

## 🚫 Rules

### Never:
- Edit files in `raw/` (append-only)
- Delete wiki articles (archive instead)
- Write without reading INDEX.md first
- Create duplicate articles (check INDEX.md)
- Use external links that might rot (prefer local content)

### Always:
- Read INDEX.md before writing
- Add backlinks to related articles
- Update INDEX.md when creating/updating articles
- Include timestamps on all articles
- Flag uncertainties with "TODO: Verify"

---

## 🎯 User Commands

The user can trigger actions by saying:

- **"Process new sessions"** → Ingest all new files in `raw/`
- **"Query: [question]"** → Answer question using wiki
- **"Run linting pass"** → Health check on wiki
- **"Show INDEX"** → Display current wiki index
- **"What do we know about [topic]?"** → Summarize topic from wiki

---

## 📊 Example Workflow

### User adds session:
```bash
# User saves session transcript
cp session-transcript.md raw/2026-04-15-session-abc.md
```

### User triggers processing:
```
User: "Process new sessions"
```

### LLM responds:
```
🔍 Found 1 new session in raw/

📖 Reading wiki/INDEX.md...
   - 3 projects indexed
   - 5 topics indexed
   - 2 decisions indexed

🧠 Processing 2026-04-15-session-abc.md...
   - Extracted: testing, jest, chrome-extension
   - Updated: wiki/projects/chrome-extension.md
   - Updated: wiki/topics/testing.md
   - Created: wiki/decisions/jest-over-promptfoo.md
   - Updated: wiki/INDEX.md

✅ Processing complete!
   - 1 session processed
   - 3 articles updated
   - 1 article created
   - INDEX.md updated
```

---

## 🔗 Inspiration

- **Karpathy's Original Gist:** https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- **Codersera Article:** https://codersera.com/blog/karpathy-llm-knowledge-base-second-brain

**Key Insight:** LLMs are knowledge compilers, not just query responders. The human's role is writing the schema (this file), the LLM's role is executing at scale.

---

*Created: 2026-04-15*  
*Version: 1.0 (Karpathy-style)*
