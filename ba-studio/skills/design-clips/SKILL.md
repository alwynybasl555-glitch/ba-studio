---
name: design-clips
description: Build ad-quality animated design scenes in Ba Studio with create_design_clip (HTML/CSS recorded to video) — cards, product cut-outs, phone mockups, hand-drawn lines and starbursts, pop and slide motion, in an editorial human style. Use for promos, app/product ads, explainers, "make it look like this reference" requests.
---

# Design clips (HTML → video)

`create_design_clip` records an HTML/CSS page frame by frame. Anything a web page can draw, you can animate. The page has **no internet**, and **time is controlled**: only CSS animations/transitions and the Web Animations API move; `setTimeout`/`Date.now` do not advance. For JS drawing, listen to `window.addEventListener("ba-frame", e => …e.detail…)`.

## Workflow
1. **Brief:** brand colours, logo, product images (from the project: `src="{{media:<id>}}"`), what each scene says. If the user gave a reference video, copy its *system* (palette, type, element style, rhythm), not its content.
2. **Storyboard:** 5–8 scenes of 2.5–5 s, one idea each. Tell the user how many before rendering (each scene costs tokens).
3. **First scene:** render it alone, preview it, and adjust; only then make the rest **reusing the same CSS** (copy the style block) so the whole ad is consistent.
4. Place the scenes in order (`add_to_timeline_at`), add short transitions, and call `generate_preview`.

## House style: the same every time, but never identical
This is the user's chosen house style for all designs. Keep its DNA: paper grain, one strong accent, editorial Arabic titles, hand-drawn black details, and real product images. **But no two videos may look alike.** Before designing, pick a fresh combination and say it in one line:
- **Palette** (one): mustard on paper · brick on warm sand · cobalt on stone · sage on cream · coral + deep teal · teal on off-white · night version (dark ink background, paper-coloured cards).
- **Layouts** (3–4, never the same layout in two consecutive scenes): central hero with orbiting details · asymmetric card grid · split screen · winding path or timeline · tilted polaroids with tape · giant stat · revealing list · macro product with callout · phone held by a hand · before and after.
- **Motion** (2–3): pop with overshoot · mask wipes · parallax pan · cascade · card flips · lines that lead the eye · elastic drop · slide and settle · zoom-through between scenes · typewriter with underline.
- **Details** (2): starbursts · wavy connectors · dotted arrows · circled word · tape and sticky notes · stamps and badges · sparkles · price tags.
- Vary where titles sit and how long scenes last. If the project already has design clips, choose a different palette and layouts from theirs.

## Adjusting after a request
The user can reply in the same conversation to change the result. Change only what they asked: re-render just those scenes, reusing the same CSS, and replace those clips at the same timeline position. Keep everything else as it is.

## The design system that makes it look premium (not "AI")
- **One palette:** paper base `#efe7d8` + one strong accent (`#e3a92b` mustard, `#c8553d` brick, `#2f6b4f` green) + near-black `#1b1b1b`. Put `class="ba-paper"` on the background for real paper grain.
- **Type:** titles `font-family:"Thmanyah","Amiri"; font-weight:900` (or `"Aref Ruqaa"` for a calligraphic accent word), size 5.5–7% of height. Body 3–3.6%. Right-aligned (RTL) or centred, with lots of space around.
- **Objects:** rounded cards (`border-radius:28px`) in the accent colour holding a product photo or a CSS phone; soft long shadows (`box-shadow:0 30px 60px -20px rgba(0,0,0,.35)`).
- **Hand-drawn details** (SVG, black, slightly imperfect): squiggly connector lines, small starbursts on card corners, dotted lines linking items. Draw lines on with `stroke-dasharray` + animated `stroke-dashoffset`.
- **Motion:** elements **pop** (scale .6→1.06→1 with `cubic-bezier(.2,1.4,.4,1)`), **slide** in from off-screen, start **0.12–0.3 s apart**. Add one slow "camera" move by animating a wrapper's `transform` (a slight zoom or pan) through the whole scene. End each scene still, for 0.4 s, before the cut.
- **Avoid:** neon gradients, glow, emoji, generic stock phrases, and everything appearing at once.

