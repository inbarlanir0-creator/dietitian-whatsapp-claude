# Install (written for Claude Code)

Your user is a clinical dietitian who talks to clients on WhatsApp (often WhatsApp Business).
Speak Hebrew with her. Do the steps in order; she only answers questions and types a code on her phone.

## 1. The WhatsApp bridge

Clone https://github.com/tatarco/whatsapp-claude-bridge to `~/agents/whatsapp-claude-bridge` and follow
its `INSTALL.md` completely (ask which number(s), pair, start the service, install the `whatsapp` skill,
verify). Things to stress to her while doing it:

- Install on one machine that stays on most of the day and that only she logs into.
- Client messages are health data. On macOS check FileVault is on (`fdesetup status`) and tell her if not.
- The verification test message goes to *her own* number or someone she names, after she approves the text.

## 2. The dietitian skill

```bash
git clone https://github.com/inbarlanir0-creator/dietitian-whatsapp-claude ~/agents/dietitian-whatsapp-claude
mkdir -p ~/.claude/skills && cp -r ~/agents/dietitian-whatsapp-claude/skill/dietitian-clients ~/.claude/skills/
mkdir -p ~/clinic/clients && cp -n ~/agents/dietitian-whatsapp-claude/templates/clients.json ~/clinic/clients.json
chmod 700 ~/clinic
```

## 3. Build the client list with her

Run `curl -s "localhost:8788/chats?limit=200"` and show her the 1:1 chats (names only, no message text).
Ask which of them are clients. For each one she confirms, add an entry to `~/clinic/clients.json`
(name, every JID that chat appears under - `@s.whatsapp.net` and `@lid` -, phone, optional weigh-in day
and goal notes she gives you). Never guess who is a client.

## 4. First run

Ask her to say "מה המטופלים כתבו לי השבוע?" and run the morning-summary routine from the skill, so she
sees it working. Then show her the list of things she can ask (README.md, section "מה אפשר לבקש").

## Optional: voice notes

Hebrew voice notes need transcription. Either a local whisper (`brew install whisper-cpp ffmpeg` plus the
large-v3-turbo model) or an API key from her (Groq / OpenAI). Store any key in the OS keychain
(`security add-generic-password` on macOS), never in a file or in chat logs.
