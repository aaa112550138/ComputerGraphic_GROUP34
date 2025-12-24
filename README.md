## Real-Time Watercolor Simulation
A C++ application built with OpenFrameworks that simulates realistic watercolor fluid dynamics in real-time. This project implements a shader-based approach where pigment advection, diffusion, and paper topology interaction are simulated on the GPU.

Based on the research: "Real-Time Watercolor Rendering of 3D Objects and Animation with Enhanced Control" (Artineering.io).

🛠 System Requirements
OS: Linux (Ubuntu 20.04 / 22.04) or Windows Subsystem for Linux (WSL2).

Framework: OpenFrameworks (v0.11.2 or v0.12.0).

Compiler: GCC / G++.

⚙️ Environmental Setup (Crucial for WSL)
Running graphical applications and video playback on WSL (Windows Subsystem for Linux) requires specific configurations to bridge the gap between Linux OpenGL calls and Windows drivers.

1. Install System Dependencies & Codecs
OpenFrameworks requires core libraries to compile. Additionally, GStreamer plugins are strictly required to play MP4/H.264 video files, which are not installed by default on Ubuntu.

Run the following commands in your terminal:

``` Bash

# 1. Update package lists
sudo apt update

# 2. Install OpenFrameworks core dependencies (if not already done)
cd /path/to/openFrameworks/scripts/linux/ubuntu
sudo ./install_dependencies.sh

# 3. Install Video Codecs (REQUIRED for video playback)
# Without this, videos will appear as black screens or static noise.
sudo apt install libgstreamer1.0-dev gstreamer1.0-plugins-base \
gstreamer1.0-plugins-good gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools
```
2. Configure OpenGL for WSL
WSL drivers (Mesa) often default to a strictly compliant Core Profile that may reject OpenFrameworks' window initialization. We need to force a Compatibility Profile.

Option A: Permanent Fix (Recommended) Add the configuration to your bash profile so it applies every time you open the terminal.

```Bash

echo 'export MESA_GL_VERSION_OVERRIDE=3.3COMPAT' >> ~/.bashrc
echo 'export MESA_GLSL_VERSION_OVERRIDE=330' >> ~/.bashrc
source ~/.bashrc
```

Option B: Temporary Fix Run this before executing the program every time:

```Bash

export MESA_GL_VERSION_OVERRIDE=3.3COMPAT
```

🚀 Compilation & Running
Navigate to the project directory:

```Bash

cd /path/to/apps/myApps/WaterColor
```
Compile the project:

```Bash

make
# If you encounter errors, try 'make clean' first

cd bin
./WaterColor
```

🎮 Controls
* GUI Panel: Use the mouse to adjust simulation parameters (Gradient Step, Advection, Expand).

* Keyboard Shortcuts:
    - 1, 2, 3, 4: Load different static images.

    - Space: Hide/Show GUI.

    - ESC: Quit application.

* Mouse Interaction:

    - Click and drag on the canvas to add "water/pigment" manually (if feature enabled).

🔧 Troubleshooting
Q: The window does not open (Process finishes immediately).
A: This is likely an OpenGL profile mismatch in WSL. Ensure you have set the environment variable MESA_GL_VERSION_OVERRIDE=3.3COMPAT as described in the Setup section. Also, ensure your main.cpp uses ofSetupOpenGL(1024, 768, OF_WINDOW); instead of ofGLFWWindowSettings.

Q: The video plays but shows only noise or colors are wrong.
A: You are missing the GStreamer bad/ugly plugins. Refer to Step 1 in the Setup section to install gstreamer1.0-plugins-ugly and gstreamer1.0-libav. Reboot your terminal after installation.

Q: The video is still black after installing codecs.
A: Linux video decoding can be finicky. As a fallback, convert your video to Photo-JPEG (MOV) or Image Sequence using ffmpeg:

``` Bash

ffmpeg -i input.mp4 -vcodec mjpeg -q:v 2 output.mov

```
Then update player.load("output.mov") in your code.

📂 File Structure
* src/: Contains source code (main.cpp, ofApp.cpp).

* bin/data/: Assets directory. Must contain:

    - wcolor.vert, wcolor.frag (Shaders).

    - 1.jpg (Source images).

    - 1.mp4 (Source video).
