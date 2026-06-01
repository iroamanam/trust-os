# HEXAMARK Standard — Cryptographic Truth Seal

**Version:** 1.0
**Owner:** Jason Wineti (Ngāi Tahu)
**Status:** Patent-Ready | Part of the Unified Platform

---

## 1. Overview

HEXAMARK is the cryptographic truth seal of the Trust OS. It is a visible, verifiable, tamper-proof mark applied to every authenticated action, every settled transaction, and every governed AI decision. It is the padlock of the next internet.

HEXAMARK is not a logo. It is a cryptographic commitment — a hash-based proof that links a truth event to its immutable ledger entry, its identity anchor, and its governance context.

---

## 2. The Six Colours of Truth

HEXAMARK uses six colours to encode the class of truth that has been sealed. The colour is semantic, not decorative.

| Colour | Hex Code | Truth Class | Source | Example Events |
|--------|----------|-------------|--------|----------------|
| **Blue** | #3B82F6 | Social Truth | VTT-069 to VTT-078 | Bond strengthened, conflict resolved, respect validated |
| **Green** | #22C55E | Presence Truth | VTT-079 to VTT-086 | Check-in confirmed, boundary respected, proximity bond formed |
| **Rose Gold** | #E11D48 | Emotional Truth | VTT-087 to VTT-094 | Calm achieved, stress detected, resilience loop completed |
| **Amber** | #F59E0B | World Truth | VTT-095 to VTT-100 | Arena fairness validated, weather modifier applied, rivalry intensity sealed |
| **Gold** | #D4A843 | Identity Truth | MyID, NoID, TRUE-ID | Sovereign identity verified, ownership confirmed, guardian authority validated |
| **White** | #FFFFFF | Settlement Truth | TruTOSS, TruFLIP | Flip resolved, stake settled, arbitration complete |

---

## 3. Cryptographic Backing

Each HEXAMARK seal contains:
- **Event type and outcome**
- **Timestamp (ISO 8601 UTC)**
- **Identity proof (ZKP assertion — no PII)**
- **VTT engine source references**
- **TruTOSS resolution hash**
- **Pointer to the immutable ledger entry**
- **Seal verification URL**

The seal is generated as a SHA-256 hash over these fields, signed by the Trust OS sealing key. Any relying party can verify the seal by querying the verification endpoint with the `entry_id` and `trutoss_resolution_hash`.

---

## 4. Seal Visibility Rules

- **MyID Mode:** Full HEXAMARK visible with Gold seal. Identity attributes accessible per user consent.
- **NoID Mode:** White HEXAMARK only. ZKP proves a verified human was present without revealing who.
- **AI-ID Mode:** Cyan HEXAMARK. Distinguishes synthetic agent actions from human actions.
- **Kid Mode:** Simplified seal display. Age-appropriate visual feedback without cryptographic detail.

---

## 5. Verification Endpoint