## Template scene (copy, then change the content)
```html
<style>
  :root{--paper:#efe7d8;--accent:#e3a92b;--ink:#1b1b1b}
  .stage{position:absolute;inset:0;background:var(--paper);overflow:hidden}
  .cam{position:absolute;inset:0;animation:cam 4s ease-in-out both}
  @keyframes cam{from{transform:scale(1.06)}to{transform:scale(1)}}
  h1{position:absolute;top:9%;right:8%;left:8%;margin:0;text-align:center;color:var(--ink);
     font:900 6.2vh/1.25 "Thmanyah","Amiri";animation:up .7s cubic-bezier(.2,.9,.2,1) .15s both}
  h1 em{font-family:"Aref Ruqaa";font-style:normal;color:var(--accent)}
  @keyframes up{from{opacity:0;transform:translateY(4vh)}to{opacity:1;transform:none}}
  .card{position:absolute;width:34vw;height:34vw;border-radius:28px;background:var(--accent);
        box-shadow:0 30px 60px -20px rgba(0,0,0,.35);display:grid;place-items:center;
        animation:pop .6s cubic-bezier(.2,1.4,.4,1) both}
  .card img{width:78%;height:78%;object-fit:contain;filter:drop-shadow(0 12px 14px rgba(0,0,0,.25))}
  @keyframes pop{from{opacity:0;transform:scale(.6) rotate(-6deg)}to{opacity:1;transform:none}}
  .phone{position:absolute;width:30vw;aspect-ratio:9/19;border-radius:4.2vw;background:#111;padding:1.1vw;
         box-shadow:0 40px 70px -25px rgba(0,0,0,.45);animation:slide .8s cubic-bezier(.2,.9,.2,1) both}
  .phone .screen{width:100%;height:100%;border-radius:3.2vw;background:#fff;overflow:hidden}
  @keyframes slide{from{transform:translateY(60vh) rotate(8deg)}to{transform:rotate(-4deg)}}
  .line{fill:none;stroke:var(--ink);stroke-width:5;stroke-linecap:round;stroke-dasharray:1400;
        animation:draw 1.2s ease .5s both}
  @keyframes draw{from{stroke-dashoffset:1400}to{stroke-dashoffset:0}}
  .star{position:absolute;width:7vw;animation:pop .5s cubic-bezier(.2,1.4,.4,1) both}
</style>
<div class="stage ba-paper"><div class="cam">
  <h1>كل طلباتك <em>بمكان واحد</em></h1>
  <div class="card" style="right:10%;top:34%;animation-delay:.4s"><img src="{{media:PRODUCT_ID}}"></div>
  <svg style="position:absolute;inset:0;width:100%;height:100%" viewBox="0 0 1080 1920">
    <path class="line" d="M300 980 C 380 1080, 220 1160, 330 1260 S 470 1420, 380 1520"/>
  </svg>
  <div class="phone" style="left:12%;top:52%;animation-delay:.7s"><div class="screen"><!-- UI here --></div></div>
  <svg class="star" style="right:9%;top:32%;animation-delay:.9s" viewBox="0 0 100 100">
    <path d="M50 2 L58 38 L96 30 L64 52 L90 84 L54 66 L44 98 L38 62 L4 74 L32 48 L8 18 L44 34Z" fill="#1b1b1b"/>
  </svg>
</div></div>
```
Use `vw`/`vh` so the same scene works for vertical, square and landscape projects.

## Assets
- **Product photos:** ask the user to import them (with transparent backgrounds when possible). A real photo beats any drawing.
- **App screens:** build them in CSS inside `.screen` (headers, list rows, numbers, mini bar charts) using the brand colours.
- **AI images:** if an image-generation tool is connected, generate the cut-outs there, then use `import_media_from_url`.
- Never invent logos or brands the user does not own.
