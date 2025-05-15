# Deep-Live-Cam for macOS

Real-time face swap and video deepfake with a single click and only a single image, optimized for macOS.

## Prerequisites

* **macOS**: This guide is designed for macOS, supporting both Apple Silicon (M1/M2/M3) and Intel-based Macs
* **Terminal**: Basic familiarity with Terminal commands is helpful

## Installation Steps

### 1. Install System Requirements

```sh
# Install Xcode Command Line Tools (if not already installed)
xcode-select --install
```

```sh
# Install Homebrew (if not already installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

```sh

# Install Python 3.10 and Tcl/Tk (for GUI)
brew install python@3.10 tcl-tk ffmpeg
```

### 2. Clone the Repository

```sh
git clone https://github.com/hacksider/Deep-Live-Cam.git
```

```sh
cd Deep-Live-Cam
```

### 3. Download Required Models

Download these models and place them in the `./models` directory:
- [GFPGANv1.4.pth](https://huggingface.co/hacksider/deep-live-cam/resolve/main/GFPGANv1.4.pth)
- [inswapper_128.onnx](https://huggingface.co/hacksider/deep-live-cam/blob/main/inswapper_128.onnx)

### 4. Set Up Python Environment

```sh
# Create virtual environment
python3.10 -m venv venv
```

```sh
# Activate the environment
source venv/bin/activate
```

### 5. Configure Tcl/Tk in Virtual Environment

Edit venv/bin/activate and add these lines at the end:

```sh
# Add these lines to venv/bin/activate
export PATH="/opt/homebrew/opt/tcl-tk/bin:$PATH"
export LDFLAGS="-L/opt/homebrew/opt/tcl-tk/lib"
export CPPFLAGS="-I/opt/homebrew/opt/tcl-tk/include"
export PKG_CONFIG_PATH="/opt/homebrew/opt/tcl-tk/lib/pkgconfig"
```

### 6. Install Dependencies

```sh
# Make sure your virtual environment is activated
source venv/bin/activate
```

```sh
# Install required packages
pip install -r requirements.txt
```

```sh
# IMPORTANT: Do NOT run these commands (they're in the original README but cause issues):
# pip uninstall onnxruntime onnxruntime-silicon
# pip install onnxruntime-silicon==1.13.1
```

### 7. Run Deep-Live-Cam

```sh
# For Apple Silicon Macs (recommended for best performance)
python run.py --execution-provider coreml
```

```sh
# For Intel Macs or if you prefer CPU-only mode
python run.py
```

## Camera Access

The first time you run Deep-Live-Cam with webcam features, you'll need to grant camera access:

1. Open System Settings (or System Preferences on older macOS)
2. Go to Privacy & Security > Camera
3. Find your terminal application (Terminal, iTerm, etc.)
4. Ensure the checkbox next to your terminal application is checked
5. Restart the application

## Troubleshooting

### Common Issues
* GUI doesn't appear: Make sure Tcl/Tk is properly installed and configured
* Camera not working: Check camera permissions in System Settings
* Slow performance: Ensure you're using the CoreML execution provider on Apple Silicon

### Environment Reset

If you encounter persistent issues, try resetting your environment:
```sh

# Deactivate and remove the environment
deactivate
rm -rf venv

# Then recreate it following steps 4-7 above
```
