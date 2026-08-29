---
title: SPEC-EXPORT — Spesifikasi Export Dokumen Profesional
document_id: SPEC-EXPORT
version: 1.0
cb_reference: [CB §19]
status: DRAFT
owner: Backend Team
last_updated: 2026-08-29
---

# SPEC-EXPORT — Spesifikasi Export Dokumen Profesional

Detail implementasi export PDF/DOCX.

---

## Export Templates

### Standard Template
- Clean, professional layout
- Margins: 1 inch all sides
- Font: Calibri 11pt body, Calibri Bold 12pt headers
- Spacing: 1.5x line height
- No watermarks

### Formal Template
- Legal-style formatting
- Standard legal margins
- Font: Times New Roman 12pt
- Spacing: Double-spaced
- Page numbers + footers
- Document reference number

### Memorandum Template
- Memo header (TO/FROM/DATE/RE)
- Subject line prominent
- Indented body
- Signature block

### Opinion Letter Template
- Letterhead
- Date and address
- "Re:" line
- Opinion statement prominent
- Signature and seal

---

## PDF Generation

```rust
pub struct PdfExporter {
    template: Template,
    font_embedding: bool,
}

impl PdfExporter {
    pub async fn export(
        &self,
        session: &Session,
        template: &str,
    ) -> Result<Vec<u8>> {
        // 1. Render template with data
        // 2. Add metadata
        // 3. Add bookmarks/TOC
        // 4. Generate PDF
        // 5. Sign (optional)
    }
}
```

**Lib:** `printpdf` or `pdfium-render`

---

## DOCX Generation

```rust
pub struct DocxExporter {
    template: Template,
}

impl DocxExporter {
    pub async fn export(
        &self,
        session: &Session,
        template: &str,
    ) -> Result<Vec<u8>> {
        // 1. Create document
        // 2. Apply styles
        // 3. Add content
        // 4. Add properties
        // 5. Generate DOCX
    }
}
```

**Lib:** `docx-rs`

---

## Metadata Embedding

### PDF Properties
```
Title: Chat session title
Author: Username
Creator: PAUGERAN v1.0
CreationDate: Export timestamp
Subject: Legal research
Keywords: Extracted tags
```

### DOCX Properties
```xml
<dc:title>Title</dc:title>
<dc:creator>User</dc:creator>
<dcterms:created>2026-08-29</dcterms:created>
```

---

## Bookmark & Hyperlink

### PDF Bookmarks
- Each Pasal → Bookmark
- Each citation → Clickable link
- Table of contents links

### DOCX Hyperlinks
- Citations as blue underlined links
- External links to sources
- TOC with field codes

---

## Image Handling

- Embed all images (no external refs)
- Max resolution: 300 DPI (print quality)
- Max file size: 5MB total images
- Compress if needed

---

## Font Embedding

- Embed fonts in PDF (no missing fonts)
- Support Unicode for Indonesian characters
- Fallback fonts if primary unavailable

---

## Performance

- Small export (< 2000 words): < 1s
- Medium (2000-5000): < 3s
- Large (5000+): < 5s

---

## Checklist Implementasi

- [ ] All 4 templates created
- [ ] PDF export working
- [ ] DOCX export working
- [ ] Metadata embedded
- [ ] Bookmarks and links created
- [ ] Image handling optimized
- [ ] Performance targets met
- [ ] Export tested with various sizes

