# gpu screen recorder — ubuntu wayland setup

## 1. overview

Install and configure GPU Screen Recorder for lightweight desktop recording on Ubuntu using GNOME Wayland.

**Configuration:**

- operating system: Ubuntu 26.04
- desktop: GNOME
- display server: Wayland
- GPU: Intel
- resolution: 1920x1080
- frame rate: 30 FPS
- video codec: H.264
- container: MP4
- desktop audio: enabled
- microphone: built-in analog microphone
- recording directory: `~/Videos`

**Keyboard shortcuts:**

| Shortcut   | Action          |
| ---------- | --------------- |
| Ctrl + F9  | Start recording |
| Ctrl + F10 | Pause / resume  |
| Ctrl + F11 | Stop and save   |

## 2. install dependencies

Install Flatpak and the required desktop capture components:

```bash
sudo apt update
sudo apt install flatpak xdg-desktop-portal xdg-desktop-portal-gnome pipewire wireplumber pipewire-pulse
```

These components provide:

- flatpak: application installation and sandbox
- pipewire: audio and screen capture infrastructure
- wireplumber: pipewire session manager
- xdg-desktop-portal: screen-sharing interface
- xdg-desktop-portal-gnome: GNOME screen-sharing implementation

Most of these components are already installed on a standard Ubuntu GNOME installation.

## 3. add flathub repository

```bash
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## 4. install gpu screen recorder

Install system-wide:

```bash
sudo flatpak install --system flathub com.dec05eba.gpu_screen_recorder
```

**Important:** System-wide installation is required for the Intel/AMD Flatpak setup described in the upstream documentation.

## 5. verify installation

Check the version:

```bash
flatpak run --command=gpu-screen-recorder com.dec05eba.gpu_screen_recorder --version
```

Display GPU and capture information:

```bash
flatpak run --command=gpu-screen-recorder com.dec05eba.gpu_screen_recorder --info
```

List available capture sources:

```bash
flatpak run --command=gpu-screen-recorder com.dec05eba.gpu_screen_recorder --list-capture-options
```

List available audio devices:

```bash
flatpak run --command=gpu-screen-recorder com.dec05eba.gpu_screen_recorder --list-audio-devices
```

For this setup, the relevant devices are:

```text
default_output
default_input
alsa_output.pci-0000_00_1f.3.analog-stereo.monitor
alsa_input.pci-0000_00_1f.3.analog-stereo
```

The built-in microphone is:

```text
alsa_input.pci-0000_00_1f.3.analog-stereo
```

Device names can differ between computers.

## 6. create recording directory

```bash
mkdir -p "$HOME/Videos"
```

## 7. start recording

This command records the entire desktop through the GNOME Wayland screen-sharing portal.

```bash
sh -c 'flatpak run --command=gpu-screen-recorder com.dec05eba.gpu_screen_recorder -w portal -f 30 -k h264 -a "default_output|device:alsa_input.pci-0000_00_1f.3.analog-stereo" -o "$HOME/Videos/recording-$(date +%F_%H-%M-%S).mp4" >/tmp/gsr.log 2>&1 &'
```

Options:

| Option              | Description                            |
| ------------------- | -------------------------------------- |
| `-w portal`         | Capture desktop through Wayland portal |
| `-f 30`             | Maximum 30 FPS                         |
| `-k h264`           | H.264 video encoding                   |
| `-a default_output` | Record desktop audio                   |
| `-a device:...`     | Record specified microphone            |
| `-o`                | Output recording file                  |

Each recording receives a unique filename containing the date and time.

**Note:** GNOME may display a screen-sharing dialog. Select your monitor and confirm sharing. The Save Selection checkbox may not persist between sessions.

## 8. pause and resume recording

Send SIGUSR2 to toggle pause/resume:

```bash
pkill -SIGUSR2 -f '^gpu-screen-recorder'
```

Press once to pause and again to resume.

## 9. stop recording

Send SIGINT to stop and save:

```bash
pkill -SIGINT -f '^gpu-screen-recorder'
```

The resulting MP4 file is saved in:

```text
~/Videos/
```

## 10. configure keyboard shortcuts

Open:

**Settings → Keyboard → View and Customize Shortcuts → Custom Shortcuts**

Create three shortcuts.

### Start recording

Name:

```text
Start recording
```

Command:

```bash
sh -c 'flatpak run --command=gpu-screen-recorder com.dec05eba.gpu_screen_recorder -w portal -f 30 -k h264 -a "default_output|device:alsa_input.pci-0000_00_1f.3.analog-stereo" -o "$HOME/Videos/recording-$(date +%F_%H-%M-%S).mp4" >/tmp/gsr.log 2>&1 &'
```

Shortcut:

```text
Ctrl + F9
```

### Pause / resume recording

Name:

```text
Pause / resume recording
```

Command:

```bash
pkill -SIGUSR2 -f '^gpu-screen-recorder'
```

Shortcut:

```text
Ctrl + F10
```

### Stop recording

Name:

```text
Stop recording
```

Command:

```bash
pkill -SIGINT -f '^gpu-screen-recorder'
```

Shortcut:

```text
Ctrl + F11
```

## 11. test the configuration

1. Press Ctrl + F9
2. Select your monitor in the screen-sharing dialog
3. Confirm sharing
4. Speak to test microphone recording
5. Press Ctrl + F10 to pause
6. Press Ctrl + F10 again to resume
7. Press Ctrl + F11 to stop
8. Open `~/Videos` and play the recording

Check that both desktop audio and microphone audio are present.

**Important:** Do not press the start shortcut again while a recording is already running.

## 12. troubleshooting

### No microphone audio

List available audio devices:

```bash
flatpak run --command=gpu-screen-recorder com.dec05eba.gpu_screen_recorder --list-audio-devices
```

Replace the microphone device name in the recording command with the correct device.

### Recording does not start

Inspect the log:

```bash
cat /tmp/gsr.log
```

### Check PipeWire services

```bash
systemctl --user status pipewire wireplumber
```

### Check portal services

```bash
systemctl --user status xdg-desktop-portal xdg-desktop-portal-gnome
```

### Check Flatpak installation

```bash
flatpak list | grep -i gpu
```

## 13. uninstall

Remove GPU Screen Recorder:

```bash
sudo flatpak uninstall com.dec05eba.gpu_screen_recorder
```

Optionally remove the Flathub repository if it is no longer needed:

```bash
sudo flatpak remote-delete flathub
```

Do not remove Flathub if other installed applications depend on it.
