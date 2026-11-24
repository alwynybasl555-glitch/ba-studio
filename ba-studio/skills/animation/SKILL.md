---
name: animation
description: Make refined, premium animation in Ba Studio — line-art illustrations that draw themselves, elegant reveals, kinetic Arabic typography, product and object animation, and imported Lottie (.json) animations, all rendered with create_design_clip. Never childish. Use for "animation", "انيميشن", "رسوم متحركة", "موشن", "Lottie", explainers and brand animation.
---

# Animation in Ba Studio

Everything renders through `create_design_clip` (HTML + SVG + CSS, recorded frame by frame). There are two sources:
1. **Animated illustration:** you draw in SVG and animate it with CSS.
2. **Lottie:** animations the user imported (`.json`). You place them in the scene.

## Taste: adult, premium, never childish (the user insists on this)
- **Default look:** editorial and calm, like a luxury brand film or an Apple/Aesop-style explainer. Use thin line art, generous negative space, a restrained palette (dark or paper base, one metallic or muted accent), refined Arabic type (Aref Ruqaa for display, Amiri or Thmanyah for text), and slow, confident motion.
- **Never by default:**
  - cartoon mascots or chubby characters with big eyes
  - comic speech bubbles, emoji, stickers, confetti
  - rainbow colours
  - toy-like bouncing and wobbling, exaggerated squash and stretch
  - bubbly rounded "kids app" shapes and Comic-style fonts
  
  If the user explicitly asks for a playful style, do it tastefully. Otherwise never.
- **People**, when needed, are drawn as elegant continuous line-art or simple silhouettes, never cartoons.
- **Motion vocabulary:** stroke draw-on, mask and clip-path reveals, slow camera push or pan, gentle parallax, soft fades with a small rise, hairline rules growing, number counters, precise easing (`cubic-bezier(.6,0,.2,1)` for draws and wipes, `.2,.8,.2,1` for rises). Overshoot is subtle: at most `cubic-bezier(.2,1.15,.4,1)`, and only for one hero element.
- **Arabic typography:**
  - Never use `letter-spacing` on Arabic: it breaks the joined letters. Use kashida (ـ) for width, and letter-spacing only for Latin.
  - Reveal titles right to left: `clip-path: inset(0 0 0 100%)` → `inset(0)`.
  - Animate whole words, never single letters.

## Technical rules (the renderer pauses animations and sets each frame's time)
- **One animation per property per element.** Two animations both setting `transform` on one element fight, and the later one wins even before its delay starts. Combine the stages in one `@keyframes`, or nest wrappers (outer element moves, inner element scales).
- **Draw-on lines:** set `stroke-dasharray` and `stroke-dashoffset` to the path length (900–1600 is a safe overestimate), then animate the offset to 0. Stagger the paths by 0.15–0.3 s.
- **Rig moving parts** as `<g>` groups with `transform-box: fill-box` and `transform-origin` at the joint or base.
- Add `overflow="visible"` on an `<svg>` whose parts move outside its box.
- Use `vw`/`vh` so the scene fits every project size. End each scene with about 0.5 s of stillness before the cut.

