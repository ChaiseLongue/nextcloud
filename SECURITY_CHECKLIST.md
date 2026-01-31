# Security Verification Checklist

This checklist can be used to verify the security of the Nextcloud AIO deployment before and after installation.

## Pre-Deployment Security Checklist

### 1. Environment Preparation
- [ ] Host OS is up-to-date with latest security patches
- [ ] Docker is updated to the latest stable version
- [ ] Firewall is configured to allow only necessary ports
- [ ] SELinux/AppArmor is enabled (if applicable)
- [ ] Disk encryption is enabled on host (recommended)

### 2. Network Security
- [ ] Reverse proxy configured (if using one)
- [ ] WAF (Web Application Firewall) configured (recommended)
- [ ] Network segmentation implemented
- [ ] Only required ports (80, 443, 8080, 8443) are exposed
- [ ] Internal management ports restricted to admin networks only

### 3. Access Control
- [ ] Strong password policy defined (min 32 characters)
- [ ] AIO_TOKEN generated using cryptographically secure method
- [ ] Admin credentials stored in password manager
- [ ] 2FA planned for all user accounts

### 4. Backup Planning
- [ ] Backup location determined (local or remote)
- [ ] Backup encryption password generated and secured
- [ ] Backup retention policy defined
- [ ] Restore procedure documented and tested

### 5. Monitoring Setup
- [ ] Log aggregation configured (Syslog/SIEM)
- [ ] Alert rules defined for security events
- [ ] Health monitoring configured
- [ ] Incident response plan documented

## Post-Deployment Security Checklist

### 6. Initial Configuration
- [ ] AIO interface accessible only via HTTPS (port 8080)
- [ ] Default password changed immediately after first login
- [ ] Let's Encrypt certificate successfully issued
- [ ] All optional security containers enabled:
  - [ ] ClamAV (Antivirus)
  - [ ] Docker Socket Proxy (if needed)

### 7. Nextcloud Security Hardening
- [ ] 2FA enabled for all user accounts
- [ ] File access control policies configured
- [ ] External storage (if any) configured securely
- [ ] Brute-force protection enabled (Fail2Ban)
- [ ] Email notifications configured
- [ ] Security scan passed (A+ rating)

### 8. Container Security
- [ ] All containers running and healthy
- [ ] No privileged containers (except necessary ones)
- [ ] Resource limits configured appropriately
- [ ] Container logs reviewed for errors/warnings
- [ ] Watchtower auto-updates configured (or manual update schedule)

### 9. Backup Verification
- [ ] First backup completed successfully
- [ ] Backup encryption verified
- [ ] Restore procedure tested in staging
- [ ] Backup integrity check passed
- [ ] Offsite backup copy created (if applicable)

### 10. Network Verification
- [ ] TLS/SSL certificate valid and trusted
- [ ] HTTP redirects to HTTPS properly
- [ ] Only necessary ports open (verified with port scan)
- [ ] Internal container communication working
- [ ] DNS resolution working correctly

## Regular Maintenance Checklist (Monthly)

### 11. Security Updates
- [ ] AIO containers updated to latest versions
- [ ] Nextcloud apps updated
- [ ] Host OS security patches applied
- [ ] Docker updated to latest stable version
- [ ] Security advisories reviewed

### 12. Security Monitoring
- [ ] Failed login attempts reviewed
- [ ] Container logs reviewed for anomalies
- [ ] Disk space usage monitored
- [ ] Backup status verified
- [ ] Certificate expiration checked (auto-renewed)

### 13. Access Review
- [ ] User accounts reviewed (remove inactive users)
- [ ] Admin privileges reviewed
- [ ] Shared links reviewed and expired
- [ ] External access logs reviewed
- [ ] API token usage reviewed

### 14. Backup Testing
- [ ] Backup completion verified
- [ ] Backup size reasonable
- [ ] Test restore performed in staging
- [ ] Backup encryption key accessible
- [ ] Offsite backup verified

## Incident Response Checklist

### 15. Security Incident Response
- [ ] Incident detected and documented
- [ ] Affected systems identified
- [ ] Containers stopped if necessary
- [ ] Logs preserved for investigation
- [ ] Backup restoration plan ready
- [ ] Stakeholders notified
- [ ] Root cause analysis performed
- [ ] Remediation applied
- [ ] Post-incident review completed

## Compliance Checklist (If Applicable)

### 16. Regulatory Compliance
- [ ] Data residency requirements met
- [ ] Encryption requirements met (at rest and in transit)
- [ ] Audit logging enabled and retained
- [ ] Access control policies documented
- [ ] Breach notification procedures documented
- [ ] Data retention policies configured
- [ ] Privacy policies updated

### 17. Documentation
- [ ] Architecture diagram created
- [ ] Network diagram created
- [ ] Security controls documented
- [ ] Disaster recovery plan documented
- [ ] Change management procedures documented
- [ ] Runbooks created for common operations

## Security Audit Findings (Completed)

✅ **Repository Security Audit Completed** (January 31, 2026)
- ✅ Code security review passed
- ✅ No hardcoded secrets found
- ✅ Dependencies verified (no vulnerabilities)
- ✅ Docker security best practices verified
- ✅ Authentication mechanisms verified
- ✅ CSRF protection verified
- ✅ Input validation verified
- ✅ Cryptographic implementations verified
- ✅ Network communications verified
- ✅ Backup security verified

**Verdict:** ✅ **SAFE FOR CYBERSECURITY COMPANY USE**

See [SECURITY_AUDIT_REPORT.md](./SECURITY_AUDIT_REPORT.md) for detailed findings.

---

## Quick Security Check Commands

### Check Container Security
```bash
# List all running containers
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"

# Check container user
docker exec nextcloud-aio-apache id

# Review container logs for security events
docker logs nextcloud-aio-apache --tail 100 | grep -i "error\|failed\|denied"
```

### Network Security Check
```bash
# Check open ports
sudo ss -tlnp | grep -E ":(80|443|8080|8443|3478)"

# Test TLS certificate
openssl s_client -connect yourdomain.com:443 -servername yourdomain.com < /dev/null

# Check DNS resolution
nslookup yourdomain.com
```

### Verify Backups
```bash
# Check last backup time
docker exec nextcloud-aio-borgbackup borg list /mnt/borgbackup

# Verify backup integrity
docker exec nextcloud-aio-borgbackup borg check /mnt/borgbackup
```

### Security Scan
```bash
# Run Nextcloud security scan
# Visit: https://yourdomain.com/index.php/settings/admin/security

# Check for outdated apps
docker exec --user www-data nextcloud-aio-nextcloud php occ app:update --all --showonly
```

---

**Last Updated:** January 31, 2026  
**Maintained By:** Security Team  
**Review Frequency:** Quarterly or after significant changes
