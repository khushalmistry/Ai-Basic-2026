# Module 5: AI Image, Video & Audio Tools

## Overview

AI-generated media has evolved from novelty to professional-grade production tools. In 2026, AI creates photorealistic images, cinematic videos, and human-quality voices that are indistinguishable from real content. This module covers every major tool for visual, video, and audio AI creation.

**By the end of this module, you will:**
- Master 15+ AI image generation tools and their unique strengths
- Create professional videos with AI (Sora, Kling, Runway)
- Generate realistic voices and music with AI
- Build complete media production workflows
- Understand the ethical and legal landscape

---

## 1. AI Image Generation

### 1.1 Midjourney v7 -- Best Artistic Quality

**What:** The gold standard for artistic, photorealistic AI image generation.

**Access:** Discord bot + Web app (alpha) | **Website:** midjourney.com

**Pricing:**
| Plan | Price | Fast GPU | Relax GPU | Features |
|------|-------|----------|-----------|----------|
| Basic | $10/mo | 3.3 hrs | None | 200 images/mo |
| Standard | $30/mo | 15 hrs | Unlimited | Unlimited relax |
| Pro | $60/mo | 30 hrs | Unlimited | Stealth mode |
| Mega | $120/mo | 60 hrs | Unlimited | 12x concurrent jobs |

**v7 New Features (2025-2026):**
```
- Draft Mode: 10x faster generation, 50% fewer credits
- Default Personalization: Learns your aesthetic preferences
- Improved anatomy: 40% fewer errors in hands/faces
- Omni Reference: Style transfer from reference images
- Video generation: 8x GPU cost, short clips
- Character consistency across multiple images
- 4K+ upscaling
```

**Prompting Midjourney:**
```
Basic: /imagine a cyberpunk hacker in a neon-lit server room

Advanced: /imagine a cyberpunk hacker in a neon-lit server room, 
photorealistic, volumetric lighting, shallow depth of field, 
shot on Sony A7III, 35mm lens --ar 16:9 --v 7 --style raw

Key Parameters:
--ar 16:9      Aspect ratio (16:9, 3:2, 1:1, 9:16)
--v 7          Model version
--style raw    Less Midjourney "style", more photorealistic
--q 2          Higher quality (uses more GPU)
--s 250        Stylization level (0-1000)
--c 50         Chaos/variety (0-100)
--no text      Negative prompt (exclude elements)
--sref URL     Style reference image
--cref URL     Character reference image
```

**Best For:** Marketing materials, concept art, product mockups, editorial images, social media content.

### 1.2 Flux 2 (Black Forest Labs) -- Best Photorealism & Open-Source

**What:** The most photorealistic AI image model, with open-source variants.

**Access:** API, Replicate, ComfyUI, local | **Variants:**

| Variant | Price | Size | Best For |
|---------|-------|------|----------|
| Flux 2 Pro | $0.03/MP | 32B | Production quality |
| Flux 2 Flex | $0.06/MP | 32B | Configurable, best text |
| Flux 2 Dev | $0.012/MP | 32B | Open-weight, LoRA training |
| Flux 2 Klein | Free | 4-9B | Local generation, fast |

**Flux 2 Strengths:**
```
- Photorealism matching professional photography
- Best text rendering in images (60% first-attempt accuracy)
- Multi-reference capability (up to 10 reference images)
- 4-megapixel native resolution
- Exact hex color control
- LoRA fine-tuning support (train on your own style)
- Run locally on consumer GPUs (Klein variant)
```

**Using Flux via API:**
```python
import replicate

output = replicate.run(
    "black-forest-labs/flux-2-pro",
    input={
        "prompt": "A cybersecurity operations center with multiple monitors "
                  "showing real-time threat dashboards, photorealistic, "
                  "dramatic blue lighting, ultra-detailed",
        "width": 1920,
        "height": 1080,
        "guidance_scale": 3.5,
        "num_inference_steps": 28
    }
)
print(output)  # Returns image URL
```

**Best For:** Product photography, e-commerce, stock photos, any use case requiring photorealism.

### 1.3 DALL-E 3 (OpenAI)

**What:** OpenAI's image generator, deeply integrated with ChatGPT.

**Access:** ChatGPT Plus/Pro, API | **Pricing:** $0.04-0.12 per image (API)

**Strengths:**
```
- Best prompt understanding (ChatGPT rewrites your prompt for optimal results)
- Excellent text rendering in images
- Strong at complex scenes with multiple elements
- Automatic safety filtering
- Direct ChatGPT integration (conversational image creation)
```

**Using DALL-E in ChatGPT:**
```
"Create an infographic showing the OWASP Top 10 vulnerabilities 
for 2025. Use a dark theme with neon accents. Include icons for 
each vulnerability and brief descriptions."

ChatGPT will:
1. Rewrite your prompt for DALL-E optimization
2. Generate the image
3. Let you iterate ("Make the text larger", "Change the color scheme")
```

