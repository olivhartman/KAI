# MANUAL MAPPING GUIDE: Create Video Draft Node

## 📋 Complete Column Mapping for YouTube Long-Form Tracker

Copy these exact expressions into your "Create Video Draft" node in n8n.

---

## ✅ ALL 20 COLUMN MAPPINGS

Click **"Add Column"** in the node, then add these one by one:

### 1. VideoID
```
Left field:  VideoID
Right field: ={{ 'YT' + $now.format('yyyyMMdd') + '_' + $itemIndex }}
```
**Generates:** YT20260211_0, YT20260211_1, YT20260211_2, etc.

---

### 2. PublishDate
```
Left field:  PublishDate
Right field: ={{ $json.Date }}
```
**Gets:** Date from Master Control sheet

---

### 3. Status
```
Left field:  Status
Right field: DRAFT
```
**Sets:** DRAFT (plain text, no expression needed)

---

### 4. TopicTitle
```
Left field:  TopicTitle
Right field: ={{ $json.TopicEvent }}
```
**Gets:** Topic from Master Control sheet

---

### 5. VideoType
```
Left field:  VideoType
Right field: Reaction
```
**Sets:** Reaction (plain text)

---

### 6. WardrobeSelected
```
Left field:  WardrobeSelected
Right field: (leave empty)
```
**Note:** This will be filled by Workflow 4 when generating video

---

### 7. EmotionMood
```
Left field:  EmotionMood
Right field: (leave empty)
```
**Note:** This will be filled by Workflow 4 when selecting character

---

### 8. ScriptGenerated
```
Left field:  ScriptGenerated
Right field: (leave empty)
```
**Note:** This will be filled by Workflow 4 when script is created

---

### 9. IntroClipURL
```
Left field:  IntroClipURL
Right field: https://r2.kai.ai/templates/intro-5s.mp4
```
**Sets:** Standard intro clip URL (from config)

---

### 10. MainContentURLs
```
Left field:  MainContentURLs
Right field: (leave empty)
```
**Note:** This will be filled by Workflow 4 with generated video URLs

---

### 11. OutroClipURL
```
Left field:  OutroClipURL
Right field: https://r2.kai.ai/templates/outro-5s.mp4
```
**Sets:** Standard outro clip URL (from config)

---

### 12. MergedVideoURL
```
Left field:  MergedVideoURL
Right field: (leave empty)
```
**Note:** This will be filled by Workflow 4 after video merging

---

### 13. ThumbnailURL
```
Left field:  ThumbnailURL
Right field: (leave empty)
```
**Note:** This will be filled by Workflow 4 when thumbnail is generated

---

### 14. SEOTitle
```
Left field:  SEOTitle
Right field: (leave empty)
```
**Note:** This will be filled by Workflow 4 based on topic

---

### 15. Description
```
Left field:  Description
Right field: (leave empty)
```
**Note:** This will be filled by Workflow 4 with full video description

---

### 16. Tags
```
Left field:  Tags
Right field: ={{ '#F1 #Formula1 #' + $json.TeamDriver.split(',')[0].trim().replace(/\s+/g, '') }}
```
**Generates:** #F1 #Formula1 #Mercedes (from first team mentioned)

---

### 17. YouTubeURL
```
Left field:  YouTubeURL
Right field: (leave empty)
```
**Note:** This will be filled after video is uploaded to YouTube

---

### 18. CommentPosted
```
Left field:  CommentPosted
Right field: NO
```
**Sets:** NO (will change to YES after pinned comment is posted)

---

### 19. CTAURL
```
Left field:  CTAURL
Right field: https://kai.bio/subscribe
```
**Sets:** Standard CTA link

---

### 20. ErrorLog
```
Left field:  ErrorLog
Right field: (leave empty)
```
**Note:** Will be filled if any errors occur during processing

---

## 🎯 QUICK REFERENCE TABLE

| # | Column Name | Value Type | Expression |
|---|-------------|------------|------------|
| 1 | VideoID | Generated | `={{ 'YT' + $now.format('yyyyMMdd') + '_' + $itemIndex }}` |
| 2 | PublishDate | From input | `={{ $json.Date }}` |
| 3 | Status | Fixed | `DRAFT` |
| 4 | TopicTitle | From input | `={{ $json.TopicEvent }}` |
| 5 | VideoType | Fixed | `Reaction` |
| 6 | WardrobeSelected | Empty | (leave empty) |
| 7 | EmotionMood | Empty | (leave empty) |
| 8 | ScriptGenerated | Empty | (leave empty) |
| 9 | IntroClipURL | Fixed | `https://r2.kai.ai/templates/intro-5s.mp4` |
| 10 | MainContentURLs | Empty | (leave empty) |
| 11 | OutroClipURL | Fixed | `https://r2.kai.ai/templates/outro-5s.mp4` |
| 12 | MergedVideoURL | Empty | (leave empty) |
| 13 | ThumbnailURL | Empty | (leave empty) |
| 14 | SEOTitle | Empty | (leave empty) |
| 15 | Description | Empty | (leave empty) |
| 16 | Tags | Generated | `={{ '#F1 #Formula1 #' + $json.TeamDriver.split(',')[0].trim().replace(/\s+/g, '') }}` |
| 17 | YouTubeURL | Empty | (leave empty) |
| 18 | CommentPosted | Fixed | `NO` |
| 19 | CTAURL | Fixed | `https://kai.bio/subscribe` |
| 20 | ErrorLog | Empty | (leave empty) |

