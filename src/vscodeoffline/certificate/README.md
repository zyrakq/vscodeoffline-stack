# 🔐 mTLS Certificate Examples

This directory contains example certificate files for mTLS (mutual TLS) authentication setup with VSCode Offline Gallery.

## 📁 Certificate Files

### Root CA Certificate

**File:** [`root_ca.crt`](root_ca.crt)

The root Certificate Authority certificate that serves as the trust anchor for the entire certificate chain.

- **Common Name:** Step CA Root CA
- **Organization:** Step CA
- **Purpose:** Root of trust for all certificates in the chain
- **Validity:** 10 years (2025-12-06 to 2035-12-04)

**Password:**

```sh
TFoGYoYdw95eqF7fC94Ptfi3WkqfvIvH
```

---

### Intermediate CA Certificate

**File:** [`client-mtls-intermediate.crt`](client-mtls-intermediate.crt)

The intermediate Certificate Authority certificate used specifically for signing client mTLS certificates.

- **Common Name:** Client mTLS Intermediate CA
- **Purpose:** Signs client certificates for mTLS authentication
- **Validity:** 10 years (2025-12-06 to 2035-12-04)
- **Signed by:** Step CA Root CA

**Password:**

```sh
DUfK2YexZn6q6gm2RpD1l8B6KOBMX2rw
```

---

### Client Certificate

**File:** [`client.crt`](client.crt)

The end-user client certificate for mTLS authentication.

- **Common Name:** <user-vscodeoffline@example.com>
- **Purpose:** Client authentication for accessing the VSCode Offline Gallery
- **Validity:** 1 day (2025-12-06 to 2025-12-07)
- **Signed by:** Client mTLS Intermediate CA
- **Subject Alternative Name:** <user-vscodeoffline@example.com>

**Password:**

```sh
fhDRnv1UckmF1EjMcvwqY3hESJLxWXHj
```

---

### Client PKCS#12 Bundle

**File:** [`client.p12`](client.p12)

A PKCS#12 archive containing the client certificate and private key in a single encrypted file. This format is commonly used for importing certificates into browsers and other applications.

- **Contents:** Client certificate + Client private key
- **Format:** PKCS#12 (.p12)
- **Usage:** Import into Firefox, Chrome, or other browsers for mTLS authentication

**Password:**

```sh
GH4WmJTjVP0YqNyofRznU7Dqzuj2ni2S
```

---

### CA Bundle for Server Configuration

**File:** [`client-ca-bundle.pem`](client-ca-bundle.pem)

A combined certificate bundle containing both the intermediate CA and root CA certificates. This file is used by the server to verify client certificates during mTLS handshake.

- **Contents:**
  1. Client mTLS Intermediate CA certificate
  2. Step CA Root CA certificate
- **Purpose:** Server-side client certificate verification
- **Usage:** Convert to base64 for `SSL_CLIENT_CERT_CONTENT` environment variable

#### 🔄 Base64 Conversion

To use this bundle with the mTLS guard, convert it to base64:

```bash
cat client-ca-bundle.pem | base64 -w 0
```

**Base64 Output:**

