# ClipZero — Corporate Pro Tools & Layman Guidance System

Expand ClipZero with high-value video utilities typically locked behind corporate SaaS paywalls (High-Res Frame Snapshots, Animated GIF Export, and Aspect Ratio Social Cropping) accompanied by a 4-step visual guidance workflow and intuitive player gestures.

---

### User Review & Critical Decisions

> [!IMPORTANT]
> The following product decisions were confirmed in the planning interview:

- **Corporate Features to Integrate**:
  1. **Instant High-Res Snapshot (Photo Capture)**: Grab a crystal-clear, full-resolution image (`PNG` or `JPEG`) of any frame directly from the playhead with one click (commonly paywalled as "HD Frame Export" in corporate tools).
  2. **Animated GIF Creator**: Convert any trimmed clip directly into a lightweight, looping GIF ready for Slack, Teams, email, or social media with quality/framerate options (almost universally paywalled in web trimmers).
  3. **Aspect Ratio Crop Presets**: Reframe video for social and corporate distribution with instant presets: Original, 16:9 (Presentation/YouTube), 9:16 (Shorts/Reels/TikTok), 1:1 (Square Feed), and 4:5 (Portrait) with live monitor crop guides.
- **Guidance Structure**:
  - A prominent, numbered 4-step workflow progress bar (`1. Load Video` → `2. Set Cut Range` → `3. Choose Tools & Style` → `4. Export / Save`) that automatically highlights the active step and explains what to do in plain layman terms.
  - Contextual "Quick Help" toggle that displays plain-English explainers next to technical controls.
- **Intuitive Gestures & Vector Badges**:
  - Clean inline SVG vector badges for all tools (Snapshot camera, Animated GIF, Aspect Crop, Audio extraction, Scissors).
  - Double-click/double-tap gestures on the video monitor (left half jumps back 5s with `⏪ 5s` visual ripple, right half jumps forward 5s with `⏩ 5s` ripple).

---

### 1. Overview & Core Concept

- **What It Does**: Provides users with corporate-level creative capabilities (cropping for modern platforms, generating GIFs, exporting high-res thumbnails, and trimming clips) for free, with zero setup, zero telemetry, and zero server uploads.
- **Target Audience**: Business professionals creating quick deck assets, social media managers cutting horizontal footage for vertical reels, engineers capturing bug reproduction frames, and everyday users who refuse to pay $15/month subscriptions for basic video trimming.
- **Key Value**: Democratizes professional video utilities through an intuitive, guidance-first interface where every tool is explained in everyday terms.

---

### 2. User Experience & Visual Design

#### Key User Flows
1. **Guided Onboarding**: When a user arrives, Step 1 (`Load Video`) is actively highlighted on the guidance header.
2. **Range Selection**: Once loaded, Step 2 (`Set Cut Range`) activates. Clear helper chips explain that the purple left bar is the Start Point and the right bar is the End Point.
3. **Pro Tool Tuning (Step 3)**:
   - **Capture Snapshot**: Click the camera icon at any moment to instantly download a lossless full-res image of the currently displayed video frame.
   - **Aspect Ratio Selector**: Switch between `Original`, `16:9`, `9:16 Vertical`, `1:1 Square`, and `4:5 Portrait`. An interactive canvas frame dynamically shows the framing box.
   - **GIF vs Video Mode**: Choose whether to export as a trimmed video or an animated GIF.
4. **Export & Download (Step 4)**: The export button updates dynamically with the active mode (`Export Trimmed Video`, `Generate Animated GIF`, `Extract Sound Track`) with clear progress bars and byte size estimates.

#### Visual Identity & Theme
- **Guidance Bar**: Refined horizontal strip below the top navigation with 4 numbered waypoints connected by quiet progress lines, adhering to anti-slop rules (no pulsing candy dots, clean typography with tabular numbering).
- **Tool Badges**: Crisp vector icon badges (`<svg>` with stroke styling) paired with descriptive layman labels:
  - 📷 **Take Photo**: "Save current frame as high-res image"
  - 🎞️ **Make GIF**: "Convert clip into looping animated GIF"
  - 📐 **Social Crop**: "Change shape for YouTube, TikTok, or Instagram"
  - ✂️ **Trim Video**: "Cut and save selected duration"
