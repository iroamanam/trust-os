Here is the next file, Jason—step by step.

---

Step 8: Create docs/API_REFERENCE.md

This is the largest file. Take your time pasting it.

1. In your trust-os repository, click "Add file" → "Create new file".
2. In the "Name your file..." field, type exactly:
   docs/API_REFERENCE.md
3. In the large text area, paste this entire content:

```markdown
# Unified Platform — API Reference

**Version:** 1.0  
**Owner:** Jason Wineti (Ngāi Tahu)  
**Status:** Public Technical Surface

---

## 1. Overview

This document consolidates the core API endpoints for the Unified Platform. All endpoints are served over HTTPS. Authentication is via MyID V2 session tokens (V2 ecosystem) or API keys (standalone apps). Responses are JSON. Every mutating action produces a HEXAMARK-sealed truth event recorded on the Immutable Ledger.

**Base URLs:**
- Trust Agent Engine: `https://api.v2.trustagent.io/v3`
- Hexamark Verification: `https://verify.hexamark.io/v1`
- Trust OS Settlement: `https://api.trustos.io/agent/v1`

---

## 2. Authentication

All requests must include:
- **Authorization header:** `Bearer <session_token>` (MyID V2) or `X-API-Key: <key>` (standalone app)
- **Content-Type:** `application/json`

Rate limit: 60 requests per minute per session. Rate-limited responses return `429 Too Many Requests`.

---

## 3. Trust Agent Engine — Menu & Shopping

### 3.1 Generate Weekly Menu
**`POST /v3/menu/generate`**

Generates a 7-day meal plan from family profile, preferences, pantry, and budget.

**Request:**
```json
{
  "family_profile": {
    "adults": 2,
    "children": [{"age": 7, "dietary": []}, {"age": 14, "dietary": ["vegetarian"]}]
  },
  "preferences": {
    "cuisine": ["australian", "italian"],
    "cooking_time_max_minutes": 45,
    "budget_weekly_aud": 250,
    "include_lunches": true,
    "include_snacks": false
  },
  "pantry_items": ["pasta", "rice", "olive oil"],
  "meal_history": ["spaghetti_bolognese", "tacos"]
}
```

Response:

```json
{
  "menu_id": "menu_ghi789",
  "week_start": "2026-06-01",
  "days": [
    {
      "day": "Monday",
      "dinner": {
        "recipe": "chicken_stir_fry",
        "prep_minutes": 15,
        "cook_minutes": 20,
        "estimated_cost_aud": 18.50
      },
      "lunch_kids": {
        "items": ["sandwich", "apple", "yoghurt", "juice_box"]
      },
      "lunch_adults": {
        "recipe": "leftover_stir_fry"
      }
    }
  ],
  "total_estimated_cost_aud": 238.75,
  "budget_remaining_aud": 11.25
}
```

Mode Gate: Parent Mode, Household Mode. Blocked for Kid, Teen, Brand.

---

3.2 Modify Menu

PATCH /v3/menu/{menu_id}

Request:

```json
{
  "day": "Wednesday",
  "action": "swap",
  "new_recipe": "fish_and_chips"
}
```

Returns updated menu object with recalculated costs.

---

3.3 Generate Shopping List

POST /v3/shopping/generate

Builds optimised shopping list from a menu. Compares retailers.

Request:

```json
{
  "menu_id": "menu_ghi789",
  "retailers": ["coles", "woolworths"],
  "optimisation_goal": "lowest_price"
}
```

Response:

```json
{
  "list_id": "list_jkl012",
  "strategy": "split_basket",
  "retailer_allocations": [
    {
      "retailer": "coles",
      "items": [{"item": "chicken_breast_500g", "qty": 3, "price_aud": 8.50, "total": 25.50}],
      "subtotal_aud": 142.30
    },
    {
      "retailer": "woolworths",
      "items": [{"item": "broccoli", "qty": 2, "price_aud": 2.90, "total": 5.80}],
      "subtotal_aud": 96.45
    }
  ],
  "total_aud": 238.75,
  "savings_vs_single_retailer_aud": 17.30
}
```

---

3.4 Export Basket to Retailer

POST /v3/shopping/{list_id}/export

Returns a deep link or basket payload for the retailer's checkout. No payment processing occurs within the Trust Agent.

---

3.5 Price Comparison

POST /v3/shopping/compare

Accepts a raw shopping list (not tied to a menu) and returns the best retailer strategy.

---

4. Trust Agent Engine — Tasks & Creator

4.1 Explain Task

POST /v3/tasks/explain

Returns step-by-step guidance for a task, tailored to age bracket. Mode-gated to Kid and Teen.

Request:

```json
{
  "task_id": "task_mno345",
  "user_age_bracket": "13-15"
}
```

---

4.2 Track Task Progress

GET /v3/tasks/{task_id}/progress

Returns current progress on a multi-step task or questline.

---

4.3 Suggest Creative Direction

POST /v3/creator/suggest

Returns colour palettes, naming ideas, and template references. Mode-gated to Teen and Parent.

Request:

```json
{
  "category": "cosmetics",
  "theme": "neon_retro",
  "target_age": "13-15"
}
```

---

5. Hexamark Verification

5.1 Verify a Seal

GET /v1/verify/{entry_id}?hash={trutoss_resolution_hash}

Public endpoint. No authentication required. Returns the validity status of any hexamark seal.

Response:

```json
{
  "status": "VALID",
  "entry_id": "550e8400-e29b-41d4-a716-446655440000",
  "timestamp": "2026-05-30T14:32:00Z",
  "truth_class": "IDENTITY",
  "hexamark_colour": "#D4A843",
  "outcome": "Sovereign identity verified at Shed #4421",
  "identity_mode": "MyID",
  "sealed_by": "TruTOSS v2.1",
  "merkle_proof": "0xbeef..."
}
```

