---
title: SPEC-CITATION — Spesifikasi Inline Citation
document_id: SPEC-CITATION
version: 1.0
cb_reference: [CB §18], [CB §20]
status: DRAFT
owner: Frontend & Backend Team
last_updated: 2026-08-29
---

# SPEC-CITATION — Spesifikasi Inline Citation

Standarisasi format dan visualisasi citation.

---

## Citation Format

### Regulation (UU)
```
[UU] No. 8/2024 Pasal 12 Ayat 3
```

### Government Regulation (PP)
```
[PP] No. 10/2024 Pasal 5
```

### Court Decision (Putusan)
```
[Putusan] MK No. 123/PUU/2023
```

### Web Source
```
[Web] Source Title (date) https://...
```

### Document/KB Entry
```
[Doc] Title | Section
```

---

## Markdown Syntax

```markdown
This is text with [citation](pasal:UU-No.8-2024-12-3) inline.

[^1]: Reference Pasal 12 Ayat 3 of UU No. 8/2024

Or link format:
See [UU No. 8/2024 Pasal 12](source:uu-8-2024-12)
```

---

## Frontend Rendering

### Inline Citation Display
- **Color:** Primary blue
- **Underline:** Dotted
- **Hover:** Show tooltip with full citation
- **Click:** Open detail panel

### Tooltip Format
```
┌──────────────────────────┐
│ UU No. 8 Tahun 2024      │
│ Pasal 12 Ayat 3          │
│                          │
│ "Setiap orang yang..."   │
│                          │
│ [View Full] [Copy]       │
└──────────────────────────┘
```

### Citation Panel
```
Left side: Main content
Right side: Citation details
├─ Source: UU No. 8/2024
├─ Pasal: 12 Ayat 3
├─ Full text: "..."
├─ Related: [Pasal 13] [Pasal 14]
└─ Link: [jdih.kemenkumham.go.id]
```

---

## Validation Rules

- ✅ Citation source exists in KB
- ✅ Pasal/section correct format
- ✅ No hallucinated references
- ✅ Date information accurate
- ❌ Reject invalid Pasal numbers

---

## Export Formats

### PDF Export
- Inline citations as hyperlinks
- Footnotes with full citation text
- Bibliography at end

### DOCX Export
- Track changes (if different from original)
- Comments with citation info
- Hyperlinks to sources

### Markdown Export
```markdown
[^1]: Full citation text
```

---

## Checklist Implementasi

- [ ] Citation format standardized
- [ ] Frontend rendering complete
- [ ] Validation rules enforced
- [ ] Export formats working
- [ ] PDF/DOCX/Markdown export tested