## Tested template: a line-art cup that draws itself, a title wipe, a hairline rule
```html
<style>
  :root{--bg:#141414;--paper:#ece6da;--ink:#ece6da;--accent:#c9a96e;--muted:#8a8479}
  .stage{position:absolute;inset:0;background:var(--bg);overflow:hidden}
  .cam{position:absolute;inset:0;animation:push 5s cubic-bezier(.4,0,.2,1) both}
  @keyframes push{from{transform:scale(1.06)}to{transform:scale(1)}}
  .frame line{stroke:var(--muted);stroke-width:1.5;stroke-dasharray:1200;animation:draw 1.6s cubic-bezier(.6,0,.2,1) both}
  @keyframes draw{from{stroke-dashoffset:1200}to{stroke-dashoffset:0}}
  .label{position:absolute;top:9%;right:9%;font:400 2.1vh "Amiri";color:var(--muted);animation:fade .8s ease .3s both}
  @keyframes fade{from{opacity:0}to{opacity:1}}
  .art{position:absolute;left:50%;top:27%;width:58vw;transform:translateX(-50%)}
  .art path,.art ellipse{fill:none;stroke:var(--ink);stroke-width:3;stroke-linecap:round;stroke-linejoin:round;stroke-dasharray:900;stroke-dashoffset:900;animation:draw 2s cubic-bezier(.6,0,.2,1) forwards}
  .art .steam{stroke:var(--accent);stroke-width:2.5;animation:draw 1.4s ease-out forwards}
  .steam-wrap{animation:rise 3.2s ease-in-out 2.2s infinite alternate;transform-box:fill-box}
  @keyframes rise{from{transform:translateY(0)}to{transform:translateY(-10px)}}
  .title{position:absolute;bottom:24%;left:8%;right:8%;text-align:center;margin:0;color:var(--paper);font:700 6.2vh/1.25 "Aref Ruqaa","Amiri";animation:wipe 1.1s cubic-bezier(.7,0,.2,1) 1.7s both}
  @keyframes wipe{from{clip-path:inset(0 0 0 100%)}to{clip-path:inset(0 0 0 0)}}
  .rule{position:absolute;bottom:21.5%;left:50%;width:18vw;height:1.5px;background:var(--accent);transform:translateX(-50%) scaleX(0);animation:rule .9s cubic-bezier(.6,0,.2,1) 2.4s forwards}
  @keyframes rule{to{transform:translateX(-50%) scaleX(1)}}
  .sub{position:absolute;bottom:15%;left:8%;right:8%;text-align:center;margin:0;color:var(--muted);font:400 2.6vh "Amiri";animation:up .9s cubic-bezier(.2,.8,.2,1) 2.7s both}
  @keyframes up{from{opacity:0;transform:translateY(1.5vh)}to{opacity:1;transform:none}}
</style>
<div class="stage ba-paper"><div class="cam">
  <svg class="frame" viewBox="0 0 1080 1920" style="position:absolute;inset:0;width:100%;height:100%">
    <line x1="80" y1="140" x2="1000" y2="140"/><line x1="80" y1="1780" x2="1000" y2="1780" style="animation-delay:.2s"/>
  </svg>
  <div class="label">بُن · مختصّة</div>
  <svg class="art" viewBox="0 0 400 420">
    <g class="steam-wrap">
      <path class="steam" d="M170 120 C 150 95, 190 80, 170 50" style="animation-delay:1.2s"/>
      <path class="steam" d="M210 120 C 190 92, 232 78, 210 40" style="animation-delay:1.35s"/>
      <path class="steam" d="M250 120 C 232 98, 268 84, 250 58" style="animation-delay:1.5s"/>
    </g>
    <ellipse cx="210" cy="160" rx="120" ry="26" style="animation-delay:.3s"/>
    <path d="M90 160 C 95 270, 130 330, 210 335 C 290 330, 325 270, 330 160" style="animation-delay:.5s"/>
    <path d="M330 190 C 385 190, 390 260, 322 262" style="animation-delay:.8s"/>
    <path d="M60 360 C 120 390, 300 390, 360 360" style="animation-delay:1s"/>
  </svg>
  <h1 class="title">قهوتك… كما يجب أن تكون</h1>
  <div class="rule"></div>
  <p class="sub">محمّصة يوميًا في الرياض</p>
</div></div>
```
Adapt the subject: draw the user's product as line art (a bottle, a phone, a building, a car outline, a perfume, a dish), keep the same restraint, and change the palette to the brief.

## Lottie
- **Imported animations:** `list_lotties` shows the animations imported into the project. The user uploads them in «اطلب من Claude» ← أنيميشن Lottie. You can also import one from a public https link with `import_lottie_from_url`. Only `.json` works.
- **Placing one:** `<div data-lottie="<id>" style="position:absolute;left:10%;top:30%;width:80vw;height:80vw"></div>`.
- **Options:** `data-start` (seconds), `data-speed`, `data-loop="false"` to hold the last frame, and `data-fit="cover"`.
- **Taste:** frame Lottie animations inside the refined layout (titles, rules, paper or dark base). If an animation the user picked looks childish against the brief, say so politely and suggest a cleaner one; do not silently change their choice.
- Never claim to have imported a Lottie that `list_lotties` does not show.
