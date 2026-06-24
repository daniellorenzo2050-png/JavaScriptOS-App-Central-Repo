# JavaScriptOS App Central Repo (JACR)

Welcome to the official repository for **JACR**, the central hub for storing, verifying, and distributing applications for the **JavaScriptOS** ecosystem. JACR works hand-in-hand with the **JPM (JavaScript Package Manager)** to deliver signed, ultra-secure applications (`.sjapp`) directly to users.

---

## 🚀 Overview

JACR is not just a file server; it is a high-performance verification pipeline. When a developer publishes an application via JPM, JACR ingests the source, runs automated security audits, signs the package using robust cryptographic standards, and prepares it for distribution over modern, low-latency network protocols.

---

## 🔒 Security & Packaging Architecture

Every application distributed by JACR is converted into a **Signed JAPP (`.sjapp`)** container enforcing the following specifications:

### 1. Structure & Integrity
* **Magic Number:** Packages start with `JAPP#` hex headers for instant OS-level identification.
* **Hashing:** Code integrity verified using **SHA-512**.
* **Asymmetric Signing:** Packages are digitally signed using an institutional **RSA-4096** private key.

### 2. Encryption at Rest
* Content is fully encrypted using **AES-256-GCM** (for authenticated application logic) and **AES-256-CBC** (for structured media assets).

### 3. Transport Layer
* Outbound distribution utilizes **QUIC** combined with **TLS 1.3** over **HTTPS**, enabling 0-RTT handshakes and immune-to-mitm streaming.

---

## 🛠️ API Architecture

The JACR service exposes a secure REST and Streaming API for the JPM CLI client.

### Endpoints

* `GET /v1/packages` — Lists available packages (paginated).
* `GET /v1/packages/:name` — Returns metadata, sandbox manifest (`jpm.json`), and download URL.
* `POST /v1/packages/publish` — **Protected.** Endpoint for developers to push new `.ujapp` bundles for verification and signing.

---

## 📦 Publishing Pipeline Workflow

1. **Submission:** Developer runs `jpm publish` uploading an unsigned bundle (`.ujapp`).
2. **Static Analysis:** JACR inspects the `jpm.json` permission manifest and scans code for unauthorized Sandbox/Ring 3 bypass attempts.
3. **Sealing:** Upon passing validation, JACR hashes the bundle via SHA-512, signs it with RSA-4096, and encrypts the payload with AES-256.
4. **Deployment:** The newly generated `.sjapp` package is replicated across the global CDN network.

---

## 🤝 Contributing

We welcome contributions to the JACR pipeline core, signature infrastructure, and scanning engines. Please ensure all pull requests include unit tests for the cryptographic modules.

```javascript
// Example of core signing test verification
const isSignedCorrectly = JACR.Crypto.verify(payload, signature, publicKey);
