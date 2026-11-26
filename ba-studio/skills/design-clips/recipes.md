# Scene recipes (Motion Kit)

These are proven scene structures. Each one is a starting point: change the palette, the layout side, the timings and the content every time, so no two videos look alike. They all use the Motion Kit (`data-ba…` attributes), which is built into every design clip.

Shared base for every recipe (copy once per video, then change the colours):
```html
<style>
  :root{--paper:#ebe6dc;--accent:#2f4fb3;--ink:#17171a;--muted:#4a4a52}
  .stage{position:absolute;inset:0;background:var(--paper);overflow:hidden}
  .cam{position:absolute;inset:0}
  .t1{font:900 7.2vh/1.22 "Thmanyah","Amiri";color:var(--ink);margin:0}
  .t2{font:400 3.2vh/1.5 "Thmanyah","Amiri";color:var(--muted);margin:0}
  .lbl{font:700 2.6vh "Thmanyah","Amiri";color:var(--accent)}
</style>
```
End every scene with `<div class="ba-vignette"></div><div class="ba-grain"></div>` inside `.stage`.

## 1. Editorial hook (first scene, 1.8–2.6 s)
The question or claim lands word by word behind a mask, one word gets the marker, and the camera pushes slowly.
```html
<div class="stage ba-paper"><div class="cam" data-ba-cam="push" data-amount="0.7">
  <div class="lbl" style="position:absolute;top:24%;right:9%" data-ba="fade" data-at="0">٠١</div>
  <h1 class="t1" style="position:absolute;top:28%;right:9%;left:9%" data-ba-words="mask" data-at="0.1" data-gap="0.08">
    تبي تشتري <span data-ba-mark="marker" data-at="0.9" data-color="rgba(47,79,179,.2)">سيرفر</span>؟</h1>
</div><div class="ba-vignette"></div><div class="ba-grain"></div></div>
```

## 2. Giant stat
One huge number counts up, the unit is quiet, a hairline grows under it, and one line explains it.
```html
<div class="stage ba-paper"><div class="cam" data-ba-cam="pull" data-amount="0.6">
  <div style="position:absolute;top:34%;right:9%;font:900 17vh/1 'Thmanyah','Amiri';color:var(--ink)" data-ba="rise" data-at="0.1">
    <span data-ba-count="98" data-at="0.15" data-dur="1.2" data-digits="ar">0</span><span style="font-size:.4em;color:var(--accent)">٪</span></div>
  <div style="position:absolute;top:52%;right:9%;width:30vw;height:3px;background:var(--ink)" data-ba-mask="right" data-at="0.7" data-dur="0.6"></div>
  <p class="t2" style="position:absolute;top:55%;right:9%;left:12%" data-ba-words="blur" data-at="0.9">من عملائنا يجددون كل شهر</p>
</div><div class="ba-vignette"></div><div class="ba-grain"></div></div>
```
Only use numbers the user gave you. Never invent statistics.

## 3. Product hero with depth
The product photo sits on a mid plane, a large shape behind it moves slower, and a detail in front moves faster. Callouts draw on.
```html
<div class="stage ba-paper"><div class="cam" data-ba-cam="pan-left" data-amount="0.8">
  <div data-depth="0.4" data-ba="zoom" data-feel="heavy" style="position:absolute;left:8%;top:30%;width:70vw;height:70vw;border-radius:50%;background:#dcd5c7"></div>
  <img src="{{media:PRODUCT_ID}}" data-depth="1" data-ba="pop" data-at="0.2" data-feel="snappy" class="ba-grade-warm"
       style="position:absolute;left:14%;top:34%;width:56vw;filter:drop-shadow(0 30px 40px rgb(20 14 8/.28))">
  <svg data-depth="1.3" viewBox="0 0 1080 1920" style="position:absolute;inset:0;width:100%;height:100%">
    <path data-ba-draw data-at="0.9" data-dur="0.8" d="M720 900 C 800 860, 860 820, 900 760" fill="none" stroke="#17171a" stroke-width="4" stroke-linecap="round"/></svg>
  <div class="lbl" style="position:absolute;top:36%;right:6%" data-ba="fade" data-at="1.5">تصميم يدوي</div>
</div><div class="ba-vignette"></div><div class="ba-grain"></div></div>
```