### 1.4 Imagen 4 (Google)

**What:** Google's latest image model, integrated with Gemini.

**Access:** Gemini Advanced, Vertex AI | **Strengths:**
```
- Exceptional photorealism
- Strong text rendering
- Best understanding of spatial relationships
- Integrated with Google's ecosystem
- Available free in Gemini (limited)
```

### 1.5 Stable Diffusion 3.5 / SDXL (Stability AI)

**What:** The most popular open-source image generation ecosystem.

**Access:** Local (ComfyUI, Automatic1111), API | **Price:** Free (local) or API pricing

**Why It Matters:**
```
- Fully open source -- run locally with zero cost
- Massive ecosystem of models, LoRAs, and tools
- ComfyUI provides node-based workflow automation
- Thousands of community fine-tunes for specific styles
- Complete control over generation process
- Best for custom workflows and automation pipelines
```

### 1.6 Image Tool Comparison

| Tool | Quality | Speed | Text in Images | Cost | Local | Best For |
|------|---------|-------|---------------|------|-------|----------|
| Midjourney v7 | 10/10 | Medium | Good | $10-120/mo | No | Art, marketing |
| Flux 2 Pro | 9.5/10 | Fast | Best | $0.03/MP | No (Dev: Yes) | Photorealism |
| DALL-E 3 | 8.5/10 | Fast | Good | $0.04-0.12 | No | ChatGPT users |
| Imagen 4 | 9/10 | Fast | Good | Free-Paid | No | Google users |
| SD 3.5 | 8/10 | Varies | Medium | Free (local) | Yes | Custom workflows |
| Ideogram 2.0 | 8.5/10 | Fast | Excellent | Free tier | No | Typography |

---

## 2. AI Video Generation

### 2.1 OpenAI Sora 2 -- Cinematic AI Video

**What:** OpenAI's text-to-video and image-to-video model creating cinematic-quality clips.

**Access:** ChatGPT Plus/Pro | **Pricing:** Included with subscription (credit-based)

**Capabilities:**
```
- Text-to-video (up to 60 seconds)
- Image-to-video (animate still images)
- Video-to-video (style transfer, editing)
- Storyboard mode (multi-shot sequences)
- Camera control (pan, zoom, dolly, crane)
- 1080p resolution
- Realistic physics and motion
```

**Sora 2 Prompting:**
```
"A slow-motion shot of a hacker typing on a keyboard in a dark room, 
the screen reflecting in their glasses, green code scrolling on the 
monitor, cinematic lighting, shallow depth of field, 4K quality"

Tips:
- Describe camera movement: "tracking shot", "dolly in", "crane up"
- Specify duration: "5 seconds", "10 seconds"
- Include lighting details: "golden hour", "neon lights", "dramatic shadows"
- Reference film styles: "shot like a Christopher Nolan film"
- Use image input for character/scene consistency
```

### 2.2 Kling 3.0 (Kuaishou) -- Best Motion Quality

**What:** Chinese AI video model with industry-leading motion quality.

**Access:** kling.kuaishou.com | **Pricing:** Free tier + paid plans

**Strengths:**
```
- Best motion quality and physics simulation
- Up to 2 minutes video length
- Image-to-video with excellent consistency
- Lip-sync support
- Motion brush (control where things move)
- 1080p output
- Very generous free tier
```

### 2.3 Runway Gen-4.5 -- Professional Video Editing

**What:** The most comprehensive AI video platform for professional creators.

**Access:** runwayml.com | **Pricing:** Free tier + $12-$76/mo

**Key Tools:**
```
- Gen-4.5 (text/image to video)
- Motion Brush (control movement in specific areas)
- Camera Controls (precise camera movement)
- Director Mode (multi-shot sequences)
- Video-to-Video (style transfer)
- Remove Background
- Inpainting (edit specific areas)
- Extend Video (make clips longer)
- Lip Sync
- Green Screen
```

**Best For:** Professional video editing, content creators, filmmakers.

### 2.4 Other Video Tools

| Tool | Specialty | Price | Notable Feature |
|------|----------|-------|----------------|
| Pika 2.0 | Fun, creative | Free tier | Sound effects, morphing |
| Luma Dream Machine | 3D-aware video | Free tier | Best 3D understanding |
| Hailuo/MiniMax | Long videos | Free tier | Up to 5 min clips |
| Veo 2 (Google) | High quality | Vertex AI | 4K, 2+ min, best physics |
| Haiper 2.0 | Speed | Free tier | Fast generation |
| Stable Video Diffusion | Open source | Free (local) | Full control, fine-tunable |

### 2.5 Video Tool Comparison

