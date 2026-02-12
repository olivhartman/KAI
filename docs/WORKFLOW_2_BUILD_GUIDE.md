# WORKFLOW 2: Single Image Post Generator - Complete Build Guide

## 🎯 OVERVIEW

**Purpose:** Automatically generate and post F1 infographic images to multiple platforms from medium-priority research topics.

**Trigger:** Daily 7:00 AM SGT (1 hour after Workflow 1)

**Input:** COMPLETED topics from Master Control (scores 6-7)

**Output:** Image posts on Twitter, TikTok, YouTube Community, Telegram Channel

**Platforms:** 4 platforms via Blotato + Telegram API

---

## 📊 WORKFLOW LOGIC

```
⏰ 7:00 AM SGT - Daily Trigger
     │
     ▼
📖 Read Master Control Sheet
     │ Get ALL rows
     ▼
🔍 Filter Today's COMPLETED Topics
     │ Status = "COMPLETED"
     │ Date = Today
     │ Result: 0-3 topics (typically 2)
     ▼
📝 Prepare Image Prompt
     │ Extract: topic, teams, key points
     │ Generate: KIE.ai prompt
     │ Generate: Caption & hashtags
     ▼
🎨 Generate Image (KIE.ai)
     │ API: https://api.kie.ai/v1/generate
     │ Size: 1080x1080px
     │ Style: F1 infographic
     │ Output: Image URL
     ▼
┌──────────────┬─────────────────┐
│              │                 │
▼              ▼                 │
📤 Blotato    📱 Telegram     │
Post          Channel Post     │
│              │                 │
Twitter       Direct API       │
TikTok                          │
YouTube                         │
│              │                 │
▼              ▼                 │
📊 Parse Results & Update Tracker
     │ Extract post URLs
     │ Mark as POSTED
     │ Record in Single Image Posts sheet
     ▼
✅ COMPLETE!
```

---

## 🛠️ SETUP INSTRUCTIONS

### Step 1: Upload Clean Single Image Posts Sheet

1. **Download:** `5_Single_Image_Posts_CLEAN.xlsx`
2. **Upload to Google Drive** → Your KAI folder
3. **Right-click → Open with Google Sheets**
4. **Save as Google Sheets** format
5. **Share** with n8n service account (Editor access)
6. **Copy Sheet ID** from URL
7. **Update Master Control if needed** - delete old Single Image Posts and use this new one

**Sheet ID Location in URL:**
```
https://docs.google.com/spreadsheets/d/[SHEET_ID]/edit
```

Current ID in workflow: `17SiArmBA_fSBn30Q2urJtKkZTyRgQYSNA1jXOt6a5gY`

---

### Step 2: Verify API Credentials in n8n

#### KIE.ai Configuration:
- **API Key:** `982817ed1ca2d1eaab7301e47995b933`
- **Method:** Bearer token in Authorization header
- **Already configured in workflow JSON**

#### Blotato Configuration:
- **API Key:** `blt_Z9W4FrPVz+uL5Gz8KDyVBZhZhkncTX1avrsI+2uZcgo=`
- **Method:** Bearer token in Authorization header
- **Already configured in workflow JSON**
- **Connected Platforms:** Twitter, TikTok, YouTube (Instagram not yet)

#### Telegram Configuration:
- **Bot:** boxboxagent_bot (already configured)
- **Channel ID:** `-1003246455176` (different from personal chat)
- **Already configured in workflow JSON**

---

### Step 3: Import Workflow

1. **Open n8n**
2. **Click "+" → Import from File**
3. **Select:** `workflow_2_single_image.json`
4. **Click Import**

The workflow will import with all:
- ✅ API keys pre-configured
- ✅ Sheet IDs set
- ✅ Telegram channel ID set
- ✅ Schedule set to 7:00 AM SGT

---

### Step 4: Verify Node Configuration

Check these nodes:

#### 1. "Read Master Control" Node:
- Document ID: `1bmBgdCDNcefQhaEpuUmS5TeNMEUnoW2H--yIrynYzYQ`
- Sheet Name: `F1 Research & Control`
- Operation: Read

#### 2. "Generate Image (KIE.ai)" Node:
- URL: `https://api.kie.ai/v1/generate`
- Authorization: `Bearer 982817ed1ca2d1eaab7301e47995b933`
- Body includes: prompt, width (1080), height (1080), format (png)

**⚠️ IMPORTANT:** KIE.ai API structure may vary. You may need to adjust:
- The API endpoint URL
- The request body structure
- The response parsing in "Extract Image URL" node

#### 3. "Post to Platforms (Blotato)" Node:
- URL: `https://api.blotato.com/v1/post`
- Authorization: `Bearer blt_Z9W4FrPVz+uL5Gz8KDyVBZhZhkncTX1avrsI+2uZcgo=`
- Platforms: `["twitter", "tiktok", "youtube"]`