## 4. Three reasons (list that builds)
Cards arrive with an uneven rhythm. The middle one is in the accent colour to break the pattern.
```html
<div class="stage ba-paper"><div class="cam" data-ba-cam="drift">
  <h2 class="t1" style="position:absolute;top:18%;right:9%;left:9%;font-size:6vh" data-ba-words="mask" data-at="0">ليش يختارونا؟</h2>
  <div data-ba-stagger="tilt" data-at="0.6" data-gap="0.16" style="position:absolute;top:32%;right:9%;left:9%;display:grid;gap:2.2vh">
    <div class="ba-shadow" style="background:#fff;border-radius:24px;padding:3vh 5vw;font:700 3.4vh 'Thmanyah','Amiri'">تشغيل فوري</div>
    <div class="ba-shadow" style="background:var(--accent);color:#fff;border-radius:24px;padding:3vh 5vw;font:700 3.4vh 'Thmanyah','Amiri'" data-base="rotate(-1.5deg)">دعم بالعربي</div>
    <div class="ba-shadow" style="background:#fff;border-radius:24px;padding:3vh 5vw;font:700 3.4vh 'Thmanyah','Amiri'">بدون التزام</div>
  </div>
</div><div class="ba-vignette"></div><div class="ba-grain"></div></div>
```

## 5. Before / after split
The screen wipes from the right to reveal the "after" half while the "before" half dims.
```html
<div class="stage" style="background:#1d1d21"><div class="cam" data-ba-cam="handheld">
  <div style="position:absolute;inset:0;background:#2a2a30"><p class="t1" style="position:absolute;top:40%;right:9%;color:#8a8a93" data-ba-words="rise" data-at="0">قبل: ساعات تعديل</p></div>
  <div data-ba-mask="right" data-at="1.1" data-dur="0.9" style="position:absolute;inset:0;background:var(--paper)">
    <p class="t1" style="position:absolute;top:40%;right:9%" data-ba-words="mask" data-at="1.6">بعد: دقيقة وحدة</p></div>
</div><div class="ba-grain"></div></div>
```

## 6. Quote / testimonial
A large calligraphic quote mark draws in, the quote arrives word by word slowly, and the name fades in last.
```html
<div class="stage ba-paper"><div class="cam" data-ba-cam="push" data-amount="0.5">
  <div style="position:absolute;top:22%;right:8%;font:400 22vh/1 'Aref Ruqaa';color:var(--accent)" data-ba="blur" data-at="0" data-feel="gentle">«</div>
  <p class="t1" style="position:absolute;top:36%;right:9%;left:9%;font-size:5.4vh;line-height:1.5" data-ba-words="blur" data-at="0.4" data-gap="0.11">أول مرة أحس إن الإعلان يشبهنا فعلاً.</p>
  <div class="lbl" style="position:absolute;top:58%;right:9%" data-ba="slide-right" data-at="1.9" data-amount="0.2">— عميل حقيقي</div>
</div><div class="ba-vignette"></div><div class="ba-grain"></div></div>
```
Use only quotes the user provided.

## 7. End card with logo
The logo pops in on a soft spring, the call to action wipes in under it, and the scene holds still for at least 0.8 s at the end.
```html
<div class="stage ba-paper"><div class="cam" data-ba-cam="pull" data-amount="0.4">
  <img src="{{brand:logo}}" data-ba="pop" data-at="0.1" data-feel="lively" style="position:absolute;top:34%;left:50%;width:34vw;margin-left:-17vw">
  <div style="position:absolute;top:56%;right:0;left:0;text-align:center" data-ba-mask="right" data-at="0.6" data-dur="0.7">
    <span class="t1" style="font-size:4.6vh">ابدأ اليوم</span></div>
</div><div class="ba-vignette"></div><div class="ba-grain"></div></div>
```

## 8. Scene exit (any scene)
Instead of a hard cut, let the key elements leave 0.3–0.4 s before the scene ends: add `data-out="<scene end − 0.35>"` to the title and the main object. Leave the background in place so the next scene's entrance reads as a continuation.
