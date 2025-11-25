# 🌐 mTLS Browser Configuration Guide

This guide explains how to import client certificates into web browsers for mutual TLS (mTLS) authentication with VSCode Offline Gallery.

## 📋 Prerequisites

- Client certificate in PKCS#12 format (`.p12` file)
- Password for the `.p12` file
- Completed [certificate creation process](MTLS_CERTIFICATE_CREATION.md)

## 🦊 Firefox Configuration

### Open Firefox Certificate Manager

1. Open Firefox
2. Navigate to: `about:preferences#privacy`
3. Scroll down to **"Certificates"** section
4. Click **"View Certificates..."** button

### Import Certificate in Firefox

1. Switch to **"Your Certificates"** tab
2. Click **"Import..."** button
3. Select your `client.p12` file
4. Enter the password you set during PKCS#12 creation
5. Click **"OK"**

### Verify Firefox Installation

The certificate should now appear in the list. Firefox will automatically present it when accessing mTLS-protected sites.

### 🔍 Troubleshooting Firefox

#### Certificate not prompting

- Restart Firefox after import
- Check certificate validity dates in Certificate Manager
- Verify the certificate chain is complete

#### Access denied

- Ensure the server trusts your CA bundle
- Check Firefox console for detailed error messages
- Verify the certificate matches the expected client DN

## 🌐 Chromium/Chrome Configuration

### Open Chromium Certificate Manager

Choose one of these methods:

- **Direct URL**: Navigate to `chrome://settings/certificates`
- **Via Settings**: Menu (⋮) → Settings → Privacy and security → Security → Manage certificates

### Import Certificate in Chromium

1. Navigate to **"Your certificates"** tab
2. Click **"Import"** button
3. Select your `client.p12` file
4. Enter the password you set during PKCS#12 creation
5. Click **"OK"**

### Verify Chromium Installation

The certificate should now be listed. Chromium will prompt you to select it when accessing mTLS-protected sites.

### 🔍 Troubleshooting Chromium

#### Certificate not appearing

- Restart Chromium after import
- Check if the certificate is in the correct store
- Verify file permissions on the certificate

#### Selection prompt not showing

- Clear browser cache and cookies
- Check if the site is properly configured for mTLS
- Review Chromium's net-internals (`chrome://net-internals/#events`)

## 🔐 Security Recommendations

### Certificate Management

- **Protect Your .p12 File**: Store it securely and use a strong password
- **Regular Rotation**: Replace certificates before expiration
- **Revocation**: Report compromised certificates immediately
- **Backup**: Keep secure backups of certificates and passwords

### Browser Security

- **Keep Browsers Updated**: Install security updates promptly
- **Use Profiles**: Consider separate browser profiles for different certificates
- **Clear Sessions**: Log out and clear sessions when using shared computers
- **Monitor Usage**: Review certificate usage in browser settings periodically

## 🛠️ Common Issues

### Certificate Not Prompting

**Symptoms:** Browser doesn't ask to select a certificate

**Solutions:**

1. Verify the certificate is properly imported
2. Check certificate validity dates
3. Ensure the certificate chain is complete
4. Restart the browser after import
5. Clear browser cache and cookies

### Access Denied After Selection

**Symptoms:** Certificate selected but access still denied

**Solutions:**

1. Verify the server has the correct CA bundle
2. Check that the certificate DN matches server expectations
3. Review server logs for detailed error messages
4. Ensure the certificate hasn't been revoked

### Multiple Certificates Prompting

**Symptoms:** Browser shows multiple certificates to choose from

**Solutions:**

1. Remove expired or unused certificates
2. Ensure only one valid certificate for the domain
3. Check certificate purposes and key usage
4. Verify certificate subject alternative names

## 📱 Mobile Browsers

### iOS Safari

1. Email the `.p12` file to yourself or use AirDrop
2. Open the file on your iOS device
3. Follow the prompts to install the certificate
4. Go to Settings → General → Profile to verify installation

### Android Chrome

1. Go to Settings → Security → Encryption & credentials
2. Tap "Install a certificate" → "VPN & app user certificate"
3. Select your `.p12` file
4. Enter the password
5. Give the certificate a name

## 📚 Additional Resources

- [Firefox Certificate Management](https://support.mozilla.org/en-US/kb/certificates-tab)
- [Chrome Certificate Management](https://support.google.com/chrome/answer/95617)
- [mTLS Best Practices](https://smallstep.com/docs/tutorials/mutual-tls)
- [PKCS#12 Format](https://en.wikipedia.org/wiki/PKCS_12)
