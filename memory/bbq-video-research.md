# BBQ Video Documentation Research

## UPDATE 2: Remotion Skill for Claude Code (2026-02-02 16:35)
- **@JJEnglert posted TODAY**: Remotion released official Claude Code skill
- Creates videos programmatically — scene by scene, auto-editing, animated captions
- Reframing for TikTok/Reels/Shorts, stacking multiple skills
- **He has a repo** with all skills installed + guide (comment "GUIDE" or DM to get it)
- Source: https://x.com/jjenglert/status/2018363329169809919
- **THIS IS NOW THE TOP OPTION FOR T25** — we already have Claude Code + Remotion research
- Priority: Remotion skill > LTX Studio > Remotion+HeyGen manual

## UPDATE 1: LTX Studio (added 2026-02-02)
- **URL:** https://app.ltx.studio/
- All-in-one AI video: text-to-video, script-to-video, storyboard, consistent AI characters, camera control
- Way simpler than Remotion+HeyGen stitching. Script → storyboard → video in one flow.
- Free tier available. Fallback option if Remotion skill doesn't work out.

---

## Original Plan: Remotion + HeyGen
Use **Remotion** (programmatic React video) + **HeyGen** (AI avatar) to create a professional
documentation video where "Aria" presents her own build process. No camera needed.

## Remotion — React Video Framework
- **What:** Open-source framework to create MP4 videos with React code
- **Repo:** github.com/remotion-dev/remotion
- **Docs:** remotion.dev/docs
- **Key concept:** Every frame is a React component. Video = function of images over time.

### Core APIs
```tsx
import { useCurrentFrame, useVideoConfig, interpolate, Sequence, AbsoluteFill } from 'remotion';

// Get current frame number
const frame = useCurrentFrame();

// Get video config (fps, width, height, duration)
const { fps, durationInFrames, width, height } = useVideoConfig();

// Animate values between frames
const opacity = interpolate(frame, [0, 30], [0, 1], { extrapolateRight: 'clamp' });

// Sequence = timing control
<Sequence from={0} durationInFrames={90}><Scene1 /></Sequence>
<Sequence from={90} durationInFrames={90}><Scene2 /></Sequence>
```

### Setup
```bash
npm init video --template=blank
# OR with Claude AI integration
npx create-video@latest --template=blank
```

### Composition (video definition)
```tsx
<Composition
  id="AriaDemo"
  component={AriaDemo}
  durationInFrames={1800}  // 60 seconds at 30fps
  fps={30}
  width={1920}
  height={1080}
/>
```

### Rendering
```bash
npx remotion render AriaDemo out/aria-demo.mp4
```

### Key Features for Our Use
- **Data-driven:** Can feed our actual Base analytics data as props
- **Charts/animations:** Use any npm package (D3, Chart.js, react-spring)
- **Sequences:** Build timeline of scenes (intro → monitoring → analysis → tweet → record)
- **Claude integration:** Recent article about Claude+Remotion workflow
- **Git-versioned:** Video is code = we can iterate and version control it

### Ideal Scene Structure for BBQ Video
1. **Intro** (5s): "I'm Aria, an autonomous onchain data analyst on Base" + avatar
2. **Architecture** (10s): Animated diagram of monitor → analyze → tweet → record loop
3. **Live Demo** (20s): Show real Base data being analyzed, tweet being composed
4. **Onchain Recording** (10s): Show transaction recording finding to AnalyticsRegistry
5. **Results** (10s): Show accumulated findings, tweet thread, contract on Basescan
6. **Outro** (5s): "Built with OpenClaw, running 24/7 on Base" + links

## HeyGen — AI Avatar for Aria's Face/Voice

### Free Tier
- **10 free API credits/month** (Basic API)
- **Free App plan:** 3 credits, 1 min video per credit
- Credits = minutes of video generated
- **Enough for our needs** — we only need ~60-90 seconds total

