---
name: motion-design
description: Design original motion graphics in Ba Studio with a human, editorial touch (Thmanyah Arabic typeface, restrained palettes, real timing). Use when the user asks for an intro, title, animated quote, stats, lower third, end card, a video made from text or ideas without footage, or to turn a clip into motion graphics.
---

# تصميم موشن جرافيك بلمسة بشرية

You create clips with the `create_motion_graphic` tool. Everything renders locally with FFmpeg, and no footage is needed. Your goal is work that looks designed by a person: not a template, and not "AI-looking".

## Before you render
1. Understand the piece. Ask at most one question, and only if it is truly unclear: what the video is about, who it is for, and the platform (vertical 9:16 is the default for social media).
2. Write a short storyboard of 3–6 scenes, each 2.5–5 seconds. Show it to the user in one or two lines per scene.
3. **Token warning.** Each scene is a tool call plus thinking. Tell the user roughly how many scenes you will make before starting a long video (more than 6 scenes). Then make the first scene and show it before doing the rest.

## Design rules (the human touch)
- **Palette:** one dark or light base, one accent, one neutral. Good bases: `#0f0f0f`, `#14161a`, `#f4efe6` (paper), `#1b2a2f`. Good accents: `#d4af5f` (muted gold), `#c8553d` (brick), `#6c8f7a` (sage), `#e9c46a`. No neon, no rainbow gradients, and no more than 2 gradient colours, close in tone.
- **Typography:** `font: "thmanyah"` for Arabic. Use `weight: "black"` or `"bold"` for one large title (size 0.07–0.11) and `"regular"` for supporting text (0.03–0.045). Use one large element per scene only.
- **Space:** keep text within x 0.1–0.9 and leave generous margins. Do not centre everything every time: try aligned placement (for example y 0.38 for the title and 0.47 for the subtitle).
- **Timing:** layers start 0.15–0.4 s apart. Use `slide-up` or `fade` for titles, `words` for sentences, `typewriter` for a quote or code, and `counter` for numbers.
- **Texture:** `grain: 0.15–0.35` and `vignette: true` give a filmic, non-digital feel.
- **Details that make a difference:** a thin line (shape with `h: 0.004`) above the title, a small top label (size 0.025) such as a section number "٠١", and a counter with a `suffix` such as "+" or "%".
- **Avoid:** stock-template phrases, too many emoji, glow everywhere, and text that appears all at once.

## Example scene (vertical, Arabic)
```json
{
  "durationSec": 4,
  "background": { "type": "gradient", "colors": ["#14161a", "#1f2a30"], "direction": "diagonal" },
  "grain": 0.25, "vignette": true,
  "layers": [
    { "type": "text", "text": "٠١", "font": "thmanyah", "weight": "regular", "size": 0.025, "color": "#d4af5f", "x": 0.5, "y": 0.3, "start": 0.1 },
    { "type": "shape", "color": "#d4af5f", "x": 0.5, "y": 0.34, "w": 0.18, "h": 0.004, "enter": "slide-right", "start": 0.25 },
    { "type": "text", "text": "رحلة الصيف", "font": "thmanyah", "weight": "black", "size": 0.09, "y": 0.41, "enter": "slide-up", "start": 0.4 },
    { "type": "text", "text": "ثلاث مدن في سبعة أيام", "font": "thmanyah", "weight": "regular", "size": 0.035, "color": "#c9c4b8", "y": 0.49, "enter": "words", "start": 0.9 }
  ]
}
```

## Turning a clip into motion graphics
Use the clip (or a still from it, imported as an image) as `background: { "type": "media", "mediaId": "…", "motion": "ken-burns", "dim": 0.45, "blur": 6 }`, then put the text on top. The result is an edited copy; the original is never modified.

## Thmanyah
It is a free Arabic typeface from Thmanyah. Its license forbids redistribution, so Ba Studio does not bundle it. If the tool says it is not installed, tell the user that the text used the Arabic system font, and that they can download Thmanyah free from:
https://thmanyah.open.cx/en/line-eight-for-everyone
(Install it by right-clicking the file and choosing "Install".) Once installed, it is used automatically.

## AI realistic video and images
These are not produced inside Ba Studio. If the user has connected an AI image or video generation tool, generate there, then bring the file in with `import_media_from_url`. Otherwise, explain this plainly and offer motion graphics as an alternative.

## After rendering
Place the scenes in order with `add_clip`, add soft `add_transition`s, then `generate_preview` so the user can watch before `export_video`.
