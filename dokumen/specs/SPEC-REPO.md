---
title: SPEC-REPO — Spesifikasi Struktur Repositori
document_id: SPEC-REPO
version: 1.0
cb_reference: [CB §37]
status: DRAFT
owner: Development Team
last_updated: 2026-08-29
---

# SPEC-REPO — Spesifikasi Struktur Repositori

Panduan struktur kode untuk konsistensi tim PAUGERAN.

## Referensi CB
- [CB §37] — Repository structure dan configuration management

---

## Struktur Direktori Lengkap

```
PAUGERAN/
├── README.md                          # Dokumentasi root repository
├── INSTRUKSI.md                       # Daftar dokumen turunan
├── PAUGERAN CONTRACT BASELINE.md     # Contract Baseline
│
├── /specs/                            # Dokumentasi spesifikasi teknis
│   ├── SPEC-REPO.md                  # Struktur repository (dokumen ini)
│   ├── SPEC-ARCH.md                  # Arsitektur teknis
│   ├── SPEC-DB.md                    # Database dan migrasi
│   ├── SPEC-API.md                   # OpenAPI specification
│   ├── SPEC-GRAPH.md                 # Custom graph engine
│   ├── SPEC-LLM.md                   # Multi-provider LLM router
│   ├── SPEC-KB.md                    # Legal knowledge base
│   ├── SPEC-LICENSE.md               # Sistem lisensi
│   ├── SPEC-AUTH.md                  # Autentikasi & otorisasi
│   ├── SPEC-UI.md                    # Antarmuka pengguna
│   ├── SPEC-WEB.md                   # Penelitian web
│   ├── SPEC-CITATION.md              # Inline citation
│   ├── SPEC-EXPORT.md                # Export dokumen profesional
│   └── SPEC-A11Y.md                  # Aksesibilitas
│
├── /tests/                            # Dokumentasi pengujian
│   ├── TEST-PLAN.md                  # Rencana pengujian keseluruhan
│   ├── TEST-CASES.md                 # Kumpulan test cases
│   ├── TEST-HALLUCINATION.md         # Protokol uji anti-halusinasi
│   ├── TEST-PERFORMANCE.md           # Protokol uji performa
│   ├── TEST-SECURITY.md              # Protokol uji keamanan
│   └── /test-data/                   # Data uji
│       ├── hallucination-test-set.json
│       └── performance-benchmarks.json
│
├── /ops/                              # Dokumentasi operasional
│   ├── DEPLOY-GUIDE.md               # Panduan deployment
│   ├── OPS-RUNBOOK.md                # Runbook operasional
│   ├── OPS-BACKUP.md                 # Prosedur backup & recovery
│   ├── OPS-CICD.md                   # Pipeline CI/CD
│   └── /scripts/                     # Script deployment dan operasional
│       ├── deploy.sh
│       ├── backup.sh
│       └── restore.sh
│
├── /docs/                             # Dokumentasi pengguna
│   ├── USER-GUIDE.md                 # Panduan pengguna
│   ├── ADMIN-GUIDE.md                # Panduan administrator
│   ├── COPY-STANDARDS.md             # Standar penulisan konten
│   └── /images/                      # Gambar dan diagram untuk dokumentasi
│
├── /legal/                            # Dokumen legal & bisnis
│   ├── LEGAL-TOS.md                  # Terms of Service
│   ├── LEGAL-PRIVACY.md              # Privacy Policy
│   ├── LEGAL-LICENSE-AGREEMENT.md    # Perjanjian Lisensi
│   └── BUSINESS-PRICING.md           # Strategi Harga
│
├── /design/                           # Sistem desain
│   ├── DESIGN-SYSTEM.md              # Design system
│   ├── /tokens/                      # Design tokens
│   │   ├── colors.json
│   │   ├── typography.json
│   │   └── spacing.json
│   └── /components/                  # Component specifications
│
├── /src/                              # Source code
│   ├── /backend/                     # Rust backend
│   │   ├── Cargo.toml
│   │   ├── Cargo.lock
│   │   └── /crates/                 # Cargo workspace
│   │       ├── paugeran-core/       # Inti aplikasi
│   │       ├── paugeran-graph/      # Graph engine
│   │       ├── paugeran-llm/        # LLM router
│   │       ├── paugeran-kb/         # Knowledge base
│   │       ├── paugeran-license/    # License validation
│   │       ├── paugeran-web/        # Web research
│   │       └── paugeran-api/        # HTTP API server
│   │
│   └── /frontend/                    # SolidJS frontend
│       ├── package.json
│       ├── pnpm-workspace.yaml
│       └── /packages/
│           ├── @paugeran/ui          # UI components
│           ├── @paugeran/core        # Core logic
│           ├── @paugeran/app         # Main app
│           └── @paugeran/desktop     # Tauri desktop
│
├── /.github/                          # GitHub configuration
│   ├── workflows/                     # CI/CD pipelines
│   │   ├── test.yml
│   │   ├── build.yml
│   │   └── release.yml
│   └── CODEOWNERS                     # Code ownership
│
├── /.config/                          # Konfigurasi development
│   ├── .editorconfig
│   ├── .prettierrc
│   ├── .rustfmt.toml
│   └── .env.example
│
├── /docker/                           # Docker configuration
│   ├── Dockerfile.backend
│   ├── Dockerfile.frontend
│   └── docker-compose.yml
│
└── agen.md                            # Kontrak AI Agent
└── IMPLEMENTASI-STATUS.md             # Status implementasi (living document)
```

