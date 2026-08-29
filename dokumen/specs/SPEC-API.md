---
title: SPEC-API — OpenAPI Specification
document_id: SPEC-API
version: 1.0
cb_reference: [CB §13]
status: DRAFT
owner: API Team
last_updated: 2026-08-29
---

# SPEC-API — OpenAPI Specification

Kontrak API yang dapat di-generate dan divalidasi otomatis.

## Referensi CB
- [CB §13] — API specification dan endpoints

---

## OpenAPI 3.1 Metadata

```yaml
openapi: 3.1.0
info:
  title: PAUGERAN API
  description: AI-powered legal research assistant API
  version: 1.0.0
  contact:
    name: PAUGERAN Support
    url: https://paugeran.dev
  license:
    name: MIT

servers:
  - url: http://localhost:3000/api/v1
    description: Local development
  - url: https://api.paugeran.dev/api/v1
    description: Production
```

---

## Authentication

### JWT Authentication

```yaml
components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: |
        JWT token issued by authentication endpoint.
        Include in Authorization header: `Bearer <token>`

security:
  - BearerAuth: []
```

### Token Structure

```json
{
  "sub": "user_id_uuid",
  "email": "user@example.com",
  "role": "user",
  "iat": 1693411200,
  "exp": 1693497600,
  "iss": "paugeran"
}
```

---

## Core Endpoints

### 1. Authentication

#### POST /auth/register
```yaml
post:
  summary: Register new user
  tags: [Authentication]
  requestBody:
    required: true
    content:
      application/json:
        schema:
          type: object
          required: [email, password]
          properties:
            email:
              type: string
              format: email
            password:
              type: string
              minLength: 8
            username:
              type: string
  responses:
    '201':
      description: User created successfully
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/User'
    '409':
      description: Email already registered
```

#### POST /auth/login
```yaml
post:
  summary: Authenticate user
  tags: [Authentication]
  requestBody:
    required: true
    content:
      application/json:
        schema:
          type: object
          required: [email, password]
          properties:
            email:
              type: string
              format: email
            password:
              type: string
  responses:
    '200':
      description: Login successful
      content:
        application/json:
          schema:
            type: object
            properties:
              token:
                type: string
              expires_in:
                type: integer
              user:
                $ref: '#/components/schemas/User'
    '401':
      description: Invalid credentials
```

### 2. Chat Sessions

#### POST /chat
```yaml
post:
  summary: Create or start chat session
  tags: [Chat]
  security:
    - BearerAuth: []
  requestBody:
    required: true
    content:
      application/json:
        schema:
          type: object
          properties:
            title:
              type: string
              default: "New Chat"
            system_prompt:
              type: string
              default: "You are a helpful legal research assistant"
            query:
              type: string
              description: Initial query to process
  responses:
    '201':
      description: Session created
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Session'
    '400':
      description: Invalid request
```

#### GET /chat/{session_id}
```yaml
get:
  summary: Get session details
  tags: [Chat]
  security:
    - BearerAuth: []
  parameters:
    - name: session_id
      in: path
      required: true
      schema:
        type: string
        format: uuid
  responses:
    '200':
      description: Session details
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Session'
```

#### WS /chat/{session_id}/stream
```yaml
get:
  summary: Stream chat responses via WebSocket
  tags: [Chat]
  security:
    - BearerAuth: []
  parameters:
    - name: session_id
      in: path
      required: true
      schema:
        type: string
        format: uuid
  responses:
    '101':
      description: Switching to WebSocket protocol
    '404':
      description: Session not found
```

**WebSocket Events:**

```yaml
# Server → Client
PhaseStartEvent:
  type: object
  properties:
    type:
      type: string
      enum: [phase_start]
    phase:
      type: integer
      minimum: 0
      maximum: 11
    timestamp:
      type: string
      format: date-time

ResponseChunkEvent:
  type: object
  properties:
    type:
      type: string
      enum: [response_chunk]
    content:
      type: string
    timestamp:
      type: string
      format: date-time

CitationFoundEvent:
  type: object
  properties:
    type:
      type: string
      enum: [citation_found]
    citation:
      $ref: '#/components/schemas/Citation'
    timestamp:
      type: string
      format: date-time
```

### 3. Knowledge Base

#### GET /knowledge-base
```yaml
get:
  summary: List knowledge bases
  tags: [Knowledge Base]
  security:
    - BearerAuth: []
  responses:
    '200':
      description: List of knowledge bases
      content:
        application/json:
          schema:
            type: array
            items:
              $ref: '#/components/schemas/KnowledgeBase'
```

#### GET /knowledge-base/search
```yaml
get:
  summary: Search knowledge base
  tags: [Knowledge Base]
  security:
    - BearerAuth: []
  parameters:
    - name: q
      in: query
      required: true
      schema:
        type: string
      description: Search query
    - name: type
      in: query
      schema:
        type: string
        enum: [semantic, keyword, hybrid]
        default: hybrid
    - name: limit
      in: query
      schema:
        type: integer
        default: 10
  responses:
    '200':
      description: Search results
      content:
        application/json:
          schema:
            type: object
            properties:
              results:
                type: array
                items:
                  $ref: '#/components/schemas/KBDocument'
              total:
                type: integer
```

