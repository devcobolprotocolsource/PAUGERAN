---
title: USER-GUIDE — Panduan Pengguna
document_id: USER-GUIDE
version: 1.0
cb_reference: [CB §29], [CB §28], [CB §27]
status: DRAFT
owner: Documentation Team
last_updated: 2026-08-29
---

# USER-GUIDE — Panduan Pengguna

Panduan lengkap untuk pengguna akhir PAUGERAN.

---

## Quick Start (5 Menit)

### 1. Installation
- Download dari paugeran.dev
- Run installer
- Launch application

### 2. Create Account
- Click "Register"
- Enter email and password
- Verify email
- Set username

### 3. First Chat
- Click "New Chat"
- Type legal question (e.g., "Apa hukuman untuk korupsi?")
- Click "Send"
- Wait for AI response with citations

### 4. Export Document
- Click "Export" (top right)
- Select format (PDF/DOCX)
- Click "Download"

---

## Installation

### System Requirements
- OS: Windows 10+, macOS 11+, Ubuntu 20.04+
- RAM: 200MB minimum, 500MB recommended
- Disk: 500MB free space
- Internet: Required for online mode

### Installation Steps

**Windows:**
1. Download paugeran-setup-x64.exe
2. Run installer
3. Follow wizard
4. Launch from Start menu

**macOS:**
1. Download paugeran-x64.dmg
2. Drag app to Applications folder
3. Launch from Applications

**Linux:**
1. Download paugeran-x64.AppImage
2. chmod +x paugeran-x64.AppImage
3. ./paugeran-x64.AppImage

---

## Activation & License

### Trial License
- Duration: 14 days
- Features: All
- No payment required

### Activate Personal License
1. Purchase at paugeran.dev/pricing
2. Receive license key via email
3. Copy license key
4. Settings → License → Paste key → Activate
5. Restart app

### Offline License
1. Open Settings → License
2. Click "Generate Offline License"
3. Copy installation ID
4. Visit paugeran.dev/offline-license
5. Paste installation ID
6. Download license file
7. Load in Settings

---

## Configuration

### LLM Provider Setup

**Anthropic Claude (Recommended)**
1. Go to Settings → LLM Provider
2. Select "Anthropic"
3. Paste API key from https://console.anthropic.com
4. Test connection
5. Save

**OpenAI**
1. Settings → LLM Provider → OpenAI
2. Paste API key from https://platform.openai.com
3. Test & Save

**Local Ollama**
1. Install Ollama: https://ollama.ai
2. Run: `ollama serve`
3. In PAUGERAN: Settings → LLM Provider → Ollama
4. URL: http://localhost:11434
5. Save

---

## Using PAUGERAN

### Chat Interface

```
┌─────────────────────────────────────┐
│ Title __________ | ⚙️ Menu          │
├────────┬─────────────────────────────┤
│ Sidebar│ Messages                    │
│ [Chats]│ ─────────────────           │
│ [1] ✓  │ You: Apa itu UU PDP?       │
│ [2]    │                             │
│ [3]    │ AI: UU Perlindungan Data... │
│        │ [Citation] Pasal 1         │
│        │                             │
│ [+ New]│ Type message... │ [Send→] │
└────────┴─────────────────────────────┘
```

### Creating Session
- Click "New Chat"
- Give it a title (optional)
- Start typing query
- Press Enter or click Send

### Citations & References
- Blue highlighted text = citation
- Hover to see source
- Click to view full text
- All sources stored

### Search Knowledge Base
- Click "Knowledge Base" (sidebar)
- Enter search term
- Browse results
- Click to view full document

---

## Export & Share

### Export Document
1. Click "Export" (top toolbar)
2. Choose format:
   - PDF (most compatible)
   - DOCX (editable in Word)
3. Choose template:
   - Standard (simple)
   - Formal (legal)
   - Memorandum (internal)
   - Opinion Letter (legal opinion)
4. Options:
   - Include citations ✓
   - Include metadata ✓
5. Click "Export"
6. Download file

### Copy Link (Premium)
- Click share icon
- Generate shareable link
- Link expires in 30 days
- Recipient can view in browser (read-only)

---

## Knowledge Base

### Import Documents
1. Settings → Knowledge Base
2. Click "Import Document"
3. Select file (PDF, TXT, DOCX)
4. Classify type: UU / PP / Putusan / Document
5. Click "Import"
6. Wait for processing

### Create Custom KB Entry
1. New Document
2. Enter title
3. Enter content
4. Tag with category
5. Save

### Delete Document
1. Knowledge Base view
2. Find document
3. Click "..." menu
4. "Delete" (permanent)

---

## Settings & Preferences

### Appearance
- Light / Dark / Sepia mode
- Font size (100% - 200%)
- Show/hide sidebar
- Compact view

### Accessibility
- High contrast mode
- Reduce motion
- Screen reader optimized
- Large text

### Privacy
- Clear chat history
- Delete all data
- Disable analytics (optional)

### Shortcuts (Keyboard)

| Shortcut | Action |
|----------|--------|
| Cmd/Ctrl + N | New chat |
| Cmd/Ctrl + K | Search |
| Cmd/Ctrl + E | Export |
| Cmd/Ctrl + S | Settings |
| Escape | Close modal |

---

## Troubleshooting

### Can't login?
- Check email/password
- Verify account created
- Reset password via email
- Clear browser cache

### LLM Provider not working?
- Check API key valid
- Check internet connection
- Verify API key has credits
- Try different provider

### License expired?
- Grace period: 14 days
- Try renewing online
- Generate offline license
- Contact support

### Export failed?
- Check disk space
- Verify format selected
- Try PDF instead of DOCX
- Restart app

---

## FAQ

**Q: Is my data secure?**
A: Your queries are only sent to your configured LLM provider. We don't store your chats on our servers (local mode). License server only receives installation ID, not content.

**Q: Can I use offline?**
A: Yes with offline license. Offline mode uses local Ollama or cached KB.

**Q: How many chats can I keep?**
A: Unlimited. Limited by disk space.

**Q: Can I export to Google Docs?**
A: Not directly, but export to DOCX and upload to Google Docs.

**Q: How do I update to new version?**
A: Auto-update checks available. Settings → About → Check for updates.

---

## Support

- Website: https://paugeran.dev
- Email: support@paugeran.dev
- Community: https://github.com/paugeran/paugeran/discussions
- Report Issues: https://github.com/paugeran/paugeran/issues

---

## Legal

- Terms of Service: https://paugeran.dev/terms
- Privacy Policy: https://paugeran.dev/privacy
- License Agreement: https://paugeran.dev/license

**Disclaimer:** PAUGERAN is an AI assistant, not a substitute for a qualified lawyer. Always verify information with legal professionals.

