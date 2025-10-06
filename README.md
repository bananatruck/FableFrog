# 🐸 FableFrog

**Text‑to‑Speech Storytelling, from prompt to performance — in minutes.**

> Hackathon prototype built and shipped in **36 hours** using **OpenAI function calling** + **ElevenLabs** with **20+ emotion controls**, presented via a **Gradio** front‑end.

---

## Table of Contents

* [Highlights](#highlights)
* [Demo](#demo)
* [How It Works](#how-it-works)
* [Features](#features)
* [Tech Stack](#tech-stack)
* [Quickstart](#quickstart)

  * [Prerequisites](#prerequisites)
  * [Environment Variables](#environment-variables)
  * [Installation](#installation)
  * [Run Locally](#run-locally)
* [Usage](#usage)

  * [Gradio UI](#gradio-ui)
  * [CLI (optional)](#cli-optional)
* [Emotion Controls](#emotion-controls)
* [Project Structure](#project-structure)
* [Benchmarks & Metrics](#benchmarks--metrics)
* [Roadmap](#roadmap)
* [Contributing](#contributing)
* [License](#license)
* [Acknowledgements](#acknowledgements)
* [Maintainers](#maintainers)

---

## Highlights

* 🚀 **Built and shipped in 36 hours** using OpenAI API (function calling) + ElevenLabs.
* 🎭 **20+ emotion parameters** mapped to narration prosody for expressive TTS.
* 🧰 **Parsed raw SDK docs & GitHub repos** to integrate LLM outputs — **50% faster** integration time.
* 🖥️ **Gradio front‑end** served **200+ users** during the hackathon; **+70% engagement** vs. baseline demo.
* 🧪 Fully reproducible: one‑command startup, .env‑first config.

---

## Demo

* **Live Demo:** *coming soon*
* **Video/GIF:** `assets/demo.gif`
* **Screenshots:** `assets/screenshot_ui.png`

> Tip: Add a quick screen capture showing prompt → scene breakdown → audio playback.

---

## How It Works

```
[User Prompt]
     ↓
[OpenAI (function calling)] ──► returns NarrationPlan JSON (scenes, lines, emotions)
     ↓
[Emotion Mapper] ──► normalizes 20+ controls → ElevenLabs style/prosody
     ↓
[ElevenLabs TTS] ──► generates per‑scene clips (streamed)
     ↓
[Gradio UI] ──► preview, stitch, download
```

**NarrationPlan (LLM function):**

```json
{
  "title": "The Moonlit Marsh",
  "scenes": [
    {
      "id": 1,
      "speaker": "narrator",
      "emotion": { "tone": "wonder", "energy": 0.6, "warmth": 0.7, "pace": 0.95 },
      "text": "In the hush of the marsh, a chorus began..."
    }
  ]
}
```

---

## Features

* **Prompt → Structured Story:** LLM returns a scene list with emotion tags.
* **Expressive TTS:** 20+ normalized emotion knobs (tone, energy, pace, pitch, breathiness, suspense, humor, etc.).
* **Per‑Scene Rendering:** Generate, preview, re‑render scenes independently.
* **Fast Iteration:** Cached prompts and voice presets for rapid tweaks.
* **Gradio Web App:** One‑page UI to author, play, and export.

---

## Tech Stack

**Languages & Frameworks:** Python • OpenAI API • ElevenLabs • OS • Gradio

Optional libs: `pydantic`, `python-dotenv`, `uvicorn`, `fastapi` (if exposing REST), `soundfile`/`pydub` for stitching.

---

## Quickstart

### Prerequisites

* Python **3.10+**
* API keys for **OpenAI** and **ElevenLabs**

### Environment Variables

Create a `.env` file in the repo root:

```
OPENAI_API_KEY=sk-...
ELEVENLABS_API_KEY=eleven-...
ELEVENLABS_VOICE_ID=<default_voice_id>
# Optional
GRADIO_SERVER_NAME=0.0.0.0
GRADIO_SERVER_PORT=7860
```

### Installation

```bash
# 1) Clone
git clone https://github.com/<you>/FableFrog.git && cd FableFrog

# 2) Create & activate venv (recommended)
python -m venv .venv && source .venv/bin/activate  # Windows: .venv\\Scripts\\activate

# 3) Install deps
pip install -r requirements.txt
```

**`requirements.txt` (minimal):**

```
openai>=1.0.0
python-dotenv
gradio>=4.0.0
elevenlabs>=1.0.0
pydantic>=2.0.0
pydub
```

### Run Locally

```bash
python app.py
# or
gradio app.py
```

Open [http://localhost:7860](http://localhost:7860).

---

## Usage

### Gradio UI

1. Enter a **story prompt** (e.g., “bedtime tale about a curious frog”).
2. Click **Generate Plan** → review scene list & emotions.
3. Click **Synthesize** to render TTS per scene.
4. **Refine** emotions/voice and re‑render selected scenes.
5. **Export** stitched audio (`.mp3`/`.wav`).

### CLI (optional)

```bash
python -m fablefrog.cli \
  --prompt "A cozy autumn story" \
  --voice $ELEVENLABS_VOICE_ID \
  --out out/story.wav
```

---

## Emotion Controls

FableFrog exposes human‑readable knobs and maps them to ElevenLabs style/prosody under the hood.

**Available controls (non‑exhaustive):**

* `tone` (neutral, wonder, suspense, humor, gentle, solemn)
* `energy` (0–1)
* `pace` (0.5–1.5)
* `pitch` (-6 to +6 semitones)
* `warmth`, `clarity`, `breathiness`, `tension`, `urgency`, `emphasis`, `curiosity`, `awe`, `joy`, `sadness`, `anger`, `fear`, `surprise`, `calm`, `whisper` (bool)

**Example:**

```json
{
  "tone": "suspense",
  "energy": 0.55,
  "pace": 0.9,
  "pitch": -1,
  "breathiness": 0.3,
  "emphasis": 0.6
}
```

> Note: These are normalized controls; internal mapping may differ per ElevenLabs voice capability.

---

## Project Structure

```
FableFrog/
├─ app.py                 # Gradio entrypoint
├─ fablefrog/
│  ├─ llm.py              # OpenAI client + function schema
│  ├─ tts.py              # ElevenLabs client + emotion mapping
│  ├─ pipeline.py         # prompt → plan → audio orchestration
│  ├─ ui.py               # Gradio components & state
│  └─ utils.py            # I/O, caching, stitching
├─ assets/                # demo.gif, screenshots
├─ requirements.txt
├─ .env.example
└─ README.md
```

---

## Benchmarks & Metrics

* **200+ users** served during hackathon session.
* **+70% user engagement** vs. baseline (time‑on‑page & re‑render interactions).
* **50% reduction** in integration time by parsing raw SDK docs & GitHub repos programmatically.

> Reproduce by running `scripts/collect_metrics.py` (optional).

---

## Roadmap

* [ ] Multi‑voice casting (narrator + characters)
* [ ] SRT/SSML export with emotion tags
* [ ] Cloud deployment template (Spaces/Render/Fly)
* [ ] Live streaming playback per token
* [ ] In‑UI audio editing (trim, crossfade)

---

## Contributing

PRs welcome! Please open an issue to discuss substantial changes.

* Run `ruff`/`black` before committing.
* Add tests for emotion mapping edge cases.

---

## License

MIT — see `LICENSE`.

---

## Acknowledgements

* **OpenAI** for function calling that structures story plans.
* **ElevenLabs** for expressive TTS.
* **Gradio** for rapid UI prototyping.

---

## Maintainers

* **Keshav Jindal** — GitHub: [bananatruck](https://github.com/bananatruck)

> Questions? Open an issue with the label `question`.
