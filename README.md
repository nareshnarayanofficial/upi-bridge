# UPI SMS Bridge

> **Zero-infrastructure, self-hosted payment confirmation bridge for UPI payments.**

[![Android Build](https://github.com/your-org/upi-sms-bridge/actions/workflows/android-build.yml/badge.svg)](https://github.com/your-org/upi-sms-bridge/actions/workflows/android-build.yml)
[![Tests](https://github.com/your-org/upi-sms-bridge/actions/workflows/tests.yml/badge.svg)](https://github.com/your-org/upi-sms-bridge/actions/workflows/tests.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## What this is

UPI SMS Bridge connects a merchant's website to their own Android phone. The phone listens for bank credit SMS messages, matches them against pending orders, and fires a signed webhook to the merchant's backend — **no cloud server, database, or third-party service operated by us**.

```
Customer pays via UPI QR
        │
        ▼
Bank sends SMS to merchant's phone
        │
        ▼
Android app parses + matches order
        │  (HMAC-signed HTTPS webhook)
        ▼
Merchant's website backend marks order paid
```

> **Important:** SMS-matched payments are **not** bank-verified settlements. They are evidence of a reported credit. Your website should distinguish `status: sms_matched` from a provider-verified payment.

---

## Features

- **Android-only processing** — all data stays on your phone
- **HMAC-SHA256 signed webhooks** — unforgeable events
- **Multi-bank SMS parser** — SBI, HDFC, ICICI, Axis, Kotak, and more
- **Retry with exponential backoff** — survives network outages
- **Three SMS input adapters** — direct SMS, notification listener, manual import
- **Pending order sync** — optionally poll your own endpoint for orders
- **Built-in test webhook** — validate integration without real payments

---

## Quick Start

### 1. Install the APK

Download the latest signed APK from [Releases](https://github.com/your-org/upi-sms-bridge/releases) and install it.

> **SMS permissions:** Android restricts READ_SMS/RECEIVE_SMS. You may need to grant via ADB:
> ```bash
> adb shell pm grant com.upismsbridge.app android.permission.READ_SMS
> adb shell pm grant com.upismsbridge.app android.permission.RECEIVE_SMS
> ```
> See [docs/installation.md](docs/installation.md) for full setup.

### 2. Configure the app

Open the app → Settings:
- **Webhook URL**: `https://your-site.com/webhook/upi`
- **Merchant ID**: a short identifier for this integration
- **Webhook Secret**: generate with `openssl rand -hex 32`

### 3. Add the receiver to your website

| Integration | Folder |
|---|---|
| Node.js / Express | [integrations/nodejs/](integrations/nodejs/) |
| PHP | [integrations/php/](integrations/php/) |
| WooCommerce | [integrations/wordpress-woocommerce/](integrations/wordpress-woocommerce/) |

### 4. Test it

Use the **Test Webhook** button in the app.

---

## Repository Layout

```
upi-sms-bridge/
├── android/          # Kotlin Android app
├── integrations/     # Website receiver libraries
├── protocol/         # Webhook event JSON schema + OpenAPI spec
├── docs/             # Architecture, security, installation guides
├── examples/         # Demo merchant site + webhook receivers
└── .github/          # CI workflows
```

---

## Security

See [SECURITY.md](SECURITY.md) and [docs/threat-model.md](docs/threat-model.md).

**Never share your webhook secret.** Each merchant integration has its own secret.

---

## License

MIT — see [LICENSE](LICENSE).
