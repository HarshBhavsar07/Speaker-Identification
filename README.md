Speaker-Identification — Personalized AI Voice Assistant

A practical implementation of a personalized AI voice assistant built during an internship at Oizom, featuring secure, real-time speaker verification using ECAPA-TDNN embeddings via SpeechBrain’s spkrec-ecapa-voxceleb model and precise voice activity detection with Silero VAD. The system enrolls a primary user, extracts robust speaker embeddings, and continuously verifies identity during live interactions to ensure an exclusive and secure experience.


Introduction

This project implements a speaker verification–driven assistant that responds only to the enrolled user by combining deep learning–based speaker embeddings with VAD-guided speech segmentation. It integrates the spkrec-ecapa-voxceleb model (ECAPA-TDNN trained on VoxCeleb) for discriminative embeddings and Silero VAD for accurate speech detection, enabling low-latency, reliable verification in real-world conditions.


Problem statement

Traditional voice assistants can respond to unintended speakers or background audio, impacting both security and usability. This project embeds voice biometrics to:

  Reliably identify the enrolled user from a short enrollment recording.
  Verify identity continuously during live interactions.
  Filter out commands from non-enrolled users.
  Operate efficiently with short, fluent recordings for real-time use.
  

System architecture

  Enrollment module
  
  Audio capture: Records ~5 seconds of speech to build a voice profile.
  Voice activity detection: Uses Silero VAD to isolate fluent speech segments.
  Embedding extraction: Generates fixed-length ECAPA-TDNN embeddings using spkrec-ecapa-voxceleb.
  Data persistence: Stores raw audio and embeddings for seamless later verification.

  Verification module

  Stream listening: Monitors live microphone input in overlapping windows.
  Speech segmentation: Applies VAD to extract intentional speech.
  On-the-fly embeddings: Computes embeddings from detected speech chunks.
  Similarity and decision: Uses cosine similarity against the enrolled profile and verifies above a defined threshold.

  Continuous recognition loop

  Streaming buffers and thread-safe queues for an always-on experience.
  Background verification with multithreading to minimize latency.
  Stability via consecutive positive matches before allowing interaction

  Technologies and libraries

  SpeechBrain: spkrec-ecapa-voxceleb (ECAPA-TDNN; VoxCeleb) for speaker embeddings.
  Silero VAD: High-accuracy speech detection.
  PyTorch and Torchaudio: Model inference and audio processing.
  NumPy: Numerical operations and data handling.
  SoundDevice: Real-time audio capture and streaming.
  Resemblyzer: Auxiliary embedding comparisons for analysis.

  Design decisions and challenges

  Short enrollment duration (~5 seconds): Balances convenience with embedding quality; at least 3 seconds of fluent speech recommended.  
  Threshold fine-tuning: Cosine similarity threshold calibrated to balance false acceptances and false rejections.
  Real-time processing: Low latency achieved through buffered streaming, multithreading, and asynchronous audio capture.
  Noise and variability: VAD pre-filtering preserves embedding quality across diverse acoustic environments.
  Integration complexity: Coordinated audio I/O, VAD, embedding models, and similarity scoring in a real-time pipeline.

  Performance evaluation
  
  FAR (False Acceptance Rate): Percentage of unauthorized speakers accepted.
  FRR (False Rejection Rate): Percentage of authorized speakers rejected.
  EER (Equal Error Rate): Balance point of FAR and FRR.
  Observed results indicate accuracy in the ~90–96% range depending on testing conditions; continuous logs support iterative threshold tuning and pipeline refinement.

  Personal learnings and contributions

  Gained practical expertise in speaker recognition, embedding generation, and verification workflows.
  Integrated state-of-the-art pretrained models for real-world deployment scenarios.
  Improved skills in streaming audio, VAD, buffering, and concurrency for low-latency systems.
  Built end-to-end biometric security pipelines from enrollment to decision logic.
  Strengthened debugging and optimization of multithreaded audio applications in a product-oriented environment.

  Future extensions
  
  Multi-user enrollment with speaker identification.
  Adaptive thresholding based on environmental noise.
  Text-dependent verification via passphrases.
  ASR integration for personalized command handling.
  Cross-device synchronization of enrollment and profiles.
  GUI for intuitive enrollment and verification management.


Installation and setup

  Required libraries:
    pip install torch torchaudio speechbrain silero-vad sounddevice numpy resemblyzer

  Models used:
    Speaker verification: spkrec-ecapa-voxceleb (ECAPA-TDNN, VoxCeleb).
    Voice activity detection: Silero VAD for speech timestamp extraction.

  Model usage overview:
    Load the SpeechBrain speaker verification model to compute embeddings and perform cosine similarity scoring for verification.
    Load Silero VAD (e.g., via torch hub or pip) to obtain speech timestamps and segment audio before embedding extraction.


Conclusion

This internship project delivers a fully functional, secure, and extensible speaker verification–powered voice assistant. It demonstrates a practical, real-time voice biometric workflow and provides a strong foundation for features like multi-user support, text-dependent verification, and ASR-driven personalized commands.
