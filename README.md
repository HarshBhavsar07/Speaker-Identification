# Speaker-Identification
Oizom Internship

Introduction:-

This project was developed during an enriching internship at Oizom, where the core task was to build a Personalized AI Voice Assistant that could securely recognize and respond only to its enrolled primary user. At its heart lies a cutting-edge speaker verification system utilizing the advanced spkrec-ecapa-voxceleb pretrained neural model. The system captures a brief voice sample during enrollment, extracts unique speaker embeddings, and continuously monitors live speech inputs to verify identity in real-time, ensuring a secure and personalized interaction experience.
This document provides a comprehensive and detailed understanding of the project’s technical flow, underlying methods, and implementation details. It walks through the key modules, including enrollment, speaker verification, voice activity detection, live audio streaming, and performance evaluation.

This repository implements a state-of-the-art personalized AI voice assistant leveraging cutting-edge deep learning models for secure and robust speaker verification. At its core, the system utilizes the spkrec-ecapa-voxceleb pretrained speaker recognition model provided by the SpeechBrain framework. This model, based on the powerful ECAPA-TDNN architecture, is trained on the VoxCeleb datasets and is capable of extracting highly discriminative speaker embeddings that enable accurate voice biometric verification with low error rates.

For precise and efficient voice activity detection (VAD), the system integrates the Silero VAD model. This lightweight, enterprise-grade VAD model delivers fast and reliable speech segmentation, filtering speech from silence and noise across diverse audio conditions. Together, these models enable the pipeline to capture short enrollment samples, extract clean speaker embeddings, and perform continuous real-time verification with low latency and high accuracy.

The combination ensures that the AI assistant responds exclusively to its enrolled primary user, providing a secure and personalized interaction experience. This repository provides modular components encompassing enrollment, embedding extraction, live microphone streaming, similarity-based verification, and decision logic, making it a comprehensive toolkit for speaker verification-driven voice applications.


Problem Statement:- 

Traditional voice assistants often do not verify speaker identity robustly, potentially responding to unauthorized users or background noise. This raises security and usability concerns. The goal here was to build a voice biometric system embedded in an AI assistant that:
Reliably identifies the enrolled user’s voice through a short initial enrollment session.
Continuously verifies the speaker identity during live interaction.
Filters out voices or commands from non-enrolled users.
Operates efficiently with short recordings suitable for real-time applications.
The project is architected with modular components working seamlessly together to achieve reliable speaker recognition:

Enrollment Module:- 

Audio Capture: The system captures 5 seconds of voice input from the user to register their voice profile.

Voice Activity Detection (VAD): Using an advanced model like Silero VAD, the system isolates fluent speech segments from raw audio.

Speaker Embedding Extraction: The cleaned speech segment is processed by the spkrec-ecapa-voxceleb pretrained model (based on SpeechBrain framework), which converts it into a fixed-dimensional vector embedding that uniquely characterizes the speaker’s voice features.

Data Persistence: The system saves both the raw processed audio and embedding vectors locally, allowing seamless usage in subsequent verification steps without the need for repeated enrollment.

Verification Module:-

Audio Stream Listening: The live microphone input is constantly monitored in 3-second overlapping chunks.

Speech Segmentation: Each audio snippet is analyzed with VAD to isolate intentional speech for verification.

Embedding Extraction: The system extracts speaker embeddings on-the-fly from these chunks.

Similarity Computation: Cosine similarity is computed between the current live speaker embedding and the enrolled user’s stored embedding.

Decision Logic: If the similarity surpasses a predefined threshold (e.g., 0.6), the speaker is verified, and the assistant responds. Otherwise, the microphone can automatically deactivate to prevent unauthorized access.

Continuous Recognition Loop:- 

To provide a real-time, always-on assistant experience, the system implements streaming audio buffering with thread-safe queues.

It executes fast verification in background threads to minimize latency.

Supports user interruption and stabilizes verification by requiring several consecutive positive matches before granting interaction permissions.

Technologies and Libraries Used:-

SpeechBrain: A powerful open-source framework for speech technologies, which provides the spkrec-ecapa-voxceleb pretrained speaker recognition model. This model leverages deep neural networks trained on VoxCeleb datasets for robust speaker embeddings.

Silero Voice Activity Detector: Enables precise and efficient detection of speech segments, improving embedding quality by eliminating silence and noise.

SoundDevice: Real-time audio recording and streaming from the system microphone.

TorchAudio: Audio processing helpers for loading, resampling, and tensor manipulation of waveform data.

NumPy: Numerical computation and data handling.

PyTorch: Underlying deep learning framework powering the speech models.

Resemblyzer (auxiliary): Sometimes used for comparative embedding extraction in related notebooks.

Design Decisions and Challenges:-

Short Enrollment Duration: Capturing audio for precisely 5 seconds balances user convenience and embedding quality. Ensuring that this short sample contains at least 3 seconds of fluent speech (detected via VAD) improves the robustness of the voiceprint.

Threshold Fine-Tuning: The cosine similarity threshold is critical. Setting it too low may grant access to impostors; too high may reject legitimate users. Empirical tuning and batch testing balance security and usability.

Real-Time Processing: Implementing an always-on verification system with low latency involved smart buffer management, multi-threading, and asynchronous audio capture.

Noise and Variability Handling: Using advanced VAD models helps maintain embedding quality amid background noise, varying microphone quality, and changing user voice intonations.

Integration Complexity: Coordinating multiple components—audio I/O, VAD, embedding models, similarity computations—into a seamless real-time pipeline posed significant architectural challenges.

Performance Evaluation:-

To quantify accuracy, the project involved batch testing with multiple recorded voices and computed metrics such as:

False Acceptance Rate (FAR): Percentage of unauthorized speakers incorrectly authenticated.

False Rejection Rate (FRR): Percentage of authorized speakers incorrectly rejected.

Equal Error Rate (EER): The balanced point between FAR and FRR for a holistic accuracy measure.

Results showed promising accuracy around 90-96% depending on test conditions, demonstrating the system’s effectiveness.

Continuous logging of similarity scores during live verification helps in further iterative system tuning and threshold refinement.

Personal Learnings and Contributions:-

Gained deep understanding of speaker recognition technologies, including voice embedding generation and speaker verification principles.

Learned to work with state-of-the-art pretrained models and adapt them for practical applications.

Improved skills in audio signal processing, real-time streaming, voice activity detection, and managing audio buffers.

Developed hands-on expertise in end-to-end voice assistant pipelines integrating biometric security.

Strengthened ability to debug multi-threaded audio applications and optimize performance.

Experience working collaboratively in a product-oriented environment with deployment-ready code.

Future Extensions:-

Possible upgrades and enhancements that can be integrated include:

Multi-user Enrollment: Supporting several authorized speakers with speaker ID classification.

Adaptive Thresholding: Dynamic adjustment of acceptance thresholds based on environmental noise levels.

Text-Dependent Verification: Adding passphrases or commands for additional security layers.

Integration with Speech Recognition: Combining speaker verification with ASR for fully personalized voice commands.

Cross-device Synchronization: Enabling enrollment and verification across multiple devices securely.

Graphical User Interface (GUI): Creating an intuitive front-end to manage enrollment and verification.

Conclusion:-

This internship project represents a significant technological achievement, wherein a cutting-edge, fully functional personalized AI voice assistant was developed leveraging deep learning and open-source speech technologies. It demonstrates the feasibility and practical workflow of biometric voice authentication for secure user interactions. The project stands as a strong foundation for building more advanced, user-friendly voice-driven AI systems.
