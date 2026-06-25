 2. Install the Python dependencies:
   ```bash
   pip install openai-whisper jiwer
   
   ```
#### Frontend Setup (Web Speech API)
No installation is required. The interface runs entirely in any modern web browser supporting the Web Speech API (Google Chrome recommended).
## 5. Sample Output
### Engine 1: OpenAI Whisper (tiny)
```json
{
    "engine": "whisper_tiny",
    "time_sec": 2.1,
    "transcript": "The quick brown fox jumps over the lazy dog.",
    "approx_wer": 0.08
}

```
### Engine 2: Web Speech API
```json
{
    "engine": "web_speech_api",
    "time_sec": 1.85,
    "transcript": "the quick brown fox jumps over the lazy dog",
    "approx_wer": 0.12
}

```
"""
with open("README.md", "w") as f:
f.write(readme_content)
print("File written successfully")
```
Your `README.md` file has been compiled and is ready for use. It contains the project context, architectural metric table, deep-dive comparison covering the third engine requirement, implementation details, and sample output structure.

[file-tag: code-generated-file-0-1782377288155905480]

Here is the markdown code directly if you would like to copy it from here:

```markdown
# T4. STT Engine Benchmark Tool

## 1. Task Overview
* **Assigned To:** Ayush Ashok Patil
* **Task Title:** STT Engine Benchmark Tool
* **Module:** Speech & Audio Processing Pipeline
* **Difficulty:** Moderate
* **Duration:** 1 Week

## 2. Objective
Build a practical comparison of the leading Speech-to-Text (STT) options so the team has data to justify which engine replaces the current pipeline.
* **Why It Matters:** This task feeds directly into the eventual decision of what replaces `workers/audio_pipeline.py`.

## 3. STT Engine Benchmarking & Comparison

A comprehensive comparison was performed between the two implemented engines (**OpenAI Whisper** and **Web Speech API**) and a third researched alternative (**Vosk**) to evaluate their feasibility for replacing the current pipeline in `workers/audio_pipeline.py`.

### Architectural & Metric Comparison

| Evaluation Metric | OpenAI Whisper (`tiny`) | Browser Web Speech API | Vosk (3rd Engine Alternative) |
| :--- | :--- | :--- | :--- |
| **Cost** | 100% Free (Open-Source / Local) | Free (Built into browser client) | 100% Free (Open-Source / Local) |
| **Offline Capability** | **Yes** (Runs entirely on local hardware) | **No** (Relies on browser vendor's cloud service) | **Yes** (Runs entirely on local hardware) |
| **Language Support** | Excellent (Robust multi-lingual pre-trained datasets) | Good (Dependent on browser vendor and OS language packs) | Great (Wide variety of downloadable model languages) |
| **Ease of Integration**| Moderate (Requires Python environment, PyTorch, and FFmpeg) | Extremely Easy (Few lines of native frontend JavaScript) | Moderate (Lightweight, simple Python pip package) |
| **Hardware Overhead** | Moderate to High (Requires RAM/GPU optimization for larger models) | Zero Server Overhead (Offloaded completely to client browser) | Low (Highly optimized for resource-constrained environments) |
| **Accuracy (WER)** | High (Highly resilient to background noise and accents) | High (Leverages Google's cloud models when run in Chrome) | Moderate to High (Dependent on the specific model size loaded) |

### Deep-Dive Analysis

#### 1. OpenAI Whisper (`tiny`)
* **Pros:** Complete data privacy, robust handling of background noise, punctuation insertion out of the box, and zero ongoing API costs. Being open-source allows us to fine-tune it on domain-specific vocabulary if needed.
* **Cons:** The execution time (`time_sec`) is highly dependent on system hardware. Running it on standard CPU worker nodes without GPU acceleration can create a bottleneck in the backend pipeline.

#### 2. Browser Web Speech API
* **Pros:** Zero backend compute costs and zero deployment overhead. Transcription is fast because it offloads processing to the user's local machine or vendor cloud.
* **Cons:** **Strict Chrome-only reliability.** Firefox and Safari support is inconsistent. More critically, it lacks offline support and requires microphone permissions from the frontend, making it completely unusable for async backend background processing in `workers/audio_pipeline.py`.

#### 3. Researched Alternative: Vosk
* **Pros:** Designed specifically for lightweight, low-latency, offline applications. It executes significantly faster than Whisper on pure CPU instances and has small binary footprints (models range from 50MB to a few GBs).
* **Cons:** Out-of-the-box text formatting (capitalization, punctuation) is minimal compared to Whisper, requiring additional post-processing scripts.

### Final Recommendation for `workers/audio_pipeline.py`
Given that this task feeds directly into a backend worker architecture (`workers/audio_pipeline.py`), the **Browser Web Speech API is disqualified** as it cannot run headlessly or offline inside a backend Python queue worker. 
* **Primary Recommendation:** Integrate **OpenAI Whisper**. If backend resources permit (or if we can provide a small GPU/optimized CPU instance), Whisper offers the highest accuracy and cleanest transcript structures required for downstream processing.
* **Fallback Recommendation:** If backend compute resources are heavily constrained, **Vosk** should serve as the primary alternative due to its low memory footprint and high processing speed on standard CPU workers.

## 4. What to Build & Installation Guide

This repository contains a backend Python script that runs an audio sample through Whisper (`tiny` model) and a minimal HTML dashboard to benchmark the browser-based Web Speech API.

### Prerequisites & Installation

#### Backend Setup (Whisper)
1. Ensure `ffmpeg` is installed on your system:
   ```bash
   # Ubuntu/Debian
   sudo apt update && sudo apt install ffmpeg
   
   # macOS
   brew install ffmpeg

```
 2. Install the Python dependencies:
   ```bash
   pip install openai-whisper jiwer
   
   ```
#### Frontend Setup (Web Speech API)
No installation is required. The interface runs entirely in any modern web browser supporting the Web Speech API (Google Chrome recommended).
## 5. Sample Output
### Engine 1: OpenAI Whisper (tiny)
```json
{
    "engine": "whisper_tiny",
    "time_sec": 2.1,
    "transcript": "The quick brown fox jumps over the lazy dog.",
    "approx_wer": 0.08
}

```
### Engine 2: Web Speech API
```json
{
    "engine": "web_speech_api",
    "time_sec": 1.85,
    "transcript": "the quick brown fox jumps over the lazy dog",
    "approx_wer": 0.12
}

```
```

```
