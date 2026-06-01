Step 14: Create sdk/ios/README.md

1. In your trust-os repository, click "Add file" → "Create new file".
2. In the "Name your file..." field, type exactly:
   sdk/ios/README.md
3. In the large text area, paste this entire content:

```markdown
# Trust OS iOS SDK

**Version:** 1.0
**Status:** Specification Complete | Implementation Ready

---

## Overview

The Trust OS iOS SDK enables any iOS application to integrate sovereign identity, instant settlement, coercion detection, and HEXAMARK sealing. Distributed as a Swift Package. Minimum: iOS 17.0.

---

## Core Modules

- **TrustAgentClient** — session management, mode switching
- **IdentityModule** — MyID V2 creation, PassID via Secure Enclave
- **VerificationModule** — 6-step method execution
- **SettlementModule** — BRICK KOIN / CYAN BRICKS transactions
- **CoercionDetectionModule** — on-device voice stress analysis
- **HEXAMARKModule** — seal generation and verification
- **PHYGITALIDAdapter** — CoreNFC integration
- **SecureEnclaveBridge** — hardware-backed key storage, Face ID / Touch ID

---

## Quick Start

```swift
let trustAgent = TrustAgentClient.Builder()
    .withApiKey("YOUR_API_KEY")
    .withMode(.parent)
    .build()

trustAgent.identity.verify(loginId: id, phygitalAssertion: nfcPayload, voiceLivenessProof: hash) { result in
    switch result {
    case .verified(let aura, let hexamarkId):
        print("Trust Aura: \(aura)")
    case .denied(let reason):
        print("Denied: \(reason)")
    }
}
```

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

4. Scroll down, enter a commit message: `Add iOS SDK specification`
5. Click **"Commit new file"**.

---

Your repo now has:
- `LICENSE`
- `README.md`
- `docs/` (8 files)
- `governance/` (2 files)
- `sdk/android/README.md`
- `sdk/ios/README.md`
