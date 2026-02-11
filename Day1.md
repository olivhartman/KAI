# WORKFLOW 1: DAILY F1 RESEARCH & TOPIC DISCOVERY
## Step-by-Step Setup Guide

---

## 📋 Overview

This workflow is the **foundation** of KAI's content automation system. It runs every morning at 6 AM Singapore time (GMT+8) and:

1. ✅ Queries Perplexity AI for the latest F1 news
2. ✅ Scores each topic from 1-10 based on trending potential
3. ✅ Writes all findings to Master Control Google Sheet
4. ✅ Auto-creates video drafts for high-priority topics (score 8-10)
5. ✅ Sends Telegram notification when breaking news is found

**Execution Time:** ~30 seconds  
**Daily Runs:** 1 time (6:00 AM SGT)  
**API Calls:** Perplexity (1), Google Sheets (2-6), Telegram (0-5)

---

## 🔧 SETUP STEPS

### Step 1: Import Workflow into n8n

1. Open your n8n instance
2. Click **"Workflows"** → **"Add Workflow"** → **"Import from File"**
3. Upload `workflow_1_daily_f1_research.json`
4. Workflow will appear with name: **"KAI - Daily F1 Research & Topic Discovery"**

---

### Step 2: Configure Perplexity API Credential

#### A. Get Perplexity API Key
1. Go to https://www.perplexity.ai/settings/api
2. Generate new API key
3. Copy the key (starts with `pplx-...`)

#### B. Add Credential in n8n
1. In n8n, go to **Credentials** → **Add Credential**
2. Search for **"Perplexity API"**
   - If not available, use **"Header Auth"** instead:
     - Header Name: `Authorization`
     - Header Value: `Bearer YOUR_PERPLEXITY_API_KEY`
3. Save credential as: `Perplexity API - KAI`

#### C. Link to Workflow Node
1. Open the workflow
2. Click on **"Perplexity F1 Research"** node
3. Under **"Authentication"**, select your Perplexity credential
4. Click **"Save"**

---

### Step 3: Configure Google Sheets OAuth2

#### A. Get Sheet IDs
1. Open **1_Master_Control_F1_Research** in Google Sheets (that you uploaded earlier)
2. Copy the Sheet ID from URL:
   ```
   https://docs.google.com/spreadsheets/d/{SHEET_ID}/edit
   ```
   Example: `1a2b3c4d5e6f7g8h9i0j`

3. Repeat for **2_YouTube_LongForm_Tracker** sheet

#### B. Update Workflow Nodes
1. Click on **"Write to Master Control"** node
2. In **"Document ID"** field, replace:
   ```
   PASTE_YOUR_MASTER_CONTROL_SHEET_ID_HERE
   ```
   with your actual Sheet ID

3. Click on **"Create Video Draft"** node
4. In **"Document ID"** field, replace:
   ```
   PASTE_YOUR_LONGFORM_TRACKER_SHEET_ID_HERE
   ```
   with your Long-Form tracker Sheet ID

5. Click **"Save"** after each change

---

### Step 4: Configure Telegram Bot (Optional but Recommended)

This sends you notifications when breaking F1 news is found.

#### A. Create Telegram Bot
1. Open Telegram, search for **@BotFather**
2. Send: `/newbot`
3. Follow prompts to create bot named: **KAI Admin Alerts**
4. Copy the **bot token** (looks like: `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`)

#### B. Get Your Chat ID
1. Search for your new bot in Telegram
2. Send it any message: `/start`
3. Visit: `https://api.telegram.org/bot{YOUR_BOT_TOKEN}/getUpdates`
   - Replace `{YOUR_BOT_TOKEN}` with actual token
4. Find `"chat":{"id":123456789}` in the JSON response
5. Copy that number (your Chat ID)

#### C. Configure in n8n
1. In n8n workflow, click **"Notify Admin (Telegram)"** node
2. In **"Chat ID"** field, replace:
   ```
   YOUR_TELEGRAM_ADMIN_CHAT_ID
   ```
   with your actual Chat ID (just the number)
3. Select your Telegram Bot credential
4. Click **"Save"**

**Alternative:** If you don't want Telegram notifications, simply **delete** the "Notify Admin (Telegram)" node and its connection.

---

### Step 5: Verify Timezone (Already Set to GMT+8)

The cron expression is already configured for 6 AM Singapore time:

1. Click **"Daily 6 AM SGT"** (Schedule Trigger) node
2. Verify cron expression shows: `0 6 * * *`
3. This means: **Every day at 6:00 AM**
4. n8n will execute in **server time**, so ensure your n8n timezone is set to `Asia/Singapore`:
   - In n8n settings → **Settings** → **Timezone**
   - Select: `Asia/Singapore (GMT+8)`

