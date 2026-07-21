# 🔐 Security Policy

## Reporting Security Vulnerabilities

If you discover a security vulnerability in **talentpipe-ai-agent**, we appreciate your responsible disclosure and ask that you **DO NOT** publicly disclose the vulnerability until we have a chance to fix it.

### How to Report

1. **Email:** muhammednazal28@gmail.com
2. **Subject:** `[SECURITY] Vulnerability Report - talentpipe-ai-agent`
3. **Include:**
   - Description of the vulnerability
   - Steps to reproduce (if applicable)
   - Potential impact
   - Your contact information (optional)

### Response Timeline

- ✅ **Acknowledgment:** Within 24-48 hours
- 🔧 **Investigation:** Within 1 week
- 📋 **Fix & Release:** Within 2 weeks (or explanation of timeline)
- 📢 **Public Disclosure:** With your permission

### Security Guidelines

When using **talentpipe-ai-agent**, please follow these practices:

1. **Never commit `.env` files** - Use `.env.example` template
2. **Always use environment variables** for secrets
3. **Rotate credentials regularly** - At least every 90 days
4. **Enable authentication** in n8n dashboard
5. **Use HTTPS** for all webhooks
6. **Restrict admin access** - Only authorized users
7. **Monitor logs** for suspicious activity
8. **Keep dependencies updated**

### Security Features

- ✅ Rate limiting (24-hour TTL per user)
- ✅ User approval workflow
- ✅ Admin verification callbacks
- ✅ Redis session management
- ✅ Environment variable support
- ✅ Credential isolation in n8n

### Vulnerability Disclosure

We follow the **Coordinated Vulnerability Disclosure** principle:

1. Report to us privately first
2. Allow reasonable time to fix
3. We'll acknowledge the issue
4. Once fixed, we'll publish credits
5. You'll be credited as discoverer

### What Qualifies as a Security Vulnerability?

✅ **YES:**
- Hardcoded credentials exposure
- Authentication bypass
- Unauthorized data access
- Injection attacks
- CSRF/XSS issues

❌ **NO:**
- Documentation improvements
- General feature requests
- Non-security bugs
- Social engineering (doesn't apply to code)

### Security Not Covered

The following are **NOT** security vulnerabilities and should be reported as feature requests:

- Adding new authentication methods
- Performance improvements
- UI/UX enhancements
- Missing error handling

### Credits

Thank you to all security researchers who responsibly disclose vulnerabilities:

- [Your name here - when issues are fixed]

---

**Version:** 1.0  
**Last Updated:** July 21, 2026