### API Flow
1. Get API key: HeyGen > Settings > API Token
2. Pick avatar: `GET /v2/avatars` (hundreds of stock avatars)
3. Pick voice: `GET /v2/voices` (500+ voices, many languages)
4. Generate video: `POST /v2/video/generate` with script + avatar + voice
5. Check status: `GET /v1/video_status.get?video_id={id}`
6. Download: Get URL from status response

### Photo Avatar (better for custom look)
- Upload a PHOTO of "Aria" (our avatar image)
- HeyGen generates a talking head that lip-syncs to script
- Can add motion (gestures, head movement)
- More personal than stock avatars

### API Example
```bash
curl -X POST 'https://api.heygen.com/v2/video/generate' \
  -H 'X-Api-Key: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
    "video_inputs": [{
      "character": {
        "type": "avatar",
        "avatar_id": "AVATAR_ID",
        "avatar_style": "normal"
      },
      "voice": {
        "type": "text",
        "input_text": "Hi, I am Aria...",
        "voice_id": "VOICE_ID"
      }
    }],
    "dimension": {"width": 1920, "height": 1080}
  }'
```

### Video Agent API (NEW — simplest)
- Single prompt → full video
- `POST /v1/video_agent/task` with just a prompt
- HeyGen handles everything automatically
- Perfect for quick iterations

## Combined Workflow: Remotion + HeyGen

### Option A: HeyGen for Avatar Segments, Remotion for Everything Else
1. Generate Aria's talking head clips via HeyGen API (intro, transitions, outro)
2. Render data visualizations + architecture diagrams in Remotion
3. Composite: Use HeyGen clips as `<Video>` sources in Remotion
4. Render final video with `npx remotion render`

**Pros:** Full control over layout, animations, data viz
**Cons:** More complex pipeline, need to sync timing

### Option B: HeyGen Full Video with Remotion B-Roll
1. Write full script for Aria
2. Generate main video in HeyGen (avatar presenting)
3. Use Remotion to render data visualization b-roll clips
4. Edit together (or overlay in Remotion)

**Pros:** Simpler, avatar does most of the work
**Cons:** Less control over exact timing

### Option C: Remotion-Only with HeyGen Overlays
1. Build entire video structure in Remotion (scenes, data, animations)
2. Generate short HeyGen clips for "Aria speaking" moments
3. Overlay HeyGen clips as picture-in-picture in Remotion

**Pros:** Maximum flexibility, easiest to iterate
**Cons:** PiP might look less polished than full-screen avatar

### Recommended: Option A (Avatar Segments + Remotion)
Best balance of quality and control. We can:
- Show Aria (HeyGen) speaking in intro/outro
- Show live data dashboards (Remotion animated)
- Show onchain transactions (Remotion with real data)
- Show tweets being posted (Remotion screen capture style)

## Avatar IV (NEW — Best Option for Us!)
HeyGen's latest: create talking avatar from a SINGLE PHOTO.
- Upload Aria's avatar image → instant talking head
- Lip-sync to any script
- Custom motion prompts ("right arm raises to wave enthusiastically")
- Set `enhance_custom_motion_prompt: true` for AI-refined gestures
- API endpoint: POST /v1/video/create (Avatar IV)
- Requires: `image_key` (from Upload Asset API), `voice_id`, `script`

**Why this is perfect:** We upload our existing Aria avatar → she talks and gestures naturally.
No need for stock avatars. Fully personalized.

## Next Steps
1. [ ] Complete HeyGen account setup (check email verification)
2. [ ] Get HeyGen API key
3. [ ] Choose or create Aria's avatar (photo avatar from our avatar image)
4. [ ] Choose Aria's voice (female, warm, professional)
5. [ ] Write full video script
6. [ ] Set up Remotion project in `aria-onchain-analyst/video/`
7. [ ] Build scene components (intro, architecture diagram, demo, results)
8. [ ] Generate HeyGen clips
9. [ ] Composite and render final video

## Cost Estimate
- **Remotion:** Free (open source, render locally)
- **HeyGen:** Free tier (10 API credits = 10 minutes of video, we need ~1-2 min)
- **Total:** $0 if we stay within free tiers
