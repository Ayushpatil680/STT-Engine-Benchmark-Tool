Intern Task T4
STT Engine Benchmark Tool
Comparing Vosk, Web Speech API, and AssemblyAI using Word Error Rate (WER)
Python  •  HTML/JS  •  3 Engines  •  WER Metric  •  3s Audio Sample
Project Structure

T4-STT-Benchmark/
├── benchmark.py      ← Vosk offline STT engine runner
├── index.html        ← Web Speech API browser sandbox
├── sample-3s.mp3     ← Shared test audio sample
└── README.md
Engine Overview

Three STT engines are evaluated using the same 3-second audio sample. WER (Word Error Rate) is the primary accuracy metric — lower is better.

Engine	Mode	Approx. WER	Speed	Setup
Vosk	Offline	~0.10	Fast	Medium
Web Speech API	Cloud	~0.12	Fast	Low
AssemblyAI	Cloud	~0.05–0.08	Moderate	Low
WER = (Substitutions + Deletions + Insertions) / Total Reference Words
Engine 1 — Vosk (Local Offline)

Runs entirely offline using a local Kaldi-based acoustic model. No internet connection or API key required.

Setup steps
1	Install the library
pip install vosk
2	Download the model
Download vosk-model-small-en-us-0.15 from alphacephei.com/vosk/models and unzip into a folder named model/ in the project root.
3	Convert audio to WAV
Vosk requires mono 16-bit PCM audio. Run: ffmpeg -i sample-3s.mp3 -ac 1 -ar 16000 sample-3s.wav Then update AUDIO_FILE = "sample-3s.wav" in benchmark.py.
4	Run the benchmark
python benchmark.py

Expected output
{
    "engine": "vosk_local_offline",
    "time_sec": 1.45,
    "transcript": "your transcribed text here",
    "approx_wer": 0.10
}
Engine 2 — Web Speech API (Browser / Cloud)

Uses the browser's built-in cloud speech recognition (Google backend on Chrome). No installation required — runs entirely in index.html.

Setup steps
1	Serve locally (Chrome only)
python -m http.server 8080 Then visit: http://localhost:8080/index.html
2	Start recognition
Click 'Start Voice Recognition', then speak or play the audio near the microphone. The JSON metric block populates automatically on completion.

⚠️  Note: The Web Speech API captures microphone input only — it does not process audio files directly. Use a virtual audio cable for reproducible results.

Expected output
{
    "engine": "web_speech_api",
    "time_sec": 2.10,
    "transcript": "your transcribed text here",
    "approx_wer": 0.12
}
Engine 3 — AssemblyAI (Written Comparison)

AssemblyAI is a cloud REST API with high accuracy and rich features. Evaluated as a written benchmark point — no live integration script is included.

Property	Details
Type	Cloud REST API
Accuracy	High — supports speaker diarization, punctuation, and custom vocabulary
Latency	~2–5s for short clips (network dependent)
Approx. WER	~0.05–0.08 on clean audio
Pricing	Free tier available; pay-per-minute above quota
Best For	Production-grade transcription pipelines
Requirements

Dependency	Purpose
Python 3.7+	Runtime for benchmark.py
vosk	Offline STT engine
ffmpeg	MP3 → WAV audio conversion
Google Chrome	Web Speech API sandbox (browser)
Notes

•	Vosk's WER improves significantly with the full vosk-model-en-us-0.22 model (~1.8 GB) compared to the small model (~40 MB).
•	AssemblyAI requires an API key from assemblyai.com (free tier available).
•	The Web Speech API is only supported in Google Chrome; Firefox and Safari do not implement it.
•	All three engines use the same sample-3s.mp3 file as the ground-truth audio input.
T4 — STT Engine Benchmark Tool  •  Intern Task Report
