# 3D Audio HRTF Controller

Turn your regular audio into immersive 3D spatial audio! This app uses HRTF (Head-Related Transfer Function) magic to make sounds feel like they're coming from different directions around you. Perfect for ASMR, music, gaming, or just making your audio experience way cooler.

## Why I Made This

So here's the story: I was listening to audiobooks and kept getting sleepy. I'd been tinkering with spatial audio stuff, so I thought - why not make my own 3D audio effect? Since it mimics how your ears actually hear sounds, headphones work best.

Then I tried it with YouTube ASMR videos... wow. It made everything way more immersive and tingly. For some videos, the effect is pretty wild. You'll have to try it yourself!

## What It Does

- **Real-time magic**: Grabs your system audio and processes it on the fly with almost no delay
- **3D positioning**: Makes sounds feel like they're coming from specific directions using HRTF
- **Easy controls**: Sliders to move sounds around horizontally and vertically
- **Auto spin**: Automatically rotates the audio position - you can adjust how fast it goes
- **Works everywhere**: Windows and macOS support
- **Just works**: Captures whatever audio your system is playing

## What You Need

- **Your computer**: Windows 10/11 or macOS 10.14+
- **Virtual audio thingy**: 
  - Windows folks: VB-CABLE
  - Mac folks: BlackHole 2ch
- **Headphones**: Seriously, use headphones. It's way better that way.

## Getting Started

### First: Install the Virtual Audio Driver

You need this so the app can "listen" to what your system is playing.

#### macOS
1. Grab BlackHole 2ch from: https://existential.audio/blackhole/download/
2. Install it (just follow the installer)
3. Go to System Preferences > Sound and set BlackHole 2ch as your output device

#### Windows
1. Download VB-CABLE from: https://download.vb-audio.com/Download_CABLE/VBCABLE_Driver_Pack45.zip
2. Run `VBCABLE_Setup_x64.exe` and install it
3. In Sound Settings, set 'CABLE Input' as your Default Playback Device

### Then: Run the App

Just download the latest release for your platform and double-click to run - no installation needed!

## How to Use It

### Quick Start

1. **Launch the app** - Open 3D Audio HRTF Controller
2. **Check the status** - Should say "Virtual Audio: OK" (if not, check your driver install)
3. **Play something** - Start any audio on your computer (music, video, game, whatever)
4. **Move it around** - Use the sliders to position the sound:
   - **Azimuth**: Left/right/around you (0° = front, 90° = right, 180° = behind, 270° = left)
   - **Elevation**: Up/down (-40° = below, 0° = level, 90° = above)

### The Controls

- **Azimuth slider**: Spin the sound source around you (0-360°)
- **Elevation slider**: Move it up or down (-40° to 90°)
- **Auto Rotate checkbox**: Turn on automatic spinning
- **Speed control**: How fast it spins (1-180° per second)
- **Reset button**: Snap back to center and stop auto-rotation
- **Output device dropdown**: Pick where you want the sound to come out

### Pro Tips

- **Wear headphones**: The 3D effect is way more noticeable with headphones
- **Try auto-rotate first**: Turn it on and feel the sound moving around you
- **Play with speed**: Different speeds feel different - find what you like
- **Get creative**: Manually position sounds to match what you're watching/listening to

## How It Works (The Nerdy Part)

### The Big Picture

Your system plays audio → Virtual audio device catches it → App processes it with HRTF → You hear 3D audio. Simple!

### The Technical Stuff

#### Audio Capture
The app listens to your system audio through a virtual audio device (VB-CABLE on Windows, BlackHole on Mac). It grabs audio at 48 kHz, stereo, in small chunks of 512 samples for low latency.

#### HRTF Processing

**HRTF** stands for Head-Related Transfer Function. Fancy name, but it's basically a way to simulate how your head and ears change sounds coming from different directions. It accounts for:
- **Time differences**: Sounds reach your ears at slightly different times
- **Volume differences**: Your head blocks some sound, so one ear hears it louder
- **Frequency filtering**: Your head and ear shape change how different frequencies sound

**What the app does**:
1. **Finds the right HRTF**: Based on where you've positioned the sound (azimuth/elevation), it picks the closest matching HRTF from a database
2. **Positions it**: Applies a ±30° offset to left and right channels to create stereo separation
3. **Processes it**: Convolves (fancy word for "filters") the audio with HRTF data for each ear
4. **Keeps it smooth**: Uses overlap buffers so there are no clicks or pops
5. **Adjusts volume**: Applies a gain of 0.3 to prevent clipping

#### Audio Output
Sends the processed audio to whatever output device you picked, in 32-bit float format, in real-time.

### The Processing Flow

```
Your system audio
    ↓
Virtual audio device (catches it)
    ↓
App processes it (HRTF magic happens here)
    ↓
Your headphones/speakers
    ↓
Your ears (enjoying 3D audio!)
```

### About the HRTF Data

The app uses precomputed HRTF data that comes from KEMAR (a dummy head used for acoustic research), which is a standard dataset in the audio world. This data helps simulate how sounds change based on direction.

### Position Control

- **Azimuth (0-360°)**: How far around you the sound is
  - 0° = Right in front
  - 90° = To your right
  - 180° = Behind you
  - 270° = To your left

- **Elevation (-40° to 90°)**: How high or low
  - -40° = Below you
  - 0° = At ear level
  - 90° = Straight up

### Auto Rotation

The auto-rotate feature smoothly spins the sound around you. You control:
- **How fast**: 1-180 degrees per second
- It uses your system clock to keep the rotation smooth
- Runs in a separate thread so it doesn't freeze the app

## Technical Specs (For the Curious)

- **Audio format**: 32-bit float, 48 kHz
- **Processing**: Real-time convolution with overlap-add
- **Latency**: About 10-20ms (depends on block size)
- **HRTF data**: KEMAR measurements
- **GUI**: PySide6 (Qt6)
- **Audio I/O**: sounddevice (PortAudio)

## Current Limitations

- Only Windows and macOS (sorry Linux users, maybe later!)
- You need that virtual audio driver installed
- Headphones recommended (speakers work but headphones are way better)
- Gain is fixed at 0.3 (can't change it yet)
- HRTF is based on average head measurements (might not be perfect for everyone)

## What's Coming Next

Ideas for future versions:
- Adjustable gain control
- Preset positions (like "front", "behind", "above")
- Visual feedback (see where the sound is positioned)
- More HRTF datasets to choose from
- Linux support
- Record/export processed audio

## Open Source

This project is open source! The source code is available on GitHub:

**Repository**: [https://github.com/xunfeng1980/3dasmr-pri](https://github.com/xunfeng1980/3dasmr-pri)

Feel free to explore the code, submit issues, or contribute improvements!

## License

This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! If you have ideas for improvements or find bugs, please open an issue or submit a pull request on GitHub.

## Thanks

- HRTF data from KEMAR measurements
- Virtual audio drivers: BlackHole (macOS) and VB-CABLE (Windows)
- Libraries: NumPy, sounddevice
