---
title: BUSINESS-PRICING — Pricing Strategy
document_id: BUSINESS-PRICING
version: 1.0
cb_reference: [CB §5], [CB §6]
status: DRAFT
owner: Business & Finance
last_updated: 2026-08-29
---

# BUSINESS-PRICING — Pricing Strategy

Strategi penetapan harga PAUGERAN.

---

## Pricing Tiers

### Trial
```
Price:       FREE
Duration:    30 days
Users:       1
Features:    All features enabled
Renewal:     Cannot renew (one per account)
Restrictions:Non-commercial use

Target:      Decision-maker evaluation
Conversion:  30% to Personal license
```

### Personal
```
Price:        $99/year or $9.99/month
Duration:     1 year (auto-renew)
Users:        1
Features:     ✅ Chat (unlimited queries)
              ✅ KB Search (unlimited)
              ✅ Web Research
              ✅ Export (PDF/DOCX)
              ✅ Offline mode
Target:       Individual lawyers, students
Annual value: $99
Monthly value:$119.88

Churn risk:   Low (sticky product)
```

### Team
```
Price:        $29/user/month (minimum 5 users)
Duration:     1 year (auto-renew)
Users:        5, 10, 20, 50, custom
Minimum cost: $1,740/year (5 users)
Features:     ✅ All Personal features
              ✅ Admin dashboard
              ✅ Usage analytics
              ✅ Centralized KB
              ✅ Priority support
              ✅ Custom LLM providers
Target:       Law firms, corporate legal departments
Price/user:   $348/year

Discount:     10% for annual prepay
```

### Enterprise
```
Price:        Custom (typically $500-2000/user/year)
Duration:     1-3 years (negotiated)
Users:        Unlimited within organization
Features:     ✅ All Team features
              ✅ On-premise deployment
              ✅ API integration
              ✅ Custom integration
              ✅ Dedicated support
              ✅ SLA guarantee
              ✅ Advanced security features
Target:       Large enterprises, government
Typical deal: $50k-500k/year

Includes:     Implementation, training, 24/7 support
```

---

## Discount Strategy

### Volume Discounts

| Users | Personal | Team | Enterprise |
|-------|----------|------|------------|
| 1-4 | - | - | Case by case |
| 5-10 | - | List | -10% |
| 11-25 | - | -10% | -15% |
| 26-50 | - | -15% | -20% |
| 50+ | - | -20% | -25% |

### Time-Based Discounts

```
Annual prepay:     10% discount
2-year commitment: 15% discount
3-year commitment: 20% discount

Example:
Personal: $99/year → $89.10/year (10% off)
Team: $29/user/month × 60 months → $1,740 → $1,479 (15% off)
```

### Promotional Discounts

```
Holiday campaigns (Dec, Aug):     15% off
Black Friday/Cyber Monday:        20% off
Student/Academic:                 50% off
Open source / nonprofits:         Free

Conditions:
├─ Valid proof required
├─ Not stackable with other discounts
└─ Limited time (30-60 days)
```

### Loyalty Discounts

```
Renewal discount (Personal):      5% off
Renewal discount (Team):          10% off
Referral program:                 $20 credit per referral
```

---

## Payment Methods & Terms

### Accepted Payment

- Credit card (Visa, Mastercard, Amex)
- Bank transfer / Wire
- ACH (US organizations)
- Invoice (Enterprise, NET30)

### Billing

**Subscription (Monthly/Annually)**
```
Automatic on signup date
Failed payment: Retry × 3 over 5 days
Successful: Invoice emailed
Chargeback: Account suspended, email sent
```

**Invoice (Enterprise)**
```
NET30 terms
Custom PO required
Payment method: Bank transfer
Recurring: Annual auto-renew (if agreed)
```

---

## Refund & Money-Back Guarantee

### Money-Back Guarantee

```
30-day satisfaction guarantee:

├─ Within 30 days of purchase
├─ Unused or minimal use
├─ Full refund (no questions)
└─ Simple refund process

After 30 days:
├─ No refund (subscription model)
├─ But you can cancel anytime
└─ Service active until end of billing period
```

### Partial Refunds

```
31-60 days:     Prorated refund (50%)
61-90 days:     Prorated refund (25%)
90+ days:       No refund

Exception:
├─ Enterprise: Custom terms
├─ Price change disputes: Manager approval
└─ Service outages: Credits issued
```

---

## Revenue Projections

### Year 1 (Conservative)

```
Trial → Personal conversion:  20%
├─ Users: 1,000 trial signups
├─ Converted: 200 Personal
├─ Revenue: $19,800

Personal renewals:            80%
├─ Year 1 gross: $19,800
├─ Churn: 20%
└─ Year 1 net: $15,840

Team licenses:                50 (avg 10 users each)
├─ Revenue: $50 × 10 × $348 = $174,000
└─ Year 1 net: $174,000

Total Year 1:                 ~$190,000
```

### Year 2 (Growth)

```
Cumulative Personal:          600 active
├─ Churn-adjusted: 480
├─ Revenue: $47,520

Team licenses:                150 (avg 12 users)
├─ Revenue: $150 × 12 × $348 = $626,400
└─ Churn-adjusted: 80% = $501,120

Enterprise:                   5 deals (avg $200k)
├─ Revenue: $1,000,000

Total Year 2:                 ~$1,550,000
```

---

## Market Positioning

### Value Proposition

```
vs. Manual Research:
├─ 10x faster research
├─ Accurate citations
├─ Always available
└─ Cost-effective ($99/year vs $500+ lawyer hours)

vs. Generic ChatGPT:
├─ Indonesian law focused
├─ Hallucination protection
├─ Citation verification
└─ Export-ready documents

vs. Expensive Legal Databases:
├─ $99 vs $500-1000+/year
├─ Easier to use
├─ AI-powered analysis
└─ No contracts required
```

### Target Market

**Primary:**
- Individual lawyers (2,000+ in Indonesia)
- Solo practitioners
- Small firms (5-20 lawyers)

**Secondary:**
- Law students
- Corporate legal departments
- Government agencies

**Tertiary:**
- Large enterprises
- International law firms

---

## Monetization Options (Future)

### API Access

```
Price: $0.05-0.10 per query (Usage-based)
Target: Third-party integrations
Example: Legal tech startups embedding PAUGERAN

Projected: 10-20% of revenue by Year 3
```

### Premium KB

```
Price: $500-1000/year (Curated legal databases)
Features: Pre-built KB for specific practice areas
Target: Corporate lawyers

Example: "Tax Law Database" (UU PPh, PPnBM, etc.)
```

### Consulting Services

```
Price: $200-500/hour (Optional add-on)
Service: Setup, training, custom integration
Target: Enterprise customers
Projected: 15% of Enterprise revenue
```

---

## Financial Goals

| Metric | Year 1 | Year 2 | Year 3 |
|--------|--------|--------|--------|
| Revenue | $200k | $1.5M | $5M |
| Users | 1k | 5k | 15k |
| CAC | $50 | $40 | $30 |
| LTV | $250 | $400 | $600 |
| Profitability | -50% | +10% | +40% |

---

## Pricing Review Schedule

```
Quarterly:  Review conversion, churn, NPS
Annually:   Full pricing strategy review
Trigger:    Market changes, cost changes, feedback
```

---

## Checklist Implementasi

- [ ] Pricing tiers finalized
- [ ] Payment processor integrated (Stripe)
- [ ] License keys generated
- [ ] Discount codes automated
- [ ] Invoice system ready
- [ ] Refund procedures documented
- [ ] Finance dashboard set up
- [ ] Revenue tracking live
- [ ] Marketing messaging aligned