---

## Konvensi Penamaan

### Direktori
- Gunakan `/lowercase/` untuk direktori (kebab-case)
- Direktori penting: `/src`, `/tests`, `/docs`, `/specs`, `/ops`, `/legal`, `/design`

### File Source Code

#### Rust Backend
```
paugeran-core/
├── src/
│   ├── lib.rs               # Root module
│   ├── main.rs              # Entry point (jika binary)
│   ├── config.rs            # Configuration
│   ├── error.rs             # Error types
│   ├── models.rs            # Data models
│   ├── utils.rs             # Utility functions
│   └── modules/
│       ├── auth.rs
│       ├── graph.rs
│       └── llm.rs
├── tests/                   # Integration tests
├── benches/                 # Performance benchmarks
├── Cargo.toml
└── Cargo.lock
```

- **File names:** `snake_case.rs`
- **Module names:** `snake_case` (dalam deklarasi)
- **Struct names:** `PascalCase`
- **Trait names:** `PascalCase`
- **Function names:** `snake_case`
- **Constant names:** `SCREAMING_SNAKE_CASE`
- **Type aliases:** `PascalCase`

#### SolidJS Frontend
```
@paugeran/app/
├── src/
│   ├── App.tsx              # Root component
│   ├── index.tsx            # Entry point
│   ├── /components/         # Reusable components
│   │   ├── Button.tsx
│   │   ├── Modal.tsx
│   │   └── ChatInterface.tsx
│   ├── /pages/              # Page components
│   │   ├── ChatPage.tsx
│   │   └── SettingsPage.tsx
│   ├── /hooks/              # Custom hooks
│   │   ├── useLLM.ts
│   │   └── useKnowledgeBase.ts
│   ├── /services/           # API services
│   │   ├── api.ts
│   │   └── llmService.ts
│   ├── /types/              # TypeScript types
│   │   └── index.ts
│   ├── /styles/             # Global styles
│   │   └── index.css
│   └── /utils/              # Utility functions
│       └── format.ts
├── tsconfig.json
├── package.json
└── solid.config.ts
```

- **File names:** `PascalCase.tsx` (components), `camelCase.ts` (utilities)
- **Component names:** `PascalCase`
- **Function names:** `camelCase`
- **Hook names:** `useXxx` (camelCase dengan prefix `use`)
- **Constant names:** `SCREAMING_SNAKE_CASE` atau `camelCase`
- **Type names:** `PascalCase` (interface, type, enum)

### Dokumentasi & Spec
- **File names:** `UPPERCASE-KEBAB-CASE.md` (e.g., `SPEC-ARCH.md`, `TEST-PLAN.md`)
- **Dokumen khusus:** `lowercase-kebab-case.md` (e.g., `agen.md`, `contributing.md`)

---

## Konvensi Penamaan Modul Rust

### Struktur Crate
```
paugeran-core/
├── src/
│   ├── lib.rs               # Public API
│   ├── auth/
│   │   ├── mod.rs           # Module re-exports
│   │   ├── jwt.rs
│   │   └── password.rs
│   ├── graph/
│   │   ├── mod.rs
│   │   ├── engine.rs
│   │   └── state.rs
│   └── errors.rs            # Centralized error types
```

- **Module root:** `mod.rs` (re-exports public API)
- **Module files:** `snake_case.rs`
- **Public modules:** deklarasi di `lib.rs` atau `mod.rs` parent
- **Private modules:** dengan prefix `_` jika diperlukan (jarang)

---

## Konvensi Penamaan Komponen SolidJS

### Component Structure
```
@paugeran/ui/
├── src/
│   ├── index.ts             # Public exports
│   ├── /components/
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.module.css
│   │   │   └── index.ts
│   │   └── Modal/
│   │       ├── Modal.tsx
│   │       ├── Modal.module.css
│   │       └── index.ts
│   └── /hooks/
│       ├── index.ts
│       └── useCustom.ts
```

- **Component folder:** `PascalCase/` matching component name
- **Component file:** `ComponentName.tsx`
- **Styles:** `ComponentName.module.css` (CSS modules)
- **Index export:** `index.ts` untuk re-export public API

---

## Cargo Workspace Configuration

