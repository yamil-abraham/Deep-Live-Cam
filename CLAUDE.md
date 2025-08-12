# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Deep-Live-Cam is an educational research project for studying real-time face swapping and deepfake technology. The codebase is used for understanding AI-generated media, detection methods, and defensive security research.

## Development Commands

### Environment Setup
```bash
# Create and activate virtual environment (Python 3.10 required for macOS)
python3.10 -m venv venv
source venv/bin/activate  # On macOS/Linux
venv\Scripts\activate     # On Windows

# Install dependencies
pip install -r requirements.txt
```

### Running the Application
```bash
# Basic execution (CPU mode)
python run.py

# With GPU acceleration
python run.py --execution-provider cuda     # NVIDIA GPUs
python run.py --execution-provider coreml   # Apple Silicon (M1/M2/M3)
python run.py --execution-provider directml # Windows DirectML

# macOS users must use Python 3.10 specifically
python3.10 run.py --execution-provider coreml
```

### Type Checking
```bash
# Run mypy for type checking (configuration in mypi.ini)
mypy modules/
```

## Architecture Overview

The project follows a modular architecture with clear separation of concerns:

### Core Components

1. **Entry Point** (`run.py`): Minimal entry that delegates to `modules.core.run()`

2. **Core Module** (`modules/core.py`): 
   - Command-line argument parsing
   - Execution provider configuration (CPU, CUDA, CoreML, DirectML)
   - Resource management and memory limits
   - Main processing pipeline coordination

3. **UI Module** (`modules/ui.py`):
   - CustomTkinter-based GUI
   - Camera enumeration and preview
   - Real-time webcam processing interface
   - Face mapping configuration UI

4. **Face Analysis** (`modules/face_analyser.py`):
   - InsightFace integration for face detection
   - Face clustering and mapping logic
   - Multi-face processing support

5. **Frame Processors** (`modules/processors/frame/`):
   - `face_swapper.py`: Core face swapping using InSwapper model
   - `face_enhancer.py`: GFPGAN-based face enhancement
   - Modular processor pipeline system

6. **Utilities** (`modules/utilities.py`):
   - FFmpeg integration for video processing
   - Temporary file management
   - Video frame extraction and audio restoration

### Model Files

Models are stored in the `models/` directory:
- `inswapper_128.onnx` / `inswapper_128_fp16.onnx`: Face swapping model
- `GFPGANv1.4.pth`: Face enhancement model

### Key Global Settings

Managed through `modules/globals.py`:
- Execution providers and threading configuration
- Video processing options (keep_fps, keep_audio, keep_frames)
- Face processing modes (many_faces, map_faces, mouth_mask)
- NSFW filtering and safety checks

## Platform-Specific Notes

### macOS (Apple Silicon)
- Must use Python 3.10 (not 3.11 or 3.13)
- Requires `python-tk@3.10` from Homebrew
- Use `onnxruntime-silicon==1.13.1` for M1/M2/M3 chips
- Run with `python3.10` command explicitly

### Windows
- Requires Visual Studio 2022 Runtimes
- FFmpeg installation via PowerShell: `iex (irm ffmpeg.tc.ht)`
- DirectML support available for AMD GPUs

### Linux
- Standard Python 3.10+ support
- CUDA 11.8.0 required for NVIDIA GPU acceleration

## Testing Guidelines

According to CONTRIBUTING.md, before making changes test:
- Realtime faceswap with face enhancer enabled/disabled
- Map faces functionality
- Camera enumeration accuracy
- Real-time FPS stability (no drops)
- GPU usage over 15+ minutes (prevent overloading)
- Application responsiveness

## Branching Strategy

- `premain`: Initial push target for testing
- `experimental`: Large/disruptive changes
- `main`: Production branch after rigorous testing

## Security Considerations

This codebase is for educational and defensive security research only. It includes:
- NSFW content filtering
- Ethical use disclaimers
- Content restriction checks

When analyzing this code, focus on:
- Detection methods for synthetic media
- Understanding deepfake artifacts
- Defensive countermeasures
- Security implications analysis

## Important File Paths

- Main entry: `run.py`
- Core logic: `modules/core.py`
- UI components: `modules/ui.py`
- Face processing: `modules/processors/frame/`
- Configuration: `modules/globals.py`
- Models directory: `models/`
- Temporary files: `temp/` (auto-created)