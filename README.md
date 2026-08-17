# Shorts Clipper — AI YouTube Shorts & Meme Maker

Turn a long horizontal video into a batch of ready-to-post vertical YouTube
Shorts, then remix any clip into a meme — freeze frames, meme text, zoom/shake
effects, sound effects, background music — all running locally, with
scheduled YouTube uploads on top. No cloud rendering, no per-export fees.

## What it does

**Clip → Short, automatically**
- Slices a long video into sequential vertical (9:16) clips, centered over a
  background template with no stretching
- Auto-generates subtitles locally (Whisper) with multiple styled presets,
  including word-by-word karaoke timing
- Background music with ducking and fade in/out
- AI-generated titles/descriptions/hashtags per clip

**Meme Studio — turn any clip into a meme**
- **Freeze frames** — hold on a moment for as long as you want, at any
  timestamp, with optional background-music boost timed to the freeze
- **Meme text** — classic top/middle/bottom captions over the video, or a
  dedicated caption bar above it that never covers the footage
- **Effects** — dramatic/slow zoom, shake, flash, motion blur, black & white,
  saturation boost, bass boost, echo, reverb, record scratch — all
  time-scoped: whole clip, at the playhead, during a freeze's hold, or right
  after one
- **Sound effects & dynamic music volume** — drop in stingers, automate the
  background music's volume over time
- Live preview with scrubbing, trim, and split

**Ship it**
- Fixed-slot upload scheduler (e.g. 6 AM / 9 AM / 12 PM / 3 PM / 6 PM) that
  correctly reflows the rest of the queue when something is deleted or added
- YouTube OAuth upload with native `publishAt` scheduling — YouTube's own
  servers fire the publish, not this app
- Runs as a queue, so you can batch a dozen clips and walk away

## Why local-first

Everything — clipping, subtitles, meme rendering, effects — runs on your own
machine via `ffmpeg` and a local Whisper model. Nothing is uploaded anywhere
until you explicitly choose to publish to YouTube. No per-render cost, no
watermarks, no waiting on a queue you don't control.

## Tech stack

- **Backend:** FastAPI, SQLModel/SQLite, ffmpeg, faster-whisper, Pillow
  (caption rendering), APScheduler
- **Frontend:** React, Tailwind CSS, Vite
- **Desktop packaging:** Tauri (optional — the web app runs standalone via
  the dev server too)

## Getting started

```bash
# Backend
cd backend
python -m venv venv && source venv/bin/activate   # venv\Scripts\activate on Windows
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000

# Frontend (new terminal)
cd frontend
npm install
npm run dev
```

Open the printed Vite URL (usually `http://localhost:5173`). You'll need
`ffmpeg`/`ffprobe` on your `PATH`, and a Google Cloud OAuth client (YouTube
Data API v3 enabled) if you want to use the upload/scheduling features.

## Project layout

```
backend/
  app/
    main.py                  # FastAPI app + router wiring
    routers/                 # HTTP endpoints (clips, meme studio, queue, ...)
    services/
      video_processor.py     # ffmpeg cut + composite + thumbnail
      subtitles.py           # faster-whisper + styled .ass captions
      scheduler.py           # fixed-slot scheduling + reflow-on-delete
      youtube_client.py      # OAuth + upload/schedule
      meme_studio/           # freeze frames, meme text, effects, audio automation
frontend/
  src/
    pages/                   # Dashboard, Upload Video, Meme Studio, Music Library, Upload Queue, Settings
    components/meme/         # Meme Studio panels (timeline, text, effects, freeze frame, audio)
    lib/api.js                # typed calls to the backend
```

## Status

Actively evolving — core clipping, subtitles, scheduling/upload, and the full
Meme Studio (freeze frames, meme text, effects, audio automation) are working
end to end. Contributions and issue reports welcome.

## License

Add your preferred license here (MIT is a common choice for a project like
this) before publishing.