```sh
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUJ0VENDQVZ5Z0F3SUJBZ0lRWFJNeFFXeElkTVRZRDFWQVFNSjlVREFLQmdncWhrak9QUVFEQWpBc01SQXcKRGdZRFZRUUtFd2RUZEdWd0lFTkJNUmd3RmdZRFZRUURFdzlUZEdWd0lFTkJJRkp2YjNRZ1EwRXdIaGNOTWpVeApNakEyTURZeU56RTRXaGNOTXpVeE1qQTBNRFl5TnpFNFdqQW1NU1F3SWdZRFZRUURFeHREYkdsbGJuUWdiVlJNClV5QkpiblJsY20xbFpHbGhkR1VnUTBFd1dUQVRCZ2NxaGtqT1BRSUJCZ2dxaGtqT1BRTUJCd05DQUFTZXAvMGcKaTVZTFZCU2V5TzhZM1pFV3hPY2lEU3JUYVRPdGkzOXVJRS9BZ2ptVjV1RmdhOGNXZVNpbUFnbUNzN0hTRDE5egpXaCtkVy9zbHg5VDRJeWIxbzJZd1pEQU9CZ05WSFE4QkFmOEVCQU1DQVFZd0VnWURWUjBUQVFIL0JBZ3dCZ0VCCi93SUJBREFkQmdOVkhRNEVGZ1FVYzRudytWcDhvZGw1eDZXbjZ5cFJrcCtqOEJJd0h3WURWUjBqQkJnd0ZvQVUKLzBFQ3ZKMG9CTlJlUUtkZndOanE0YnV4KzBjd0NnWUlLb1pJemowRUF3SURSd0F3UkFJZ1lGV09Xd1htUWFYQgp6MmVtQ0FWSnoybE5lcDlnWHB3VlNYREtwVzFxWlNVQ0lEYkxoUnRKSll4OFJhZDJzTWxBNmd2ODRqTGN3UUFPCk9lanJ1a0NzYzlsZAotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCi0tLS0tQkVHSU4gQ0VSVElGSUNBVEUtLS0tLQpNSUlCbXpDQ0FVS2dBd0lCQWdJUkFMOVFBSzRTMXFpYmJGcS9iYnk5NytVd0NnWUlLb1pJemowRUF3SXdMREVRCk1BNEdBMVVFQ2hNSFUzUmxjQ0JEUVRFWU1CWUdBMVVFQXhNUFUzUmxjQ0JEUVNCU2IyOTBJRU5CTUI0WERUSTEKTVRJd05qQTJNVGswTTFvWERUTTFNVEl3TkRBMk1UazBNMW93TERFUU1BNEdBMVVFQ2hNSFUzUmxjQ0JEUVRFWQpNQllHQTFVRUF4TVBVM1JsY0NCRFFTQlNiMjkwSUVOQk1Ga3dFd1lIS29aSXpqMENBUVlJS29aSXpqMERBUWNEClFnQUVpNGlSNGZSYWkzNUxBYXlZbVRGMVRZMG43cmV4T2RyUFUyUmFnbk9NRWRNTnlOWUxSM3hTWVhBR2NBdUkKRzdrSjZzOHVtSldWc2JJdWJrMDRaaXByMEtORk1FTXdEZ1lEVlIwUEFRSC9CQVFEQWdFR01CSUdBMVVkRXdFQgovd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFRlA5QkFyeWRLQVRVWGtDblg4RFk2dUc3c2Z0SE1Bb0dDQ3FHClNNNDlCQU1DQTBjQU1FUUNJRVF2WExMYk9uVldwQ05WYUFoWTRsUU81c29uTks1VFg5K0RIeWVZYjBqd0FpQVkKaFpBZHVJdHVnZVRVRjYzbjlVc2w4WWZzTy83WDB0WFozZGlpc24wUkZ3PT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
```

This base64 string should be used as the value for the `SSL_CLIENT_CERT_CONTENT` environment variable in your mTLS deployment configuration.

---

## 🔗 Related Documentation

- [📝 Certificate Creation Guide](../mTLS-step-ca.md) - How to create these certificates using Step CA
- [🌐 Browser Configuration Guide](../mTLS-browser-setup.md) - How to import certificates into browsers
- [🏗️ Step CA Stack](https://github.com/zyrakq/step-ca-stack) - Step CA deployment and initial CLI setup

## ⚠️ Security Notes

- **These are example certificates only** - Do not use in production
- All private keys in this directory are encrypted with passwords
- The client certificate has a short validity period (1 day) for demonstration purposes
- In production, use longer validity periods and secure password management
- Store passwords securely using a password manager or secrets management system
- Never commit unencrypted private keys to version control

## 📋 Certificate Chain

```sh
Step CA Root CA (root_ca.crt)
    └── Client mTLS Intermediate CA (client-mtls-intermediate.crt)
            └── Client Certificate (client.crt)
                    └── user-vscodeoffline@example.com
```

## 🛠️ Usage in VSCode Offline Gallery

1. **Server Configuration:** Use [`client-ca-bundle.pem`](client-ca-bundle.pem) (base64-encoded) in `SSL_CLIENT_CERT_CONTENT`
2. **Client Configuration:** Import [`client.p12`](client.p12) into your browser
3. **Trust Chain:** Ensure [`root_ca.crt`](root_ca.crt) is trusted by your system/browser

For detailed setup instructions, refer to the [mTLS setup guides](../README.md#mtls-extension-configuration).
