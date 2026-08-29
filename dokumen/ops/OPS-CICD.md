---
title: OPS-CICD — CI/CD Pipeline Definition
document_id: OPS-CICD
version: 1.0
cb_reference: [CB §24]
status: DRAFT
owner: DevOps Team
last_updated: 2026-08-29
---

# OPS-CICD — CI/CD Pipeline Definition

Definisi pipeline GitHub Actions untuk build, test, dan release.

---

## Pipeline Stages

```
Commit
  ↓
Lint & Format Check
  ↓
Build (Backend + Frontend)
  ↓
Unit Tests
  ↓
Integration Tests
  ↓
Security Scan
  ↓
E2E Tests
  ↓
Package (Docker, Binary)
  ↓
Push to Registry
  ↓
Deploy to Staging
  ↓
Smoke Tests
  ↓
Deploy to Production (manual approval)
  ↓
Post-deployment Tests
```

---

## GitHub Actions Workflows

### 1. PR Validation (.github/workflows/pr.yml)

```yaml
name: PR Validation

on:
  pull_request:
    branches: [main, develop]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: dtolnay/rust-toolchain@stable
      - run: cargo fmt --all -- --check
      - run: cargo clippy -- -D warnings
      
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: pnpm
      - run: pnpm install
      - run: pnpm run lint

  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    
    steps:
      - uses: actions/checkout@v3
      - run: cargo test --all
      - run: pnpm run test
```

### 2. Build & Release (.github/workflows/release.yml)

```yaml
name: Build & Release

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      # Build backend
      - uses: dtolnay/rust-toolchain@stable
      - run: cargo build --release
      - run: ./target/release/paugeran-api --version
      
      # Build frontend
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: pnpm install
      - run: pnpm run build
      
      # Create archives
      - run: |
          tar -czf paugeran-linux-amd64.tar.gz -C target/release paugeran-api
          zip -r paugeran-macos-amd64.zip target/release/paugeran-api
      
      # Sign binaries (GPG)
      - run: |
          echo "${{ secrets.GPG_PRIVATE_KEY }}" | gpg --import
          gpg --armor --detach-sign paugeran-linux-amd64.tar.gz
      
      # Create release
      - uses: softprops/action-gh-release@v1
        with:
          files: |
            paugeran-linux-amd64.tar.gz
            paugeran-linux-amd64.tar.gz.asc
            paugeran-macos-amd64.zip
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  docker:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      
      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      
      - name: Build and push
        uses: docker/build-push-action@v4
        with:
          context: .
          push: true
          tags: |
            paugeran/paugeran:latest
            paugeran/paugeran:${{ github.ref_name }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

### 3. Deploy to Staging (.github/workflows/deploy-staging.yml)

```yaml
name: Deploy to Staging

on:
  push:
    branches: [develop]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build Docker image
        run: docker build -t paugeran:staging .
      
      - name: Push to staging registry
        run: |
          docker tag paugeran:staging ${{ secrets.STAGING_REGISTRY }}/paugeran:staging
          docker push ${{ secrets.STAGING_REGISTRY }}/paugeran:staging
      
      - name: Deploy via SSH
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.STAGING_HOST }}
          username: ${{ secrets.STAGING_USER }}
          key: ${{ secrets.STAGING_SSH_KEY }}
          script: |
            docker pull ${{ secrets.STAGING_REGISTRY }}/paugeran:staging
            docker-compose up -d
            
      - name: Run smoke tests
        run: |
          curl -f http://staging.paugeran.dev/api/health || exit 1
```

---

## Test Coverage Requirements

| Component | Min Coverage | Target |
|-----------|-------------|--------|
| Backend (Rust) | 75% | 85% |
| Frontend (SolidJS) | 70% | 80% |
| API | 75% | 85% |
| Auth | 80% | 90% |
| KB/Search | 75% | 85% |
| License | 85% | 95% |
| Overall | 75% | 80% |

---

## Deployment Strategy

### Staging Deployment
```
Automatic on every commit to 'develop'
└─ Runs test suite
└─ Builds Docker image
└─ Deploys to staging.paugeran.dev
└─ Runs smoke tests
└─ Notifies team if failures
```

### Production Deployment
```
Manual approval required
└─ Tag version: git tag v1.0.0
└─ Push tag: git push origin v1.0.0
└─ GitHub Actions builds + signs binaries
└─ Team reviews + approves
└─ Actions deploys to production
└─ Post-deployment tests run
└─ Rollback available if needed
```

---

## Versioning Strategy

```
Version: MAJOR.MINOR.PATCH

MAJOR: Breaking changes
MINOR: New features, backward-compatible
PATCH: Bug fixes

Examples:
v1.0.0 - First release
v1.0.1 - Bug fix
v1.1.0 - New feature
v2.0.0 - Breaking changes
```

---

## Binary Distribution

```
Artifacts published to:
├─ GitHub Releases
├─ Docker Hub (docker.io/paugeran/paugeran)
├─ Homebrew (brew install paugeran)
└─ Binary downloads (paugeran.dev/download)

Checksums:
├─ SHA256 hashes published
├─ Binaries GPG-signed
└─ Verification instructions provided
```

---

## Code Signing

```
GPG Key:
├─ Store in GitHub Secrets
├─ Used to sign releases
└─ Public key: https://paugeran.dev/pgp-key

Verification:
$ gpg --verify paugeran-linux-amd64.tar.gz.asc
```

---

## Security Scanning

```yaml
- name: Run security scan (Trivy)
  run: trivy image paugeran/paugeran:latest

- name: Dependency check
  run: |
    cargo audit
    npm audit

- name: SAST scan
  uses: securego/gosec-action@master
```

---

## Checklist Implementasi

- [ ] GitHub Actions workflows created
- [ ] Build pipeline working
- [ ] Test suite automated
- [ ] Docker builds working
- [ ] Staging deployment automated
- [ ] Production deployment manual+approved
- [ ] Security scanning enabled
- [ ] Release process documented
- [ ] Rollback procedures ready
- [ ] Team trained

