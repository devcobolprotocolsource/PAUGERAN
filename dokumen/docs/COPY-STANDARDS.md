---
title: COPY-STANDARDS — Content Writing Standards
document_id: COPY-STANDARDS
version: 1.0
cb_reference: [CB §4]
status: DRAFT
owner: Marketing & Content Team
last_updated: 2026-08-29
---

# COPY-STANDARDS — Content Writing Standards

Standar menulis konten untuk PAUGERAN.

---

## Tone of Voice

### Professional yet Approachable
✅ **Good:** "Let's find the law you need quickly"
❌ **Bad:** "PAUGERAN AI will optimize your legal research algorithm"

### Authoritative but Clear
✅ **Good:** "UU No. 8/2024 regulates data privacy"
❌ **Bad:** "The aforementioned statute pertains to the aforesaid matter"

### Action-Oriented
✅ **Good:** "Upload your documents to search instantly"
❌ **Bad:** "Documents may be uploaded for potential search functionality"

### Honest about Limitations
✅ **Good:** "AI can help, but always verify with a lawyer"
❌ **Bad:** "Get instant legal answers"

---

## Legal Language Standards

### Indonesian Legal Terms (Baku)

| Use | Instead of | Example |
|-----|-----------|---------|
| Undang-Undang (UU) | UU / Law | UU No. 8/2024 |
| Peraturan Pemerintah (PP) | PP / Regulation | PP No. 5/2024 |
| Peraturan Menteri (Permen) | Permen / Minister Reg | Permen Kominfo No. 4/2024 |
| Keputusan Menteri (Kepmen) | Kepmen / Minister Decision | Kepmen LHK No. 123/2024 |
| Putusan Mahkamah | Putusan MK / Court Decision | Putusan MK No. 123/PUU/2023 |
| Pasal | Article / Section | Pasal 12 Ayat (3) |

### Avoid

❌ "The law says..."
✅ "UU No. 8/2024 Pasal 12 states..."

❌ "Regulation..."
✅ "Peraturan Pemerintah..."

---

## Format Standards

### Document References

| Format | Example |
|--------|---------|
| Regulation | UU No. 8 Tahun 2024 atau UU No. 8/2024 |
| Section | Pasal 12 |
| Subsection | Pasal 12 Ayat (3) |
| Point | Pasal 12 Ayat (3) huruf a |
| Court Decision | Putusan MK No. 123/PUU-VI/2008 |
| Court | Pengadilan Negeri Jakarta Selatan |

### Date Format

| Context | Format | Example |
|---------|--------|---------|
| Formal document | DD Bulan YYYY | 29 Agustus 2026 |
| Long form | DD MMMM YYYY | 29 August 2026 |
| Short form (ID) | DD/MM/YYYY | 29/08/2026 |
| Short form (ISO) | YYYY-MM-DD | 2026-08-29 |

### Numbers & Currency

| Type | Format | Example |
|------|--------|---------|
| Regulation number | No. X/YYYY | No. 8/2024 |
| Currency (Rupiah) | Rp X.XXX.XXX | Rp 1.000.000 |
| Currency (USD) | $ or USD | $99/year |
| Decimal | Comma (ID) | 1,5 juta |
| Million | juta / jutaan | 1 juta dokumen |

---

## Error Messages

### User-Friendly

✅ **Good:** "Email already registered. Try logging in instead."
❌ **Bad:** "ERR_DUPLICATE_EMAIL: Unique constraint violation"

✅ **Good:** "Your internet connection seems slow. Please try again."
❌ **Bad:** "Network timeout: ECONNREFUSED"

### Actionable

✅ **Good:** "Invalid email. Did you mean john.doe@company.com?"
❌ **Bad:** "Invalid input format"

✅ **Good:** "Your license expired. Renew here."
❌ **Bad:** "License validation failed"

### Honest

✅ **Good:** "We're having trouble reaching the AI service. Retrying..."
❌ **Bad:** "Service unavailable"

---

## Empty State Messages

### Friendly & Helpful

```
No chats yet
"Start your first legal research now. 
 Just type a question or paste a regulation number."
[Start Chatting →]

No search results
"We didn't find anything matching 'UU No. 99/2024'. 
 Try searching for existing regulations or upload a new document."
[Browse Regulations →] [Upload Document →]

No documents in knowledge base
"Your knowledge base is empty. 
 Upload legal documents to start searching and analyzing."
[Upload Documents →]
```

---

## Button & Link Text

### Clear & Action-Oriented

✅ Good:
- "Upload Documents"
- "Start New Chat"
- "Export as PDF"
- "View Full Text"

❌ Avoid:
- "Click Here"
- "Submit"
- "OK"
- "Go"

### Consistent

Always use same text for same action:
- "Upload Document" (singular) - for single
- "Upload Documents" (plural) - for multiple
- Not "Upload Files" then "Upload PDF"

---

## Localization (ID/EN)

### Indonesian (ID) Priority

- Default language: Indonesian
- Indonesian first, English secondary
- Indonesian grammar & conventions

### Numbers & Dates (Mixed Context)

```
Indonesia context:
├─ "29 Agustus 2026" (long form)
├─ "Rp 1.000.000" (with periods)
└─ Comma as decimal: "1,5 juta"

English context:
├─ "August 29, 2026" (long form)
├─ "$99" (simple format)
└─ Period as decimal: "1.5 million"
```

---

## UI Copy Guidelines

### Login/Register
```
Login:
├─ "Email atau username"
├─ "Kata sandi"
└─ "Masuk"

Register:
├─ "Email"
├─ "Buat kata sandi (min 12 karakter)"
├─ "Setujui Syarat & Ketentuan"
└─ "Buat akun"
```

### Chat Interface
```
Input:
├─ "Tanya tentang regulasi..."
└─ "Kirim" (button)

Response:
├─ "[Tunggu...]"
└─ "[Salin] [Ekspor] [Beri tahu masalah]"
```

### License Status
```
Active:
├─ "✅ Lisensi aktif"
└─ "Berlaku hingga 29 Agustus 2027"

Expiring:
├─ "⚠️ Lisensi akan kadaluarsa"
└─ "Perpanjang dalam 7 hari"

Expired:
├─ "❌ Lisensi kadaluarsa"
└─ "Perpanjang sekarang untuk melanjutkan"
```

---

## Checklist Implementasi

- [ ] All UI text reviewed
- [ ] Legal terms standardized
- [ ] Error messages tested
- [ ] Empty states friendly
- [ ] Localization complete
- [ ] Button text consistent
- [ ] Date formats standardized
- [ ] Team trained on standards

