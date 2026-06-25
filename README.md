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

🔧 The Build Process: An Engineering Case Study
This project was uniquely challenging because it was developed entirely via a mobile phone environment, requiring creative problem-solving to bypass hardware limitations.

Phase 1: Data Pipeline (Google Colab + Kaggle API)

Accessed a cloud supercomputer (Google Colab) via a mobile browser to download a 3.2GB dataset of 3,630 ISL videos from Kaggle.
Built a custom Python pipeline using OpenCV and MediaPipe to extract 126-dimensional hand landmarks from every video, converting heavy video files into lightweight NumPy arrays.
Constraint Solved: Colab's free tier disconnects after ~90 minutes. To prevent losing 2.5 hours of processing time, I engineered a chunked-saving system that backed up progress to Google Drive every 50 videos, allowing seamless resuming.
Phase 2: Model Training & Keras 3 Bug

Architected a Bi-LSTM neural network in TensorFlow to classify the 30-frame temporal sequences into 61 sign categories.
Constraint Solved: Google Colab upgraded to Keras 3, which broke the tensorflowjs_converter (it stripped the InputLayer shape, crashing the browser). I solved this by rolling back to Legacy Keras (tf-keras) to successfully export the model for the web.

Phase 3: Edge Deployment & Mobile Optimization

Deployed the model as a Progressive Web App (PWA) using TensorFlow.js so inference runs 100% client-side.
Constraint Solved: Initial versions caused mobile browsers to freeze because the CPU was trying to run MediaPipe tracking and AI predictions 30 times a second. I optimized this by switching from MediaPipe Holistic (533 points) to MediaPipe Hands (42 points) and implementing an isPredicting lock to throttle predictions and keep the video feed smooth.

Phase 4: Hosting

Connected the GitHub repository to Vercel for continuous deployment and global CDN hosting, resulting in a zero-cost, production-ready web app.
Why this is powerful for your resume:
If you just write "I trained a model," it sounds like a school project. But when you write about "engineering a chunked-saving system to bypass Colab timeouts" and "implementing an isPredicting lock to throttle predictions," you sound like a Senior AI Engineer who knows how to ship products in the real world.

Put this in your README, and recruiters will be incredibly impressed!


Send a Message


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

Built with resourcefulness and a desire to make technology accessible to all.♥️♥️