---

### Step 6: Test the Workflow Manually

Before scheduling, test manually to ensure everything works:

#### A. Manual Test Execution
1. Click **"Execute Workflow"** button (top right)
2. Watch each node execute in sequence:
   - ✅ Trigger fires
   - ✅ Perplexity queries F1 news
   - ✅ Data gets parsed
   - ✅ Rows added to Google Sheets
   - ✅ High-priority topics filtered
   - ✅ Video drafts created (if score 8-10)
   - ✅ Telegram notification sent (if enabled)

#### B. Verify Results
1. Open **1_Master_Control_F1_Research** Google Sheet
2. Check for new rows with today's date
3. Verify columns are filled:
   - Date, Research Time, Status, Topic, Trending Score, Source URLs, Key Points, etc.

4. If any topic scored 8-10:
   - Open **2_YouTube_LongForm_Tracker** sheet
   - Verify new row created with Status = `DRAFT`
   - Check Telegram for notification

#### C. Troubleshooting
If execution fails, check:
- **Perplexity node:** Valid API key? Check credential
- **Google Sheets node:** Correct Sheet ID? Shared with n8n service account?
- **Parse node:** Click to see parsed JSON output
- **Telegram node:** Correct Bot token and Chat ID?

---

### Step 7: Activate the Workflow

Once manual test succeeds:

1. Toggle **"Active"** switch (top right) → **ON**
2. Workflow will now run automatically at 6 AM daily
3. You'll see **"Active"** badge appear

---

## 📊 HOW IT WORKS - NODE BREAKDOWN

### Node 1: Daily 6 AM SGT (Schedule Trigger)
- **Type:** Cron trigger
- **Schedule:** `0 6 * * *` = Every day at 6:00 AM
- **Purpose:** Starts the workflow automatically

### Node 2: Perplexity F1 Research (HTTP Request)
- **API Endpoint:** `https://api.perplexity.ai/chat/completions`
- **Model:** `llama-3.1-sonar-large-128k-online` (with web search)
- **Prompt:** Asks for top 5 trending F1 stories with:
  - Catchy headlines
  - Trending scores (1-10)
  - Key points summary
  - Teams/drivers involved
  - Content ideas
- **Output Format:** JSON array of topics
- **Temperature:** 0.2 (more factual, less creative)

**Why Perplexity?**
- Real-time web search capability
- Can access latest news (within last 24 hours)
- Better than Deepseek/Anthropic for current events
- Returns source citations

