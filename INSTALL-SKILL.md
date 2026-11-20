# Install as a skill

The skill lives in [`skills/last-moment-bfcm/`](skills/last-moment-bfcm/) (`SKILL.md` + the checklist). Once
installed, just say "get my Shopify store ready for Black Friday" and the agent uses it.

**Claude Code** (one line in your terminal):

```bash
git clone --depth 1 https://github.com/ECORN-Agency/last-moment-bfcm /tmp/lmb && mkdir -p ~/.claude/skills && cp -r /tmp/lmb/skills/last-moment-bfcm ~/.claude/skills/
```

**Codex** (one line, for Codex versions with skills support):

```bash
git clone --depth 1 https://github.com/ECORN-Agency/last-moment-bfcm /tmp/lmb && mkdir -p ~/.codex/skills && cp -r /tmp/lmb/skills/last-moment-bfcm ~/.codex/skills/
```

**Claude app (web or desktop):** download [`last-moment-bfcm-skill.zip`](last-moment-bfcm-skill.zip), then
Settings › Capabilities › Skills › Upload skill.

**No install, any agent with web access** (paste into the chat):

```text
Read https://github.com/ECORN-Agency/last-moment-bfcm/blob/main/skills/last-moment-bfcm/SKILL.md and https://github.com/ECORN-Agency/last-moment-bfcm/blob/main/skills/last-moment-bfcm/references/checklist.md, follow them exactly, and start the Last Moment BFCM audit of my Shopify store.
```