**⚠️ IMPORTANT:** Blotato API structure may vary. Verify:
- Correct API endpoint
- Correct request format
- Response structure for URL extraction

#### 4. "Post to Telegram Channel" Node:
- Chat ID: `-1003246455176`
- Photo URL: From previous node
- Caption: Includes topic + hashtags

#### 5. "Write to Single Image Tracker" Node:
- Document ID: `17SiArmBA_fSBn30Q2urJtKkZTyRgQYSNA1jXOt6a5gY`
- Sheet Name: `Single Image Posts`
- All 21 columns mapped

---

## 🧪 TESTING THE WORKFLOW

### Test Plan:

#### Phase 1: Test Without Posting (5 min)

1. **Disable posting nodes temporarily:**
   - Disable "Post to Platforms (Blotato)"
   - Disable "Post to Telegram Channel"
   - Disable "Write to Single Image Tracker"

2. **Run up to image generation:**
   - Click "Daily 7 AM SGT" → Execute Node
   - Click "Read Master Control" → Execute Node
   - Click "Filter Today's COMPLETED" → Should show 2 items (your COMPLETED topics)
   - Click "Prepare Image Prompt" → Verify prompt looks good
   - Click "Generate Image (KIE.ai)" → **CHECK IF THIS WORKS**

3. **Verify image generation:**
   - Does KIE.ai return an image URL?
   - Can you open the URL in a browser?
   - Is the image quality good?

#### Phase 2: Test Telegram Only (5 min)

1. **Enable only Telegram node**
2. **Run workflow through to Telegram**
3. **Check your Telegram Channel** → Image should appear
4. **Verify:**
   - Image displays correctly
   - Caption is readable
   - Hashtags are present

#### Phase 3: Test Blotato (10 min)

1. **Enable Blotato node**
2. **Run workflow**
3. **Check each platform:**
   - Twitter/X: Did post appear?
   - TikTok: Did post appear?
   - YouTube Community: Did post appear?
4. **Verify URLs:**
   - Does Blotato return post URLs?
   - Are they accessible?

#### Phase 4: Full Test (5 min)

1. **Enable all nodes**
2. **Run complete workflow**
3. **Check Single Image Posts sheet:**
   - New row added?
   - All URLs recorded?
   - Status = POSTED?

---

## ⚠️ KNOWN ISSUES & FIXES

### Issue 1: KIE.ai API Response Format

**Problem:** The "Extract Image URL" node assumes KIE.ai returns `{ "url": "..." }`

**If it returns something different:**

Update the "Extract Image URL" node code:

```javascript
// Check actual KIE.ai response structure
const response = $input.item.json;
console.log('KIE.ai response:', JSON.stringify(response, null, 2));

// Adjust based on actual structure
const imageUrl = response.url ||          // Try standard
                 response.image_url ||     // Try alternative
                 response.data?.url ||     // Try nested
                 response.data?.image ||   // Try nested alternative
                 '';

if (!imageUrl) {
  console.error('Full response:', response);
  throw new Error('No image URL found in KIE.ai response');
}

return {
  json: {
    ...$json,
    ImageURL: imageUrl,
    ImageGenerated: 'YES'
  }
};
```

---

### Issue 2: Blotato API Response Format

**Problem:** The "Parse Post Results" node assumes specific Blotato response structure

**If it returns something different:**

Update the "Parse Post Results" node code:

```javascript
// Check actual Blotato response structure
const blotato = $input.item.json;
console.log('Blotato response:', JSON.stringify(blotato, null, 2));

// Adjust based on actual structure
const posts = blotato.posts || blotato.data || [];

// Extract platform-specific posts
const twitterPost = posts.find(p => p.platform === 'twitter' || p.platform === 'x');
const tiktokPost = posts.find(p => p.platform === 'tiktok');
const youtubePost = posts.find(p => p.platform === 'youtube');

return {
  json: {
    ...$json,
    TwitterPosted: twitterPost ? 'YES' : 'NO',
    TwitterURL: twitterPost?.url || twitterPost?.link || '',
    TikTokPosted: tiktokPost ? 'YES' : 'NO',
    TikTokURL: tiktokPost?.url || tiktokPost?.link || '',
    YouTubePosted: youtubePost ? 'YES' : 'NO',
    YouTubeURL: youtubePost?.url || youtubePost?.link || '',
    TelegramPosted: 'YES',
    TelegramURL: 'Channel post',
    Status: 'POSTED'
  }
};
```

---

### Issue 3: No COMPLETED Topics Today

**Problem:** Filter returns 0 items if no COMPLETED topics exist for today

**Solution:** This is expected behavior. The workflow will:
- Run at 7 AM daily
- Check for COMPLETED topics
- If none exist, workflow ends gracefully
- No posts created (which is correct)

**To test with older data:**

Temporarily modify the filter:

