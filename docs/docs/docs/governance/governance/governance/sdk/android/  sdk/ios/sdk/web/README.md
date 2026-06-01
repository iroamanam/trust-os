Here is the next file, Jason—step by step. This one creates the sdk/web folder.

---

Step 15: Create sdk/web/README.md

1. In your trust-os repository, click "Add file" → "Create new file".
2. In the "Name your file..." field, type exactly:
   sdk/web/README.md
3. In the large text area, paste this entire content:

```markdown
# Trust OS Web SDK

**Version:** 1.0
**Status:** Specification Complete | Implementation Ready

---

## Overview

The Trust OS Web SDK turns any browser into a Trust Agent endpoint. It provides WebAuthn/passkey authentication, Web NFC for PHYGITALID scanning, and a WebAssembly-based Trust Agent for mode-aware interactions. No installation required.

---

## Core Features

- **WebAuthn Integration** — passwordless login via FIDO2
- **Web NFC** — PHYGITALID card scanning (Chrome/Android)
- **Web Bluetooth** — BLE device communication
- **WebAssembly Trust Agent** — lightweight, sandboxed AI
- **HEXAMARK Web Verifier** — standalone seal verification page
- **Service Worker Offline Support** — cached trust verification

---

## Quick Start

```html
<script type="module">
  import { TrustAgent } from 'https://cdn.trustos.io/sdk/web/trust-agent.js';
  
  const agent = new TrustAgent({ apiKey: 'YOUR_KEY', mode: 'parent' });
  const result = await agent.verify({ loginId: 'brick.maker.42' });
  console.log(result.status); // "VERIFIED"
</script>
```

---

Licensing

Tier Limit
Developer 1,000 calls/month (free)
Business Per-call pricing
Enterprise Unlimited, on-premise option

---

Full SDK specification available in the private data room under NDA.

```

4. Scroll down, enter a commit message: `Add Web SDK specification`
5. Click **"Commit new file"**.

---

Your repo now has:
- `LICENSE`
- `README.md`
- `docs/` (8 files)
- `governance/` (2 files)
- `sdk/android/README.md`
- `sdk/ios/README.md`
- `sdk/web/README.md`

Ready for the next one — `VTT/DESIGN_PRINCIPLES.md`? This creates the `VTT` folder with the 35 safe-to-publish design principles. 🤎
