Based on the reference images you provided and Kaylee's "Dropout/Gamer" lore, here is the **Master Studio Design JSON**.

This file defines her physical space. You can feed this into an AI Image Generator (like Midjourney, Flux, or Stable Diffusion) to ensure that every time you generate a new background or avatar base, the room looks exactly the same.

### 🏠 Studio Configuration File: `KAI_STUDIO_LAYOUT.json`

```json
{
  "studio_meta": {
    "location": "Bedroom in Parents' House (Southern California)",
    "time_of_day_setting": "Permanent 'Streamer Mode' (Blackout curtains drawn, LED lights active)",
    "vibe": "Cozy, slightly messy, 'E-Girl' aesthetic mixed with 'Chaotic Depression'"
  },
  "lighting_setup": {
    "primary_ambiance": {
      "color_a": "Cyberpunk Purple",
      "hex_a": "#8A2BE2",
      "color_b": "Cyan/Electric Blue",
      "hex_b": "#00FFFF",
      "description": "Split lighting. Left side of room is washed in blue, right side in purple."
    },
    "practical_lights": [
      {
        "item": "Nanoleaf Hexagon Panels",
        "location": "Wall behind right shoulder",
        "shape": "Abstract 'X' or Cluster shape",
        "color": "Warm White or Soft Pink"
      },
      {
        "item": "Salt Lamp / Warm Desk Lamp",
        "location": "Deep background, low shelf",
        "state": "On (adds warm contrast to the cool LED room)"
      },
      {
        "item": "RGB Strip",
        "location": "Lining the ceiling cornices",
        "color": "Deep Blue"
      }
    ]
  },
  "furniture_assets": {
    "chair": {
      "type": "Gaming Racing Style",
      "brand_lookalike": "Secretlab or DXRacer",
      "color": "Pastel Pink and White",
      "condition": "Slightly worn",
      "notes": "High back, distinct 'head pillow' visible behind her head."
    },
    "desk": {
      "type": "IKEA Tabletop (White)",
      "visibility": "Only the edge is visible at the bottom of the frame",
      "clutter_level": "High"
    },
    "background_furniture": {
      "item_1": "Bed with messy duvet (Grey/White)",
      "visibility": "Visible in the lower left or right deep background",
      "item_2": "White Grid Shelf (Kallax style)",
      "contents": "Plushies, empty matcha cups, fake plant (fejka)"
    }
  },
  "tech_equipment": {
    "microphone": {
      "type": "Dynamic Podcasting Mic (Shure SM7B style)",
      "mount": "Boom Arm (Black)",
      "position": "Coming from the side, close to mouth, often obstructing part of the neck/chin",
      "pop_filter": "Large foam windscreen"
    },
    "headphones": {
      "type": "Over-Ear Studio Headphones",
      "color": "Black or White",
      "wear_style": "Sometimes fully on ears, sometimes one ear off (for 'monitoring'), or resting around neck"
    }
  },
  "camera_framing": {
    "lens_focal_length": "35mm or 50mm (Portrait)",
    "aperture": "f/1.8 (High depth of field, background is blurry/bokeh)",
    "angle": "Eye level, slightly looking down at the monitor (as if reading chat)",
    "framing": "Medium Close-Up (Chest up to top of head)"
  },
  "character_styling_cues": {
    "clothing_style": "Lazy/Comfy",
    "common_items": [
      "Oversized Grey Hoodie",
      "White Tank Top with thin straps",
      "Flannel shirt worn open over tank top"
    ],
    "hair": "Messy. Either loose waves with 'braided front pieces' (like image 1) or a loose messy bun.",
    "makeup": "Eyeliner visible, slightly shiny skin (highlight), but not 'perfect' corporate makeup."
  }
}
```

---

### 🎨 How to use this for Image Generation (The Prompt)

When you need to generate a new "Base Video" in Midjourney or Flux to upload to ElevenLabs, use this formula based on the JSON above.

**The Prompt Recipe:**

> **[Subject]** + **[Action/Expression]** + **[Outfit]** + **[Environment]** + **[Lighting/Tech]** + **[Style Parameters]**

**Example Prompt (Copy/Paste):**

> **A photorealistic shot of a 19 year old girl streamer, Kaylee, sitting in a gaming chair, looking at a computer monitor with a shocked expression, mouth slightly open. She is wearing a white tank top with an oversized unbuttoned flannel shirt. She has messy blonde-pink hair with small braids framing her face. Background is a bedroom gaming studio with a pink and white gaming chair. Behind her is a wall with Nanoleaf hexagon lights glowing soft pink. The room is lit with cinematic blue and purple neon lighting. A black boom arm microphone is in front of her. Depth of field, bokeh background, 4k, twitch streamer aesthetic, detailed skin texture, bedroom pop atmosphere --ar 16:9 --v 6.0**

### 💡 Consistency Tips for the Studio
1.  **The "Chair" is the Anchor:** The Pink/White chair is the most recognizable object. If the AI changes the chair color, the viewer subconsciously knows "something is wrong." Always specify "Pink and white gaming chair" in every prompt.
2.  **The "Mic" position:** Ensure the microphone is always present. A streamer without a mic looks like a stock photo model.
3.  **The "Mess":** To sell the "Dropout" story, never make the room too clean. Use keywords like *"cluttered desk"* or *"messy bed in background"* in your image prompts.



