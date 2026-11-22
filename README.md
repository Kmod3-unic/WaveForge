# WaveForge — Audacity Clone for Android

WaveForge is an open-source multi-track audio recorder, waveform editor, and DSP studio for Android built with **Kotlin**, **Jetpack Compose**, **Material 3**, and Android's native `AudioRecord`, `AudioTrack`, and `MediaCodec` APIs.

## Key Features

- **Multi-Track Timeline & Interactive Waveform**: Real-time Compose `Canvas` peak rendering with zoom controls, tap-to-seek playhead, and drag-to-select ranges.
- **Per-Track Controls**: Individual **Mute (M)**, **Solo (S)**, **Gain** slider, and track deletion for every track.
- **Live Recording & Real-Time Mixing**: 44.1 kHz 16-bit PCM microphone capture via `AudioRecord` with live waveform updates and dBFS input/output VU metering, paired with a low-latency `AudioTrack` (`MODE_STREAM`) stereo mixer.
- **Non-Destructive Editing**: **Cut**, **Copy**, **Paste**, **Trim to Selection**, **Delete Selection**, **Split at Playhead**, **Loop Selection**, and a 25-step **Undo / Redo** history stack.
- **Audacity-Style DSP Effects & Generators**:
  - **Effects (`AudioDsp.kt`)**: Normalize, Amplify, Fade In, Fade Out, Reverse, 3-Band Biquad EQ (120 Hz / 1 kHz / 6 kHz), Echo / Delay, Noise Gate, and Change Speed / Pitch.
  - **Generators**: Sine Tone, Square Wave, Sawtooth Wave, White Noise, and Silence tracks.
- **Audio Import & WAV Export**: Import MP3, M4A, AAC, OGG, FLAC, and WAV files via `MediaExtractor` + `MediaCodec` (with a fast-path `WavCodec` reader), and export stereo 16-bit PCM `.wav` mixdowns via Android's Storage Access Framework.

## Tech Stack

| Layer | Technology |
| :--- | :--- |
| Language | Kotlin 2.0 |
| UI Framework | Jetpack Compose & Material 3 |
| Architecture | MVVM (`EditorViewModel` + `StateFlow`) |
| Audio Capture & Playback | Android `AudioRecord` & `AudioTrack` (`MODE_STREAM`) |
| Audio Decoding | `MediaExtractor`, `MediaCodec`, custom `WavCodec` |
| DSP Processing | Pure Kotlin (biquad filters, resampling, envelope gate) |
| Testing | JUnit 4 (`AudioDspTest.kt`) |

## Project Structure

```text
WaveForge/
├── build.gradle.kts
├── settings.gradle.kts
├── gradle/libs.versions.toml
└── app/
    ├── build.gradle.kts
    └── src/
        ├── main/
        │   ├── AndroidManifest.xml
        │   └── java/com/waveforge/audioeditor/
        │       ├── MainActivity.kt
        │       ├── audio/
        │       │   ├── AudioEngine.kt
        │       │   ├── AudioImporter.kt
        │       │   └── WavCodec.kt
        │       ├── dsp/AudioDsp.kt
        │       ├── model/AudioModels.kt
        │       ├── state/EditorViewModel.kt
        │       └── ui/
        │           ├── EditorScreen.kt
        │           ├── components/
        │           │   ├── EffectsSheet.kt
        │           │   ├── TransportBar.kt
        │           │   └── WaveformTrackView.kt
        │           └── theme/Theme.kt
        └── test/java/com/waveforge/audioeditor/
            └── AudioDspTest.kt
```

## Getting Started

1. Clone or extract the repository and open the `WaveForge` directory in **Android Studio** (Ladybug / 2024.2+ recommended).
2. Ensure **JDK 17** and **Android SDK 35** are installed and let Gradle sync.
3. Run the `app` configuration on a physical Android device or emulator running **Android 8.0 (API 26)+**.
4. Grant the **Microphone (`RECORD_AUDIO`)** permission when prompted to record live audio.

## Permissions

- `RECORD_AUDIO` — Required for live microphone recording.
- `READ_MEDIA_AUDIO` / `READ_EXTERNAL_STORAGE` (maxSdkVersion 32) — Used when importing audio files from device storage.
