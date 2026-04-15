# Installation Guide

## Quick Start

### 1. Clone the Repo

```bash
cd /Users/openclawassistent/.openclaw/workspace
git clone https://github.com/JimboBoy/openclaw-memory-wiki.git
cd openclaw-memory-wiki
```

### 2. Create Directory Structure

```bash
cd /Users/openclawassistent/.openclaw/workspace
mkdir -p logbook memory/projects memory/topics memory/decisions
cp openclaw-memory-wiki/templates/*.md memory/
cp openclaw-memory-wiki/templates/logbook-template.md logbook/TEMPLATE.md
```

### 3. Install Cron Job (macOS/Linux)

```bash
# Copy cron configuration
sudo cp cron/openclaw-memory-index /etc/cron.d/openclaw-memory-index

# Or add to your crontab manually
crontab -e
# Add this line:
0 2 * * * /usr/bin/python3 /Users/openclawassistent/.openclaw/workspace/openclaw-memory-wiki/scripts/daily-indexer.py >> /var/log/openclaw-memory-index.log 2>&1
```

### 4. Test Run

```bash
python3 scripts/daily-indexer.py
```

Expected output:
```
🚀 OpenClaw Memory Wiki - Daily Indexer
==================================================
🔍 Finding sessions since 2026-04-14...
✅ No new sessions found. Nothing to index.
```

---

## Configuration

### Environment Variables

Set these in your shell or `.env` file:

```bash
export OPENCLAW_WORKSPACE=/Users/openclawassistent/.openclaw/workspace
export MEMORY_WIKI_LOG_LEVEL=info  # debug, info, warn, error
```

### Cron Schedule

Default: Daily at 2:00 AM

Change in `cron/openclaw-memory-index`:
```
# Format: minute hour day month weekday user command
0 2 * * * ...  # 2:00 AM daily
```

Common schedules:
- `0 2 * * *` - Daily at 2 AM
- `0 2 * * 1-5` - Weekdays at 2 AM
- `0 */6 * * *` - Every 6 hours

---

## Testing

### Manual Test Run

```bash
cd /Users/openclawassistent/.openclaw/workspace/openclaw-memory-wiki
python3 scripts/daily-indexer.py
```

### Check Logs

```bash
tail -f /var/log/openclaw-memory-index.log
```

### Verify Output

Check that these files are created/updated:
```bash
ls -la /Users/openclawassistent/.openclaw/workspace/logbook/
ls -la /Users/openclawassistent/.openclaw/workspace/memory/
cat /Users/openclawassistent/.openclaw/workspace/memory/INDEX.md
```

---

## Troubleshooting

### "No sessions found"

The indexer looks for sessions in `SESSIONS_DIR`. This path needs to be updated based on where OpenClaw actually stores session transcripts.

Check OpenClaw documentation or run:
```bash
find /Users/openclawassistent/.openclaw -name "*.md" -o -name "*.json" | grep -i session
```

### Permission Issues

Make sure the cron job runs as the correct user:
```bash
# Check cron job owner
cat /etc/cron.d/openclaw-memory-index

# Should run as your user, not root (unless necessary)
```

### Python Not Found

```bash
# Find Python path
which python3

# Update cron job with correct path
```

---

## Next Steps

After installation:

1. **Update Session Discovery** - Modify `find_new_sessions()` in `daily-indexer.py` to match OpenClaw's session storage

2. **Implement LLM Extraction** - Update `extract_metadata()` to call OpenClaw's LLM for analysis

3. **Test with Real Sessions** - Run manually first, then enable cron

4. **Monitor** - Check logs daily for first week

---

## Support

- **Issues:** https://github.com/JimboBoy/openclaw-memory-wiki/issues
- **Inspiration:** [Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
