SignBridge 🤟: Real-Time Indian Sign Language Decoder (MVP)
Live Demo: https://sign-bridge-chi.vercel.app/

🌟 The Vision
SignBridge is a proof-of-concept (PoC) web application that translates Indian Sign Language (ISL) into text in real-time. It was built to solve a specific problem: bridging the communication gap between the deaf community and hearing society.

The core philosophy of this project was Accessibility and Zero-Cost Edge AI. The entire product—from data processing to AI training to web deployment—was designed to run entirely on the user's device (client-side), ensuring 100% privacy and zero recurring server costs.

⚠️ Honest Disclaimer: This is an MVP
This project is a functional Minimum Viable Product (MVP) built to prove that real-time, browser-based sign language translation is achievable with free tools.

Current Limitations:

Isolated Word Recognition: The AI currently recognizes individual words (isolated signs), not continuous sentence framing. Continuous Sign Language Recognition (CSLR) requires more complex architectures (like Transformers or CTC models) and sentence-level datasets.
Accuracy: The model achieves ~65% validation accuracy. While functional for a demo, real-world deployment requires a larger, more diverse dataset to handle varying lighting, skin tones, and signing speeds.
Hardware Constraints: Running MediaPipe tracking and TensorFlow.js inference simultaneously can cause CPU bottlenecking on older mobile devices, resulting in reduced frame rates.
Environment Dependent: Performance is optimal in well-lit environments with a plain background.
🛠️ Tech Stack & Integrations
AI Inference: TensorFlow.js (Running 100% in the browser)
Model Architecture: Bidirectional LSTM (Bi-LSTM) Neural Network
Computer Vision: Google MediaPipe (Hands) for landmark extraction
Training Environment: Google Colab (Python, TensorFlow/Keras, OpenCV)
Data Source: Kaggle Indian Sign Language Video Dataset (3,630 videos, 61 signs)
Hosting: Vercel (PWA)
🧠 How It Works (The AI Pipeline)
Instead of processing heavy raw video pixels, the app uses a lightweight 3-step pipeline:

Landmark Extraction (MediaPipe): The camera feed is processed by MediaPipe Hands, which extracts 21 3D coordinates (x, y, z) for each hand (42 total). This reduces the data size by 95% compared to raw video.
Sequence Buffering: The app collects 30 frames of hand coordinates (30 x 126 features) to capture the temporal motion of a sign.
Bi-LSTM Inference (TensorFlow.js): The 30-frame sequence is fed into a pre-trained Bi-LSTM neural network. The network understands the forward and backward motion of the sequence to predict the specific ISL sign.
🚀 Development Journey & Problem Solving
This project was uniquely challenging as it was developed entirely via a mobile phone environment, requiring creative problem-solving:

Data Pipeline Optimization: Processed 3,600+ videos using MediaPipe Holistic in Google Colab, saving progress in chunks to Google Drive to bypass Colab's session timeout limits.
Keras 3 to TFJS Conversion: Overcame a critical compatibility bug where the tensorflowjs_converter failed to read Keras 3 InputLayers. Solved by downgrading to Legacy Keras 2 (tf-keras) for the final model export.
Mobile CPU Throttling: Initial versions caused mobile browsers to freeze. Solved by switching from MediaPipe Holistic (533 points) to MediaPipe Hands (42 points) and implementing an isPredicting lock to prevent overlapping inference cycles.
📁 Repository Structure
index.html - The complete front-end UI, MediaPipe integration, and TF.js inference logic.
model.json - The trained Bi-LSTM model architecture (TensorFlow.js format).
group1-shard1of1.bin - The trained model weights.
🔮 Future Scope (Phase 3 & Beyond)
Train on a larger, open-source ISL dataset (e.g., ISLRTC) to improve accuracy.
Implement a Continuous Sign Language Recognition (CSLR) model for full sentence translation.
Add Text-to-Speech (TTS) output to translate signs directly into spoken audio.
Package as a native React Native/Flutter app for better hardware acceleration.
Built with resourcefulness and a desire to make technology accessible to all.






