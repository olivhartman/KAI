# WORKFLOW 4: Long-Form Video Generator - Architecture Plan

## OVERVIEW
Input:  YouTube Long-Form Tracker rows with Status = DRAFT
Output: KAI reaction video uploaded to YouTube + posted to Telegram

## TRIGGER
- Schedule: 9:00 AM SGT daily (2 hours after Workflow 2)
- OR: Manual trigger when DRAFT videos are ready

---

## FULL FLOW

```
⏰ 9 AM SGT Trigger
    ↓
📖 Read YouTube Long-Form Tracker
    │ Filter: Status = DRAFT
    │ Limit: 1 per run (video generation is slow/expensive)
    ↓
📖 Get Source Data from Master Control
    │ Match: TopicTitle → TopicEvent
    │ Get: SourceURLs, KeyPoints, TeamDriver, TrendingScore
    ↓
🖼️ Extract Article Images (Jina.ai)
    │ Input:  First URL from SourceURLs
    │ Output: og:image URL (main article photo)
    │ Use as: Reference image for KIE.ai video generation
    ↓
📝 Generate Video Script (Perplexity/Claude)
    │ Input:  Topic + KeyPoints + Teams
    │ Output: KAI reaction script (30-90 seconds)
    │ Format: Hook (5s) + Reaction (60s) + CTA (5s)
    ↓
🎬 Generate KAI Video (KIE.ai - Sora2 or similar)
    │ Input:  Script + Article image as reference
    │ Output: taskId (async)
    ↓
⏳ Wait + Poll (same pattern as Workflow 2)
    │ Wait 60s → Poll → Check state → retry up to 10x
    ↓
🎵 Merge Video + Audio (need to decide: n8n or external)
    │ Option A: FFmpeg via n8n Execute Command node
    │ Option B: Upload raw clips to a video merge API
    │ Steps:
    │   1. Download KAI video clip
    │   2. Prepend intro.mp4
    │   3. Append outro.mp4
    │   4. Add background music (loop music track)
    ↓
☁️ Upload Final Video to R2
    │ Store: /videos/YYYYMMDD_topicSlug.mp4
    │ Get:   Public CDN URL
    ↓
📤 Upload to YouTube (via Blotato or YouTube API)
    │ Title: SEO-optimized from topic
    │ Description: KeyPoints + hashtags + links
    │ Tags: TeamDriver tags + F1 tags
    │ Thumbnail: Article image (og:image)
    ↓
📱 Post to Telegram Channel
    │ Message: Topic + YouTube link + hashtags
    ↓
📊 Update YouTube Long-Form Tracker
    │ Status: PUBLISHED
    │ YouTubeURL: live link
    │ MergedVideoURL: R2 CDN link
    │ ThumbnailURL: og:image URL
    ↓
📊 Update Master Control
    │ Status: VIDEO_POSTED
```

---

## KEY DECISIONS TO MAKE

### 1. Video Generation Model
KIE.ai has several video models. Options:
- **Sora2**: Best quality, slower (config already references this)
- **Kling**: Fast, good quality
- **Wan**: Good for talking head / reaction style

Recommendation: **Sora2** for quality, matches your config file

### 2. Video Merging
The intro.mp4 + KAI clip + outro.mp4 + music merge:

Option A: **FFmpeg in n8n** (Execute Command node)
- Pros: Free, full control, runs on your server
- Cons: Requires FFmpeg installed on n8n server
- Command: `ffmpeg -i intro.mp4 -i kai_clip.mp4 -i outro.mp4 -filter_complex concat -i music.mp3 output.mp4`

Option B: **Creatomate API** (cloud video merge)
- Pros: No server setup, handles complex compositions
- Cons: Costs ~$0.10-0.50 per video

Option C: **Upload raw + use YouTube's editor**
- Pros: Zero code
- Cons: Manual step, defeats automation

**Recommended: FFmpeg** if it's installed on your n8n server
**Check with:** `which ffmpeg` in your server terminal

### 3. YouTube Upload
Blotato supports YouTube posting but typically for Shorts/Community.
For long-form video upload, need to decide:
- Blotato (if it supports video upload)
- YouTube Data API v3 directly (need OAuth2 setup)
- n8n YouTube node (built-in, needs OAuth2)

### 4. Script Generation
Two options for the KAI reaction script:
- **Perplexity**: Already configured, can generate script + check for latest news
- **Claude API**: Better at creative writing, better script quality

Recommendation: **Claude API** for script (creative/personality driven)
                **Perplexity** already handled research in Workflow 1

---

## SCRIPT TEMPLATE (KAI Reaction Style)

```
[HOOK - 5 seconds]
"Guys, [SHOCKING_ELEMENT] just happened in F1 and we need to talk about it."

[CONTEXT - 15 seconds]  
"So here's what went down. [KEY_POINT_1]. [KEY_POINT_2]."

[KAI REACTION - 30 seconds]
"And my take on this? [OPINION]. What this means for [TEAM/DRIVER] is [IMPLICATION]."

[ANALYSIS - 20 seconds]
"Looking at this from a [technical/strategic/drama] angle - [DEEPER_INSIGHT]."

[CTA - 10 seconds]
"Drop your thoughts below. Do you think [DISCUSSION_QUESTION]?
Follow boxbox.wtf for more F1 takes. Link in bio."
```

---

## IMAGE EXTRACTION NODE (Jina.ai)

```
GET https://r.jina.ai/{source_url}
Headers: { "Accept": "application/json" }

Response includes:
{
  "data": {
    "title": "Article title",
    "content": "Full article text...",
    "images": [
      { "url": "https://cdn.skysports.com/...", "alt": "George Russell..." },
      ...
    ]
  }
}

Extract: data.images[0].url  ← main article photo
```

---

## COST PER VIDEO

| Service | Cost |
|---------|------|
| KIE.ai Sora2 video | ~$0.50-2.00 per clip |
| Perplexity (already done in W1) | $0 |
| Claude script generation | ~$0.01 |
| Creatomate merge (if used) | ~$0.20 |
| YouTube upload | Free |
| **Total per video** | **~$0.70-2.20** |

Monthly (30 videos): ~$21-66/month

---

## PENDING FROM YOU

1. ✅ intro.mp4 generated (needs music added)
2. ✅ outro.mp4 generated (needs music added)
3. ❓ Is FFmpeg available on your n8n server?
4. ❓ Which KIE.ai video model do you want? (Sora2 / Kling / Wan)
5. ❓ Do you have a background music track ready for R2?
6. ❓ YouTube API or Blotato for upload?

---

## WHAT WE BUILD NEXT

1. **Article Image Extractor** - standalone sub-flow, reusable in W2 and W4
2. **Script Generator** - Claude API call with KAI personality prompt
3. **KIE.ai Video Generator** - same async pattern as W2
4. **Video Merger** - FFmpeg or Creatomate
5. **YouTube Uploader** - Blotato or YouTube API
6. **Tracker Updates** - Single Image Posts + Master Control
