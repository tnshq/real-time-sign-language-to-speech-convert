# Sign Language to Text and Speech Conversion

A real-time American Sign Language (ASL) recognition system that converts hand gestures into text and speech using deep learning and computer vision.

## 📋 Overview

This project implements a complete pipeline for recognizing fingerspelling-based American Sign Language gestures from a webcam feed. The system uses:

- **Computer Vision**: Hand detection and tracking with MediaPipe and OpenCV
- **Deep Learning**: Convolutional Neural Network (CNN) for gesture classification
- **Text-to-Speech**: Real-time audio output of recognized gestures
- **GUI**: Interactive tkinter-based interface for user-friendly interaction

The application displays live video feed, recognizes individual sign language letters/gestures, builds words from sequences of gestures, and provides both text output and audio feedback.

## 🎯 Features

- **Real-time Hand Detection**: Uses MediaPipe for robust hand tracking from webcam
- **Gesture Recognition**: CNN model trained on American Sign Language fingerspelling (A-Z)
- **Text Output**: Displays recognized characters and full words in the GUI
- **Speech Synthesis**: Converts recognized text to speech using pyttsx3
- **Smart Word Prediction**: Spell-checking and word suggestions using pyenchant
- **Interactive GUI**: Built with tkinter showing:
  - Live video feed from webcam
  - Current detected character
  - Full sentence being built
  - Word suggestions for corrections

## 📦 Project Structure

```
Sign-Language-To-Text-and-Speech-Conversion/
├── final_pred.py                 # Main application with GUI
├── prediction_wo_gui.py          # Prediction without GUI interface
├── data_collection_final.py      # Data collection tool for training
├── data_collection_binary.py     # Alternative data collection method
├── cnn8grps_rad1_model.h5        # Pre-trained CNN model
├── white.jpg                     # Blank image for hand gesture processing
├── requirements.txt              # Python dependencies
├── AtoZ_3.1/                     # Training dataset
│   ├── A/ through Z/            # Image folders for each letter
├── README.md                     # This file
└── INSTRUCTIONS_FOR_REPUBLISH.md # GitHub republish instructions
```

## ⚙️ Prerequisites

- **Python 3.8+**
- **macOS** (for optimal performance) or Linux/Windows
- **Webcam**: Required for real-time video capture
- **Conda** or **pip** for package management

## 🚀 Installation

### Step 1: Clone the Repository

```bash
git clone https://github.com/tnshq/real-time-sign-language-to-speech-convert.git
cd Sign-Language-To-Text-and-Speech-Conversion
```

### Step 2: Create a Python Virtual Environment

Using Conda:
```bash
conda create -n asl-recognition python=3.10
conda activate asl-recognition
```

Or using venv:
```bash
python3 -m venv asl-env
source asl-env/bin/activate  # On macOS/Linux
# or
asl-env\Scripts\activate     # On Windows
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

**Dependencies include:**
- `opencv-python` - Computer vision library
- `mediapipe` - Hand detection and tracking
- `tensorflow-macos` / `tensorflow` - Deep learning framework
- `keras` - Neural network API
- `pyttsx3` - Text-to-speech engine
- `pyenchant` - Spell checking and word suggestions
- `cvzone` - Computer vision utilities
- `Pillow` - Image processing

## 🎮 How to Run

### ✅ Recommended Method (macOS with VS Code)

1. Open the project in VS Code
2. Install the Python extension if needed
3. Select your Python environment (bottom-right corner)
4. Open `final_pred.py`
5. Press **F5** to run with debugging

This method provides the most stable performance on macOS.

### 💻 Terminal Method

```bash
# Activate virtual environment
conda activate asl-recognition

