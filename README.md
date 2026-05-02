# Translating-ASL-Fingerspelling-into-Audible-Speech

SignLingua is a real-time American Sign Language (ASL) recognition system designed to bridge the communication gap between deaf and hearing individuals using deep learning and computer vision. The platform uses a lightweight custom CNN to recognize ASL alphabet gestures from a standard webcam feed, translating them into text (and optionally speech) with low latency and high accuracy. Built as a full-stack application with a React frontend and a FastAPI/WebSocket backend, the system emphasizes accessibility, affordability, and real-time performance without requiring specialized hardware. By combining efficient preprocessing, adaptive thresholding, and optimized model design, SignLingua demonstrates a practical AI-driven approach to inclusive communication.

## Project Overview
This project presents a critical analysis of deep learning pipeline design methodologies through the development of a Sign-to-Voice translation system. Sign-to-Voice translation systems convert American Sign Language (ASL) fingerspelling gestures into audible speech. We document a methodological pivot from a cloud-native architecture leveraging Google Cloud Platform (GCP) to a local, model-centric pipeline that prioritizes controllability, modularity, and reproducibility.

## Pipeline Architecture: Cloud vs. Local
Initially, the project utilized a cloud-native approach with GCP services, but encountered fundamental limitations:
* **Dataset Scale:** The initial dataset exceeded 1TB. This required high storage costs and 30-60 minute downloads per iteration.
* **Pipeline Opacity:** Distributed services made debugging difficult due to limited visibility into intermediate states.
* **Computational Constraints:** Job launch overheads of 5-10 minutes discouraged rapid iteration.

Pivoting to a local pipeline enabled rapid iteration, reducing development velocity from 5-10 minutes per iteration on the cloud to just 30 seconds locally. This transition provided complete reproducibility and transparent debugging, utilizing a monolithic Jupyter Notebook (`Final Project.ipynb`) for the final submission.

## Data Collection & Preprocessing
* **Base Dataset:** Utilized the Sign Language MNIST dataset, consisting of 28x28 grayscale images across 24 classes (A-Y, excluding J and Z).
* **Custom Validation Dataset:** Constructed a custom dataset of 1,500-2,000 images captured via team members' webcams. This validated the model on real-world data. It included varied lighting and backgrounds to bridge the gap between curated datasets and deployment.
* **Preprocessing:** Image vectors were reshaped to 28x28x1 tensors and normalized to a [0, 1] scale.
* **Adaptive Gaussian Thresholding:** Implemented for real-world deployment, emphasizing hand silhouettes. This improved live accuracy from 35% to 87.3%.

## Machine Learning Models & Performance
Three machine learning architectures were systematically explored:
* **CNN (Convolutional Neural Network):** Optimized for spatial pattern recognition. It achieved a 98.7% test accuracy with 476,264 parameters and an inference time of 0.83 ms.
* **RNN/LSTM:** Modeled temporal patterns by treating image rows as sequences. It achieved a 97.3% test accuracy with 2,145,520 parameters and an inference time of 8.69 ms.
* **Transformer:** Utilized Multi-Head Attention to capture global pixel relationships. It achieved a 97.1% test accuracy with 3,452,352 parameters and an inference time of 40.62 ms.

The CNN model emerged as the optimal solution for real-time deployment due to its high efficiency, low latency, and best accuracy-efficiency tradeoff. Character-level recognition was effectively solved with 98.7% test accuracy and 87.3% real-world accuracy.

## Future Directions
* **Character-to-Sentence Transition:** Extending the system to process continuous sign streams using sequence-to-sequence models and language modeling.
* **Domain Adaptation:** Employing adversarial learning to further bridge the gap between dataset and real-world deployment.
* **Edge Optimization:** Quantizing the model for mobile device deployment using TensorFlow Lite.
