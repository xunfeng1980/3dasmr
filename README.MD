# 3D Audio HRTF Processor for ASMR

Turn any stereo audio into immersive 3D spatial audio using HRTF (Head-Related Transfer Function) technology. No special hardware needed - perfect for ASMR content creation.

## The Story Behind This

I spent $26 on ads to test this idea, and 71 ASMR creators clicked on it.

While I didn't end up pursuing commercialization, the data proved there's real market demand.

So I decided to open source it, hoping it helps people who actually need it.

## What It Does

- Converts any stereo audio into 3D spatial audio effects
- Based on HRTF algorithm, no special hardware required
- Perfect for ASMR content creation
- Real-time processing with low latency
- Easy controls for positioning sounds in 3D space

## Quick Start

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

### How to Use

1. **Launch the app** - Open 3D Audio HRTF Controller
2. **Check the status** - Should say "Virtual Audio: OK" (if not, check your driver install)
3. **Play something** - Start any audio on your computer (music, video, game, whatever)
4. **Move it around** - Use the sliders to position the sound:
   - **Azimuth**: Left/right/around you (0° = front, 90° = right, 180° = behind, 270° = left)
   - **Elevation**: Up/down (-40° = below, 0° = level, 90° = above)

### Controls

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

## How It Works

The app captures your system audio through a virtual audio device, processes it with HRTF algorithms to create 3D spatial positioning, and outputs the processed audio to your headphones or speakers.

**HRTF** (Head-Related Transfer Function) simulates how your head and ears change sounds coming from different directions. It accounts for time differences, volume differences, and frequency filtering based on your head and ear shape.

The app uses precomputed HRTF data from KEMAR (a standard acoustic research dummy head) to process audio in real-time.

## Why I Open Sourced This

I realized I'm not truly passionate about the ASMR field. Rather than letting the code rot on my hard drive, I'd rather share it with people who actually need it.

If you build something cool based on this, I'd love to hear about it!

## If You Want to Commercialize

This code is under the MIT License, so you're free to use it for commercial purposes.

If you make it work, remember to send me an email - I want to know how it turns out 😊

## Open Source

This project is open source! The source code is available on GitHub:

https://github.com/xunfeng1980/3dasmr-pri

Feel free to explore the code, submit issues, or contribute improvements!

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! If you have ideas for improvements or find bugs, please open an issue or submit a pull request on GitHub.

## Thanks

- HRTF data from KEMAR measurements
- Virtual audio drivers: BlackHole (macOS) and VB-CABLE (Windows)
- Libraries: NumPy, sounddevice
