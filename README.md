Speaker-Identification — Personalized AI Voice Assistant

A production-style implementation of a personalized AI voice assistant built during an internship at Oizom, featuring secure, real-time speaker verification using ECAPA-TDNN embeddings from the SpeechBrain spkrec-ecapa-voxceleb model and precise voice activity detection via Silero VAD. The system enrolls a primary user, extracts robust speaker embeddings, and verifies identity continuously during live interactions for a secure, user-exclusive experience.

Introduction

This repository implements a speaker verification–driven assistant that recognizes and responds exclusively to the enrolled primary user, combining deep learning–based speaker embeddings with reliable VAD-driven speech segmentation. It integrates SpeechBrain’s spkrec-ecapa-voxceleb model—an ECAPA-TDNN architecture trained on VoxCeleb—to compute discriminative embeddings, and Silero VAD for fast, accurate speech/non-speech separation, enabling low-latency, high-accuracy verification in realistic environments.

Problem statement

Conventional voice assistants risk responding to unintended speakers or background audio, affecting security and usability. This project embeds voice biometrics into the assistant to:

Reliably identify the enrolled user from a short enrollment sample.

Continuously verify identity during live interactions.

Filter out non-enrolled voices and accidental audio.

Operate in real time with short, fluent recordings.

System architecture

Enrollment module

Audio capture: Record ~5 seconds to establish a voice profile.

Voice activity detection: Use Silero VAD to isolate fluent speech segments.

Embedding extraction: Generate fixed-length ECAPA-TDNN embeddings via spkrec-ecapa-voxceleb.

Persistence: Store raw audio and embeddings for subsequent verification.

Verification module

Stream listening: Monitor microphone input in overlapping windows.

Speech segmentation: Apply VAD to extract intentional speech.

On-the-fly embeddings: Compute embeddings for each speech chunk.

Similarity and decisioning: Use cosine similarity against enrolled embedding; accept above threshold.

Continuous loop

Streaming buffers with thread-safe queues for always-on behavior.

Low-latency background verification via multithreading.

Stability via consecutive positive matches before enabling interaction.

Technologies and libraries

SpeechBrain spkrec-ecapa-voxceleb (ECAPA-TDNN; VoxCeleb-trained) for speaker embeddings.

Silero VAD for high-performance speech detection via torch.hub or pip.

PyTorch/Torchaudio for modeling and audio processing.

NumPy for numerical operations.

SoundDevice for real-time audio I/O.

Resemblyzer for auxiliary comparisons and analysis.

Design decisions and challenges

Short enrollment (≈5 seconds): Balances convenience and embedding quality; ≥3 seconds fluent speech recommended.

Threshold tuning: Calibrate cosine similarity to balance FAR/FRR and minimize errors.

Real-time pipeline: Buffered streaming and multithreading to reduce verification latency.

Noise robustness: VAD pre-filtering preserves embedding integrity in varied conditions.

Integration: Coordinating audio I/O, VAD, embeddings, and similarity within a tight loop.

Performance evaluation

FAR: Rate of unauthorized users accepted.

FRR: Rate of authorized users rejected.

EER: Balance point of FAR and FRR; ECAPA-TDNN models achieve low EER on VoxCeleb benchmarks.
Observed accuracy ranges between ~90% and 96% depending on conditions; iterative logs aid threshold and pipeline refinement.

Personal learnings and contributions

Advanced speaker recognition: Embedding generation, scoring, and verification workflows.

Model integration: Practical use of ECAPA-TDNN with SpeechBrain and VAD pre-processing.

Audio engineering: Streaming, segmentation, buffering, and concurrency for low-latency UX.

End-to-end security pipeline: From enrollment to decision logic for biometric access control.

Production-minded debugging: Multithreaded optimization and stability improvements.

Future extensions

Multi-user enrollment with speaker ID classification.

Adaptive thresholds responsive to environmental noise.

Text-dependent verification via passphrases.

ASR integration for personalized command understanding.

Cross-device enrollment sync.

GUI for intuitive enrollment and verification management.

Installation and setup

Install core libraries:
pip install torch torchaudio speechbrain silero-vad sounddevice numpy resemblyzer

Models used:

Speaker verification: SpeechBrain spkrec-ecapa-voxceleb (ECAPA-TDNN, VoxCeleb).

Voice activity detection: Silero VAD for speech timestamps.

How to load models in code:

SpeechBrain speaker verification: Load from Hugging Face model hub; compute embeddings and cosine similarity for verification.

Silero VAD: Load via torch.hub and extract speech timestamps, or install via pip.

Conclusion

This internship project delivers a fully functional, speaker verification–powered AI assistant using modern speech technologies, demonstrating a practical, secure voice-biometric workflow that is extensible to multi-user, text-dependent, and ASR-integrated scenarios.
