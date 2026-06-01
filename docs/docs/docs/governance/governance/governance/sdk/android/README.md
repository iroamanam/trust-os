Step 13: Create sdk/android/README.md

1. In your trust-os repository, click "Add file" → "Create new file".
2. In the "Name your file..." field, type exactly:
   sdk/android/README.md
3. In the large text area, paste this entire content:

```markdown
# Trust OS Android SDK

**Version:** 1.0
**Status:** Specification Complete | Implementation Ready

---

## Overview

The Trust OS Android SDK enables any Android application to integrate sovereign identity, instant settlement, coercion detection, and HEXAMARK sealing. Distributed as a single AAR file via Gradle. Minimum SDK: Android 8.0 (API 26).

---

## Core Modules

- **TrustAgentClient** — session management, mode switching
- **IdentityModule** — MyID V2 creation, PassID authentication
- **VerificationModule** — 6-step method execution
- **SettlementModule** — BRICK KOIN / CYAN BRICKS transactions
- **CoercionDetectionModule** — voice stress analysis
- **HEXAMARKModule** — seal generation and verification
- **PHYGITALIDAdapter** — NFC card reading
- **KeyStoreBridge** — hardware-backed key storage

---

## Quick Start

```kotlin
val trustAgent = TrustAgentClient.Builder(context)
    .withApiKey("YOUR_API_KEY")
    .withMode(TrustAgentMode.PARENT)
    .build()

trustAgent.identity.verify(loginId, nfcPayload, voiceHash) { result ->
    when (result) {
        is VerificationResult.Verified -> println("Trust Aura: ${result.trustAura}")
        is VerificationResult.Denied -> println("Reason: ${result.reason}")
    }
}
```

---

Security

· API authentication via HMAC-SHA256
· PassID keys stored in Android Keystore (TEE/StrongBox)
· Voice liveness processed on-device
· Offline caching up to 200 events

---

Licensing

Tier Limit
Developer 1,000 calls/month (free)
Business Per-call pricing
Enterprise Unlimited, on-premise option
Sovereign Full source access

---

Full SDK specification and sample app available in the private data room under NDA.

```

4. Scroll down, enter a commit message: `Add Android SDK specification`
5. Click **"Commit new file"**.

---

Your repo now has:
- `LICENSE`
- `README.md`
- `docs/` (8 files)
- `governance/` (2 files)
- `sdk/android/README.md`

Ready for the next one — `sdk/ios/README.md`?
