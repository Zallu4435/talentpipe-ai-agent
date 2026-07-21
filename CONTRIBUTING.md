# 🤝 Contributing to talentpipe-ai-agent

Thank you for your interest in contributing! This document provides guidelines and instructions for contributing to the project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Making Changes](#making-changes)
- [Submitting Changes](#submitting-changes)
- [Reporting Bugs](#reporting-bugs)
- [Suggesting Features](#suggesting-features)

---

## Code of Conduct

This project adheres to the Contributor Covenant [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to muhammednazal28@gmail.com.

---

## Getting Started

### Prerequisites

- n8n instance (self-hosted or cloud)
- Telegram Bot API token
- Google Gemini API key
- Redis instance
- Supabase account
- Google Sheets with service account

### Initial Setup

1. **Fork the repository**
   ```bash
   # Click "Fork" on GitHub
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/talentpipe-ai-agent.git
   cd talentpipe-ai-agent
   ```

3. **Create a development branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

---

## Development Setup

### 1. Configure Environment Variables

```bash
# Copy the example file
cp .env.example .env

# Edit with your credentials
nano .env
```

Fill in all values from your services:
```
TELEGRAM_BOT_TOKEN=your_token
ADMIN_TELEGRAM_ID=your_id
GOOGLE_GEMINI_API_KEY=your_key
GOOGLE_SHEETS_DOC_ID=your_doc_id
REDIS_HOST=localhost
REDIS_PORT=6379
SUPABASE_API_URL=your_url
SUPABASE_API_KEY=your_key
```

### 2. Set Up n8n

```bash
# Option A: Docker
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8n

# Option B: Local Installation
npm install -g n8n
n8n start

# Access at: http://localhost:5678
```

### 3. Import Workflow

1. Open n8n (`http://localhost:5678`)
2. Create new workflow
3. Import `workflow.json`
4. Add credentials from `.env`
5. Test workflow

### 4. Local Testing

```bash
# Start n8n
n8n start

# In another terminal, test webhook
curl -X POST http://localhost:5678/webhook/test \
  -H "Content-Type: application/json" \
  -d '{"message":{"text":"test"}}'
```

---

## Making Changes

### Code Organization

```
talentpipe-ai-agent/
├── workflow.json        ← Main workflow (don't edit directly in Git)
├── docs/
│   ├── SETUP.md        ← Setup guide
│   ├── DEPLOYMENT.md   ← Deployment instructions
│   └── ARCHITECTURE.md ← System design
├── .env.example        ← Environment template
├── SECURITY_GUIDE.md   ← Security information
└── README.md           ← Project overview
```

### Workflow Modifications

**Option 1: Visual Editor (Recommended)**
1. Open n8n interface
2. Make changes visually
3. Export workflow (no credentials)
4. Commit to Git

**Option 2: Direct JSON Edit**
1. Edit `workflow.json` carefully
2. Never add credentials directly
3. Use `{{ $env.VARIABLE_NAME }}` syntax
4. Test before committing

### Commit Messages

Follow conventional commits:

```bash
# Feature
git commit -m "feat: add new webhook authentication"

# Bug fix
git commit -m "fix: resolve timeout in Gemini calls"

# Documentation
git commit -m "docs: update setup guide for Docker"

# Security fix
git commit -m "security: remove hardcoded credentials"

# Refactor
git commit -m "refactor: optimize Redis connection pool"
```

### Code Style

- Keep JSON formatted (2-space indent)
- Use descriptive node names
- Document complex logic in comments
- Test all edge cases

---

## Submitting Changes

### 1. Push Your Branch

```bash
git push origin feature/your-feature-name
```

### 2. Create Pull Request

1. Go to GitHub repository
2. Click "New Pull Request"
3. Select your branch
4. Fill in PR template:

```markdown
## Description
Brief description of what this PR does

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Security improvement

## Testing
How did you test this change?

## Checklist
- [ ] My code follows the project guidelines
- [ ] I have tested this locally
- [ ] I have updated documentation
- [ ] I have verified no credentials are exposed
- [ ] I have added security considerations (if applicable)
```

### 3. Code Review

- Address review comments
- Push updates to the same branch
- Keep conversation professional and constructive

### 4. Merge

Once approved, maintainers will merge your PR.

---

## Reporting Bugs

### Before Reporting

1. Check [existing issues](https://github.com/Zallu4435/talentpipe-ai-agent/issues)
2. Check [discussions](https://github.com/Zallu4435/talentpipe-ai-agent/discussions)
3. Review documentation

### Bug Report Template

```markdown
## Description
Brief description of the bug

## Steps to Reproduce
1. First step
2. Second step
3. Expected behavior
4. Actual behavior

## Environment
- n8n version: X.X.X
- OS: Windows/Linux/Mac
- Node version: X.X.X

## Logs
```
Relevant error logs here
```

## Screenshots
If applicable, add screenshots

## Additional Context
Any other relevant information
```

### Security Vulnerabilities

**Do NOT open a public issue for security vulnerabilities.**

Instead, email: **muhammednazal28@gmail.com**

Include:
- Description of vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (optional)

---

## Suggesting Features

### Before Suggesting

1. Check [existing issues](https://github.com/Zallu4435/talentpipe-ai-agent/issues)
2. Check [discussions](https://github.com/Zallu4435/talentpipe-ai-agent/discussions)
3. Review [roadmap](README.md#roadmap) if available

### Feature Request Template

```markdown
## Description
What would you like to add?

## Motivation
Why is this feature needed?

## Proposed Solution
How should it work?

## Alternatives
Have you considered other approaches?

## Additional Context
Screenshots, examples, etc.
```

---

## Development Tips

### n8n Best Practices

1. **Use Environment Variables**
   ```javascript
   // ✅ Good
   const apiKey = $env.API_KEY;
   
   // ❌ Bad
   const apiKey = "sk-12345";
   ```

2. **Error Handling**
   ```javascript
   try {
     // Your code
   } catch (error) {
     return {
       error: error.message,
       code: error.code
     };
   }
   ```

3. **Testing Nodes**
   - Use "Test" button in n8n
   - Check node outputs
   - Verify credentials work

4. **Performance**
   - Minimize API calls
   - Use caching where possible
   - Batch operations

### Useful Resources

- [n8n Documentation](https://docs.n8n.io/)
- [n8n Community Forum](https://community.n8n.io/)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Google Gemini API](https://ai.google.dev/)
- [Redis Documentation](https://redis.io/documentation/)

---

## Project Maintainers

- **MUHAMMED NAZAL K** - [@Zallu4435](https://github.com/Zallu4435)

---

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

## Questions?

- Open a [discussion](https://github.com/Zallu4435/talentpipe-ai-agent/discussions)
- Check [documentation](./docs/)
- Review [security policy](.github/SECURITY.md)

Thank you for contributing! 🚀
