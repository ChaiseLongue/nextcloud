# Privacy and Data Handling Policy

**Last Updated:** January 31, 2026

## Overview

Nextcloud All-in-One (AIO) is a **self-hosted** solution where **you maintain complete control** over all your data. This document explains what data is stored, where it resides, and what external communications occur.

## ✅ Core Privacy Principle

**Your data never leaves your infrastructure unless you explicitly configure it to do so.**

Nextcloud AIO is designed for organizations that require complete data sovereignty, including:
- Healthcare organizations (HIPAA compliance)
- Financial institutions
- Government agencies
- Cybersecurity companies
- Any organization with strict data privacy requirements

---

## Where Your Data is Stored

### 1. User Data
- **Location:** Your server's local storage or attached volumes
- **Control:** 100% under your control
- **Access:** Only accessible by users you authorize
- **Encryption:** Optional encryption at rest using your encryption keys

### 2. Database
- **Type:** PostgreSQL
- **Location:** Containerized on your server
- **Data:** User accounts, file metadata, app configurations
- **Access:** Internal to your Nextcloud installation only

### 3. Backups (Optional)
- **Local Backups:** Stored on your chosen local path or attached storage
- **Remote Backups:** Only if YOU configure a remote Borg repository
- **Encryption:** All backups are encrypted with YOUR password
- **Control:** You control backup location, retention, and encryption keys

---

## External Communications

Nextcloud AIO makes **minimal external connections**, and **NONE of them involve sending your data to third parties**.

### Legitimate External Connections

| Connection | Purpose | Your Data Sent? | Frequency |
|------------|---------|-----------------|-----------|
| **GitHub Container Registry (ghcr.io)** | Download container images | ❌ NO | During updates only |
| **Docker Hub (docker.io)** | Check for image updates | ❌ NO | Periodic checks |
| **Let's Encrypt** | TLS certificate generation | ❌ NO* | Certificate renewal only |

*Let's Encrypt only receives your domain name (which is already public in DNS) to issue TLS certificates. No file data, user data, or document content is ever sent.

### What is NEVER Sent Externally

- ❌ User files and documents
- ❌ User credentials or passwords
- ❌ Database contents
- ❌ File metadata or directory structures
- ❌ User activity logs
- ❌ IP addresses of your users
- ❌ Telemetry or analytics data
- ❌ Usage statistics
- ❌ Error reports (unless you manually choose to report them)

---

## Network Architecture

### Isolation by Design

```
┌─────────────────────────────────────────────┐
│ Your Server / Infrastructure                │
│                                             │
│  ┌────────────────────────────────────┐    │
│  │  Nextcloud AIO Containers          │    │
│  │                                    │    │
│  │  • Nextcloud                       │    │
│  │  • PostgreSQL (database)           │    │
│  │  • Redis (cache)                   │    │
│  │  • Apache (web server)             │    │
│  │  • All optional containers         │    │
│  │                                    │    │
│  │  All containers in isolated        │    │
│  │  Docker network                    │    │
│  └────────────────────────────────────┘    │
│                                             │
│  Your data NEVER leaves this boundary      │
│  unless YOU explicitly configure it         │
└─────────────────────────────────────────────┘
         │
         │ (Only for container updates)
         ▼
    Internet (Container Registries)
```

### Key Security Features

1. **Isolated Docker Network:** All containers communicate through an internal network not exposed to the internet
2. **Minimal Port Exposure:** Only necessary ports (80/443 for web access, 8080 for AIO interface) are exposed
3. **No Telemetry:** No phone-home functionality, no analytics, no tracking
4. **No External APIs:** No connections to external APIs for functionality
5. **Self-Contained:** All services run locally on your infrastructure

---

## Optional External Services

Some features require external services **only if you choose to enable them**:

### 1. Collabora Online / Nextcloud Office
- **Privacy:** Runs as a container on YOUR server
- **Data Handling:** Documents processed locally, not sent externally
- **Updates:** Container images fetched from official registry during updates

### 2. TURN Server (for Nextcloud Talk)
- **Privacy:** Runs on YOUR server
- **Data Handling:** WebRTC traffic proxied through your server
- **External:** No data sent to external services

### 3. Cloudflare Tunnel (Optional)
⚠️ **Privacy Consideration:** If you choose to use Cloudflare Tunnel:
- Cloudflare performs TLS termination (decrypts traffic)
- Your data passes through Cloudflare infrastructure
- **Not recommended for privacy-sensitive deployments**
- See [README.md](./readme.md) for details

**Alternative:** Use standard reverse proxy (nginx, Apache, Caddy) to maintain complete control

---

## Compliance and Certifications

Nextcloud AIO supports compliance with:

- ✅ **GDPR** (General Data Protection Regulation)
- ✅ **HIPAA** (Health Insurance Portability and Accountability Act)
- ✅ **SOC 2**
- ✅ **ISO 27001**

As a self-hosted solution, YOU are the data controller, not Nextcloud or any third party.

---

## Data Export Capabilities

You can export your data at any time:

