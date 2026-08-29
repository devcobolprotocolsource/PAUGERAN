---
title: SPEC-LLM — Spesifikasi Multi-Provider LLM Router
document_id: SPEC-LLM
version: 1.0
cb_reference: [CB §9]
status: DRAFT
owner: Backend Team
last_updated: 2026-08-29
---

# SPEC-LLM — Spesifikasi Multi-Provider LLM Router

Detail implementasi router LLM multi-provider.

## Referensi CB
- [CB §9] — LLM provider integration dan routing

---

## Provider Support Matrix

| Provider | Status | Models | Features |
|----------|--------|--------|----------|
| Anthropic | ✅ Required | Claude 3 (Opus, Sonnet, Haiku) | Streaming, vision, tool use |
| OpenAI | ✅ Required | GPT-4, GPT-3.5 | Streaming, vision, tool use |
| OpenAI-compatible | ✅ Supported | Any OpenAI-compatible | Via custom URL |
| Ollama | ✅ Supported | Any local model | Streaming |

---

## LLM Provider Trait

```rust
#[async_trait]
pub trait LlmProvider: Send + Sync {
    /// Get provider name
    fn name(&self) -> &str;
    
    /// Check if model is available
    async fn is_available(&self, model: &str) -> Result<bool>;
    
    /// Generate completion
    async fn generate(
        &self,
        request: GenerationRequest,
    ) -> Result<GenerationResponse>;
    
    /// Stream completion
    async fn stream(
        &self,
        request: GenerationRequest,
    ) -> Result<BoxStream<'static, Result<StreamEvent>>>;
    
    /// Get model information
    async fn get_model_info(&self, model: &str) -> Result<ModelInfo>;
    
    /// Get current usage
    fn get_usage(&self) -> ProviderUsage;
    
    /// Check rate limit status
    async fn check_rate_limit(&self) -> Result<RateLimitStatus>;
}
```

### Request Structure

```rust
pub struct GenerationRequest {
    pub model: String,
    pub messages: Vec<Message>,
    pub system_prompt: Option<String>,
    pub max_tokens: Option<usize>,
    pub temperature: Option<f32>,
    pub top_p: Option<f32>,
    pub tools: Option<Vec<Tool>>,
    pub metadata: HashMap<String, String>,
}

pub struct Message {
    pub role: Role,  // user, assistant, system
    pub content: String,
    pub images: Option<Vec<Image>>,
}
```

### Response Structure

```rust
pub struct GenerationResponse {
    pub content: String,
    pub finish_reason: FinishReason,
    pub usage: TokenUsage,
    pub tool_calls: Option<Vec<ToolCall>>,
    pub metadata: HashMap<String, String>,
}

pub struct TokenUsage {
    pub prompt_tokens: usize,
    pub completion_tokens: usize,
    pub total_tokens: usize,
    pub cost: f32,
}
```

---

## Provider Implementations

### Anthropic Claude

```rust
pub struct AnthropicProvider {
    api_key: String,
    base_url: String,
    timeout: Duration,
}

impl AnthropicProvider {
    pub fn new(api_key: String) -> Self {
        Self {
            api_key,
            base_url: "https://api.anthropic.com".to_string(),
            timeout: Duration::from_secs(60),
        }
    }
}

#[async_trait]
impl LlmProvider for AnthropicProvider {
    fn name(&self) -> &str { "anthropic" }
    
    async fn generate(
        &self,
        request: GenerationRequest,
    ) -> Result<GenerationResponse> {
        // Implement Anthropic API call
        // Convert PAUGERAN request to Anthropic format
        // Handle streaming or non-streaming
    }
    
    // ... other trait methods
}
```

**Supported Models:**
- `claude-3-opus-20240229` (Most capable)
- `claude-3-sonnet-20240229` (Balanced)
- `claude-3-haiku-20240307` (Fastest, cheapest)

**Cost Matrix:**
- Input: $15-1.50 per M tokens
- Output: $75-7.50 per M tokens

### OpenAI GPT

```rust
pub struct OpenAiProvider {
    api_key: String,
    base_url: String,
}

#[async_trait]
impl LlmProvider for OpenAiProvider {
    fn name(&self) -> &str { "openai" }
    
    async fn generate(&self, request: GenerationRequest) -> Result<GenerationResponse> {
        // Implement OpenAI API call
    }
}
```

**Supported Models:**
- `gpt-4-turbo-preview` (Most capable)
- `gpt-4` (Fast)
- `gpt-3.5-turbo` (Cheap)

