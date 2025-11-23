# 3D Audio HRTF Controller

A real-time 3D spatial audio processing application that transforms your system audio into immersive binaural audio using Head-Related Transfer Function (HRTF) technology. Perfect for ASMR, music listening, gaming, and any audio experience that benefits from spatial positioning.

## Why I Made This

While listening to audiobooks, I found myself getting drowsy. Since I'd been researching spatial audio recently, I created a spatial audio-like effect. Because it uses a human ear model, it's best experienced with headphones.

Then while listening to YouTube ASMR, I discovered that adding this made it even more immersive and tingly. For certain unusual videos, the effect is probably quite remarkable...

## Features

- **Real-time Processing**: Captures and processes system audio in real-time with low latency
- **HRTF-based Spatial Audio**: Uses precomputed HRTF data to create authentic 3D audio positioning
- **Interactive Controls**: Adjust azimuth (0-360°) and elevation (-40° to 90°) in real-time
- **Auto Rotation**: Automatically rotates the audio position with adjustable speed
- **Cross-platform Support**: Works on Windows and macOS
- **Virtual Audio Device Integration**: Seamlessly captures system audio output

## System Requirements

- **Operating System**: Windows 10/11 or macOS 10.14+
- **Audio Setup**: 
  - Windows: VB-CABLE virtual audio driver
  - macOS: BlackHole 2ch virtual audio driver
- **Hardware**: Headphones recommended for best experience
- **Python**: Python 3.8+ (if running from source)

## Installation

### Step 1: Install Virtual Audio Driver

#### macOS
1. Download BlackHole 2ch from: https://existential.audio/blackhole/download/
2. Install BlackHole 2ch
3. Set BlackHole 2ch as your audio output device in System Preferences > Sound

#### Windows
1. Download VB-CABLE from: https://download.vb-audio.com/Download_CABLE/VBCABLE_Driver_Pack45.zip
2. Install `VBCABLE_Setup_x64.exe`
3. Set 'CABLE Input' as Default Playback Device in Sound Settings

### Step 2: Run the Application

#### Option A: Use Pre-built Executable
- Download the latest release for your platform
- Run the executable (no installation required)

#### Option B: Run from Source
```bash
# Install dependencies
pip install numpy sounddevice PySide6

# Run the application
python main.py
```

## Usage

### Basic Operation

1. **Start the Application**: Launch the 3D Audio HRTF Controller
2. **Verify Setup**: Check the status label - it should show "Virtual Audio: OK"
3. **Play Audio**: Play any audio from your system (music, videos, games, etc.)
4. **Adjust Position**: Use the sliders to control the audio position:
   - **Azimuth**: Horizontal position (0° = front, 90° = right, 180° = back, 270° = left)
   - **Elevation**: Vertical position (-40° = below, 0° = horizontal, 90° = above)

### Controls

- **Azimuth Slider**: Rotate the audio source horizontally (0-360°)
- **Elevation Slider**: Adjust vertical position (-40° to 90°)
- **Auto Rotate Checkbox**: Enable/disable automatic rotation
- **Speed Control**: Adjust rotation speed (1-180°/s)
- **Reset Position**: Return to center position (0°, 0°) and stop auto-rotation
- **Output Device**: Select your preferred audio output device

### Tips for Best Experience

- **Use Headphones**: HRTF effects work best with headphones, as they provide proper channel separation
- **Start with Auto Rotation**: Enable auto-rotation to experience the 3D effect immediately
- **Adjust Speed**: Try different rotation speeds to find what feels most immersive
- **Experiment with Position**: Manually adjust azimuth and elevation to position sounds precisely

## How It Works

### Overview

The application captures system audio output through a virtual audio device, applies HRTF-based spatial audio processing, and outputs the processed audio to your speakers or headphones.

### Technical Details

#### 1. Audio Capture
- **Input Source**: Virtual audio device (VB-CABLE on Windows, BlackHole on macOS)
- **Sample Rate**: 48 kHz
- **Channels**: Stereo (2 channels)
- **Block Size**: 512 samples (low-latency processing)

#### 2. HRTF Processing

**HRTF (Head-Related Transfer Function)** is a mathematical model that describes how sound waves interact with the human head, ears, and torso. It captures:
- **Interaural Time Difference (ITD)**: Time delay between ears
- **Interaural Level Difference (ILD)**: Volume difference between ears
- **Spectral Cues**: Frequency-dependent filtering from head and ear shape