# Run the main application
python final_pred.py
```

### Alternative: Run Without GUI

For prediction-only mode without the graphical interface:

```bash
python prediction_wo_gui.py
```

## 🛠️ How It Works

### 1. **Hand Detection**
   - MediaPipe detects and tracks the user's hand in the webcam feed
   - The hand bounding box is extracted with offset padding
   - Only one hand is tracked at a time

### 2. **Hand Gesture Extraction**
   - The detected hand region is processed and normalized
   - Hand landmarks (finger joints) are extracted
   - The gesture is drawn on a white background for consistency

### 3. **CNN Classification**
   - The normalized gesture image is fed to the pre-trained CNN model (`cnn8grps_rad1_model.h5`)
   - The model outputs a confidence score for each of the 26 letters (A-Z)
   - The letter with the highest confidence is selected

### 4. **Character Recognition**
   - A temporal filter (counting mechanism) is applied to prevent flickering
   - Multiple consecutive detections of the same character confirm the gesture
   - The character is added to the current word when confidence is sufficient

### 5. **Word Building & Correction**
   - Characters are combined to form words
   - The system maintains a history of the last 10 characters
   - pyenchant provides spell-checking and word suggestions
   - Users can select corrections from suggested words

### 6. **Text-to-Speech**
   - Recognized text is converted to speech using pyttsx3
   - Speech rate can be adjusted (default: 100 words per minute)
   - Users can click the "Speak" button to hear the output anytime

### 7. **GUI Display**
   - Real-time video feed with detected hand highlighted
   - Current detected character displayed
   - Full sentence/word being built shown in real-time
   - Word suggestions displayed as interactive buttons
   - Clear and Speak buttons for user control

## 🎓 Training Your Own Model

To train a custom model with your own data:

### Collect Data

```bash
python data_collection_final.py
```

This script will:
- Capture hand gestures from your webcam
- Save images organized by letter (A-Z)
- Store them in the `AtoZ_3.1/` directory

### Train the CNN

Prepare your training script to:
- Load images from `AtoZ_3.1/` directories
- Normalize and augment the data
- Train a CNN model with appropriate layers
- Save the trained model as `cnn8grps_rad1_model.h5`

## 🖱️ Using the GUI

### Main Controls

- **Webcam Display**: Shows real-time video feed
- **Character Display**: Shows the currently detected sign language gesture
- **Sentence Display**: Shows the complete sentence being built
- **Word Suggestions**: 4 buttons showing spell-check suggestions
- **Clear Button**: Clears the current sentence
- **Speak Button**: Speaks the entire sentence aloud

### Workflow

1. Position your hand in front of the webcam
2. Perform sign language gestures for each letter
3. The application recognizes and displays characters
4. Complete words are formed automatically
5. Use word suggestions to correct misspellings
6. Click "Speak" to hear the complete sentence
7. Click "Clear" to start a new sentence

## 📊 Model Details

- **Architecture**: Convolutional Neural Network (CNN)
- **Input**: 400x400 pixel hand gesture images
- **Output**: 26 classes (A-Z letters)
- **Training Data**: 8 groups with radius 1 augmentation
- **Model File**: `cnn8grps_rad1_model.h5` (Keras/TensorFlow format)
- **Accuracy**: ~97% without background constraints, ~99% with clean background and good lighting

## ⚠️ Troubleshooting

### Application Crashes on macOS

**Problem**: "Python quit unexpectedly" error  
**Solution**: Run from VS Code (F5) instead of terminal. This avoids GPU context conflicts between tkinter, OpenCV, and MediaPipe.

### Hand Not Detected

- Ensure adequate lighting
- Position hand within the webcam frame
- Try moving closer or adjusting distance
- Make sure hand is visible in the frame

### Poor Recognition Accuracy

- Ensure proper hand positioning and lighting
- Train the model with more diverse hand gesture samples
- Verify the model file `cnn8grps_rad1_model.h5` is in the project root
- Try adjusting character confidence threshold in the code

### Missing Dependencies

```bash
pip install --upgrade -r requirements.txt
```

## 📝 License

Maintained by: **tnshq** (GitHub: [tnshq](https://github.com/tnshq))

## 🤝 Contributing

Contributions are welcome! Feel free to submit issues or pull requests to improve the project.

## 📬 Support

For issues, questions, or suggestions, please open a GitHub issue in the repository.

---

**Note**: This project is designed for accessibility and educational purposes, helping to bridge communication gaps for individuals using American Sign Language.
