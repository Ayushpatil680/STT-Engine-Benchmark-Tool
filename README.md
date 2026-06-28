T4 — STT Engine Benchmark Tool

A multi-engine Speech-to-Text (STT) benchmarking suite that evaluates transcription accuracy and speed across three engines: Vosk (offline), Web Speech API (browser), and AssemblyAI (cloud) — using Word Error Rate (WER) as the primary accuracy metric.


Project Structure

T4-STT-Benchmark/
├── benchmark.py       # Vosk offline STT engine runner
├── index.html         # Web Speech API browser sandbox
├── sample-3s.mp3      # Shared test audio sample
└── README.md


Engines Overview

Engine 1 — Vosk (Local Offline)

File: benchmark.py

Runs entirely offline using a local Kaldi-based acoustic model. No internet connection or API key required.

Setup:


Install dependencies:


bash   pip install vosk


Download the lightweight English model:
vosk-model-small-en-us-0.15
Unzip the model into a folder named model/ in the project root.
Place your audio file (sample-3s.mp3) in the same directory.



Note: Vosk requires a .wav file (mono, 16-bit PCM). Convert your MP3 before running:

bashffmpeg -i sample-3s.mp3 -ac 1 -ar 16000 sample-3s.wav

Then update AUDIO_FILE = "sample-3s.wav" in benchmark.py.



Run:

bashpython benchmark.py

Output (JSON):

json{
    "engine": "vosk_local_offline",
    "time_sec": 1.45,
    "transcript": "your transcribed text here",
    "approx_wer": 0.10
}


Engine 2 — Web Speech API (Browser / Cloud)

File: index.html

Uses the browser's built-in cloud speech recognition (Google's backend on Chrome). Captures microphone input and outputs a JSON metric block.

Setup: No installation needed. Requires Google Chrome.

Run:


Open index.html in Chrome (serve via a local HTTP server for best results):


bash   python -m http.server 8080
   # Then visit http://localhost:8080/index.html


Click "Start Voice Recognition" and speak (or play the audio near the mic).
The JSON metric block populates automatically on completion.


Output (JSON):

json{
    "engine": "web_speech_api",
    "time_sec": 2.10,
    "transcript": "your transcribed text here",
    "approx_wer": 0.12
}


Engine 3 — AssemblyAI (Written Comparison)

AssemblyAI is a cloud-based STT API with high accuracy and rich features. It was evaluated as a third benchmark point without a live integration script.

PropertyDetailsTypeCloud REST APIAccuracyHigh — supports speaker diarization, punctuation, and custom vocabularyLatency~2–5s for short clips (network dependent)Approx. WER~0.05–0.08 on clean audioPricingFree tier available; pay-per-minute above quotaBest ForProduction-grade transcription pipelines


Benchmark Comparison Summary

EngineModeApprox. WERSpeedSetup ComplexityVoskOffline~0.10FastMedium (local model required)Web Speech APICloud~0.12FastLow (browser only)AssemblyAICloud~0.05–0.08ModerateLow (API key required)


WER (Word Error Rate) = (Substitutions + Deletions + Insertions) / Total Reference Words. Lower is better.




Test Audio

sample-3s.mp3 — a 3-second audio clip used as the shared ground-truth input across all engines.


Requirements

DependencyPurposePython 3.7+Runtime for benchmark.pyvoskOffline STT engineffmpegMP3 → WAV conversionGoogle ChromeWeb Speech API sandbox


Notes


The Web Speech API does not process audio files directly — it listens via the microphone. Play the audio near the mic or use a virtual audio cable for reproducible results.
Vosk's WER improves significantly with the full vosk-model-en-us-0.22 model at the cost of download size (~1.8 GB vs ~40 MB for the small model).
AssemblyAI requires an API key from assemblyai.com.

ShareContentsample-3s.mp3mp3benchmark.py63 linespyindex.html116 lineshtml,