| Feature | Sora 2 | Kling 3.0 | Runway Gen-4.5 | Veo 2 |
|---------|--------|-----------|-----------------|--------|
| Max Length | 60s | 120s | 40s | 120s+ |
| Resolution | 1080p | 1080p | 4K | 4K |
| Motion Quality | 9/10 | 10/10 | 8.5/10 | 9.5/10 |
| Text Prompt | Excellent | Good | Good | Excellent |
| Image-to-Video | Yes | Yes | Yes | Yes |
| Camera Control | Yes | Limited | Excellent | Yes |
| Editing Tools | Basic | Basic | Comprehensive | Basic |
| Free Tier | No (Plus req) | Yes | Yes (limited) | No |
| API Access | Yes | Yes | Yes | Vertex AI |

---

## 3. AI Audio & Voice

### 3.1 ElevenLabs -- Best AI Voices

**What:** The leading AI voice platform for text-to-speech, voice cloning, and dubbing.

**Website:** elevenlabs.io | **Pricing:**

| Plan | Price | Characters/mo | Features |
|------|-------|--------------|----------|
| Free | $0 | 10,000 | 3 custom voices |
| Starter | $5/mo | 30,000 | 10 custom voices |
| Creator | $22/mo | 100,000 | 30 custom voices, Projects |
| Pro | $99/mo | 500,000 | Unlimited voices, API priority |
| Enterprise | Custom | Custom | Fine-tuning, SLA |

**Key Features:**
```
- Text-to-Speech: 30+ languages, dozens of built-in voices
- Voice Cloning: Clone any voice from a 30-second sample
- Voice Design: Create entirely new voices from description
- Speech-to-Speech: Real-time voice transformation
- AI Dubbing: Automatically dub videos into other languages
- Projects: Long-form audiobook creation
- Sound Effects: Generate any sound effect from text
```

**Using ElevenLabs API:**
```python
from elevenlabs import ElevenLabs

client = ElevenLabs(api_key="your-key")

# Text to speech
audio = client.text_to_speech.convert(
    text="Welcome to the AI Mastery Course. Today we're covering "
         "the latest breakthroughs in artificial intelligence.",
    voice_id="JBFqnCBsd6RMkjVDRZzb",  # "George" voice
    model_id="eleven_multilingual_v2",
    output_format="mp3_44100_128"
)

with open("intro.mp3", "wb") as f:
    for chunk in audio:
        f.write(chunk)
```

### 3.2 AI Music Generation

**Suno v4:**
```
- Generate full songs (lyrics + vocals + instruments)
- Up to 4 minutes per generation
- Multiple genres and styles
- Custom lyrics input
- Extend and remix existing generations
- Pricing: Free (10 songs/day) | Pro $10/mo | Premier $30/mo

Prompt example:
"An upbeat electronic track with cyberpunk vibes, 
synthwave bassline, female vocals singing about AI 
and the future. 120 BPM, key of A minor."
```

**Udio v2:**
```
- Competing platform to Suno
- Better audio quality in some genres
- Stronger at capturing specific musical styles
- Free tier: 10 songs/day | Standard $10/mo | Pro $30/mo
```

**Other Music Tools:**
| Tool | Type | Best For | Price |
|------|------|----------|-------|
| Suno v4 | Full songs | Complete song generation | Free-$30/mo |
| Udio v2 | Full songs | Genre accuracy | Free-$30/mo |
| AIVA | Composition | Film/game scores | Free-$49/mo |
| Soundraw | Background | Content creator music | $17/mo |
| Mubert | Ambient | Streaming/real-time | $14/mo |
| Stable Audio 2 | Open source | Custom music pipelines | Free (local) |

### 3.3 Audio Transcription & Translation

**OpenAI Whisper (v3-turbo):**
```python
from openai import OpenAI
client = OpenAI()

# Transcribe any audio
with open("meeting.mp3", "rb") as f:
    transcript = client.audio.transcriptions.create(
        model="whisper-1",
        file=f,
        response_format="verbose_json",
        timestamp_granularities=["segment", "word"]
    )

# Translate non-English audio to English
with open("japanese_podcast.mp3", "rb") as f:
    translation = client.audio.translations.create(
        model="whisper-1",
        file=f
    )
```

**Other Transcription Tools:**
| Tool | Speed | Accuracy | Languages | Price |
|------|-------|----------|-----------|-------|
| Whisper v3 | Fast | Excellent | 100+ | $0.006/min (API) |
| AssemblyAI | Fast | Excellent | 50+ | $0.01/min |
| Deepgram | Fastest | Excellent | 30+ | $0.0043/min |
| Google Speech-to-Text | Fast | Very Good | 125+ | $0.006/min |

---

## 4. AI Image Editing

### 4.1 Key Editing Capabilities

