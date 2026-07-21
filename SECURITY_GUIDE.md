# 🔐 Security Guide - Workflow Configuration

## Overview
This guide walks you through securing your n8n workflow by removing hardcoded secrets and implementing proper credential management.

---

## 🚨 **Critical Issues Found**

### 1. **Hardcoded Admin Telegram ID**
- **Location:** Lines 760, 816, 888 in `workflow.json`
- **Value:** `2116610981`
- **Risk:** Anyone can spoof admin notifications or intercept approvals
- **Fix:** Use environment variable `ADMIN_TELEGRAM_ID`

### 2. **Hardcoded Google Sheets Document ID**
- **Location:** Lines 183-187 in `workflow.json`
- **Value:** `18kLVtLtMP6TyQnl3AJq6a9HHDMusH6dooYqkzH-PYFU`
- **Risk:** Document is publicly accessible; anyone can read/modify job data
- **Fix:** Use environment variable `GOOGLE_SHEETS_DOC_ID`

### 3. **Exposed Credential IDs**
- **Location:** Throughout workflow.json
- **Example:** Telegram (`7EH0zZ01k9AuSOV2`), Google Gemini (`3qff7dZ4ZRQMjjMN`), Redis (`7Kn9RxCapTQeZLM5`)
- **Risk:** Reveals system architecture to potential attackers
- **Fix:** Store credentials in n8n's vault, not in version control

---

## ✅ **Step-by-Step Secure Configuration**

### **Step 1: Rotate All Credentials (IMMEDIATE)**

#### Telegram Bot Token
```bash
# In Telegram BotFather
/mybots
# Select your bot
# API Token → Revoke current token
# Generate new token
```

#### Google Gemini API Key
```bash
# In Google Cloud Console
1. Go to Credentials
2. Find your API key
3. Regenerate it
```

#### Google Sheets OAuth
```bash
# In Google Cloud Console
1. Go to APIs & Services
2. Delete current authorization
3. Re-authorize with fresh token
```

#### Redis Credentials
```bash
# In your Redis provider
1. Reset password
2. Update connection string
```

#### Supabase API Key
```bash
# In Supabase Dashboard
1. Settings → API Tokens
2. Revoke old token
3. Generate new token
```

---

### **Step 2: Create `.env` File (Local Only)**

```bash
# Copy the template
cp .env.example .env

# Edit .env with your actual values
nano .env
```

**Content:**
```
# Telegram
TELEGRAM_BOT_TOKEN=sk-abc123xyz...
ADMIN_TELEGRAM_ID=2116610981

# Google Gemini
GOOGLE_GEMINI_API_KEY=AIzaSyD...

# Google Sheets
GOOGLE_SHEETS_DOC_ID=18kLVtLtMP6TyQnl3AJq6a9HHDMusH6dooYqkzH-PYFU
GOOGLE_SHEETS_SHEET_ID=gid=0

# Redis
REDIS_HOST=redis.example.com
REDIS_PORT=6379
REDIS_PASSWORD=your_secure_password

# Supabase
SUPABASE_API_URL=https://project.supabase.co
SUPABASE_API_KEY=eyJhbGc...

# n8n
N8N_ENCRYPTION_KEY=your-secure-random-key-here
```

---

### **Step 3: Update n8n Credentials**

#### In n8n Dashboard:
1. Go to **Credentials**
2. For each credential type, select **Edit**
3. Update with new rotated tokens/keys
4. **Save** with encryption enabled

---

### **Step 4: Update Workflow References**

Instead of hardcoding values like:
```json
"chatId": "2116610981"
```

Use n8n environment variables:
```json
"chatId": "={{ $env.ADMIN_TELEGRAM_ID }}"
```

---

### **Step 5: Remove Hardcoded Values from workflow.json**

The following locations need updating:

#### Location 1: Admin Telegram ID (Line 760)
**Before:**
```json
"chatId": "2116610981"
```

**After:**
```json
"chatId": "={{ $env.ADMIN_TELEGRAM_ID }}"
```

#### Location 2: Google Sheets Document ID (Lines 183-187)
**Before:**
```json
"documentId": {
  "value": "18kLVtLtMP6TyQnl3AJq6a9HHDMusH6dooYqkzH-PYFU"
}
```

**After:**
```json
"documentId": {
  "value": "={{ $env.GOOGLE_SHEETS_DOC_ID }}"
}
```