- **Double-Tap Monitor Ripple**: Semi-transparent circular ripples showing `⏪ 5s` and `⏩ 5s` on double-click/double-tap over the video screen for smooth desktop and tablet interaction.

---

### 3. Key Product Decisions & Trade-Offs

#### Decision 1: Client-Side GIF Encoding via Pure Canvas/JS Engine
- **Chosen Approach**: Use an in-browser canvas frame-sampler to capture frames across the trimmed range at 10-15 FPS and encode into standard animated GIF format entirely in memory.
- **Why**: Allows users to create shareable GIFs for Slack, Teams, or GitHub without cloud conversion queues or watermarks.

#### Decision 2: Native Full-Resolution Frame Snapshotting
- **Chosen Approach**: Render the underlying video element at its natural source dimensions (`videoWidth` × `videoHeight`) onto an offscreen canvas and export directly via `canvas.toBlob('image/png')` or `toBlob('image/jpeg', 0.95)`.
- **Why**: Guarantees zero compression loss and zero resolution downscaling, giving users true studio-grade thumbnail captures.

#### Decision 3: Aspect Ratio Cropping with Canvas Re-centering
- **Chosen Approach**: Allow users to select aspect ratios (`16:9`, `9:16`, `1:1`, `4:5`, `Original`). When exporting, the rendering canvas crops to the target aspect ratio centered on the footage.
- **Why**: Solves the #1 reason creators use paid apps—converting horizontal desktop/camera footage into vertical phone-friendly video without black letterboxes.

---

### 4. Technical Architecture & Component Mapping

```
┌────────────────────────────────────────────────────────────────────────┐
│  Top Navigation (Brand · Trust Assurances · Privacy Guarantee)         │
├────────────────────────────────────────────────────────────────────────┤
│  Guidance Step Bar: [1. Load Clip] ── [2. Cut Range] ── [3. Tools] ── [4. Export]
├───────────────────────────────┬────────────────────────────────────────┤
│  Studio Sidebar Panel         │  Video Monitor & Gesture Screen        │
│  ┌─────────────────────────┐  │  ┌──────────────────────────────────┐  │
│  │ 1. Media Info           │  │  │ Interactive Video Player         │  │
│  ├─────────────────────────┤  │  │ [Double-tap ⏪ 5s | 5s ⏩]       │  │
│  │ 2. Pro Tools Suite      │  │  │ [Take Instant Snapshot 📷]       │  │
│  │  • Aspect Ratio Crop    │  │  ├──────────────────────────────────┤  │
│  │    [Orig][16:9][9:16]   │  │  │ Interactive Timeline Bar         │  │
│  │    [1:1 Square][4:5]    │  │  │ [◀ Start ────── Window ──── End ▶│  │
│  │  • Speed & Audio        │  │  ├──────────────────────────────────┤  │
│  │  • Export Type:         │  │  │ Precision Transport Toolbar      │  │
│  │    [Video][GIF][Audio]  │  │  │ [-1s][-0.1s][▶][+0.1s][+1s]      │  │
│  ├─────────────────────────┤  │  └──────────────────────────────────┘  │
│  │ 3. One-Click Export     │  │                                        │
│  └─────────────────────────┘  │                                        │
└───────────────────────────────┴────────────────────────────────────────┘
```

#### New Capabilities to Implement:
- `GuidanceStepper`: Updates active state (`step-1`, `step-2`, `step-3`, `step-4`) based on app state (file loaded, range adjusted, tool selected, export clicked).
- `SnapshotEngine`: Instant high-res frame capture to PNG or JPEG with thumbnail preview toast.
- `GifEncoder`: Lightweight, fast client-side GIF generator with frame sampler and progress bar.
- `CropManager`: Computes center crop bounding box for selected aspect ratios (`16:9`, `9:16`, `1:1`, `4:5`) during playback and canvas export.
- `GestureHandler`: Handles double-tap/double-click on left/right screen zones to jump 5 seconds with animated visual feedback.