```
Inpainting:      Edit specific areas of an image ("change the sky to sunset")
Outpainting:     Extend an image beyond its borders
Style Transfer:  Apply one image's style to another
Upscaling:       Increase resolution (2x, 4x, 8x)
Background:      Remove or replace backgrounds
Object Removal:  Remove unwanted objects seamlessly
Face Editing:    Age, de-age, expression change
Colorization:    Add color to black and white images
```

### 4.2 Editing Tool Comparison

| Tool | Inpaint | Outpaint | Upscale | Remove BG | Price |
|------|---------|----------|---------|-----------|-------|
| Adobe Firefly 3 | Excellent | Yes | Yes | Yes | $10/mo (CC) |
| Photoshop (AI) | Best | Best | Best | Best | $23/mo |
| Canva Magic | Good | Limited | Yes | Yes | Free-$13/mo |
| Remove.bg | No | No | No | Best (free) | Free tier |
| Topaz Gigapixel | No | No | Best | No | $100 (one-time) |
| ComfyUI | Full control | Yes | Yes | Yes | Free (local) |

---

## 5. Production Workflows

### 5.1 YouTube Content Pipeline

```
Step 1: Script → ChatGPT or Claude
  "Write a 10-minute YouTube script about AI security tools"

Step 2: Voiceover → ElevenLabs
  Generate professional narration from the script

Step 3: Visual Assets → Midjourney + Flux
  Create thumbnails, diagrams, and B-roll images

Step 4: Video Clips → Sora 2 or Runway
  Generate supporting video footage

Step 5: Editing → Runway or traditional editor
  Combine voiceover, visuals, and video

Step 6: Thumbnail → Midjourney or Ideogram
  Create eye-catching thumbnail with text

Total cost: $50-100/video (vs $500-2000 traditional production)
```

### 5.2 Marketing Content Pipeline

```
Step 1: Strategy → ChatGPT Deep Research
  Research audience, competitors, trends

Step 2: Copy → Claude
  Generate ad copy, social media posts, email sequences

Step 3: Images → Flux 2 Pro or Midjourney
  Product shots, lifestyle images, social graphics

Step 4: Video Ads → Kling or Sora
  Short-form video ads (15-30 seconds)

Step 5: Localization → ElevenLabs
  Dub into multiple languages
```

---

## 6. Ethics, Legal, and Safety

### 6.1 Copyright and Ownership

```
Current landscape (March 2026):
- US: AI-generated images are NOT copyrightable (no human author)
- EU: AI Act requires labeling AI-generated content
- Most platforms: You own commercial rights to what you generate
- Training data lawsuits: Ongoing (Getty v Stability, NYT v OpenAI)
- Deepfake laws: Rapidly evolving, many jurisdictions criminalizing

Best practices:
- Always disclose AI-generated content in professional settings
- Don't create non-consensual images of real people
- Check platform terms of service for commercial use rights
- Add metadata indicating AI generation
- Keep generation prompts/logs for provenance
```

### 6.2 Detection and Watermarking

```
C2PA Standard:
- Industry standard for content provenance
- OpenAI, Google, Adobe, Microsoft support it
- Embeds invisible metadata about creation method
- cr.ai website to verify content authenticity

Detection Tools:
- Hive AI Detector
- AI or Not
- Illuminarty
- Content Credentials (Adobe)
```

---

## 7. Practical Exercises

### Exercise 1: Image Generation Challenge
Create the same scene across 3 different tools (Midjourney, DALL-E, Flux). Compare quality, speed, and prompt interpretation.

### Exercise 2: Video Production
Create a 30-second promotional video for a fictional product using only AI tools (script, voiceover, visuals, music).

### Exercise 3: Voice Clone
Use ElevenLabs to clone your own voice (with consent) and generate a podcast intro.

### Exercise 4: Full Pipeline
Create a complete YouTube tutorial with AI:
- Script (LLM)
- Voiceover (ElevenLabs)
- Visuals (Midjourney/Flux)
- Music (Suno)
- Thumbnail (Ideogram)

---

## 8. Key Takeaways

1. **Midjourney v7 leads artistic quality**, Flux 2 leads photorealism
2. **Sora 2 and Kling 3.0 make video generation practical** for real content
3. **ElevenLabs dominates voice AI** -- cloning, TTS, dubbing
4. **Suno and Udio make music generation accessible** to non-musicians
5. **Open-source options (Flux, SD, Stable Audio) enable local generation** with zero ongoing cost
6. **Full media production pipelines cost 90% less** than traditional methods
7. **Ethics matter** -- always disclose AI content and respect consent
8. **The tools are changing monthly** -- stay current by following release blogs

---

**Next Module:** [Module 6: 50+ AI Tools Reference Guide](06-50-plus-ai-tools.md) -- The definitive catalog of AI tools across every category.
