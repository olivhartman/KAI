# KAI Video Generation Config - URL Reference

## ✅ Your Config File Locations

### Primary URL (R2 - RECOMMENDED):
```
https://pub-b950cb87774b4e2eb3b2293cea84bafe.r2.dev/kai_video_generation_config.json
```
**Use this in all workflows** - faster, CDN-backed, no rate limits

### Backup URL (GitHub):
```
https://raw.githubusercontent.com/olivhartman/KAI/main/asset/kai_video_generation_config.json
```
**Use as fallback** - version controlled, public

---

## 🚀 How to Use in Future Workflows

### Example 1: Fetch Config in Code Node

```javascript
// At the start of any workflow that needs config
const config = await fetch('https://pub-b950cb87774b4e2eb3b2293cea84bafe.r2.dev/kai_video_generation_config.json')
  .then(response => response.json());

// Now you have access to everything:
const characterMoods = config.video_generation_config.character_references.moods;
const wardrobeOptions = config.video_generation_config.wardrobe_library.outfits;
const videoTemplates = config.video_generation_config.video_templates;

// Use it!
const excitedMood = characterMoods.find(m => m.name === 'excited');
console.log(excitedMood.filename); // "kai-excited.png"
```

---

### Example 2: Auto-Select Wardrobe Based on Team

```javascript
// Workflow 4 - Video Generator
const config = await fetch('https://pub-b950cb87774b4e2eb3b2293cea84bafe.r2.dev/kai_video_generation_config.json')
  .then(r => r.json());

const teamMentioned = $json.TeamDriver; // "Mercedes, George Russell"

// Find matching wardrobe
const wardrobe = config.video_generation_config.wardrobe_library.outfits.find(outfit => {
  if (teamMentioned.toLowerCase().includes('mercedes') && outfit.id === 'WARD004') return true;
  if (teamMentioned.toLowerCase().includes('ferrari') && outfit.id === 'WARD002') return true;
  if (teamMentioned.toLowerCase().includes('mclaren') && outfit.id === 'WARD003') return true;
  if (teamMentioned.toLowerCase().includes('red bull') && outfit.id === 'WARD001') return true;
  return false;
});

// Fallback to default
const finalWardrobe = wardrobe || config.video_generation_config.wardrobe_library.outfits.find(
  o => o.auto_select_rule === 'DEFAULT'
);

return {
  wardrobe_url: config.video_generation_config.wardrobe_library.base_url + finalWardrobe.filename,
  wardrobe_name: finalWardrobe.name
};
```

---

### Example 3: Select Character Mood Based on Topic

```javascript
const config = await fetch('https://pub-b950cb87774b4e2eb3b2293cea84bafe.r2.dev/kai_video_generation_config.json')
  .then(r => r.json());

const topic = $json.TopicEvent.toLowerCase();
const score = $json.TrendingScore;

// Determine appropriate mood
let moodName = 'neutral'; // default

if (topic.includes('crash') || topic.includes('incident')) {
  moodName = 'shocked';
} else if (topic.includes('win') || topic.includes('victory') || topic.includes('pole')) {
  moodName = 'excited';
} else if (topic.includes('penalty') || topic.includes('controversy') || topic.includes('stewards')) {
  moodName = 'concerned';
} else if (topic.includes('record') || topic.includes('milestone')) {
  moodName = 'proud';
} else if (score >= 9) {
  moodName = 'excited';
} else if (score >= 7) {
  moodName = 'happy';
}

// Get character reference
const character = config.video_generation_config.character_references.moods.find(
  m => m.name === moodName
);

return {
  character_url: config.video_generation_config.character_references.base_url + character.filename,
  mood: character.name,
  mood_description: character.description
};
```

---

### Example 4: Get Video Specs for Generation

```javascript
const config = await fetch('https://pub-b950cb87774b4e2eb3b2293cea84bafe.r2.dev/kai_video_generation_config.json')
  .then(r => r.json());

const videoType = $json.VideoType; // "Long-form" or "Short"

const specs = videoType === 'Short' 
  ? config.video_generation_config.video_specifications.short_form
  : config.video_generation_config.video_specifications.long_form;

return {
  resolution: specs.resolution,
  aspect_ratio: specs.aspect_ratio,
  target_duration: specs.target_duration,
  fps: specs.fps,
  format: specs.format
};
```

---

### Example 5: Get Platform-Specific Requirements

```javascript
const config = await fetch('https://pub-b950cb87774b4e2eb3b2293cea84bafe.r2.dev/kai_video_generation_config.json')
  .then(r => r.json());

const platform = 'youtube'; // or 'tiktok', 'instagram', etc.

const requirements = config.video_generation_config.platform_requirements[platform];

return {
  max_title_length: requirements.title_max_length,
  max_description_length: requirements.description_max_length,
  max_tags: requirements.tags_max_count,
  thumbnail_size: requirements.thumbnail_resolution
};
```

---

## 🔧 Updating the Config

### If using R2:
1. Edit `kai_video_generation_config.json` locally
2. Upload to R2 bucket (overwrites existing)
3. Changes take effect immediately for all workflows

### If using GitHub:
1. Edit file in your repository
2. Commit and push changes
3. Changes take effect immediately (GitHub raw URL updates)

---

## 📋 Config Structure Reference

```javascript
config.video_generation_config
  ├── version
  ├── character_settings
  ├── character_references
  │   └── moods[] (16 moods)
  ├── wardrobe_library
  │   └── outfits[] (15 outfits)
  ├── studio_assets
  │   ├── background
  │   └── props[]
  ├── video_templates
  │   ├── intro_clip
  │   ├── outro_clip
  │   └── transition
  ├── video_specifications
  │   ├── long_form
  │   └── short_form
  ├── ai_generation_settings
  ├── content_guidelines
  ├── platform_requirements
  │   ├── youtube
  │   ├── tiktok
  │   ├── instagram
  │   ├── twitter
  │   └── telegram
  ├── seo_templates
  └── workflow_integration
```

---

## 💡 Best Practices

1. **Cache the config** - Fetch once at workflow start, reuse throughout
2. **Handle failures** - Add try-catch in case URL is unreachable
3. **Validate data** - Check if required fields exist before using
4. **Use defaults** - Always have fallback values

```javascript
// Good practice
let config;
try {
  config = await fetch('https://pub-b950cb87774b4e2eb3b2293cea84bafe.r2.dev/kai_video_generation_config.json')
    .then(r => r.json());
} catch (error) {
  console.error('Failed to load config:', error);
  // Use hardcoded defaults
  config = { video_generation_config: { /* defaults */ } };
}
```

---

## 🎯 Next Steps

1. ✅ Config uploaded to R2 and GitHub
2. ✅ YouTube Long-Form Tracker cleaned
3. 🔄 Map columns in "Create Video Draft" node
4. 🔄 Test workflow execution
5. ⏳ Build Workflow 4 to process DRAFT videos using config

---

Your config is live and ready to use! 🚀
