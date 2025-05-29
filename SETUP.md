# 🛠️ Setup Guide

Complete setup instructions for the Quick City Guide automation system.

## 📋 Files in This Repository

- **README.md** - Project overview and documentation
- **workflow.json** - Clean n8n workflow file (API keys removed)
- **SETUP.md** - This setup guide
- **screenshots/** - Project demonstration images

---

## 🚀 Step-by-Step Setup

### Step 1: Import Workflow
1. Open **n8n.cloud** or your local n8n instance
2. Click **Import** → **From File**
3. Upload `workflow.json`
4. **Result**: 15-node workflow will be imported

### Step 2: Create Airtable Database
Create a table with these exact fields:
- **City** (Single line text) - City name
- **Email** (Email) - User's email address
- **HTML_Content** (Long text) - Generated HTML content
- **Last Modified** (Last modified time) - For triggering workflow

### Step 3: Get API Keys

#### Groq AI (Free)
- **Sign up**: https://console.groq.com/
- **Get key**: Dashboard → API Keys → Create Key
- **Limit**: 2000-4000 tokens per request

#### Unsplash (Free)
- **Sign up**: https://unsplash.com/developers
- **Create app**: New Application → Get Access Key
- **Limit**: 50 requests per hour

#### MapBox (Free)
- **Sign up**: https://www.mapbox.com/
- **Get token**: Account → Access Tokens → Create Token
- **Limit**: 50,000 requests per month

### Step 4: Configure n8n Credentials

#### Setup Groq
1. **Credentials** → **Add New** → **Groq API**
2. Enter your API key
3. **Used by**: "Generate Attractions & Coordinates", "Generate Improved Guide"

#### Setup Gmail
1. **Credentials** → **Add New** → **Gmail OAuth2**
2. Follow OAuth setup process
3. **Used by**: "Send Initial Travel Guide", "Send Improved Travel Guide"

#### Setup Airtable
1. **Credentials** → **Add New** → **Airtable Token API**
2. Enter Personal Access Token from Airtable
3. **Used by**: All Airtable nodes

### Step 5: Update API Keys in Code

#### Update Unsplash Key
**Location**: "Fetch City Images" node
**Field**: Headers → Authorization
**Replace**: `[YOUR_UNSPLASH_ACCESS_KEY]` with your actual key

#### Update MapBox Token
**Location**: "Build HTML Travel Guide" node → JavaScript Code
**Replace**: `[YOUR_MAPBOX_ACCESS_TOKEN]` with your actual token

#### Update Airtable IDs
**Location**: All Airtable nodes
**Replace**:
- `[YOUR_AIRTABLE_BASE_ID]` with your Base ID
- `[YOUR_AIRTABLE_TABLE_ID]` with your Table ID

### Step 6: Configure Webhooks
After saving the workflow, n8n will generate new Webhook URLs.
Copy these URLs to:

**"Build HTML Travel Guide" node** → JavaScript Code:
- Line with `approveUrl` 
- Line with `rejectUrl`

**"Prepare Improvement Request" node** → JavaScript Code:
- Line with `Approve button:`
- Line with `Feedback form POST:`

### Step 7: Activate Workflow
1. **Save** the workflow (Ctrl+S)
2. **Activate**: Toggle to "Active"
3. **Verify**: Status should show "Active"

---

## 🧪 Testing the System

### Test 1: Basic Guide Generation
1. **Add new record in Airtable**:
   - City: "Paris"
   - Email: [your email]
2. **Wait**: 1-2 minutes
3. **Expected**: Email with complete city guide (images + map + attractions)
4. **Verify**: HTML_Content field populated in Airtable

### Test 2: Improvement System
1. **In received email**: Click "Submit & Improve"
2. **Enter feedback**: "Change colors to blue and green"
3. **Submit** the form
4. **Expected**: New email with improvements
5. **Verify**: HTML_Content updated in Airtable

### Test 3: Approval
1. **In email**: Click "Approve & Save"
2. **Expected**: Simple confirmation page (no errors)

---

## 🔧 Troubleshooting

### Emails Not Sending
- **Check**: Gmail OAuth permissions approved
- **Check**: Email address in Airtable is valid

### Maps Not Loading
- **Check**: MapBox token is valid and active
- **Check**: Haven't exceeded monthly quota

### Images Not Loading
- **Check**: Unsplash key is valid
- **Check**: Haven't exceeded 50 requests/hour

### Trigger Not Working
- **Check**: "Last Modified" field exists in Airtable
- **Check**: Workflow is marked as "Active"

### AI Not Responding
- **Check**: Groq API key is valid
- **Check**: Haven't exceeded token limits

---

## 📊 System Overview

- **15 nodes** total workflow
- **5 external APIs** integrated
- **3 separate flows**: creation, approval, improvement
- **Response time**: 15-30 seconds for complete guide
- **Dual HTML storage**: saved both after creation and after improvement

---

## 🚀 What You Get

After successful setup, users can:
1. Enter any city name + email in Airtable
2. Receive a beautiful HTML email with:
   - 3 AI-curated attractions with descriptions
   - 3 professional city photos
   - Interactive map with numbered pinpoints
   - Approval/improvement options
3. Request changes using natural language
4. Get improved versions based on feedback

**The system runs completely automatically once activated!**