Status values: VALID, INVALID, TAMPERED.

---

6. Trust OS — Identity & Settlement

6.1 Verify Identity

POST /agent/verify

Verifies a user's identity via MyID or NoID, using PHYGITALID assertion and voice liveness.

Request:

```json
{
  "login_id": "brick.maker.42",
  "phygital_assertion": "FIDO2_SIGNATURE_BASE64",
  "voice_liveness_proof": "VOICE_HASH_NONCE",
  "mode": "MyID",
  "context": "hospital_checkin"
}
```

Response:

```json
{
  "status": "VERIFIED",
  "identity_mode": "MyID",
  "trust_aura": "Radiant",
  "zkp_assertion": "ZKP_PROOF_BASE64",
  "hexamark_id": "CYAN-A1F9B72C...",
  "timestamp": "2026-05-30T14:32:00Z"
}
```

---

6.2 Assess Trust

POST /agent/assess

Computes a real-time trust score for an identity.

Request:

```json
{
  "identity_hash": "ID_HASH_FROM_VERIFY",
  "context": "loan_application"
}
```

Response:

```json
{
  "c_score": 0.87,
  "probability_aura": "Steady",
  "grace_streak_days": 184,
  "active_nullmarks": 0,
  "recommendation": "APPROVE_WITHIN_STANDARD_LIMITS",
  "hexamark_id": "CYAN-C3E1..."
}
```

---

6.3 Authorize Transaction

POST /agent/authorize

Executes the 6-step method and settles a transaction.

Request:

```json
{
  "identity_hash": "ID_HASH",
  "transaction": {
    "amount": 500,
    "currency": "BRICKZ",
    "recipient": "RECIPIENT_MYID_HASH"
  },
  "phygital_assertion": "FIDO2_SIGNATURE"
}
```

Response:

```json
{
  "decision": "APPROVED",
  "settlement_id": "ST-2026-0042",
  "hexamark_id": "CYAN-B72C...",
  "timestamp": "2026-05-30T14:35:00Z"
}
```

---

6.4 Resolve Dispute

POST /agent/resolve

Invokes TruFLIP, Ref Dyson Dice, or TruCOUNCIL based on dispute type.

Request:

```json
{
  "dispute_type": "binary",
  "stake_amount": 200,
  "party_a_identity": "ID_HASH_A",
  "party_b_identity": "ID_HASH_B"
}
```

Response:

```json
{
  "outcome": "PARTY_A_WINS",
  "method": "TruFLIP",
  "settlement_token": "ST-2026-0043",
  "hexamark_id": "CYAN-D4F2..."
}
```

---

6.5 Monitor Coercion

POST /agent/monitor

Analyses live voice stream for coercion patterns.

Request:

```json
{
  "identity_hash": "ID_HASH",
  "voice_stream_chunk": "BASE64_AUDIO",
  "behavioural_metrics": {
    "typing_cadence": 0.4,
    "transaction_frequency": "HIGH"
  }
}
```

Response:

```json
{
  "coercion_risk": 0.92,
  "action": "FROZEN",
  "alert_sent_to": ["SPONSOR", "TruGUARD"],
  "hexamark_id": "CYAN-E5G3..."
}
```

---

6.6 Issue Credential

POST /agent/issue

Issues a temporary, scoped identity token.

Request:

```json
{
  "identity_hash": "ID_HASH",
  "credential_type": "NoID",
  "scope": "EVENT_ENTRY_AGE_VERIFICATION",
  "validity_minutes": 60
}
```

Response:

```json
{
  "token": "TEMP_NOID_TOKEN",
  "expires_at": "2026-05-30T15:35:00Z",
  "hexamark_id": "CYAN-F6H4..."
}
```

---

6.7 Query Agent Status

GET /agent/status?agent_id=AGENT_ID

Returns the agent's current operational state.

Response:

```json
{
  "agent_id": "Dycen-Agent-001",
  "status": "ACTIVE",
  "tier": "T2",
  "last_attestation": "2026-04-01T00:00:00Z",
  "sponsor": "Jason Wineti",
  "hexamark_id": "CYAN-G7I5..."
}
```

---

7. Error Response Standard

All errors follow a consistent schema:

```json
{
  "error": {
    "code": "MODE_BLOCKED",
    "message": "This capability is not available in Kid mode.",
    "session_mode": "kid",
    "required_mode": "parent"
  }
}
```

Standard error codes: MODE_BLOCKED, AGE_BLOCKED, SPENDING_BLOCKED, IDENTITY_BLOCKED, CONTENT_FILTERED, RATE_LIMITED, INVALID_SIGNATURE, COERCION_DETECTED.

---

8. What's Not Documented

The underlying algorithms for multi-factor verification, confidence scoring, settlement token generation, and coercion detection are sealed trade secrets (VTT-001 to VTT-013). The endpoints documented here describe the public interface — what the platform does, not how it does it. Full technical due diligence available under NDA.

---

This specification is part of the Trust OS public technical surface. Core algorithms are sealed. Contact the data room for access.

```

4. Scroll down, enter a commit message: `Add Unified Platform API Reference`
5. Click **"Commit new file"**.

---

Your repo now has:
- `LICENSE`
- `README.md`
- `docs/TRUST_OS_SPEC.md`
- `docs/FAMILY_OS_SPEC.md`
- `docs/BRICK_KOIN_YOUTH_ECONOMY.md`
- `docs/BRAND_MARKETPLACE.md`
- `docs/TRUST_AGENT_ENGINE.md`
- `docs/API_REFERENCE.md`

Ready for the next one — `docs/AI_ID_GOVERNANCE.md`? 🤎
