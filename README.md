<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="web/brand/logo-light.svg">
  <source media="(prefers-color-scheme: light)" srcset="web/brand/logo.svg">
  <img src="web/brand/logo.svg" alt="HyperClips" width="320" />
</picture>

### Turn Long Videos & Live Streams into Viral Vertical Shorts — Automatically

[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-141414?style=flat-square)](https://www.python.org/)
[![FFmpeg](https://img.shields.io/badge/FFmpeg-Required-141414?style=flat-square)](https://ffmpeg.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-76E000?style=flat-square)](LICENSE)

</div>

---

**HyperClips** is a self-hosted AI clipping and scheduling studio for YouTube, Twitch, Kick, and uploaded videos. Paste a link or record a live stream, pick your framing (including 4-frame Split Screen for Facecam + Gameplay), generate zero-delay vertical Shorts with animated bounce captions and emoji titles, and schedule them into peak USA & Europe upload slots — strictly **1 Short per slot**.

---

## Key Features

- **Zero-Delay Two-Stage Rendering:** Cuts the exact `[start, end]` segment first with combined video and audio aligned at `0.000s`, then crops facecam & gameplay and burns animated captions on that cut clip — eliminating audio/video drift.
- **4-Frame Precision Split-Screen Picker:** Choose **Split screen** for any video or live stream, pick the clearest of 4 extracted frames, resize Top (Facecam) & Bottom (Gameplay) boxes with 8 corner/edge handles + live 9:16 preview, and save them as the creator's default.
- **USA & Europe Prime-Time Scheduler (1 Short per Slot):** Automatically schedules clips into peak US & US/EU overlap windows (`11:00 AM`, `4:30 PM`, `7:30 PM`, `9:30 PM ET`), displaying `ET`, `CET`, and `IST` times together.
- **Dedicated Post & Schedule Page:** View all finished clips per creator, select **Category: Gaming → Game** (including `GTA VI`, `GTA V`, `Fortnite`, `Valorant`, etc.), click any clip to schedule, or batch-schedule top-scored clips in one click.
- **Emoji Post Titles & Animated Captions:** Generates high-energy post titles with relevant emojis while keeping on-video hooks clean and word-timed bounce/clean/hype captions crisp.
- **Unlimited Clip Count & Instant Stop:** Choose how many clips you want (no 20-clip cap) — transcription and clipping stop immediately once your desired clip count is reached.
- **Local Speech-to-Text:** Downloads and runs the fast local speech model (~480 MB) automatically on first start — no speech API fees.
- **Editorial Silk Studio UI:** Clean, full-width, distraction-free web app (`web/index.html`) with no build step required.

---

## Quick Start (Install & Run)

### Prerequisites
- **Python 3.10+**
- **FFmpeg** (`ffmpeg` and `ffprobe` on `PATH`):
  - **Windows:** `winget install Gyan.FFmpeg`
  - **macOS:** `brew install ffmpeg`
  - **Ubuntu / Debian:** `sudo apt install ffmpeg`

### 1. Clone into a short permanent folder
> **Windows tip:** Clone into a short path like `C:\hyperclips` so Windows never hits the 260-character path limit when loading native libraries.

```bash
git clone https://github.com/munaf270501/HyperClips.git C:\hyperclips
cd C:\hyperclips
```

### 2. Install Python dependencies
```bash
python -m venv .venv
# Windows PowerShell:
.\.venv\Scripts\Activate.ps1
# macOS / Linux:
# source .venv/bin/activate

pip install -r requirements.txt
```

### 3. Configure `.env` (Bring Your Own Keys)
Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```
No API keys are bundled in this repository. You can either:
- Add your AI key (**Gemini**, **Groq**, **OpenAI**, **Claude**, **OpenRouter**, or **Ollama**) directly inside the app under **Settings → AI**, **or**
- Set it in `.env`.

### 4. Start HyperClips
```bash
python -m studio.server
```
Open **http://127.0.0.1:8765/app** in your browser. On first start, HyperClips automatically downloads the ~480 MB local speech model (or pre-download it with `python -m studio.fetch_model`).

### Run with Docker
```bash
cp .env.example .env
docker compose up -d --build
```

---

## How to Use HyperClips

1. **Add Your AI Key:** Go to **Settings → AI**, choose your provider (e.g. Gemini, Groq, OpenAI, or local Ollama), and save your API key.
2. **Create a Creator Workspace:** Click **+ New creator** in the sidebar, enter the streamer's name and YouTube/Twitch/Kick channel link, and choose default layout, style, and clip count.
3. **Clip a Video or Live Stream:**
   - **Videos / URLs:** Paste any YouTube, Twitch, or Kick link (or drag & drop a local video file) on **Home** or **All videos**, choose how many clips you want, and press **Make clips**.
   - **Live Streams:** Open a creator and click **Start recording**, pick your layout (`Split screen`, `Gameplay`, `Blurred fill`, `Crop`, or `Zoom`), and if using **Split screen**, select the clearest of 4 frames and size your Top (Facecam) & Bottom (Gameplay) boxes. Check **Save as creator default** to reuse those boxes automatically across all future streams/videos for that creator.
4. **Post & Schedule (US / Europe Prime Time):**
   - Connect your YouTube, Instagram, or TikTok account under **Settings → Connected apps**.
   - Open **Post & Schedule** in the sidebar, pick a creator and game (e.g. `Gaming → GTA VI`), and click **Schedule** on any clip (or **Schedule Top 4 Clips**) to queue **1 Short per US/EU prime slot**:

| Slot | US Eastern (`ET`) | Europe (`CET`) | India (`IST`) | Audience Focus |
|---|---|---|---|---|
| **1** | `11:00 AM ET` | `17:00 CET` | `9:30 PM IST` | Best US + Europe overlap |
| **2** | `4:30 PM ET` | `22:30 CET` | `3:00 AM IST` | US after-work/school + EU evening |
| **3** | `7:30 PM ET` | `01:30 CET` | `6:00 AM IST` | US East Coast prime evening |
| **4** | `9:30 PM ET` | `03:30 CET` | `8:00 AM IST` | US West Coast prime evening |

---

## Project Structure

| Path | Description |
|---|---|
| `studio/server.py` | FastAPI web routes, request limits, security headers |
| `studio/render.py` | Two-stage zero-delay FFmpeg cutting, layouts, and ASS bounce captions |
| `studio/jobs.py` | Video/URL clipping pipeline and fair background queue |
| `studio/live.py` | Live stream recording, buffering, 4-frame capture, and auto-clipping |
| `studio/posting.py` | USA/Europe prime-time 1-Short-per-slot scheduler & publisher |
| `studio/brain.py` | AI prompts for viral clip detection, emoji titles, hooks, and caption fixes |
| `studio/games.py` | Game detection, Roman-numeral labels (`GTA VI`), and gaming hashtags |
| `studio/transcribe.py` | Local speech-to-text engine (`sherpa-onnx`) |
| `web/index.html` | Complete single-file web studio (plain HTML, CSS, JS — no build step) |
| `tests/run.py` | Full end-to-end automated test suite |

---

## Running Tests

```bash
python tests/run.py
```
Starts an isolated throwaway server on a spare port with an empty data folder — no API keys required.

---

## License

MIT — see [LICENSE](LICENSE).