---

## 📝 COPY-PASTE READY SUMMARY

For quick reference when adding columns in n8n:

**Columns that need expressions (8 total):**
1. VideoID: `={{ 'YT' + $now.format('yyyyMMdd') + '_' + $itemIndex }}`
2. PublishDate: `={{ $json.Date }}`
3. TopicTitle: `={{ $json.TopicEvent }}`
4. Tags: `={{ '#F1 #Formula1 #' + $json.TeamDriver.split(',')[0].trim().replace(/\s+/g, '') }}`

**Columns with fixed values (5 total):**
- Status: `DRAFT`
- VideoType: `Reaction`
- IntroClipURL: `https://r2.kai.ai/templates/intro-5s.mp4`
- OutroClipURL: `https://r2.kai.ai/templates/outro-5s.mp4`
- CommentPosted: `NO`
- CTAURL: `https://kai.bio/subscribe`

**Columns that stay empty (7 total):**
- WardrobeSelected
- EmotionMood
- ScriptGenerated
- MainContentURLs
- MergedVideoURL
- ThumbnailURL
- SEOTitle
- Description
- YouTubeURL
- ErrorLog

---

## 🔧 CONFIGURATION STEPS IN n8n

1. **Open "Create Video Draft" node**
2. **Document ID:** `182x6TaC-CRZICpglT2AV5njY6d5tAYdp2TZqQMcZ41E`
3. **Sheet Name:** `YouTube Long-Form` (mode: name)
4. **Columns → Mapping Mode:** "Map Each Column Manually"
5. **Click "Add Column"** 20 times and fill in each mapping
6. **Click "Save"**

---

## ✅ EXPECTED RESULT

After running the workflow with your 3 high-priority topics:

**YouTube Long-Form Tracker - Row 2:**
```
VideoID: YT20260211_0
PublishDate: 2026-02-11
Status: DRAFT
TopicTitle: George Russell Tops...
VideoType: Reaction
WardrobeSelected: (empty)
EmotionMood: (empty)
ScriptGenerated: (empty)
IntroClipURL: https://r2.kai.ai/templates/intro-5s.mp4
MainContentURLs: (empty)
OutroClipURL: https://r2.kai.ai/templates/outro-5s.mp4
MergedVideoURL: (empty)
ThumbnailURL: (empty)
SEOTitle: (empty)
Description: (empty)
Tags: #F1 #Formula1 #Mercedes
YouTubeURL: (empty)
CommentPosted: NO
CTAURL: https://kai.bio/subscribe
ErrorLog: (empty)
```

**Rows 3 and 4:** Similar structure for the other 2 high-priority topics

---

## 🚀 TESTING WORKFLOW

### Step 1: Configure the Node (10 minutes)
Add all 20 column mappings as shown above

### Step 2: Test Execution
1. Click on **"Filter High Priority"** node
2. Click **"Execute Node"**
3. Should show 3 items (scores 10, 9, 8)
4. Click on **"Create Video Draft"** node  
5. Click **"Execute Node"**
6. Should see 3 successful writes

### Step 3: Verify in Google Sheet
Open your YouTube Long-Form Tracker:
- ✅ Row 2: George Russell topic
- ✅ Row 3: Consensus Builds topic
- ✅ Row 4: Mercedes Engine topic
- ✅ All with Status = DRAFT
- ✅ All with proper VideoIDs

---

## 💡 TIPS

### For Empty Fields:
Just skip adding them! n8n will leave them empty automatically.  
OR add them with empty value (right field blank).

### For Testing:
Start with just the 4 required fields to test:
1. VideoID
2. PublishDate  
3. Status
4. TopicTitle

If those work, add the rest!

### Column Order:
Doesn't matter! n8n matches by column name, not position.

---

## 🎯 AFTER SUCCESSFUL TEST

Once all 3 draft rows appear in your YouTube Long-Form Tracker:

**✅ Workflow 1 is 100% complete!**

Next steps:
1. Activate daily schedule
2. Build Workflow 4 (Long-Form Video Generator) to process these DRAFT videos
3. Build remaining workflows (2, 3, 5, 6)

---

Ready to start mapping? Let me know if you have any questions! 🚀