**Processing Pipeline**:
1. **HRTF Lookup**: For each audio position (azimuth, elevation), find the closest matching HRTF from the precomputed database
2. **Spatial Positioning**: Apply ±30° offset to left and right channels to create stereo separation
3. **Convolution**: Convolve input audio with HRTF impulse responses (FIR filters) for each ear
4. **Overlap-Add**: Use overlap buffers to handle convolution tails and maintain audio continuity
5. **Gain Control**: Apply gain (0.3) to prevent clipping and optimize output level

#### 3. Audio Output
- **Output Device**: User-selectable audio output device
- **Format**: 32-bit float, stereo
- **Real-time**: Low-latency streaming with callback-based processing

### Audio Processing Flow

```
System Audio Output
    ↓
Virtual Audio Device (VB-CABLE/BlackHole)
    ↓
InputStream (Capture at 48kHz, 512 samples/block)
    ↓
HRTF Processing:
  - Find closest HRTF for current position
  - Apply spatial positioning (±30° offset)
  - Convolve with left/right ear filters
  - Handle overlap-add for continuity
    ↓
Gain Control (0.3x)
    ↓
OutputStream (Playback to selected device)
    ↓
Your Headphones/Speakers
```

### HRTF Data

The application uses precomputed HRTF data stored in the `assets/` directory:
- **hrtf_params.txt**: Contains azimuth and elevation parameters for each measurement
- **hrtf_left.f32**: Left ear impulse responses (FIR filters) for all directions
- **hrtf_right.f32**: Right ear impulse responses (FIR filters) for all directions

These files contain measurements from a KEMAR (Knowles Electronics Manikin for Acoustic Research) dummy head, which provides a standard HRTF dataset.

### Position Control

- **Azimuth (0-360°)**: Horizontal angle around the listener
  - 0° = Front
  - 90° = Right
  - 180° = Back
  - 270° = Left

- **Elevation (-40° to 90°)**: Vertical angle
  - -40° = Below horizontal
  - 0° = Horizontal plane
  - 90° = Directly above

### Auto Rotation

The auto-rotation feature continuously updates the azimuth angle based on:
- **Rotation Speed**: Degrees per second (configurable 1-180°/s)
- **Time-based**: Uses system time to calculate smooth rotation
- **Thread-safe**: Runs in a separate thread with Qt signal-based updates

## Troubleshooting

### Virtual Audio Device Not Found

**Problem**: Application shows "Virtual Audio: NO INSTALL"

**Solution**:
- **macOS**: Ensure BlackHole 2ch is installed and appears in System Preferences > Sound
- **Windows**: Ensure VB-CABLE is installed and appears in Sound Settings
- Restart the application after installing the virtual audio driver

### No Audio Output

**Problem**: Application runs but no sound is heard

**Solutions**:
1. Check that your system audio is set to the virtual audio device (BlackHole/VB-CABLE)
2. Verify the output device selection in the application
3. Check system volume levels
4. Ensure audio is actually playing from another application

### Audio Latency or Glitches

**Problem**: Audio has noticeable delay or artifacts

**Solutions**:
1. Close other audio applications that might be using the audio device
2. Check system audio buffer settings
3. Ensure sufficient CPU resources are available
4. Try a different output device

### Application Won't Start

**Problem**: Application exits immediately with error messages

**Solutions**:
1. Ensure all HRTF files exist in the `assets/` directory
2. Verify virtual audio driver is properly installed
3. Check that at least one valid output device is available
4. Review console output for specific error messages

## Technical Specifications

- **Audio Format**: 32-bit float, 48 kHz sample rate
- **Processing**: Real-time convolution with overlap-add
- **Latency**: ~10-20ms (block size dependent)
- **HRTF Database**: KEMAR measurements
- **GUI Framework**: PySide6 (Qt6)
- **Audio I/O**: sounddevice (PortAudio)

## Limitations

- Currently supports Windows and macOS only
- Requires virtual audio driver installation
- Best experienced with headphones
- Fixed gain level (not user-adjustable in current version)
- HRTF data is based on average head measurements (may vary for individuals)

## Future Improvements

Potential enhancements for future versions:
- User-adjustable gain control
- Preset positions for common scenarios
- Audio visualization (spectrum, 3D position display)
- Support for additional HRTF datasets
- Linux support
- Recording/export functionality

## License

[Add your license information here]

## Contributing

[Add contribution guidelines if applicable]

## Acknowledgments

- HRTF data based on KEMAR measurements
- Virtual audio drivers: BlackHole (macOS) and VB-CABLE (Windows)
- Audio processing libraries: NumPy, sounddevice