**Cost Matrix:**
- GPT-4: $30-60 per M tokens
- GPT-3.5: $0.50-1.50 per M tokens

### OpenAI-Compatible

```rust
pub struct OpenAiCompatibleProvider {
    base_url: String,
    api_key: String,
    timeout: Duration,
}

impl OpenAiCompatibleProvider {
    pub fn new(base_url: String, api_key: String) -> Self {
        Self { base_url, api_key, timeout: Duration::from_secs(60) }
    }
}
```

**Use Cases:**
- LM Studio local server
- Text generation WebUI
- Custom OpenAI-compatible servers

### Ollama Local

```rust
pub struct OllamaProvider {
    base_url: String,
    timeout: Duration,
}

impl OllamaProvider {
    pub fn new(base_url: String) -> Self {
        Self {
            base_url,
            timeout: Duration::from_secs(120),  // Longer timeout for CPU
        }
    }
}
```

**Supported Models:**
- Llama 2
- Mistral
- Any GGUF-format model

---

## LLM Router

```rust
pub struct LlmRouter {
    providers: HashMap<String, Arc<dyn LlmProvider>>,
    default_provider: String,
    routing_strategy: RoutingStrategy,
    cost_budget: Option<CostBudget>,
}

impl LlmRouter {
    pub async fn generate(
        &self,
        request: GenerationRequest,
    ) -> Result<GenerationResponse> {
        let provider = self.select_provider(&request).await?;
        provider.generate(request).await
    }
    
    pub async fn stream(
        &self,
        request: GenerationRequest,
    ) -> Result<BoxStream<'static, Result<StreamEvent>>> {
        let provider = self.select_provider(&request).await?;
        provider.stream(request).await
    }
    
    async fn select_provider(
        &self,
        request: &GenerationRequest,
    ) -> Result<Arc<dyn LlmProvider>> {
        match self.routing_strategy {
            RoutingStrategy::Task => self.route_by_task(request),
            RoutingStrategy::Cost => self.route_by_cost(request).await,
            RoutingStrategy::Performance => self.route_by_performance(request).await,
            RoutingStrategy::HighAvailability => self.route_for_ha(request).await,
        }
    }
}
```

### Routing Strategies

#### Task-Based Routing
```
Query type: ?
├─ Legal document analysis → Claude 3 Opus (high quality)
├─ Citation generation → Claude 3 Sonnet (fast enough)
├─ Web research processing → Claude 3 Haiku (cheap)
├─ Simple lookup → Local Ollama (free)
└─ Default → Configured provider
```

#### Cost-Based Routing
```
Budget consumed: ?%
├─ < 50% → Use preferred provider
├─ 50-80% → Switch to cheaper provider
├─ 80-95% → Use cheapest provider (Haiku)
└─ > 95% → Local Ollama only (free)
```

#### Performance-Based Routing
```
Latency target: ?ms
├─ < 1s → Use fastest available
├─ < 5s → Use balanced provider
├─ < 30s → Use any available
└─ No limit → Default provider
```

#### High Availability Routing
```
Primary provider: down?
├─ No → Use primary
├─ Yes → Failover to secondary
├─ Secondary also down → Failover to tertiary
└─ All down → Use local Ollama (graceful degradation)
```

---

## Fallback Mechanism

```rust
pub struct FallbackConfig {
    primary: String,
    secondary: Vec<String>,
    fallback_to_local: bool,
}

impl LlmRouter {
    pub async fn generate_with_fallback(
        &self,
        request: GenerationRequest,
        fallback_config: &FallbackConfig,
    ) -> Result<GenerationResponse> {
        // Try primary provider
        if let Ok(response) = self.try_provider(&fallback_config.primary, &request).await {
            return Ok(response);
        }
        
        // Try secondary providers
        for provider_name in &fallback_config.secondary {
            if let Ok(response) = self.try_provider(provider_name, &request).await {
                return Ok(response);
            }
        }
        
        // Fallback to local Ollama
        if fallback_config.fallback_to_local {
            return self.try_provider("ollama", &request).await;
        }
        
        Err("All providers exhausted".into())
    }
}
```

---

## Cost Tracking & Budget Limits

```rust
pub struct CostTracker {
    budget_limit: f32,
    used: f32,
    by_provider: HashMap<String, f32>,
}

impl CostTracker {
    pub fn add_usage(&mut self, provider: &str, usage: &TokenUsage) {
        let cost = self.calculate_cost(provider, usage);
        self.used += cost;
        *self.by_provider.entry(provider.to_string()).or_insert(0.0) += cost;
    }
    
    pub fn remaining_budget(&self) -> f32 {
        self.budget_limit - self.used
    }
    
    pub fn is_within_budget(&self) -> bool {
        self.used < self.budget_limit
    }
}
```

