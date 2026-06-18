---
node_id: "crypto-action"
title: "Crypto Action"
description: "Generate random tokens, hash and HMAC data, sign payloads with RSA, and encrypt/decrypt with AES (CBC/GCM) or RSA — powered by Node.js native crypto"
category: "utilities"
subcategory: "security"
version: "2.0.0"
language: "en"
last_updated: "2026-06-18"
author: "Fusion Team"
tags:
  - crypto
  - security
  - hash
  - encryption
  - signing
  - hmac
  - aes
  - aes-gcm
  - rsa
  - utility
  - action
related_nodes:
  - jwt
  - function
  - whatsapp-webhook
---

<!-- SECTION: header -->
# Crypto Action

> **Category:** Utilities | **Type:** Action Node

Perform cryptographic operations natively within your workflow — generate random tokens, hash or HMAC data, sign payloads with an RSA private key, or encrypt and decrypt values using AES (CBC or authenticated GCM) and RSA.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Crypto Action** node wraps Node.js's built-in `crypto` module into a configurable workflow node. No external dependencies or API keys are required — all operations run locally within the Fusion runtime.

It supports 9 operations: random token generation, key-pair generation, hashing, message authentication (HMAC), RSA signing, symmetric encryption/decryption (AES-CBC and AES-GCM), and asymmetric encryption/decryption (RSA).

### Key Features