#### Location 3: Admin Check (Line 816)
**Before:**
```json
"rightValue": "2116610981"
```

**After:**
```json
"rightValue": "={{ $env.ADMIN_TELEGRAM_ID }}"
```

---

### **Step 6: Set Environment Variables in n8n**

#### Option A: Docker Environment
```bash
docker run -d \
  -e ADMIN_TELEGRAM_ID=2116610981 \
  -e GOOGLE_SHEETS_DOC_ID=18kLVtLtMP6TyQnl3AJq6a9HHDMusH6dooYqkzH-PYFU \
  -e N8N_ENCRYPTION_KEY=your-secure-key \
  n8n
```

#### Option B: n8n Cloud
1. Dashboard → Settings → Environment Variables
2. Add each variable from `.env.example`
3. Click **Save**

#### Option C: Self-Hosted
```bash
# In your n8n installation directory
echo "ADMIN_TELEGRAM_ID=2116610981" >> .env
echo "GOOGLE_SHEETS_DOC_ID=18kLVtLtMP6TyQnl3AJq6a9HHDMusH6dooYqkzH-PYFU" >> .env

# Restart n8n
systemctl restart n8n
```

---

### **Step 7: Clean Git History**

If sensitive data was already committed:

```bash
# Option 1: Remove from recent commits only
git filter-branch --force --index-filter \
  'git rm --cached --ignore-unmatch workflow.json' \
  --prune-empty --tag-name-filter cat -- --all

# Option 2: Use BFG Repo-Cleaner (easier)
bfg --delete-files workflow.json

# Force push to remove from remote
git push --force --all
git push --force --tags
```

---

### **Step 8: Configure Workflow for Variables**

In n8n workflow editor:

1. Select each node using hardcoded values
2. Open node settings
3. Replace hardcoded values with expressions:
   - `{{ $env.ADMIN_TELEGRAM_ID }}`
   - `{{ $env.GOOGLE_SHEETS_DOC_ID }}`
4. **Save** workflow

---

## 🛡️ **Additional Security Measures**

### 1. **Enable Webhook Security**
```json
{
  "webhookPath": "unique-secure-path-12345",
  "webhookBasicAuth": {
    "enabled": true,
    "username": "admin",
    "password": "strong-password"
  }
}
```

### 2. **Rate Limiting**
Already implemented in your workflow ✅
- Redis rate limit per user
- TTL: 24 hours

### 3. **Admin Verification**
Already implemented ✅
- Admin callback approval
- User status checking

### 4. **Secrets Management**
Options:
- ✅ n8n's built-in vault
- ✅ HashiCorp Vault
- ✅ AWS Secrets Manager
- ✅ Environment variables (for simple setups)

---

## 📋 **Pre-Deployment Checklist**

- [ ] All credentials rotated
- [ ] `.env.example` created and documented
- [ ] `.gitignore` configured
- [ ] Hardcoded values replaced with `$env` variables
- [ ] `.env` file added to `.gitignore`
- [ ] Git history cleaned of sensitive data
- [ ] n8n environment variables configured
- [ ] Workflow tested with new credentials
- [ ] Webhook security enabled
- [ ] Admin ID verification working
- [ ] Rate limiting functioning
- [ ] Backups created before deployment

---

## 🔄 **Credential Rotation Schedule**

- **API Keys**: Every 90 days
- **Bot Tokens**: Every 180 days
- **Database Credentials**: Every 30 days
- **Encryption Keys**: On breach or annually

---

## 🚨 **If Compromised**

1. **Immediately rotate ALL credentials**
2. **Revoke old tokens in provider dashboards**
3. **Force-push cleaned Git history**
4. **Review audit logs for unauthorized access**
5. **Update database access logs**
6. **Notify affected users if data was exposed**

---

## 📚 **References**

- [n8n Security Documentation](https://docs.n8n.io/hosting/environment-variables/)
- [OWASP Secrets Management](https://owasp.org/www-community/Sensitive_Data_Exposure)
- [CWE-798: Use of Hard-Coded Credentials](https://cwe.mitre.org/data/definitions/798.html)

---

## ✅ **Next Steps**

1. Follow the steps above sequentially
2. Test workflow with new credentials
3. Monitor logs for errors
4. Deploy to production
5. Document your setup

**Questions?** Review the n8n documentation or contact support.
