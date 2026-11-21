
LOCAL AI MOTION SEQUENCER (OLLAMA VLM + GPU RIFE + FFMPEG NVENC)


1. OVERVIEW
-----------
A local web-based motion sequencer for Windows 11 (NVIDIA RTX GPU).
- Uses a local Vision-Language Model (Ollama: qwen2.5vl:7b) to analyze 
  consecutive keyframes and automatically determine transition length.
- Uses GPU-accelerated optical flow interpolation (rife-ncnn-vulkan) to 
  synthesize smooth intermediate frames between keyframes.
- Uses FFmpeg with NVIDIA NVENC (h264_nvenc) to compile the final MP4.
- Note that current build essentially warps images from one keyframe to 
  the next so your milage may vary, consider it for what it is.
- released with no support makes changes at your own discretion

2. HARDWARE & OS REQUIREMENTS
-----------------------------
- OS: Windows 11 x64
- GPU: NVIDIA RTX GPU (tested on RTX 4070 12GB VRAM) with current 
  NVIDIA drivers installed (provides Vulkan & NVENC support).
- RAM: 16GB+ system RAM recommended.

3. FOLDER STRUCTURE
-------------------
ai_motion_studio\
  ├── app.py                  (Flask backend + Ollama/RIFE/FFmpeg logic)
  ├── setup_prereqs.bat       (Automated installer & system check)
  ├── launch_studio.bat       (One-click server & browser launcher)
  ├── README.txt              (This documentation)
  ├── templates\
  │     └── index.html        (Sequencer web interface)
  ├── static\
  │     └── app.js            (Timeline drag-and-drop & live SSE logs)
  ├── rife-ncnn-vulkan\       (Extracted Windows RIFE binary & model)
  ├── venv\                   (Isolated Python virtual environment)
  ├── keyframes\              (Drop source images here for batch load)
  ├── uploads\                (Auto-scaled masters & LLM thumbnails)
  └── outputs\                (Rendered MP4 videos)

4. PREREQUISITES (INSTALLED AUTOMATICALLY BY setup_prereqs.bat)
----------------------------------------------------------------
- Python 3.11+       : winget install -e --id Python.Python.3.11
- FFmpeg             : winget install -e --id Gyan.FFmpeg
- Ollama             : winget install -e --id Ollama.Ollama
- Vision LLM Model   : ollama pull qwen2.5vl:7b
- Frame Interpolator : nihui/rife-ncnn-vulkan (Windows release zip)
- Python Packages    : flask, ollama, opencv-python, werkzeug

5. HOW TO INSTALL & RUN
-----------------------
Step 1: Run setup_prereqs.bat once to install tools, pull qwen2.5vl:7b,
        download RIFE, create the Python venv, and verify all checks.
Step 2: Place sequential images (PNG, JPG, WebP) into the keyframes\ 
        folder (or drag-and-drop them directly onto the webpage).
Step 3: Double-click launch_studio.bat to start the server and open 
        http://127.0.0.1:5000 in your browser.
Step 4: Click "Load from keyframes/ Folder", adjust FPS or individual 
        transitions if desired, and click "Generate Motion Video".

6. TROUBLESHOOTING NOTES
------------------------
- Path Refresh: If winget just installed Python or FFmpeg, close the 
  terminal and re-run setup_prereqs.bat so Windows reloads system PATH.
- Token Limits: app.py automatically scales images to 384px JPEG 
  thumbnails before sending them to qwen2.5vl:7b (with num_ctx=8192) 
  while keeping full-resolution masters (up to 1920px) for RIFE.
- Output Location: Compiled videos are saved inside outputs\ and play 
  automatically in the browser preview panel.
========================================================================
