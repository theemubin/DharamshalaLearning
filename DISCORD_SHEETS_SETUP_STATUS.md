# Firebase & Google Sheets Integration - Setup Guide

## ✅ Completed Steps

1. **Discord Webhook Added** - `.env` file configured with Discord webhook URL
2. **Code Deployed** - Latest version live at https://campuslearnings.web.app

---

## 🔄 Step 1: Get Firebase Admin Service Account

You need to download the **Firebase Admin SDK** service account (different from Google Sheets service account):

### Instructions:
1. Go to **Firebase Console**: https://console.firebase.google.com/project/dharamshalacampus/settings/serviceaccounts/adminsdk
2. Click on **Service accounts** tab
3. Click **"Generate new private key"** button
4. Save the downloaded JSON file as: `scripts/firebase-service-account.json`

**Important:** This service account has full Firebase admin access.

---

## 🔄 Step 2: Share Google Sheet with Service Account

1. Open your Google Sheet: https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit
2. Click **Share** button (top right)
3. Add this email as **Editor**:
  ```
  <service-account-name>@<project-id>.iam.gserviceaccount.com
  ```
4. Click **Send**

---

## 🔄 Step 3: Run Metadata Initialization

After downloading the Firebase service account, run:

```bash
cd "/Users/mubinmac/Documents/Codespace/Campus Learning Dashboard"
node scripts/init-metadata.js
```

This creates the `system_metadata` collection in Firestore for tracking sync history.

---

## 🔄 Step 4: Configure GitHub Secrets

Go to your GitHub repository: https://github.com/theemubin/DharamshalaLearning/settings/secrets/actions

Add these 4 secrets (use underscores, not hyphens):

### 1. `FIREBASE_SERVICE_ACCOUNT`
```json
{
  "type": "service_account",
  "project_id": "<your-project-id>",
  "private_key_id": "<your-private-key-id>",
  "private_key": "-----BEGIN PRIVATE KEY-----\n<your-private-key>\n-----END PRIVATE KEY-----\n",
  "client_email": "<service-account>@<project-id>.iam.gserviceaccount.com",
  "client_id": "<client-id>",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/<service-account>%40<project-id>.iam.gserviceaccount.com",
  "universe_domain": "googleapis.com"
}
```
*(Paste the JSON downloaded from Firebase Admin SDK here.)*

### 2. `GOOGLE_SERVICE_ACCOUNT`
```json
{
  "type": "service_account",
  "project_id": "<your-project-id>",
  "private_key_id": "<your-private-key-id>",
  "private_key": "-----BEGIN PRIVATE KEY-----\n<your-private-key>\n-----END PRIVATE KEY-----\n",
  "client_email": "<service-account>@<project-id>.iam.gserviceaccount.com",
  "client_id": "<client-id>",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/<service-account>%40<project-id>.iam.gserviceaccount.com",
  "universe_domain": "googleapis.com"
}
```
*(Usually the Sheets automation account; ensure it has access to both Firebase and the spreadsheet.)*

### 3. `GOOGLE_SHEETS_ID`
```
YOUR_GOOGLE_SHEETS_ID
```

### 4. `DISCORD_WEBHOOK_URL`
```
https://discord.com/api/webhooks/YOUR_WEBHOOK_ID/YOUR_WEBHOOK_TOKEN
```

---

## 🔄 Step 5: Test GitHub Actions Workflow

1. Go to: https://github.com/theemubin/DharamshalaLearning/actions
2. Click on **"Sync Data to Google Sheets"** workflow
3. Click **"Run workflow"** dropdown
4. Click **"Run workflow"** button
5. Check the logs to verify it succeeds

---

## 🔄 Step 6: Verify Discord Notifications

1. Have a few users log in to the app
2. Check your Discord channel for login notifications
3. You should see:
   - Individual login notifications (optional, can be disabled)
   - Daily summary at 11:59 PM IST

---

## 📊 What Happens After Setup

### Automatic Daily Sync (2 AM IST)
- Goals, Reflections, and Login data sync to Google Sheets
- Runs via GitHub Actions
- Incremental sync (only new/updated records)

### Discord Notifications
- Login tracking whenever users log in
- Daily attendance summaries

### Manual Sync
Admins can trigger manual sync from the app's admin panel (feature ready in code).

---

## 🆘 Need Help?

Common issues:
1. **Permission Denied**: Make sure you shared the Google Sheet with the service account
2. **GitHub Actions Failing**: Check that all secrets are added with correct names (use underscores)
3. **No Discord Messages**: Verify webhook URL is correct and channel exists

---

## 📝 Current Status

- ✅ Code deployed with permission logging fixes
- ✅ Discord webhook configured in `.env`
- ⏳ Waiting for Firebase Admin service account
- ⏳ Waiting for Google Sheet sharing
- ⏳ Waiting for GitHub secrets configuration
