# 🏛️ Museum of Us — Setup Guide

## Your Actual Folder Structure

```
museum-of-us/
├── index.html
├── media/
│   ├── first_actual.JPG      ← frame 1 photo
│   ├── sept.png              ← frame 2 photo
│   ├── oct.png               ← frame 3 photo
│   ├── nov.png               ← frame 4 photo
│   ├── dec.png               ← frame 5 photo
│   ├── jan.png               ← frame 6 photo
│   ├── feb.png               ← frame 7 photo
│   ├── mar.png               ← frame 8 photo
│   ├── imnotsleepy.png       ← honorable mention frame
│   ├── first_pic.png         ← extra back wall frame
│   ├── apr.MOV               ← center back wall featured video
│   ├── video1.mp4            ← floating letter video 1
│   ├── video2.mp4            ← floating letter video 2
│   ├── video3.mp4
│   ├── video4.mp4
│   ├── video5.mp4
│   ├── video6.mp4
│   ├── video7.mp4
│   ├── video8.mp4
│   ├── video9.mp4
│   └── video10.mp4
├── audio/
│   ├── adura.mp3             ← background gallery music
│   └── (any per-frame audio files)
└── landing/
    ├── 001.jpg               ← landing page photo 1
    ├── 002.jpg               ← landing page photo 2
    └── ... up to 070.jpg
```

---

## Swapping In Your Real Photos

Open `index.html` and find the `memories` array (around line 843). Each entry looks like this:

```javascript
{ id:'frame1', date:'The First One', caption:'"Your quote here."', emoji:'🌙', photo:'media/first_actual.JPG', audio:'audio/adura.mp3' },
```

Just update the `photo:` path to point to your real file and the `caption:` to your real words. The fields are:

| Field | What it does |
|-------|-------------|
| `date` | Label shown at the top of the card |
| `caption` | The quote or description shown below the photo |
| `emoji` | Shown on the wall frame before you tap it, and as fallback if no photo |
| `photo` | Path to your image file in `media/` |
| `audio` | Path to an mp3 that plays when this frame is opened (optional) |
| `video` | Path to a video file — use instead of `photo` for video frames |

---

## Adding Per-Frame Audio

To play a specific song when someone taps a frame, add `audio:` to that memory:

```javascript
{ id:'frame2', date:'September', caption:'"Every ordinary moment..."', photo:'media/sept.png', audio:'audio/september.mp3' },
```

Put the mp3 file in your `audio/` folder. It will auto-play when the card opens and stop when closed. The background gallery music ducks down while it plays.

---

## Landing Page Photos

Put up to 70 photos in a folder called `landing/` next to `index.html`, named:
`001.jpg`, `002.jpg`, `003.jpg` ... `070.jpg`

They smack onto the screen one by one when the landing page loads. If the `landing/` folder doesn't exist, it falls back to your `media/` photos automatically.

**Photos must be `.jpg`**. If yours are `.png`, find this line and change the extension:
```javascript
const LANDING_PHOTOS_70 = Array.from({length:70}, (_,i) => `landing/${String(i+1).padStart(3,'0')}.jpg`);
```

---

## Floating Letter Videos

Your letter videos live in `media/` and are named `video1.mp4` through `video10.mp4`. Find the `LETTER_VIDEOS` array to see or change them:

```javascript
const LETTER_VIDEOS = [
  'media/video1.mp4',
  'media/video2.mp4',
  // ...
];
```

**The smart shuffle rules** — videos never play consecutively if they're in the same group:
- `video4` through `video8` — never follow each other back to back
- `video1`, `video9`, `video10` — never follow each other back to back
- `video2` and `video3` — play freely anywhere

To add more videos, add them to the `LETTER_VIDEOS` array and optionally assign them to a group in `LETTER_VIDEO_GROUPS`.

**Letter timing** — letters float by every 20–60 seconds. To change that, find:
```javascript
letterTimeout = setTimeout(showLetter, 20000 + Math.random() * 40000);
```
Change the numbers (they're in milliseconds — 60000 = 1 minute).

---

## The Wall Layout

```
BACK WALL:    [first_actual] [sept]   [APR.MOV — featured]   [oct] [first_pic]

LEFT WALL:    [nov] [dec]   ← evenly spaced →   [jan] [feb]

RIGHT WALL:   [mar] [sleeping beauty ★]   [OUR STORY timeline plaque]

FRONT WALL:   (door you enter through)
```

The **"Our Story" timeline plaque** is on the right wall near the entry — tap it to open the scrollable milestone timeline. The **featured April video** (`apr.MOV`) sits center back wall as the focal piece.

---

## Updating the Timeline

The timeline milestones are in two places — the HTML and the JavaScript. They're already filled in with your real story. To edit them, search for `milestone` in `index.html`.

The JavaScript array (around line 858) controls what opens when you tap a milestone:
```javascript
const milestones = [
  { year:'May 2025', text:'Jojo: "I like you"', caption:'Salem: 💃🏽💃🏽💃🏽', emoji:'✨' },
  ...
```

The `MILESTONE_TO_MEMORY` array below it maps each milestone to the correct photo frame.

---

## Background Music

Put your background music at `audio/adura.mp3`. It starts playing when the lights turn on and ducks down whenever a panel opens. To change the file, find:

```javascript
bgMusicEl.src = 'audio/adura.mp3';
```

---

## Hosting on GitHub Pages

Your repo is already on GitHub at `github.com/buildwithsalem/museum-of-us`.

1. Go to your repo → **Settings** → **Pages**
2. Under **Source**, select `main` branch → `/ (root)`
3. Click **Save**
4. Your live link will be: `buildwithsalem.github.io/museum-of-us`

It takes 1–2 minutes to go live after saving. Share that link 💌

---

## Large Video Files

GitHub blocks files over 100MB. Your videos are stored via **Git LFS** (Large File Storage). If you need to add new large videos:

```bash
# Compress first (no quality loss on phone screens)
ffmpeg -i media/newvideo.mov -c:v libx264 -crf 23 -preset slow -c:a aac -b:a 128k media/newvideo.mp4

# Then add and push
git add media/newvideo.mp4
git commit -m "Add new letter video"
git push origin main
```

---

## How the Experience Works

1. **Landing page** — photos smack onto the screen, title fades in
2. **Enter the Gallery** — door swings open, camera glides in
3. **Dark museum** — hint appears: *"Tap the light switch to begin"*
4. **Light switch** — left wall near the door, tap it → lights glow on, music starts, dust particles float
5. **Explore** — drag to look around, use arrow buttons to walk
6. **Tap any frame** — memory card opens with your photo and caption
7. **Our Story plaque** — right wall near entry, tap to open the timeline
8. **Floating letter** — appears randomly, tap to watch a video message
9. **Archive** — missed or unfinished letters saved to the 📬 button

---

## Troubleshooting

**Music doesn't play on iPhone** — this is normal. iOS blocks autoplay until the user taps something. Music starts after they tap the light switch.

**Video won't play** — make sure the filename in `LETTER_VIDEOS` exactly matches the file in `media/`, including capitalization.

**Photos not showing on wall** — the frames show emoji placeholders until tapped. That's intentional — photos only load inside the popup card.

**Landing photos not showing** — confirm your `landing/` folder exists and files are named `001.jpg` not `1.jpg`.

**Push rejected by GitHub** — files over 100MB need to be compressed with ffmpeg first (see Large Video Files above).

---

*Built with Three.js + love 💛*
