# GitHub App Setup Guide

## 🤖 Create the GitHub App

### Step 1: Basic Information

Go to: https://github.com/settings/apps/new

**App Settings:**
- **App name**: `windsurf-bot`
- **Homepage URL**: `https://depollute.run`
- **Webhook URL**: Leave empty (we use GitHub Actions)
- **Webhook secret**: Leave empty

### Step 2: Permissions

**Repository permissions:**
```
Issues: Read & Write
Pull requests: Read & Write
Contents: Read & Write
Metadata: Read-only
Checks: Read & Write
Commit statuses: Read & Write
```

**Organization permissions:**
```
Members: Read-only
Teams: Read-only
```

### Step 3: Subscribe to Events

**Issue events:**
```
- Assigned
- Closed
- Edited
- Labeled
- Opened
- Reopened
```

**Pull request events:**
```
- Assigned
- Closed
- Edited
- Opened
- Ready for review
- Reopened
- Review requested
- Synchronize
```

**Pull request review events:**
```
- Submitted
- Edited
- Dismissed
```

**Status events:**
```
- All
```

### Step 4: Create and Install

1. **Create the app**
2. **Generate private key** - Download the `.pem` file
3. **Note the App ID** - This will be shown after creation
4. **Install on repositories** - Add to target repositories

### Step 5: Get Installation ID

After installation, you'll need the Installation ID:

1. Go to your app settings
2. Click on "Install App"
3. Find the installation and note the ID from the URL or API

## 🔧 Repository Configuration

### Required Secrets

Add these secrets to your repository:

```bash
# GitHub App Authentication
WINDSURF_BOT_APP_ID=123456
WINDSURF_BOT_PRIVATE_KEY="-----BEGIN RSA PRIVATE KEY-----\n...\n-----END RSA PRIVATE KEY-----"
WINDSURF_BOT_INSTALLATION_ID=789012

# API Configuration
WINDSURF_API_URL=https://your-app.vercel.app/api/windsurf/analyze
WINDSURF_API_TOKEN=your-api-token-if-needed
```

### Workflow Installation

1. Copy `.github/workflows/bot-analysis.yml` to your repository
2. Add the required secrets
3. Test by assigning the bot to an issue or PR

## ✅ Verification

After setup, verify:

1. **Bot appears in assignees list**
2. **Bot appears in reviewers dropdown**
3. **Workflow triggers on assignment**
4. **API endpoint responds correctly**

## 🔍 Troubleshooting

### Common Issues

**Bot not appearing in assignees:**
- Check that the app is installed on the repository
- Verify permissions include "Issues: Read & Write"

**Workflow not triggering:**
- Check that the bot is actually assigned (not just mentioned)
- Verify workflow file is in `.github/workflows/`

**API calls failing:**
- Verify the API URL is correct and deployed
- Check authentication tokens
- Review API logs

### Debug Commands

```bash
# Check app installation
curl -H "Authorization: token YOUR_TOKEN" \
  https://api.github.com/apps/YOUR_APP_ID/installations

# Test JWT generation
node -e "
const jwt = require('jsonwebtoken');
const privateKey = 'YOUR_PRIVATE_KEY';
const appId = YOUR_APP_ID;
const payload = {
  iat: Math.floor(Date.now() / 1000) - 60,
  exp: Math.floor(Date.now() / 1000) + 600,
  iss: appId
};
console.log(jwt.sign(payload, privateKey, { algorithm: 'RS256' }));
"
```

## 🚀 Next Steps

Once the GitHub App is configured:

1. **Test with a simple issue assignment**
2. **Verify workflow execution**
3. **Check API response handling**
4. **Monitor bot activity in repository**

---

**Need help?** Check the [main documentation](../README.md) or open an issue.