1. **Backup Export:** Use the built-in backup system to create encrypted archives
2. **Direct File Access:** Access files directly from your server's file system
3. **Database Export:** Export PostgreSQL database using standard tools
4. **User Migration:** Use Nextcloud's user migration app to export user data

**Important:** YOU control all exports. No automatic exports to external parties occur.

---

## Third-Party Apps

Nextcloud supports optional third-party apps from the [Nextcloud App Store](https://apps.nextcloud.com/).

**Privacy Impact:**
- Core AIO installation includes only official, vetted apps
- If you install additional third-party apps, review their privacy policies
- Some apps may make external API calls (e.g., weather apps, news readers)
- You have full control over which apps are installed

**Recommendation:** Only install apps from trusted sources and review their network activity.

---

## Audit and Transparency

### Security Audits

This repository has undergone comprehensive security audits:
- See [SECURITY_AUDIT_REPORT.md](./SECURITY_AUDIT_REPORT.md) for detailed findings
- See [SECURITY_SUMMARY.md](./SECURITY_SUMMARY.md) for executive summary
- See [SECURITY_CHECKLIST.md](./SECURITY_CHECKLIST.md) for deployment best practices

### Code Transparency

- ✅ 100% open-source code
- ✅ All code publicly auditable on GitHub
- ✅ No proprietary or closed-source components in core AIO
- ✅ Active community review and contributions

### Network Monitoring

You can monitor all network connections:

```bash
# Monitor container network activity
docker logs nextcloud-aio-mastercontainer

# Check active network connections
docker exec nextcloud-aio-nextcloud netstat -tuln

# Use tools like Wireshark or tcpdump to audit network traffic
```

---

## Data Retention

**You control data retention policies:**

- User files: Retained until you or your users delete them
- Database: Retained as long as your instance runs
- Backups: Retained according to your backup configuration
- Logs: Configurable retention in Nextcloud settings

**No external party** has access to determine data retention policies for your instance.

---

## Questions and Concerns

### Can Nextcloud Inc. access my data?

**NO.** Nextcloud Inc. (the company behind Nextcloud) has:
- ❌ No access to your server
- ❌ No access to your data
- ❌ No backdoors or remote access capabilities
- ❌ No telemetry or analytics collection

Nextcloud AIO is truly self-hosted software that you deploy and control.

### What if I find external connections I don't recognize?

1. Check the [External Communications](#external-communications) section above
2. Review container logs: `docker logs <container-name>`
3. Use network monitoring tools to identify the connection
4. Report unexpected connections as potential security issues

### Can I run Nextcloud AIO completely offline?

**Yes**, with limitations:
- Initial installation requires internet (to download container images)
- After installation, can run completely air-gapped
- Updates require internet access (to download new container images)
- Let's Encrypt TLS requires internet (use manual certificates for offline)

For air-gapped deployments:
1. Download and cache container images on an internet-connected system
2. Transfer images to your offline environment
3. Load images manually using `docker load`

---

## Best Practices for Maximum Privacy

1. **Network Isolation:**
   - Deploy behind a firewall
   - Use VPN for remote access instead of public exposure
   - Consider deploying in an isolated VLAN

2. **Access Control:**
   - Restrict AIO interface access to management network only
   - Use strong, unique passwords (32+ characters)
   - Enable 2FA (two-factor authentication) for all users

3. **Encryption:**
   - Enable encryption at rest for sensitive data
   - Use TLS/SSL for all connections (enabled by default)
   - Store backup encryption keys securely (e.g., offline or in a vault)

4. **Monitoring:**
   - Enable audit logging
   - Monitor failed login attempts
   - Set up alerts for unusual activity
   - Regularly review access logs

5. **Updates:**
   - Keep Nextcloud AIO updated for security patches
   - Test updates in a staging environment first
   - Subscribe to security advisories

6. **Backup Security:**
   - Use encrypted backups (enabled by default)
   - Store backups on separate infrastructure
   - Test backup restoration procedures regularly
   - Protect backup encryption passwords

---

## Conclusion

**Nextcloud AIO is designed for complete data privacy and sovereignty.**

Key Guarantees:
- ✅ Your data stays on your infrastructure
- ✅ No telemetry, analytics, or tracking
- ✅ No data sent to Nextcloud Inc. or third parties
- ✅ You maintain complete control
- ✅ Suitable for the most privacy-sensitive use cases

**This makes Nextcloud AIO ideal for:**
- Law firms handling confidential client data
- Healthcare providers managing patient records
- Financial institutions processing sensitive transactions
- Cybersecurity companies protecting proprietary information
- Government agencies handling classified information
- Any organization requiring absolute data privacy

---

## Contact and Support

- **Security Issues:** Report via GitHub Security Advisories
- **General Questions:** [Nextcloud Community Forum](https://help.nextcloud.com/)
- **Documentation:** See [README.md](./readme.md) for complete documentation

**Remember:** As a self-hosted solution, you are responsible for your instance's security and privacy configuration. Follow the [SECURITY_CHECKLIST.md](./SECURITY_CHECKLIST.md) for deployment best practices.