Based on the reference images you provided and Kaylee's "Dropout/Gamer" lore, here is the **Master Studio Design JSON**.

This file defines her physical space. You can feed this into an AI Image Generator (like Midjourney, Flux, or Stable Diffusion) to ensure that every time you generate a new background or avatar base, the room looks exactly the same.

### 🏠 Studio Configuration File: `KAI_STUDIO_LAYOUT.json`

```json
{
  "studio_meta": {
    "location": "Bedroom in Parents' House (Southern California)",
    "time_of_day_setting": "Permanent 'Streamer Mode' (Blackout curtains drawn, LED lights active)",
    "vibe": "Cozy, slightly messy, 'E-Girl' aesthetic mixed with 'Chaotic Depression'"
  },
  "lighting_setup": {
    "primary_ambiance": {
      "color_a": "Cyberpunk Purple",
      "hex_a": "#8A2BE2",
      "color_b": "Cyan/Electric Blue",
      "hex_b": "#00FFFF",
      "description": "Split lighting. Left side of room is washed in blue, right side in purple."
    },
    "practical_lights": [
      {
        "item": "Nanoleaf Hexagon Panels",
        "location": "Wall behind right shoulder",
        "shape": "Abstract 'X' or Cluster shape",
        "color": "Warm White or Soft Pink"
      },
      {
        "item": "Salt Lamp / Warm Desk Lamp",
        "location": "Deep background, low shelf",
        "state": "On (adds warm contrast to the cool LED room)"
      },
      {
        "item": "RGB Strip",
        "location": "Lining the ceiling cornices",
        "color": "Deep Blue"
      }
    ]
  },
  "furniture_assets": {
    "chair": {
      "type": "Gaming Racing Style",
      "brand_lookalike": "Secretlab or DXRacer",
      "color": "Pastel Pink and White",
      "condition": "Slightly worn",
      "notes": "High back, distinct 'head pillow' visible behind her head."
    },
    "desk": {
      "type": "IKEA Tabletop (White)",
      "visibility": "Only the edge is visible at the bottom of the frame",
      "clutter_level": "High"
    },
    "background_furniture": {
      "item_1": "Bed with messy duvet (Grey/White)",
      "visibility": "Visible in the lower left or right deep background",
      "item_2": "White Grid Shelf (Kallax style)",
      "contents": "Plushies, empty matcha cups, fake plant (fejka)"
    }
  },
  "tech_equipment": {
    "microphone": {
      "type": "Dynamic Podcasting Mic (Shure SM7B style)",
      "mount": "Boom Arm (Black)",
      "position": "Coming from the side, close to mouth, often obstructing part of the neck/chin",
      "pop_filter": "Large foam windscreen"
    },
    "headphones": {
      "type": "Over-Ear Studio Headphones",
      "color": "Black or White",
      "wear_style": "Sometimes fully on ears, sometimes one ear off (for 'monitoring'), or resting around neck"
    }
  },
  "camera_framing": {
    "lens_focal_length": "35mm or 50mm (Portrait)",
    "aperture": "f/1.8 (High depth of field, background is blurry/bokeh)",
    "angle": "Eye level, slightly looking down at the monitor (as if reading chat)",
    "framing": "Medium Close-Up (Chest up to top of head)"
  },
  "character_styling_cues": {
    "clothing_style": "Lazy/Comfy",
    "common_items": [
      "Oversized Grey Hoodie",
      "White Tank Top with thin straps",
      "Flannel shirt worn open over tank top"
    ],
    "hair": "Messy. Either loose waves with 'braided front pieces' (like image 1) or a loose messy bun.",
    "makeup": "Eyeliner visible, slightly shiny skin (highlight), but not 'perfect' corporate makeup."
  }
}
```

---

### 🎨 How to use this for Image Generation (The Prompt)

When you need to generate a new "Base Video" in Midjourney or Flux to upload to ElevenLabs, use this formula based on the JSON above.

**The Prompt Recipe:**

> **[Subject]** + **[Action/Expression]** + **[Outfit]** + **[Environment]** + **[Lighting/Tech]** + **[Style Parameters]**

**Example Prompt (Copy/Paste):**

> **A photorealistic shot of a 19 year old girl streamer, Kaylee, sitting in a gaming chair, looking at a computer monitor with a shocked expression, mouth slightly open. She is wearing a white tank top with an oversized unbuttoned flannel shirt. She has messy blonde-pink hair with small braids framing her face. Background is a bedroom gaming studio with a pink and white gaming chair. Behind her is a wall with Nanoleaf hexagon lights glowing soft pink. The room is lit with cinematic blue and purple neon lighting. A black boom arm microphone is in front of her. Depth of field, bokeh background, 4k, twitch streamer aesthetic, detailed skin texture, bedroom pop atmosphere --ar 16:9 --v 6.0**

### 💡 Consistency Tips for the Studio
1.  **The "Chair" is the Anchor:** The Pink/White chair is the most recognizable object. If the AI changes the chair color, the viewer subconsciously knows "something is wrong." Always specify "Pink and white gaming chair" in every prompt.
2.  **The "Mic" position:** Ensure the microphone is always present. A streamer without a mic looks like a stock photo model.
3.  **The "Mess":** To sell the "Dropout" story, never make the room too clean. Use keywords like *"cluttered desk"* or *"messy bed in background"* in your image prompts.