- **9 Operations:** Full cryptographic toolkit from random/key generation to RSA decryption
- **Key-Pair Generation:** Produce RSA / EC / Ed25519 keys as PEM (PKCS#8 + SPKI) **and** JWK — directly usable by the [JWT](./jwt.md) node
- **Native Performance:** Uses Node.js `crypto` — no network calls, no external services
- **Authenticated Encryption:** AES-GCM (default) produces an authentication tag that detects tampering; AES-CBC remains available for legacy interop
- **Flat, Guided Config:** Fields are top-level and shown only for the operation that uses them — no nested objects
- **Configurable Encodings:** Key, IV, ciphertext, and result encodings are all selectable
- **Consistent Output Shape:** Every operation returns a named-field object (e.g. `{ "hash": "…" }`, `{ "ciphertext": "…", "authTag": "…" }`)
- **UUID Generation:** Shortcut for `crypto.randomUUID()` built into the Generate operation

### Use Cases

- Generate a secure random API token or session ID for a new user registration workflow
- Hash a password or sensitive field before storing it in a database
- Compute an HMAC signature to verify the integrity of a webhook payload
- Sign an assertion with an RSA private key before calling an external API
- Encrypt sensitive configuration values (authenticated, with AES-GCM) before storing them
- Decrypt an encrypted value received from an external partner system

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

All fields are **top-level** (flat). The UI shows a field only when the selected `operation` (and, for the auth tag, the chosen cipher) requires it.

### Parameters

| Parameter | Type | Applies to | Default | Description |
|-----------|------|-----------|---------|-------------|
| `operation` | `enum` | — | `Generate` | Operation to execute (see below) |
| `generateType` | `enum` | Generate | `UUID` | `UUID`, `HEX`, `BASE64`, or `BASE64URL` |
| `length` | `number` | Generate | `16` | Random bytes to generate (ignored for `UUID`) |
| `keyType` | `enum` | Generate Key Pair | `RSA` | `RSA`, `EC`, or `Ed25519` |
| `modulusLength` | `number` | Generate Key Pair (RSA) | `2048` | RSA modulus length in bits |
| `curve` | `enum` | Generate Key Pair (EC) | `P-256` | `P-256`, `P-384`, `P-521`, `secp256k1` |
| `digestAlgorithm` | `enum` | Hash, Hmac | `SHA256` | `SHA256`, `SHA384`, `SHA512`, `SHA3-256`, `SHA3-384`, `SHA3-512`, `MD5` |
| `secret` | `string` | Hmac | — | Secret key for the HMAC |
| `signAlgorithm` | `enum` | Sign | `RSA-SHA256` | `RSA-SHA256`, `RSA-SHA384`, `RSA-SHA512` |
| `symmetricAlgorithm` | `enum` | Symmetric Encrypt/Decrypt | `aes-256-gcm` | `aes-128/192/256-gcm` or `aes-128/192/256-cbc` |
| `key` | `string` | Symmetric Encrypt/Decrypt | — | AES key, encoded per `keyEncoding` |
| `keyEncoding` | `enum` | Symmetric Encrypt/Decrypt | `base64` | `base64` or `hex` |
| `iv` | `string` | Symmetric Encrypt/Decrypt | — | IV, encoded per `ivEncoding` (12 bytes for GCM, 16 for CBC) |
| `ivEncoding` | `enum` | Symmetric Encrypt/Decrypt | `base64` | `base64` or `hex` |
| `authTag` | `string` | Symmetric Decrypt (GCM only) | — | GCM auth tag captured at encryption time |
| `authTagEncoding` | `enum` | Symmetric Decrypt (GCM only) | `base64` | `base64` or `hex` |
| `publicKey` | `string` | Asymmetric Encrypt | — | PEM RSA public key |
| `privateKey` | `string` | Sign, Asymmetric Decrypt | — | PEM RSA private key |
| `passphrase` | `string` | Sign, Asymmetric Decrypt | — | Passphrase for an encrypted PEM private key (optional) |
| `value` | `string` | all except Generate | — | Plaintext (UTF-8) for hash/sign/encrypt; ciphertext for decrypt |
| `inputEncoding` | `enum` | Symmetric/Asymmetric Decrypt | `base64` | Encoding of the ciphertext in `value`: `base64` or `hex` |
| `outputEncoding` | `enum` | Hash, Hmac, Sign, Encrypt | `hex` | Encoding of the binary result: `hex`, `base64`, `base64url` |
| `textEncoding` | `enum` | Symmetric/Asymmetric Decrypt | `utf-8` | Encoding of decrypted plaintext: `utf-8`, `ascii`, `latin1`, `hex`, `base64` |

---

### Operation: Generate

Produces a cryptographically secure random value.

| Field | Required | Default | Notes |
|-------|----------|---------|-------|
| `generateType` | ✅ | `UUID` | `UUID` ignores `length` |
| `length` | ❌ | `16` | Number of random bytes (for `HEX`/`BASE64`/`BASE64URL`) |

**Output:** `{ "value": "<generated value>" }`

---

### Operation: Generate Key Pair

Generates an asymmetric key pair, returned as both PEM (PKCS#8 private / SPKI public) and JWK. The PEM/JWK output works directly with the [JWT](./jwt.md) node (RS*/PS* ← RSA, ES* ← EC, EdDSA ← Ed25519) and with this node's RSA Sign / Asymmetric operations.

| Field | Required | Default | Notes |
|-------|----------|---------|-------|
| `keyType` | ✅ | `RSA` | `RSA`, `EC`, or `Ed25519` |
| `modulusLength` | ❌ | `2048` | RSA only |
| `curve` | ❌ | `P-256` | EC only — `P-256`, `P-384`, `P-521`, `secp256k1` |

**Output:** `{ "keyType": "RSA", "publicKey": "<PEM SPKI>", "privateKey": "<PEM PKCS#8>", "publicJwk": { … }, "privateJwk": { … } }`

> For HMAC/symmetric **secrets**, use the `Generate` operation (e.g. `HEX`/`BASE64`) — a random string works as a shared secret for both HMAC and the JWT node's HS* algorithms.

---

### Operation: Hash

Computes a one-way digest of `value` (read as UTF-8).

| Field | Required | Default |
|-------|----------|---------|
| `digestAlgorithm` | ✅ | `SHA256` |
| `value` | ✅ | — |
| `outputEncoding` | ❌ | `hex` |

**Output:** `{ "hash": "<digest>" }`

> `MD5` is offered for legacy checksums only — do not use it for security.

---

### Operation: Hmac

Computes a keyed Hash-based Message Authentication Code.

| Field | Required | Default |
|-------|----------|---------|
| `digestAlgorithm` | ✅ | `SHA256` |
| `value` | ✅ | — |
| `secret` | ✅ | — |
| `outputEncoding` | ❌ | `hex` |

**Output:** `{ "hmac": "<mac>" }`

---

### Operation: Sign

Digitally signs `value` (read as UTF-8) using an RSA private key.

| Field | Required | Default |
|-------|----------|---------|
| `value` | ✅ | — |
| `signAlgorithm` | ✅ | `RSA-SHA256` |
| `privateKey` | ✅ | — |
| `passphrase` | ❌ | — |
| `outputEncoding` | ❌ | `hex` |

**Output:** `{ "signature": "<signature>" }`

---

### Operation: Symmetric Encrypt

Encrypts `value` with AES in GCM (authenticated) or CBC mode.

| Field | Required | Default |
|-------|----------|---------|
| `symmetricAlgorithm` | ✅ | `aes-256-gcm` |
| `key` / `keyEncoding` | ✅ / ❌ | — / `base64` |
| `iv` / `ivEncoding` | ✅ / ❌ | — / `base64` |
| `value` | ✅ | — |
| `outputEncoding` | ❌ | `hex` |

**Output (GCM):** `{ "ciphertext": "<ct>", "authTag": "<tag>" }`
**Output (CBC):** `{ "ciphertext": "<ct>" }`

> For GCM, **store the `authTag`** — it is required to decrypt and to detect tampering.

---

### Operation: Symmetric Decrypt

Decrypts an AES ciphertext.

| Field | Required | Default |
|-------|----------|---------|
| `symmetricAlgorithm` | ✅ | `aes-256-gcm` |
| `key` / `keyEncoding` | ✅ / ❌ | — / `base64` |
| `iv` / `ivEncoding` | ✅ / ❌ | — / `base64` |
| `value` | ✅ | — |
| `inputEncoding` | ❌ | `base64` |
| `authTag` / `authTagEncoding` | ✅ (GCM) / ❌ | — / `base64` |
| `textEncoding` | ❌ | `utf-8` |

**Output:** `{ "plaintext": "<decrypted text>" }`

---

### Operation: Asymmetric Encrypt

Encrypts `value` (read as UTF-8) using an RSA public key.

| Field | Required | Default |
|-------|----------|---------|
| `value` | ✅ | — |
| `publicKey` | ✅ | — |
| `outputEncoding` | ❌ | `hex` |

**Output:** `{ "ciphertext": "<ct>" }`

---

### Operation: Asymmetric Decrypt

Decrypts an RSA ciphertext using an RSA private key.

| Field | Required | Default |
|-------|----------|---------|
| `value` | ✅ | — |
| `inputEncoding` | ❌ | `base64` |
| `privateKey` | ✅ | — |
| `passphrase` | ❌ | — |
| `textEncoding` | ❌ | `utf-8` |

**Output:** `{ "plaintext": "<decrypted text>" }`

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->
## Inputs & Outputs

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `input` | `any` | Upstream workflow data — bind into `value`, `secret`, `key`, etc. via expressions |

### Outputs

| Output | Type | Description |
|--------|------|-------------|
| `output` | `object` | The cryptographic result (see per-operation output above) |
| `error` | `Error` | Emitted if the operation fails (e.g. invalid key/IV length, malformed PEM, GCM auth failure) |

### Output Summary by Operation

| Operation | Output |
|-----------|--------|
| `Generate` | `{ "value": "a3f8c2d1…" }` |
| `Generate Key Pair` | `{ "keyType": "RSA", "publicKey": "-----BEGIN PUBLIC KEY-----…", "privateKey": "-----BEGIN PRIVATE KEY-----…", "publicJwk": { … }, "privateJwk": { … } }` |
| `Hash` | `{ "hash": "e3b0c44298fc…" }` |
| `Hmac` | `{ "hmac": "9f86d081…" }` |
| `Sign` | `{ "signature": "base64-signature" }` |
| `Symmetric Encrypt` | `{ "ciphertext": "…", "authTag": "…" }` (GCM) / `{ "ciphertext": "…" }` (CBC) |
| `Symmetric Decrypt` | `{ "plaintext": "hello world" }` |
| `Asymmetric Encrypt` | `{ "ciphertext": "…" }` |
| `Asymmetric Decrypt` | `{ "plaintext": "original plaintext" }` |

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Generate a Secure HEX Token

```json
{
  "operation": "Generate",
  "generateType": "HEX",
  "length": 32
}
```

**Output:** `{ "value": "a3f8c2d1e4b7090f2e1c3d5a6f8e0b1d4c7a2f9e3d1b0c5a8f7e6d2b4c1a3f9" }`

---

### Example: Generate a UUID

```json
{
  "operation": "Generate",
  "generateType": "UUID"
}
```

**Output:** `{ "value": "550e8400-e29b-41d4-a716-446655440000" }`

---

### Example: Generate an RSA Key Pair (for JWT RS256)

```json
{
  "operation": "Generate Key Pair",
  "keyType": "RSA",
  "modulusLength": 2048
}
```

**Output:**
```json
{
  "keyType": "RSA",
  "publicKey": "-----BEGIN PUBLIC KEY-----\n…\n-----END PUBLIC KEY-----\n",
  "privateKey": "-----BEGIN PRIVATE KEY-----\n…\n-----END PRIVATE KEY-----\n",
  "publicJwk": { "kty": "RSA", "n": "…", "e": "AQAB" },
  "privateJwk": { "kty": "RSA", "d": "…" }
}
```

> Feed `privateKey` into a JWT `sign` (RS256) and `publicKey` into a JWT `verify` — bind them with expressions so the PEM newlines are preserved.

---

### Example: SHA-256 Hash a Value

```json
{
  "operation": "Hash",
  "digestAlgorithm": "SHA256",
  "value": "{{input.email}}",
  "outputEncoding": "hex"
}
```

**Output:** `{ "hash": "b94d27b9934d3e08a52e52d7da7dabfac484efe04294e576b4b9bdfaf9cca891" }`

---

### Example: HMAC Webhook Signature

```json
{
  "operation": "Hmac",
  "digestAlgorithm": "SHA256",
  "value": "{{input.rawBody}}",
  "secret": "my-webhook-secret",
  "outputEncoding": "hex"
}
```

**Output:** `{ "hmac": "3d9f2ab1c8e4f7d0…" }`

---

### Example: AES-256-GCM Encrypt (authenticated)

```json
{
  "operation": "Symmetric Encrypt",
  "symmetricAlgorithm": "aes-256-gcm",
  "key": "BASE64_ENCODED_32_BYTE_KEY==",
  "keyEncoding": "base64",
  "iv": "BASE64_ENCODED_12_BYTE_IV",
  "ivEncoding": "base64",
  "value": "super-secret-api-key",
  "outputEncoding": "base64"
}
```

**Output:** `{ "ciphertext": "YT88vKqassWg4A==", "authTag": "tGce8zFGYHqd+weOtcLgag==" }`

---

### Example: AES-256-GCM Decrypt

Pass the `authTag` produced during encryption.

```json
{
  "operation": "Symmetric Decrypt",
  "symmetricAlgorithm": "aes-256-gcm",
  "key": "BASE64_ENCODED_32_BYTE_KEY==",
  "keyEncoding": "base64",
  "iv": "BASE64_ENCODED_12_BYTE_IV",
  "ivEncoding": "base64",
  "value": "{{input.ciphertext}}",
  "inputEncoding": "base64",
  "authTag": "{{input.authTag}}",
  "authTagEncoding": "base64",
  "textEncoding": "utf-8"
}
```

**Output:** `{ "plaintext": "super-secret-api-key" }`

---

### Example: RSA Sign a Payload

```json
{
  "operation": "Sign",
  "value": "{{input.requestBody}}",
  "signAlgorithm": "RSA-SHA256",
  "privateKey": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----",
  "outputEncoding": "base64"
}
```

**Output:** `{ "signature": "base64EncodedRSASignature==" }`

<!-- /SECTION: examples -->

---

<!-- SECTION: workflow-example -->
## Workflow Integration

### Sample Workflow: Webhook Payload Verification → Process → Store

Verify an incoming webhook's HMAC signature, then process and store the event if valid.

```json
{
  "nodes": [
    {
      "id": "webhook",
      "type": "whatsapp-webhook"
    },
    {
      "id": "compute-hmac",
      "type": "crypto-action",
      "config": {
        "operation": "Hmac",
        "digestAlgorithm": "SHA256",
        "value": "{{input.rawBody}}",
        "secret": "webhook-secret-key",
        "outputEncoding": "hex"
      }
    },
    {
      "id": "verify-signature",
      "type": "function",
      "config": {
        "code": "if (input.hmac !== input.headers['x-signature']) throw new Error('Invalid signature'); return input;"
      }
    },
    {
      "id": "store-event",
      "type": "mongodb",
      "config": {
        "operation": "Insert One",
        "collection": "events",
        "document": "{{input.body}}"
      }
    }
  ]
}
```

### Common Patterns

- **Token Generation:** Cron → Crypto `Generate` (HEX 32B) → store as new API key
- **Password Hashing:** Registration webhook → Crypto `Hash` (SHA256) → insert into DB
- **Signed Assertions:** HTTP trigger → Crypto `Sign` (RSA-SHA256) → call downstream API with signature header
- **Encrypt Before Store:** Sensitive data → Crypto `Symmetric Encrypt` (GCM) → persist `ciphertext` + `authTag` + `iv`
- **Decrypt on Read:** DB find → Crypto `Symmetric Decrypt` (GCM) → return plaintext to caller

<!-- /SECTION: workflow-example -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### `Unsupported operation: <name>`

**Cause:** An invalid value was passed to `operation`.

**Solution:** Use one of: `Generate`, `Hash`, `Hmac`, `Sign`, `Symmetric Encrypt`, `Symmetric Decrypt`, `Asymmetric Encrypt`, `Asymmetric Decrypt`.

#### `` `<field>` is required for the <operation> operation ``

**Cause:** A required field (e.g. `value`, `key`, `authTag`) was left blank for the selected operation.

**Solution:** Provide the missing field. Note `authTag` is required when decrypting a **GCM** ciphertext.

#### `Invalid key length` / `Invalid IV length` (Symmetric)

**Cause:** The decoded key/IV size doesn't match the cipher. AES keys are 16/24/32 bytes for 128/192/256; GCM IVs are typically 12 bytes, CBC IVs 16 bytes.

**Solution:** Generate correctly-sized values and make sure `keyEncoding`/`ivEncoding` match how the strings are encoded, e.g. `crypto.randomBytes(32).toString('base64')` for a 256-bit key.

#### `Unsupported state or unable to authenticate data` (GCM Decrypt)

**Cause:** The `authTag`, `key`, `iv`, or ciphertext doesn't match what was used to encrypt — GCM rejects tampered or mismatched input.

**Solution:** Verify all four match the encryption step, and that `authTagEncoding`/`inputEncoding` are correct.

#### `error:1E08010C:DECODER routines::unsupported` / `error:0906D06C:PEM routines` (Sign/Asymmetric)

**Cause:** The PEM key is malformed — missing the `-----BEGIN/END-----` headers, or its newlines were lost when wired through an expression.

**Solution:** Provide the full PEM including headers, preserving line breaks (`\n`). When binding a key from another node, use a proper expression binding rather than pasting a stringified value.

#### Garbled output (Symmetric/Asymmetric Decrypt)

**Cause:** Wrong `inputEncoding` for the ciphertext, or wrong `textEncoding` for the result.

**Solution:** Ensure `inputEncoding` matches the encoding the ciphertext was produced in, and set `textEncoding` to how the plaintext should be rendered.

### Key Size Reference

| Algorithm | Key Size | Key (decoded bytes) | Recommended IV |
|-----------|----------|--------------------|----------------|
| `aes-128-gcm` / `aes-128-cbc` | 128-bit | 16 bytes | 12 (GCM) / 16 (CBC) bytes |
| `aes-192-gcm` / `aes-192-cbc` | 192-bit | 24 bytes | 12 (GCM) / 16 (CBC) bytes |
| `aes-256-gcm` / `aes-256-cbc` | 256-bit | 32 bytes | 12 (GCM) / 16 (CBC) bytes |

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->
## Related

- [JWT](./jwt.md) - Sign, verify, decode, encrypt JSON Web Tokens
- [Function](./function.md) - Custom JavaScript for advanced data transformations
- [Webhook Trigger](./whatsapp-webhook.md) - Receive and verify signed incoming webhooks

<!-- /SECTION: related -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 2.0.0 | 2026-06-18 | Refactor. **Breaking:** config flattened to top-level fields (nested `generate`/`hash`/… objects removed) and `outputPropertyName` dropped — every operation now returns a fixed named-field object. Added AES-GCM (authenticated, now default) with `authTag`; added `passphrase` to Sign and configurable `keyEncoding`/`ivEncoding`/`inputEncoding`. Replaced the broken `ASCII` generate type with `BASE64URL`. Added the **Generate Key Pair** operation (RSA / EC / Ed25519 → PEM + JWK), moved here from the JWT node. |
| 1.0.0 | 2026-03-10 | Initial release |

<!-- /SECTION: changelog -->