```toml
# Cargo.toml (root)
[workspace]
members = [
    "crates/paugeran-core",
    "crates/paugeran-graph",
    "crates/paugeran-llm",
    "crates/paugeran-kb",
    "crates/paugeran-license",
    "crates/paugeran-web",
    "crates/paugeran-api",
]

[workspace.package]
version = "1.0.0"
authors = ["PAUGERAN Team"]
edition = "2021"
license = "MIT"

[profile.release]
opt-level = 3
lto = true
codegen-units = 1
```

---

## pnpm Workspace Configuration

```yaml
# pnpm-workspace.yaml
packages:
  - 'packages/*'

# .npmrc
shamefully-hoist=true
strict-peer-dependencies=false
```

---

## Code Ownership (CODEOWNERS)

```
# .github/CODEOWNERS

# Backend
/src/backend/                   @backend-team
/src/backend/crates/paugeran-graph/    @graph-team
/src/backend/crates/paugeran-llm/      @llm-team

# Frontend
/src/frontend/                  @frontend-team
/src/frontend/packages/ui/      @ui-team

# Tests
/tests/                         @qa-team

# Documentation
/specs/                         @doc-team
/docs/                          @doc-team

# Legal
/legal/                         @legal-team

# DevOps
/ops/                           @devops-team
/.github/                       @devops-team
```

---

## Struktur Test

### Backend Tests
```
# Unit tests dalam file yang sama
// paugeran-core/src/auth/password.rs
#[cfg(test)]
mod tests {
    use super::*;
    
    #[test]
    fn test_hash_password() {
        // ...
    }
}
```

### Integration Tests
```
# Direktori terpisah
paugeran-core/
├── tests/
│   ├── common/mod.rs
│   ├── integration_auth.rs
│   └── integration_graph.rs
```

### Frontend Tests
```
# Collocated dengan components
@paugeran/ui/
├── src/components/Button/
│   ├── Button.tsx
│   └── Button.test.tsx
```

---

## Versionning

### Semantic Versioning
- **MAJOR.MINOR.PATCH** (e.g., `1.2.3`)
- **MAJOR:** Breaking changes
- **MINOR:** New features (backward compatible)
- **PATCH:** Bug fixes

### Version Sync
- Semua dokumen turunan harus sinkron dengan CB version
- Release notes harus mencantumkan versi

---

## Dokumentasi Kode

### Rust
```rust
/// Brief description of the function.
///
/// Longer description with details.
///
/// # Arguments
/// * `param` - Description
///
/// # Returns
/// Description of return value
///
/// # Errors
/// Description of error conditions
///
/// # Examples
/// ```
/// let result = function(param);
/// ```
pub fn function(param: Type) -> Result<ReturnType, Error> {
    // Implementation
}
```

### TypeScript
```typescript
/**
 * Brief description of the function.
 * @param param - Description
 * @returns Description of return value
 * @example
 * const result = function(param);
 */
export function function(param: Type): ReturnType {
    // Implementation
}
```

---

## Git Workflow

### Branch Naming
- `feature/feature-name` — Feature baru
- `fix/bug-name` — Bug fix
- `docs/doc-name` — Documentation
- `refactor/component-name` — Refactoring
- `chore/task-name` — Chore tasks

### Commit Message
```
<type>(<scope>): <subject>

<body>

<footer>
```

- `type`: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`
- `scope`: Modul atau fitur yang diubah
- `subject`: Imperative, lowercase, no period
- `body`: Detailed explanation
- `footer`: Issue references

---

## Quality Gates

- ✅ Linting: Semua file harus pass linting sebelum commit
- ✅ Testing: Semua test harus pass
- ✅ Documentation: Semua public API harus terdokumentasi
- ✅ Code Review: Minimal 1 approval sebelum merge

---

## Tools Wajib

| Tool | Purpose | Config File |
|------|---------|------------|
| Rust Formatter | Format kode Rust | `.rustfmt.toml` |
| Clippy | Lint Rust | `clippy.toml` |
| Prettier | Format JS/TS | `.prettierrc` |
| ESLint | Lint JS/TS | `.eslintrc.json` |
| EditorConfig | IDE settings | `.editorconfig` |
| Git Hooks | Pre-commit checks | `.husky/` |

---

## Checklist Implementasi

- [ ] Struktur direktori sesuai dengan spesifikasi
- [ ] Konvensi penamaan diterapkan secara konsisten
- [ ] Cargo workspace dikonfigurasi dengan benar
- [ ] pnpm workspace dikonfigurasi dengan benar
- [ ] CODEOWNERS file dibuat
- [ ] Dokumentasi kode diterapkan untuk semua public API
- [ ] Git workflow diadopsi oleh tim
- [ ] Quality gates diterapkan melalui CI/CD

---

## Referensi Tambahan

- [Rust Naming Conventions](https://rust-lang.github.io/api-guidelines/naming.html)
- [SolidJS Best Practices](https://docs.solidjs.com)
- [Conventional Commits](https://www.conventionalcommits.org/)
