# 🔐 mTLS Certificate Creation Guide

This guide explains how to create client certificates for mutual TLS (mTLS) authentication with VSCode Offline Gallery using Step CA.

> **Note:** While the `step` command is commonly used, this guide uses `step-cli` for clarity. Both commands are interchangeable.

## 📋 Prerequisites

- Step CA installed and configured
- Access to root CA certificate and private key
- Basic understanding of PKI concepts

## 🔑 Certificate Creation Process

### Step 1: Create Intermediate CA Certificate

The intermediate CA is used to sign client certificates, providing an additional layer of security.

#### Option A: Direct signing (online mode)

```sh
step-cli certificate create "Client mTLS Intermediate CA" \
  client-mtls-intermediate.crt client-mtls-intermediate.key \
  --profile intermediate-ca \
  --ca $(step-cli path)/certs/root_ca.crt \
  --ca-key $(step-cli path)/secrets/root_ca_key
```

#### Option B: CSR-based signing (offline root CA)

If your root CA private key is stored on an offline machine for enhanced security:

1. Generate Certificate Signing Request (CSR):

```sh
step-cli certificate create "Client mTLS Intermediate CA" \
  client-mtls-intermediate.csr client-mtls-intermediate.key \
  --csr \
  --kty RSA \
  --size 4096
```

1. Sign the CSR on the offline machine:

```sh
step-cli certificate sign \
  --profile intermediate-ca \
  client-mtls-intermediate.csr \
  $(step-cli path)/certs/root_ca.crt \
  $(step-cli path)/secrets/root_ca_key \
  > client-mtls-intermediate.crt
```

### Step 2: Create Client Certificate

Generate a client certificate for user authentication:

```sh
step-cli certificate create <email> \
  client.crt client.key \
  --profile leaf \
  --ca client-mtls-intermediate.crt \
  --ca-key client-mtls-intermediate.key \
  --not-after 720h
```

Replace `<email>` with the user's email address or identifier.

### Step 3: Create PKCS#12 Bundle

Convert the client certificate to PKCS#12 format for browser import:

```sh
step-cli certificate p12 client.p12 client.crt client.key
```

You'll be prompted to set a password for the `.p12` file.

### Step 4: Create CA Bundle

Create a bundle containing both intermediate and root CA certificates:

```sh
cat client-mtls-intermediate.crt $(step-cli path)/certs/root_ca.crt > client-ca-bundle.pem
```

### Step 5: Encode CA Bundle for Docker

Convert the CA bundle to base64 for use in Docker environment variables:

```sh
cat client-ca-bundle.pem | base64 -w 0
```

Copy the output and use it as the value for `SSL_CLIENT_CERT_CONTENT` in your `.env` file.

## 🔒 Security Best Practices

- **Protect Private Keys**: Store private keys securely and never share them
- **Use Strong Passwords**: Protect `.p12` files with strong passwords
- **Offline Root CA**: Keep root CA private key on an offline machine when possible
- **Certificate Rotation**: Regularly rotate client certificates
- **Revocation**: Implement certificate revocation for compromised certificates
- **Backup**: Securely backup certificates and keys

## 🛠️ Troubleshooting

### Certificate Not Recognized

- Verify the CA bundle includes both intermediate and root certificates
- Check that the base64 encoding is correct (no line breaks)
- Ensure `SSL_CLIENT_CERT_CONTENT` is properly set in `.env`

### Connection Refused

- Verify nginx-proxy is configured for mTLS
- Check that the server has the correct CA bundle
- Review nginx-proxy logs for detailed error messages

## 📚 Additional Resources

- [Step CLI Documentation](https://smallstep.com/docs/step-cli)
- [mTLS Best Practices](https://smallstep.com/docs/tutorials/mutual-tls)
- [PKCS#12 Format Specification](https://en.wikipedia.org/wiki/PKCS_12)
- [nginx mTLS Configuration](https://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl_verify_client)
