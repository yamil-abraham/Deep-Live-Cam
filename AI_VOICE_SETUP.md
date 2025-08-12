# AI Voice Integration Setup

This guide explains how to set up and use the AI Voice feature with ElevenLabs for real-time voice conversion.

## Features

- Real-time voice conversion using ElevenLabs Speech-to-Speech API
- **Automatic voice mapping**: When you select a face image (like Messi.jpeg), the AI Voice automatically uses the corresponding voice
- Toggle on/off directly from the UI
- Customizable voice selection and mapping
- Low latency streaming (2-second chunks)
- Smart face recognition based on filename patterns

## Prerequisites

1. An ElevenLabs account with API access
2. Python dependencies (automatically installed with requirements.txt)

## Setup Instructions

### 1. Get Your ElevenLabs API Credentials

1. Sign up at [ElevenLabs](https://elevenlabs.io) if you haven't already
2. Go to your [API Keys page](https://elevenlabs.io/api)
3. Copy your API key
4. Choose a voice ID from the [voices page](https://api.elevenlabs.io/v1/voices) or use the default

### 2. Configure Environment Variables

Create a `.env` file in the project root (if it doesn't exist):

```bash
cp .env.example .env
```

Edit the `.env` file and add your credentials:

```env
ELEVENLABS_API_KEY=your_api_key_here
ELEVENLABS_VOICE_ID=21m00Tcm4TlvDq8ikWAM  # Or your preferred voice ID
```

### 3. Install Dependencies

The required dependencies are already in requirements.txt:

```bash
pip install -r requirements.txt
```

### 4. Test the Configuration

Run the test script to verify everything is set up correctly:

```bash
python test_ai_voice.py
```

## Using AI Voice in Deep-Live-Cam

1. Launch the application:
   ```bash
   python3.10 run.py --execution-provider coreml  # For macOS
   # or
   python run.py  # For other platforms
   ```

2. **Select a face image** (this is key!):
   - Click "Select face" and choose an image like `Messi.jpeg`
   - The system will automatically detect and map the voice:
     - `Messi.jpeg` → Messi Voice
     - `Trump.png` → Trump Voice
     - `Obama.jpg` → Obama Voice
     - `Elon-Musk*.png` → Elon Voice
     - Other files → Default Voice
   - You'll see a status message showing which voice was selected

3. **Enable AI Voice**:
   - In the main window, you'll see the "AI Voice" toggle switch
   - Toggle it ON to enable real-time voice conversion
   - The status will show "AI Voice enabled - Using [Voice Name] voice"

4. When enabled:
   - Your microphone input will be captured in 2-second chunks
   - Audio is sent to ElevenLabs for voice conversion using the mapped voice
   - Converted audio is played back through your speakers

5. **Voice automatically changes** when you select different face images
   - Select different faces and the voice will automatically switch
   - No need to manually configure voices each time

6. Toggle it OFF to disable voice conversion

## Customization

### Add Custom Voice Mappings

Edit the `voice_mappings.json` file to add your own voice mappings:

```json
{
  "voice_mappings": {
    "your_person": {
      "patterns": ["yourname", "nickname"],
      "voice_id": "your_elevenlabs_voice_id",
      "voice_name": "Your Person Voice",
      "description": "Custom voice for your person"
    }
  }
}
```

### Change Existing Voice Mappings

Edit the voice IDs in `voice_mappings.json`. Popular ElevenLabs voices:
- Rachel: `21m00Tcm4TlvDq8ikWAM`
- Adam: `pNInz6obpgDQGcFmaJgB`
- Bella: `EXAVITQu4vr4xnSDxMaL`

### Test Voice Mappings

Run the test script to verify your mappings work:
```bash
python test_voice_mapping.py
```

### Adjust Chunk Duration

In `modules/elevenlabs_streaming.py`, modify:
```python
CHUNK_DURATION = 2  # seconds (1-3 recommended)
```

### Change Audio Quality

Modify these settings in `modules/elevenlabs_streaming.py`:
```python
MODEL_ID = "eleven_multilingual_sts_v2"  # or "eleven_english_sts_v2"
OUTPUT_FORMAT = "mp3_44100_128"  # or other formats
```

## Troubleshooting

### "API credentials not configured" error
- Ensure your `.env` file exists and contains valid credentials
- Check that the API key is active in your ElevenLabs dashboard

### No audio playback
- **macOS**: Uses built-in `afplay`
- **Windows**: Uses default media player
- **Linux**: Install `mpg123` or `ffmpeg`:
  ```bash
  sudo apt-get install mpg123  # Ubuntu/Debian
  sudo yum install mpg123      # Fedora/RHEL
  ```

### High latency
- Reduce `CHUNK_DURATION` to 1 second (may increase API calls)
- Check your internet connection
- Use a voice with lower latency settings

## API Limits

Be aware of your ElevenLabs plan limits:
- Free tier: Limited characters per month
- Each 2-second chunk uses approximately 100-200 characters
- Monitor your usage at [ElevenLabs Dashboard](https://elevenlabs.io)

## Security Notes

- Never commit your `.env` file to version control
- Keep your API key secure and rotate it regularly
- The `.env` file is already in `.gitignore`