#### POST /knowledge-base/documents
```yaml
post:
  summary: Add document to knowledge base
  tags: [Knowledge Base]
  security:
    - BearerAuth: []
  requestBody:
    required: true
    content:
      application/json:
        schema:
          type: object
          required: [title, content, type]
          properties:
            title:
              type: string
            content:
              type: string
            type:
              type: string
              enum: [uu, pp, putusan, document]
            metadata:
              type: object
  responses:
    '201':
      description: Document added
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/KBDocument'
```

### 4. Export

#### POST /export
```yaml
post:
  summary: Export session to PDF or DOCX
  tags: [Export]
  security:
    - BearerAuth: []
  requestBody:
    required: true
    content:
      application/json:
        schema:
          type: object
          required: [session_id, format]
          properties:
            session_id:
              type: string
              format: uuid
            format:
              type: string
              enum: [pdf, docx]
            template:
              type: string
              enum: [standard, formal, memorandum, opinion]
              default: standard
  responses:
    '200':
      description: Export successful
      headers:
        Content-Disposition:
          schema:
            type: string
      content:
        application/pdf:
          schema:
            type: string
            format: binary
        application/vnd.openxmlformats-officedocument.wordprocessingml.document:
          schema:
            type: string
            format: binary
```

### 5. License

#### GET /license/validate
```yaml
get:
  summary: Validate license
  tags: [License]
  parameters:
    - name: license_key
      in: query
      required: true
      schema:
        type: string
    - name: installation_id
      in: query
      required: true
      schema:
        type: string
  responses:
    '200':
      description: License valid
      content:
        application/json:
          schema:
            type: object
            properties:
              valid:
                type: boolean
              type:
                type: string
                enum: [trial, personal, team, enterprise]
              expires_at:
                type: string
                format: date-time
              features:
                type: array
                items:
                  type: string
    '401':
      description: License invalid or expired
```

---

## Schemas

### User
```yaml
User:
  type: object
  properties:
    user_id:
      type: string
      format: uuid
    email:
      type: string
      format: email
    username:
      type: string
    role:
      type: string
      enum: [admin, user]
    created_at:
      type: string
      format: date-time
    updated_at:
      type: string
      format: date-time
```

### Session
```yaml
Session:
  type: object
  properties:
    session_id:
      type: string
      format: uuid
    user_id:
      type: string
      format: uuid
    title:
      type: string
    status:
      type: string
      enum: [active, paused, completed]
    state_machine_phase:
      type: integer
      minimum: 0
      maximum: 11
    created_at:
      type: string
      format: date-time
    updated_at:
      type: string
      format: date-time
```

### Citation
```yaml
Citation:
  type: object
  properties:
    citation_id:
      type: string
      format: uuid
    source_type:
      type: string
      enum: [uu, pp, putusan, web, document]
    source_reference:
      type: string
    section:
      type: string
    excerpt:
      type: string
    url:
      type: string
      format: uri
    confidence:
      type: number
      minimum: 0
      maximum: 1
```

### KnowledgeBase
```yaml
KnowledgeBase:
  type: object
  properties:
    kb_id:
      type: string
      format: uuid
    title:
      type: string
    description:
      type: string
    is_system:
      type: boolean
    document_count:
      type: integer
    created_at:
      type: string
      format: date-time
```

### KBDocument
```yaml
KBDocument:
  type: object
  properties:
    document_id:
      type: string
      format: uuid
    kb_id:
      type: string
      format: uuid
    title:
      type: string
    type:
      type: string
      enum: [uu, pp, putusan, document]
    excerpt:
      type: string
    relevance_score:
      type: number
      minimum: 0
      maximum: 1
```

---

## Error Codes

| Code | Meaning | Example |
|------|---------|---------|
| 400 | Bad Request | Invalid query format |
| 401 | Unauthorized | Missing/invalid token |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Session/document not found |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Unexpected error |
| 503 | Service Unavailable | LLM service down |

### Error Response Format
```json
{
  "error": {
    "code": "invalid_query",
    "message": "Query must not be empty",
    "details": {
      "field": "query"
    }
  }
}
```

---

## Rate Limiting

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1693497600
```

- **Default:** 1000 requests per hour per user
- **Burst:** 100 requests per minute
- **Export:** 10 per hour
- **Chat:** Unlimited (rate limit per LLM provider)

---

## Versioning

- API version in URL: `/api/v1`, `/api/v2`, etc.
- Backward compatibility maintained for 2 major versions
- Deprecation notice: 6 months warning before removal

---

## Checklist Implementasi

- [ ] OpenAPI spec generated and validated
- [ ] All endpoints implemented
- [ ] Authentication tested
- [ ] Rate limiting implemented
- [ ] Error handling consistent
- [ ] WebSocket streaming working
- [ ] API documentation published
- [ ] SDK generated from spec

---

## Referensi Tambahan

- [OpenAPI Specification](https://spec.openapis.org/oas/v3.1.0)
- [REST API Best Practices](https://restfulapi.net/)
- [WebSocket Protocol](https://tools.ietf.org/html/rfc6455)
