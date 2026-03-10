# 🏛️ Museum of Us — Complete Setup Guide

## What You Have Right Now
The `index.html` file is a FULLY WORKING museum. Open it in any browser and it works immediately — no server needed for testing. To share it with a link, follow Step 6 (Hosting).

---

## STEP 1: Your Folder Structure

Create this folder on your computer:

```
museum-of-us/
├── index.html          ← (already done ✓)
├── audio/
│   ├── jazz.mp3        ← your background music
│   ├── voice1.mp3      ← voice note for memory 1
│   ├── voice2.mp3      ← voice note for memory 2
│   └── voice3.mp3      ← etc.
├── images/
│   ├── photo1.jpg      ← memory photos
│   ├── photo2.jpg
│   └── ...
└── video/
    └── letter1.mp4     ← your floating letter video
```

---

## STEP 2: Add Your Memories

Open `index.html` in VS Code. Find this section (around line 120):

```javascript
const memories = [
  {
    id: 'frame1',
    date: 'January 2019',
    caption: '"The night everything started..."',
    emoji: '🌙',
    photo: null,       // ← CHANGE TO: 'images/photo1.jpg'
    hasVoice: true,
    audio: null,       // ← CHANGE TO: 'audio/voice1.mp3'
  },
  ...
```

### For each memory, update:
| Field | What to put |
|-------|-------------|
| `date` | The real date, e.g. `'June 14, 2021'` |
| `caption` | A meaningful quote or description |
| `emoji` | Any emoji that fits if no photo |
| `photo` | Path like `'images/photo1.jpg'` (or keep null) |
| `hasVoice` | `true` if you recorded a voice note |
| `audio` | Path like `'audio/voice1.mp3'` (or null) |

---

## STEP 3: Add Your Music

1. Get an MP3 of your jazz/meaningful song
2. Put it in the `audio/` folder as `jazz.mp3`
3. That's it — the museum will auto-play it when lights turn on

**Free music sources:**
- [Pixabay](https://pixabay.com/music/) — search "jazz" or "romantic"
- [Free Music Archive](https://freemusicarchive.org)
- Your own meaningful song (any MP3)

---

## STEP 4: Add Your Video Letter

1. Record a video of yourself speaking (your phone is fine)
2. Save it as `letter1.mp4` in the `video/` folder
3. In `index.html`, find this comment (around line 335):

```html
<!-- When you have a video: <video controls src="video/letter1.mp4"></video> -->
```

Remove the comment markers so it becomes:

```html
<video controls src="video/letter1.mp4" style="width:100%"></video>
```

And remove or hide the `.video-placeholder` div above it.

---

## STEP 5: Customize Your Timeline

Find the milestone section (around line 430):

```html
<div class="milestone" onclick="openMilestone(0)">
  <div class="milestone-year">2019</div>
  ...
  <div class="milestone-text">The very first time...</div>
</div>
```

Change the years and text to your real milestones.

Also update the `milestones` array in JavaScript (around line 155):

```javascript
const milestones = [
  { year: '2019', text: 'First Met', caption: '"And there you were."', emoji: '✨' },
  ...
```

---

## STEP 6: Host It (Share With a Link)

### Option A: Netlify (Easiest — drag and drop)

1. Go to [netlify.com](https://netlify.com) and create a free account
2. Go to **Sites** → drag your entire `museum-of-us/` folder onto the page
3. Netlify gives you a link like `random-name.netlify.app`
4. Optional: rename it to something like `museum-of-us.netlify.app`
5. Share the link 💌

### Option B: Vercel

1. Install [Node.js](https://nodejs.org) (if not installed)
2. In your terminal, run:
   ```
   npm i -g vercel
   cd museum-of-us
   vercel
   ```
3. Follow prompts — you'll get a link

### Option C: GitHub Pages (Free Forever)

1. Create a free [GitHub](https://github.com) account
2. New repository → upload all your files
3. Settings → Pages → Deploy from main branch
4. Your link: `yourusername.github.io/museum-of-us`

---

## STEP 7: How the Museum Works

### The Experience Flow:
1. **Landing** → Dark screen, "Museum of Us", candles flickering
2. **Tap Enter** → Door swings open, camera glides in
3. **Dark museum** → Hint appears: *"Find the light switch to begin..."*
4. **Click the switch** (left wall, near the door) → Lights glow on, jazz starts, dust particles float
5. **Explore** → Click and drag to look around, scroll to move
6. **Frames** → Click any gold frame → memory panel opens with photo + caption + voice note
7. **Timeline plaque** (left wall) → Opens scrollable milestone timeline
8. **Floating letter** → Appears randomly after 20–60 seconds → click for video message
9. **Archive** → Missed/unfinished letters saved to 📬 icon

---

## STEP 8: Customization Tips

### Change the museum colors:
At the top of `index.html`, find `:root`:
```css
:root {
  --gold: #c9a84c;      /* frame color */
  --cream: #f5ead6;     /* text color */
  --dark: #0a0806;      /* background */
}
```

### Change the letter timing:
Find `scheduleNextLetter()` and adjust:
```javascript
const delay = 20000 + Math.random() * 40000; // 20–60 seconds
```
Change to e.g. `60000 + Math.random() * 120000` for 1–3 minutes.

### Add more frames:
In the `framePositions` array, add:
```javascript
{ pos: [X, Y, Z], ry: ROTATION, memIndex: INDEX }
```

---

## ✨ Optional Enhancements

| Feature | How |
|---------|-----|
| Custom cursor | Add `cursor: url(images/cursor.png), auto;` to `body` |
| Rain/snow particles | Duplicate particle system, change colors |
| Secret room | Add a hidden door that only opens after all frames viewed |
| Guestbook | Add a form that submits to a free service like Formspree |
| Custom domain | Buy `museumofus.com` (~$12/yr) and connect to Netlify |

---

## 🆘 Troubleshooting

**Music doesn't play:** Browsers block autoplay. The music plays when the user clicks the light switch — this is intentional.

**Video doesn't show:** Make sure the file path is exact: `video/letter1.mp4`

**Photos not showing:** Check the path in memories array matches exactly: `images/photo1.jpg`

**Runs slowly:** Compress your images to under 500KB. Use [Squoosh](https://squoosh.app).

---

*Made with Three.js + love 💛*