```javascript
// TESTING ONLY - Remove date filter
const completedTopics = items.filter(item => {
  return item.json.Status === 'COMPLETED';
});

// This will process ALL COMPLETED topics, not just today's
```

**Remember to restore the date filter after testing!**

---

### Issue 4: Image Generation Fails

**If KIE.ai fails:**

1. Check API key is valid
2. Check API endpoint is correct
3. Check request format matches KIE.ai docs
4. Check API quota/limits

**Alternative image generation services:**

**Option A: DALL-E 3 (OpenAI)**
```json
{
  "url": "https://api.openai.com/v1/images/generations",
  "headers": {
    "Authorization": "Bearer YOUR_OPENAI_KEY"
  },
  "body": {
    "model": "dall-e-3",
    "prompt": "{{ $json.ImagePrompt }}",
    "size": "1024x1024",
    "quality": "hd"
  }
}
```

**Option B: Stable Diffusion**
```json
{
  "url": "https://api.stability.ai/v1/generation/stable-diffusion-xl-1024-v1-0/text-to-image",
  "headers": {
    "Authorization": "Bearer YOUR_STABILITY_KEY"
  },
  "body": {
    "text_prompts": [{"text": "{{ $json.ImagePrompt }}"}],
    "cfg_scale": 7,
    "height": 1024,
    "width": 1024,
    "samples": 1
  }
}
```

---

## 💰 COST ESTIMATE

### Per Execution (2 posts):
- KIE.ai: $0.02 per image × 2 = **$0.04**
- Blotato: Depends on plan (check your account)
- Google Sheets: Free (under quota)
- Telegram: Free
- n8n: Self-hosted (sunk cost)

### Monthly (30 days):
- Assume avg 2 COMPLETED topics/day
- 60 images/month
- KIE.ai: 60 × $0.02 = **$1.20/month**
- Blotato: Check your plan pricing

**Total: ~$1-5/month** depending on Blotato plan

---

## 📊 EXPECTED OUTPUT

### Daily Execution:

**Input:** 2 COMPLETED topics (scores 6-7)

**Processing:**
1. Read Master Control
2. Filter 2 COMPLETED topics
3. Generate 2 image prompts
4. Call KIE.ai 2 times → 2 images
5. Post each image to 4 platforms → 8 posts total
6. Record 2 rows in Single Image Posts sheet

**Result:**
- 2 new image posts
- 8 platform posts (2 × 4 platforms)
- 2 tracker entries

**Time:** ~30-60 seconds total

---

## 🎯 SUCCESS CRITERIA

After running workflow, verify:

✅ **Single Image Posts Sheet:**
- 2 new rows added
- ImageID generated (IMG20260211_0, IMG20260211_1)
- Status = POSTED
- ImageURL present and accessible
- Platform URLs recorded (Twitter, TikTok, YouTube, Telegram)

✅ **Social Media Platforms:**
- Twitter: 2 new posts with images
- TikTok: 2 new posts with images
- YouTube Community: 2 new posts with images
- Telegram Channel: 2 new posts with images

✅ **Image Quality:**
- 1080x1080px resolution
- F1-themed design
- Clear, readable text
- Team colors represented
- Professional appearance

---

## 📝 MAINTENANCE NOTES

### Regular Checks:

**Weekly:**
- Review generated images - quality OK?
- Check platform engagement - which performs best?
- Verify all posts going through

**Monthly:**
- Review KIE.ai costs
- Check Blotato usage
- Optimize image prompts if needed
- Update hashtag strategy

### Future Improvements:

1. **A/B Testing:** Try different image styles
2. **Timing:** Experiment with posting times
3. **Hashtags:** Optimize based on engagement
4. **Platforms:** Add Instagram when connected
5. **Analytics:** Pull engagement metrics automatically

---

## 🎊 READY TO BUILD!

**Next Steps:**

1. ✅ Upload clean Single Image Posts sheet
2. ✅ Import workflow JSON
3. ✅ Verify API configurations
4. ✅ Test phase by phase
5. ✅ Activate for daily 7 AM runs

---

## 📌 NOTE ON INTRO/OUTRO VIDEOS

**You mentioned:** "I have not generated the intro.mp4 and outro.mp4 files yet"

**No problem!** These are only needed for **Workflow 4 (Long-Form Video Generator)**.

For now:
- Workflow 2 (this one) doesn't need video files ✅
- Workflow 4 will need intro/outro videos
- We'll generate those when building Workflow 4

**Suggested approach for Workflow 4:**
1. Use a simple tool like Canva, Adobe Express, or CapCut
2. Create 5-second branded intro/outro
3. Upload to R2 bucket
4. Update config JSON with URLs

**Or we can automate it:**
- Use Sora2 or similar to generate video clips
- Or use static images with audio for simplicity

We'll tackle this when building Workflow 4! 🎬

---

**Let's build Workflow 2!** 🚀
