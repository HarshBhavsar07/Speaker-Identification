# Speaker-Identification
Oizom Internship

🎙️ Personalized AI Voice Assistant Project
💼 Internship at Oizom — Deep Dive Overview

🚀 Introduction
This project was developed during an enriching internship at Oizom, where the core task was to build a Personalized AI Voice Assistant that securely recognizes and responds only to its enrolled primary user. At its heart lies a cutting-edge speaker verification system utilizing the advanced spkrec-ecapa-voxceleb pretrained neural model. The system captures a brief voice sample during enrollment, extracts unique speaker embeddings, and continuously monitors live speech inputs to verify identity in real-time, ensuring a secure and personalized interaction experience.

This repository implements a state-of-the-art voice assistant leveraging deep learning models for secure and robust speaker verification. It integrates the SpeechBrain framework’s spkrec-ecapa-voxceleb model—based on the powerful ECAPA-TDNN architecture trained on VoxCeleb datasets—to extract highly discriminative speaker embeddings for accurate biometric verification.

For precise and efficient detection of speech, it uses the Silero VAD model, delivering fast and reliable segmentation by filtering speech from silence and noise across diverse audio conditions. Together, these models enable capturing short enrollment samples, clean embedding extraction, and continuous real-time verification with low latency and high accuracy.

This combination ensures the AI assistant responds exclusively to the enrolled primary user, providing a secure and personalized experience. The repository offers modular components for enrollment, embedding extraction, live microphone streaming, similarity-based verification, and decision logic, forming a comprehensive toolkit for speaker verification-driven applications.

🛠️ Problem Statement
Traditional voice assistants often lack robust speaker identity verification, potentially responding to unauthorized users or background noise, raising security and usability concerns. This project addresses these by building a voice biometric system embedded within an AI assistant that:

🔐 Reliably identifies the enrolled user’s voice through a short enrollment session.

🔄 Continuously verifies the speaker identity during live interaction.

⛔ Filters out voices or commands from non-enrolled users.

⚡ Operates efficiently with short recordings suitable for real-time use.

🧩 System Architecture
Enrollment Module
🎤 Audio Capture: Records 5 seconds of voice input to register the user’s voice profile.

🔊 Voice Activity Detection (VAD): Uses Silero VAD to isolate fluent speech segments from raw audio.

🎯 Speaker Embedding Extraction: Processes cleaned speech with spkrec-ecapa-voxceleb (SpeechBrain) to create fixed-dimensional embeddings uniquely characterizing the speaker.

💾 Data Persistence: Saves raw audio and embeddings locally for seamless subsequent verification without repeated enrollment.

Verification Module
🎧 Audio Stream Listening: Continuously monitors live microphone input in overlapping 3-second chunks.

🗣️ Speech Segmentation: Applies VAD to isolate intentional speech.

🧠 Embedding Extraction: Computes embeddings on-the-fly from these speech chunks.

✔️ Similarity Computation: Uses cosine similarity between live embeddings and enrolled embeddings.

🚦 Decision Logic: Verifies speaker if similarity exceeds threshold (e.g., 0.6); otherwise, restricts microphone usage.

Continuous Recognition Loop
🔄 Implements streaming audio buffering with thread-safe queues for real-time, always-on experience.

⚙️ Executes fast verification in background threads to minimize latency.

🎯 Ensures stable verification by requiring consecutive positive matches before allowing interaction.

🧰 Technologies & Libraries Used
SpeechBrain: Open-source speech tech framework providing the spkrec-ecapa-voxceleb pretrained model.

Silero Voice Activity Detector: Precise and efficient speech segment detection.

SoundDevice: Real-time audio recording and streaming.

TorchAudio: Audio processing utilities for loading, resampling, and waveform tensor manipulation.

NumPy: Numerical computation and data handling.

PyTorch: Deep learning framework powering neural network models.

Resemblyzer: Auxiliary embeddings extraction for comparative analysis.

⚙️ Design Decisions & Challenges
⏱️ Short Enrollment Duration: 5 seconds recording balances user convenience and embedding quality; at least 3 seconds of fluent speech ensures robustness.

🎚️ Threshold Fine-Tuning: Critical cosine similarity threshold calibrated to balance false acceptances and rejections.

🔄 Real-Time Processing: Achieved low latency with smart buffer management, multithreading, and asynchronous audio capture.

🔇 Noise & Variability Handling: Advanced VAD models maintain embedding quality amid diverse acoustic environments.

🏗️ Integration Complexity: Seamlessly coordinated audio I/O, VAD, embedding models, and similarity computations within a real-time pipeline.

📊 Performance Evaluation
False Acceptance Rate (FAR): Percentage of unauthorized speakers incorrectly accepted.

False Rejection Rate (FRR): Percentage of authorized speakers incorrectly rejected.

Equal Error Rate (EER): Balanced accuracy measure between FAR and FRR.

Results show promising accuracy between 90-96%, depending on testing conditions. Continuous live verification logs assist in iterative tuning and threshold refinement.

🌟 Personal Learnings and Contributions
Gained deep expertise in speaker recognition, embedding generation, and verification methods.

Familiarized with state-of-the-art pretrained models and adapted them for real-world use.

Enhanced skills in audio signal processing, streaming, voice activity detection, and buffer management.

Developed end-to-end biometric security voice assistant pipelines.

Strengthened debugging and optimization of multithreaded audio applications.

Experienced collaborative development in a product-oriented environment with deployment-ready code.

🚀 Future Extensions
👥 Multi-user enrollment with speaker ID classification.

⚙️ Adaptive thresholding based on environmental noise.

🔐 Text-dependent verification using passphrases.

🎙️ Integration with Automatic Speech Recognition (ASR) for personalized command processing.

🔄 Cross-device synchronization for enrollment and verification.

🖥️ Intuitive Graphical User Interface (GUI) for enrollment and verification management.

🎯 Conclusion
This internship project represents a significant technological achievement in developing a fully functional personalized AI voice assistant using deep learning and open-source speech technologies. It showcases a practical, secure voice biometric authentication workflow, laying a strong foundation for advanced, user-friendly voice-driven AI systems.

Conclusion:-

This internship project represents a significant technological achievement, wherein a cutting-edge, fully functional personalized AI voice assistant was developed leveraging deep learning and open-source speech technologies. It demonstrates the feasibility and practical workflow of biometric voice authentication for secure user interactions. The project stands as a strong foundation for building more advanced, user-friendly voice-driven AI systems.