### Budget Configuration
```json
{
  "monthly_budget": 100.0,
  "daily_limit": 10.0,
  "provider_limits": {
    "anthropic": 50.0,
    "openai": 30.0,
    "ollama": 0.0
  }
}
```

---

## Token Usage Monitoring

```rust
pub struct TokenMonitor {
    total_tokens: usize,
    by_provider: HashMap<String, usize>,
    by_model: HashMap<String, usize>,
}

impl TokenMonitor {
    pub fn record_usage(&mut self, provider: &str, model: &str, usage: &TokenUsage) {
        self.total_tokens += usage.total_tokens;
        *self.by_provider.entry(provider.to_string()).or_insert(0) += usage.total_tokens;
        *self.by_model.entry(model.to_string()).or_insert(0) += usage.total_tokens;
    }
    
    pub fn get_daily_average(&self) -> usize {
        // Calculate based on tracking period
    }
}
```

---

## Rate Limiting per Provider

```rust
pub struct RateLimiter {
    provider: String,
    requests_per_minute: u32,
    concurrent_limit: u32,
}

impl RateLimiter {
    pub async fn acquire(&self) -> Result<()> {
        // Wait if needed to respect rate limits
        // Use token bucket algorithm
    }
}
```

**Default Limits:**
| Provider | Requests/min | Concurrent | Tokens/min |
|----------|-------------|-----------|-----------|
| Anthropic | 10 | 5 | 40K |
| OpenAI | 20 | 10 | 100K |
| Ollama | 100 | 20 | Unlimited |

---

## Testing Strategy per Provider

### Unit Tests
- [ ] Request format validation
- [ ] Response parsing
- [ ] Error handling
- [ ] Cost calculation

### Integration Tests
- [ ] API connectivity (with mock)
- [ ] Token counting accuracy
- [ ] Rate limit respect
- [ ] Fallback triggering

### E2E Tests
- [ ] Full workflow with real API
- [ ] Streaming functionality
- [ ] Error recovery
- [ ] Cost tracking

---

## Extension Guide: Adding New Provider

### Step 1: Implement LlmProvider Trait

```rust
pub struct NewProvider {
    api_key: String,
    config: ProviderConfig,
}

#[async_trait]
impl LlmProvider for NewProvider {
    fn name(&self) -> &str { "new-provider" }
    async fn generate(&self, request: GenerationRequest) -> Result<GenerationResponse> { /* ... */ }
    async fn stream(&self, request: GenerationRequest) -> Result<BoxStream<'static, Result<StreamEvent>>> { /* ... */ }
    // ... other methods
}
```

### Step 2: Add to Router

```rust
let new_provider = Arc::new(NewProvider::new(api_key));
router.register_provider("new-provider", new_provider);
```

### Step 3: Update Configuration

```yaml
# config.yaml
providers:
  new-provider:
    enabled: true
    api_key: ${NEW_PROVIDER_API_KEY}
    default_model: "model-name"
    timeout: 60s
```

### Step 4: Add Tests

- Unit tests for provider implementation
- Integration tests with mock API
- E2E tests with real API (optional)

### Step 5: Update Documentation

- Add to SPEC-LLM provider matrix
- Document models and costs
- Add to USER-GUIDE

---

## Monitoring & Observability

### Metrics
- Requests per provider per minute
- Average latency per provider
- Error rate per provider
- Token usage trends
- Cost trends

### Logs
- Provider selection reasoning
- API errors and retries
- Fallback triggers
- Token usage per request

### Alerts
- Provider API down
- Rate limit approaching
- Budget limit approaching
- High error rate

---

## Security Considerations

- Store API keys in environment variables, never in code
- Implement API key rotation
- Log API calls without sensitive data
- Rate limit to prevent abuse
- Monitor for unusual token usage

---

## Checklist Implementasi

- [ ] LlmProvider trait implemented
- [ ] All 4 providers implemented (Anthropic, OpenAI, OpenAI-compatible, Ollama)
- [ ] Router with all strategies working
- [ ] Fallback mechanism tested
- [ ] Cost tracking accurate
- [ ] Rate limiting enforced
- [ ] Comprehensive test coverage
- [ ] Monitoring dashboards setup
- [ ] Documentation complete

---

## Referensi Tambahan

- [Anthropic API Documentation](https://docs.anthropic.com/)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [Ollama Documentation](https://ollama.ai/)