### Node 3: Parse Research Data (Code)
- **Purpose:** Converts Perplexity response to Google Sheets format
- **Logic:**
  1. Extracts JSON from Perplexity's response
  2. Handles markdown code blocks (```json)
  3. Adds current date/time (Singapore timezone)
  4. Calculates status: `HIGH_PRIORITY` if score ≥8, else `COMPLETED`
  5. Auto-assigns workflow: "Long-form + Short" for high priority
  6. Sets priority: HIGH (8-10), MEDIUM (6-7), LOW (1-5)
- **Output:** Array of structured objects ready for Sheets

### Node 4: Write to Master Control (Google Sheets Append)
- **Operation:** Append rows (adds to bottom of sheet)
- **Mapping:** Each field mapped to specific column
- **Columns written:**
  - A: Date (YYYY-MM-DD)
  - B: Research Time (HH:MM in SGT)
  - C: Status (HIGH_PRIORITY or COMPLETED)
  - D: Topic/Event
  - E: Trending Score (1-10)
  - F: Source URLs (from Perplexity citations)
  - G: Key Points
  - H: Content Ideas Generated
  - I: Assigned to Workflow
  - J: Priority (HIGH/MEDIUM/LOW)
  - K: Team/Driver Mentioned
  - L: Notes (empty, for manual entry)
  - M: Perplexity Query

### Node 5: Filter High Priority (8-10) (IF Node)
- **Condition:** `status = HIGH_PRIORITY`
- **Purpose:** Only continue workflow for topics scoring 8-10
- **True path:** Goes to "Create Video Draft" + "Notify Admin"
- **False path:** Workflow ends (low priority topics just logged)

### Node 6: Create Video Draft (Google Sheets Append)
- **Triggered when:** Topic scores 8-10
- **Sheet:** YouTube Long-Form Tracker
- **Creates row with:**
  - Video ID: Auto-generated (YT20260211_0, YT20260211_1, etc.)
  - Publish Date: Today's date
  - Status: `DRAFT` (ready for next workflow to process)
  - Topic: Copied from research
  - Video Type: "Reaction" (default)
  - Intro/Outro URLs: Pre-filled with fixed R2 URLs
  - CTA URL: `https://kai.bio/subscribe`
  - All other fields: Empty (filled by subsequent workflows)

### Node 7: Notify Admin (Telegram)
- **Triggered when:** High priority topic found
- **Message format:**
  ```
  🔔 KAI Research Alert
  📅 2026-02-11
  🔥 High Priority Topic Found!
  
  Topic: Verstappen Crashes in FP1
  Trending Score: 9/10
  Teams/Drivers: Red Bull, Verstappen
  
  Key Points:
  [Perplexity summary...]
  
  Content Ideas:
  Reaction video, infographic
  
  ✅ Draft video created automatically
  🎬 Ready for script generation workflow
  ```
- **Purpose:** Alerts you to review high-priority topics

---

## 🔄 DAILY EXECUTION FLOW

**6:00 AM SGT:**
```
[Cron Trigger Fires]
    ↓
[Perplexity API Call]
    ↓
[Parse 5 Topics]
    ↓
[Write All to Master Control Sheet]
    ↓
[Filter: Are any scored 8-10?]
    ↓
YES → [Create Video Draft in Long-Form Tracker]
    ↓
      [Send Telegram Alert]
    
NO → [Workflow Complete - Topics logged only]
```

---

## 📈 EXPECTED RESULTS

### What you'll see in Master Control Sheet (typical day):
- **5 new rows** added at 6:00 AM
- **Status distribution:**
  - 0-2 topics: HIGH_PRIORITY (8-10)
  - 2-3 topics: COMPLETED (6-7)
  - 1-2 topics: COMPLETED (3-5)

### What triggers next workflows:
- **HIGH_PRIORITY topics** → Auto-create video drafts
- **Next workflow** (Script Generation) will process these DRAFT videos
- **COMPLETED topics** with scores 6-7 → Good for Single Image or Multi-Image posts

---

## 🛠️ CUSTOMIZATION OPTIONS

### Want more/fewer topics?
In **Perplexity F1 Research** node, change prompt:
```
"What are the top 5 most trending..."
```
to:
```
"What are the top 10 most trending..." (for more topics)
"What are the top 3 most trending..." (for fewer topics)
```

### Want different trigger times?
In **Daily 6 AM SGT** node, change cron:
- `0 8 * * *` = 8 AM
- `0 6,18 * * *` = 6 AM and 6 PM (twice daily)
- `0 */4 * * *` = Every 4 hours

### Want to adjust priority thresholds?
In **Parse Research Data** code node, find:
```javascript
status: topic.trending_score >= 8 ? 'HIGH_PRIORITY' : 'COMPLETED',
```
Change `>= 8` to `>= 9` (more selective) or `>= 7` (less selective)

---

## 📞 SUPPORT & TROUBLESHOOTING

### Common Issues:

**1. "Perplexity API returns 401 Unauthorized"**
- Solution: Check API key in credential, ensure it starts with `pplx-`

**2. "Google Sheets: Document not found"**
- Solution: Verify Sheet ID is correct, check if sheet is shared with n8n service account

**3. "No topics returned"**
- Solution: Check Perplexity node execution data, may need to adjust prompt

**4. "Telegram notification not sent"**
- Solution: Verify bot token and chat ID, send /start to bot first

**5. "Workflow doesn't trigger at 6 AM"**
- Solution: Check n8n timezone setting (should be Asia/Singapore)

---

## ✅ NEXT STEPS

Once this workflow runs successfully for 2-3 days:

1. **Review Master Control sheet** to see trending score patterns
2. **Adjust threshold** if needed (currently 8-10 triggers videos)
3. **Move to Workflow 2:** Single Image Generator (uses COMPLETED topics)
4. **Move to Workflow 3:** Script Generation (processes DRAFT videos)

---

## 📊 MONITORING & METRICS

### Daily checks (first week):
- [ ] Did workflow execute at 6 AM? (Check n8n execution history)
- [ ] Are 5 topics added to Master Control sheet?
- [ ] Do trending scores seem accurate? (8-10 = truly breaking news?)
- [ ] Did high-priority topics create video drafts?
- [ ] Did Telegram alerts arrive?

### Weekly review:
- [ ] Average trending scores: Should be 5-7 range
- [ ] High-priority topics per week: Target 2-4
- [ ] Source URL quality: Are they reputable F1 sites?
- [ ] Team/driver distribution: Balanced across teams?

---

Ready to proceed? Let me know:
1. ✅ if you've successfully imported and tested this workflow
2. 🆘 if you encounter any issues during setup
3. ➡️ if you're ready to build Workflow 2 (Single Image Generator)
