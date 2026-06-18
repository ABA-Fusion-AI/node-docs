---
node_id: "jwt"
title: "JWT"
description: "Sign, verify, decode, encrypt, and decrypt JSON Web Tokens (JWS & JWE) — across HS, RS, PS, ES, and EdDSA algorithms, powered by jose"
category: "utilities"
subcategory: "security"
version: "1.0.0"
language: "en"
last_updated: "2026-06-18"
author: "Fusion Team"
tags:
  - jwt
  - jws
  - jwe
  - security
  - signing
  - verification
  - encryption
  - jose
  - utility
  - action
related_nodes:
  - crypto-action
  - function
  - whatsapp-webhook
---

<!-- SECTION: header -->
# JWT

> **Category:** Utilities | **Type:** Action Node

Create and consume JSON Web Tokens within your workflow — sign and verify signed tokens (JWS), encrypt and decrypt encrypted tokens (JWE), and decode tokens without verifying. Built on the [`jose`](https://github.com/panva/jose) library.

> Need signing keys? Use the [Crypto](./crypto.md) node's **Generate Key Pair** operation — its PEM/JWK output plugs straight into this node.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **JWT** node wraps `jose` into a configurable workflow node. It gives you full control over the header, payload/claims, and keys across the HMAC (HS), RSA (RS/PS), ECDSA (ES), and EdDSA algorithm families, plus encrypted JWEs.

### Security Model

- **`none` is never offered or accepted** — unsecured tokens are the classic JWT forgery vector.
- **Verification always uses an algorithm allow-list**, defaulting to the selected algorithm. Restricting accepted algorithms prevents algorithm-confusion attacks.
- **Failures throw** (bad signature, expired, claim mismatch, decryption failure) and route to the node's `error` output.

### Key Features

- **5 Operations:** `sign`, `verify`, `decode`, `encrypt`, `decrypt`
- **Many Algorithms:** HS256/384/512, RS256/384/512, PS256/384/512, ES256/ES256K/ES384/ES512, EdDSA
- **JWE Support:** Encrypted tokens with `dir`, AES key-wrap, RSA-OAEP, ECDH-ES, and PBES2 key management
- **Flexible Keys:** PEM (PKCS#8 / SPKI) or JWK, with optional passphrase for encrypted private keys
- **Claim Validation:** issuer, audience, subject, max age, clock tolerance, required claims, `typ`

### Use Cases

- Issue a signed session/access token after authentication
- Verify a bearer token (and its claims) on an incoming request
- Decode a token to inspect its header/claims without verifying
- Encrypt sensitive claims into a JWE for a downstream partner

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

Fields are **top-level** and shown only for the operation (and algorithm family) that uses them.

### Parameters

| Parameter | Type | Applies to | Default | Description |
|-----------|------|-----------|---------|-------------|
| `operation` | `enum` | — | `sign` | `sign`, `verify`, `decode`, `encrypt`, `decrypt` |
| `algorithm` | `enum` | sign, verify | `HS256` | Signature algorithm (HS/RS/PS/ES/EdDSA) |
| `keyManagementAlg` | `enum` | encrypt, decrypt | `dir` | JWE `alg` (key management) |
| `contentEncryptionAlg` | `enum` | encrypt, decrypt | `A256GCM` | JWE `enc` (content encryption) |
| `keyFormat` | `enum` | asymmetric sign/verify/encrypt/decrypt | `auto` | `auto`, `pem`, or `jwk` |
| `secret` | `string` | HMAC sign/verify, symmetric JWE | — | Shared secret |
| `privateKey` | `string` | asymmetric sign, asymmetric JWE decrypt | — | PEM (PKCS#8) or JWK private key |
| `publicKey` | `string` | asymmetric verify, asymmetric JWE encrypt | — | PEM (SPKI) or JWK public key |
| `passphrase` | `string` | asymmetric sign/decrypt | — | Passphrase for an encrypted PEM private key |
| `payload` | `json` | sign, encrypt | — | The JWT Claims Set (registered + custom claims) |
| `header` | `json` | sign, encrypt | — | Extra header params (`kid`, `cty`, etc.); `alg`/`enc` are forced |
| `token` | `string` | verify, decode, decrypt | — | The token to consume (may also come from node input) |
| `algorithms` | `string[]` | verify | `[algorithm]` | Accepted-algorithm allow-list |
| `expectedIssuer` | `string` | verify, decrypt | — | Required `iss` (comma-separated to allow several) |
| `expectedAudience` | `string` | verify, decrypt | — | Required `aud` (comma-separated to allow several) |
| `expectedSubject` | `string` | verify, decrypt | — | Required `sub` |
| `maxTokenAge` | `string` | verify, decrypt | — | Reject tokens older than this (e.g. `5m`, `24h`) |
| `clockTolerance` | `string` | verify, decrypt | — | Skew tolerance for exp/nbf/iat (e.g. `5s`) |
| `requiredClaims` | `string[]` | verify, decrypt | — | Claim names that must be present |
| `typ` | `string` | verify, decrypt | — | Required protected-header `typ` |
| `currentDate` | `string` | verify, decrypt | — | Override reference time for exp/nbf (ISO string) |

### Notes on Claims & Headers

- Put **all** claims in `payload` — registered (`iss`, `sub`, `aud`, `exp`, `nbf`, `iat`, `jti`) and custom. Date claims are NumericDate (seconds since epoch, e.g. `exp: 1735689600`).
- The standard header is generated for you (`typ: "JWT"` plus `alg`/`enc`). Use `header` only for advanced params like `kid`. `alg`/`enc` are always forced from the algorithm fields; `typ` can be overridden.
- For `sign`/`encrypt`, the payload may also be supplied via the node input (`payload`, `claims`, or the raw input object). For `verify`/`decode`/`decrypt`, the token may be supplied via the node input (a string, or `{ token }`).

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->
## Inputs & Outputs

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `input` | `any` | For sign/encrypt: the claims (object, or `{ payload }` / `{ claims }`). For verify/decode/decrypt: the token (string or `{ token }`). |

### Outputs

| Output | Type | Description |
|--------|------|-------------|
| `output` | `object` | Operation result (see below) |
| `error` | `Error` | Invalid signature, expired/claim mismatch, decryption failure, missing key, etc. |

### Output Shape by Operation

| Operation | Output |
|-----------|--------|
| `sign` | `{ "token": "<jws>", "header": { … }, "payload": { … } }` |
| `verify` | `{ "valid": true, "payload": { … }, "protectedHeader": { … } }` |
| `decode` | `{ "header": { … }, "payload": { … }, "signature": "<b64>" }` |
| `encrypt` | `{ "token": "<jwe>", "header": { … }, "payload": { … } }` |
| `decrypt` | `{ "payload": { … }, "protectedHeader": { … } }` |

> `decode` does **not** verify the signature — never trust its output for authorization.

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Sign an HS256 Token

```json
{
  "operation": "sign",
  "algorithm": "HS256",
  "secret": "my-shared-secret",
  "payload": {
    "sub": "user-123",
    "iss": "my-service",
    "exp": 2097129600
  }
}
```

**Output:** `{ "token": "eyJhbGciOiJIUzI1Ni…", "header": { "alg": "HS256", "typ": "JWT" }, "payload": { … } }`

---

### Example: Verify a Token with Claim Checks

```json
{
  "operation": "verify",
  "algorithm": "HS256",
  "secret": "my-shared-secret",
  "token": "{{input.token}}",
  "expectedIssuer": "my-service",
  "expectedAudience": "my-api",
  "maxTokenAge": "1h"
}
```

**Output:** `{ "valid": true, "payload": { … }, "protectedHeader": { "alg": "HS256", "typ": "JWT" } }`

---

### Example: Get an RS256 Key Pair (Crypto node)

Key generation lives in the [Crypto](./crypto.md) node. Use its **Generate Key Pair** operation:

```json
{
  "operation": "Generate Key Pair",
  "keyType": "RSA",
  "modulusLength": 2048
}
```

It outputs `{ "publicKey": "<PEM>", "privateKey": "<PEM>", "publicJwk": { … }, "privateJwk": { … } }` — feed those into the JWT node below.

---

### Example: Sign with an RS256 Private Key (PEM)

Wire the `privateKey` from the Crypto Generate Key Pair node **via an expression** so its PEM newlines are preserved.

```json
{
  "operation": "sign",
  "algorithm": "RS256",
  "keyFormat": "pem",
  "privateKey": "{{output.GenerateKeyPair.privateKey}}",
  "payload": { "sub": "user-123" }
}
```

**Output:** `{ "token": "eyJhbGciOiJSUzI1Ni…", "header": { … }, "payload": { … } }`

---

### Example: Verify with the Matching RS256 Public Key

```json
{
  "operation": "verify",
  "algorithm": "RS256",
  "keyFormat": "pem",
  "publicKey": "{{output.GenerateKeyPair.publicKey}}",
  "token": "{{output.Sign.token}}"
}
```

**Output:** `{ "valid": true, "payload": { … }, "protectedHeader": { "alg": "RS256", "typ": "JWT" } }`

---

### Example: Decode (no verification)

```json
{
  "operation": "decode",
  "token": "{{input.token}}"
}
```

**Output:** `{ "header": { "alg": "RS256", "typ": "JWT" }, "payload": { … }, "signature": "…" }`

<!-- /SECTION: examples -->

---

<!-- SECTION: workflow-example -->
## Workflow Integration

### Sample Workflow: Issue → Verify

Generate a key pair (Crypto node), sign a token, then verify it.

```json
{
  "nodes": [
    {
      "id": "keys",
      "type": "crypto-action",
      "config": { "operation": "Generate Key Pair", "keyType": "RSA" }
    },
    {
      "id": "sign",
      "type": "jwt",
      "config": {
        "operation": "sign",
        "algorithm": "RS256",
        "keyFormat": "pem",
        "privateKey": "{{output.keys.privateKey}}",
        "payload": { "sub": "user-123", "iss": "my-service" }
      }
    },
    {
      "id": "verify",
      "type": "jwt",
      "config": {
        "operation": "verify",
        "algorithm": "RS256",
        "keyFormat": "pem",
        "publicKey": "{{output.keys.publicKey}}",
        "token": "{{output.sign.token}}",
        "expectedIssuer": "my-service"
      }
    }
  ]
}
```

### Common Patterns

- **Auth issuance:** Login handler → JWT `sign` (HS256/RS256) → return token
- **Request verification:** HTTP/webhook trigger → JWT `verify` (claim checks) → branch on `valid`
- **Inspect without trust:** JWT `decode` → read `kid`/claims for routing (then verify properly)
- **Confidential claims:** JWT `encrypt` (JWE) → send to partner → partner `decrypt`s

<!-- /SECTION: workflow-example -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### `error:1E08010C:DECODER routines::unsupported` (`ERR_OSSL_UNSUPPORTED`)

**Cause:** The PEM key could not be decoded. Most often the key field was given a **literal string that isn't a key** (e.g. an un-evaluated expression path), or a PEM whose newlines were lost in transit.

**Solution:** Bind keys with a proper **expression** (e.g. `{{output.KeyNode.privateKey}}`), not a pasted/stringified value. When pasting a PEM, include the full `-----BEGIN/END-----` headers and preserve line breaks. JWK keys should be the JSON object (set `keyFormat: jwk` or leave `auto`).

#### `` `privateKey` / `publicKey` / `secret` is required for <ALG> … ``

**Cause:** The key field for the chosen algorithm family is empty. HMAC (HS*) needs `secret`; RS/PS/ES/EdDSA need `privateKey` (sign) or `publicKey` (verify).

**Solution:** Provide the field matching the algorithm. The UI shows the correct field once the algorithm is selected.

#### `signature verification failed`

**Cause:** Wrong key, wrong algorithm, or the token was altered.

**Solution:** Verify with the public key (or secret) that pairs with the signing key, and make sure `algorithm`/`algorithms` includes the token's actual `alg`.

#### `"exp" claim timestamp check failed` / `"aud" claim check failed`

**Cause:** The token is expired (or not yet valid), or a claim doesn't match `expectedIssuer`/`expectedAudience`/`expectedSubject`.

**Solution:** Confirm the claim values, or relax with `clockTolerance` / `maxTokenAge` / `currentDate` as appropriate.

#### Algorithm-confusion concerns

**Cause:** Accepting more algorithms than intended during verification.

**Solution:** Keep `algorithms` tight (it defaults to just the selected `algorithm`). Never accept `none`.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->
## Related

- [Crypto Action](./crypto.md) - Hashing, HMAC, RSA signing, AES/RSA encryption
- [Function](./function.md) - Custom JavaScript for advanced data transformations
- [Webhook Trigger](./whatsapp-webhook.md) - Receive requests carrying bearer tokens

<!-- /SECTION: related -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-06-18 | Initial documentation. Key generation moved to the Crypto node's **Generate Key Pair** operation (the JWT node no longer has a `generateKey` operation). |

<!-- /SECTION: changelog -->
