# LibrePods Linux

A native Linux application to control your AirPods, with support for:

- Noise Control modes (Off, Transparency, Adaptive, Noise Cancellation)
- Conversational Awareness
- Battery monitoring
- Auto play/pause on ear detection
- Seamless handoff between phone and PC

## Prerequisites

1. Your phone's Bluetooth MAC address (can be found in Settings > About Device)
2. Qt6 packages

   ```bash
   # For Arch Linux / EndeavourOS
   sudo pacman -S qt6-base qt6-connectivity qt6-multimedia-ffmpeg qt6-multimedia

   # For Debian
   sudo apt-get install qt6-base-dev qt6-declarative-dev qt6-connectivity-dev qt6-multimedia-dev \
        qml6-module-qtquick-controls qml6-module-qtqml-workerscript qml6-module-qtquick-templates \
        qml6-module-qtquick-window qml6-module-qtquick-layouts

    # For Fedora
    sudo dnf install qt6-qtbase-devel qt6-qtconnectivity-devel \
        qt6-qtmultimedia-devel qt6-qtdeclarative-devel
   ```
3. OpenSSL development headers

    ```bash
    # On Arch Linux / EndevaourOS, these are included in the OpenSSL package, so you might already have them installed.
    sudo pacman -S openssl
    
    # For Debian / Ubuntu
    sudo apt-get install libssl-dev
    
    # For Fedora
    sudo dnf install openssl-devel
    ```
## Setup

1. Build the application:

   ```bash
   mkdir build
   cd build
   cmake ..
   make -j $(nproc)
   ```

2. Run the application:

   ```bash
   ./librepods
   ```

## Troubleshooting

### Media Controls (Play/Pause/Skip) Not Working

If tap gestures on your AirPods aren't working for media control, you need to enable AVRCP support. The solution depends on your audio stack:

#### PipeWire/WirePlumber (Recommended)

Create `~/.config/wireplumber/wireplumber.conf.d/51-bluez-avrcp.conf`:

```conf
monitor.bluez.properties = {
  # Enable dummy AVRCP player for proper media control support
  # This is required for AirPods and other devices to send play/pause/skip commands
  bluez5.dummy-avrcp-player = true
}
```

Then restart WirePlumber:

```bash
systemctl --user restart wireplumber
```

**Note:** Do NOT run `mpris-proxy` with WirePlumber - it will conflict and break media controls.

#### PulseAudio

If you're using PulseAudio instead of PipeWire, enable and start `mpris-proxy`:

```bash
systemctl --user enable --now mpris-proxy
```

## Usage

- Left-click the tray icon to view battery status
- Right-click to access the control menu:
  - Toggle Conversational Awareness
  - Switch between noise control modes
  - View battery levels
  - Control playback
