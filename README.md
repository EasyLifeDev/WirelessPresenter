# Wireless Presenter PC

Wireless Presenter PC is the desktop companion app for the Wireless Presenter mobile app.

It starts a local server on your computer, shows a QR code, and lets your phone control presentations, slides, mouse movement, touchpad actions, Air Mouse, and laser pointer mode over the same local network.

## How It Works

1. Install and open Wireless Presenter PC on your computer.
2. Open the Wireless Presenter mobile app on your phone.
3. Keep the phone and the computer on the same Wi-Fi network.
4. If no Wi-Fi is available, enable a mobile hotspot on your phone and connect the computer to it.
5. Scan the QR code shown by the PC app.
6. Start presenting from your phone.

The app does not require an internet connection for pairing and control. It only needs the phone and computer to be reachable on the same local network.

## Features

- QR code pairing with the mobile app
- Start slideshow
- Next and previous slide
- Black screen
- Escape / exit presentation
- Mouse left click and right click
- Touchpad cursor control
- Air Mouse cursor control
- Laser pointer overlay
- Timer and vibration alert support from the mobile app

## Download

Download the latest desktop release from the GitHub Releases page:

https://github.com/EasyLifeDev/WirelessPresenter/releases

Choose the package for your operating system:

- Windows: ZIP package
- Linux: ZIP, AppImage, DEB, or RPM package
- macOS: app bundle or DMG package when available

## Windows Install

1. Download the Windows ZIP package.
2. Extract the ZIP file.
3. Open the extracted folder.
4. Run:

```text
wireless-presenter-desktop.exe
```

If Windows SmartScreen appears, choose `More info`, then `Run anyway`, only if you downloaded the app from the official release page.

### Windows Uninstall

Wireless Presenter PC is portable on Windows.

To remove it:

1. Close the app.
2. Delete the extracted Wireless Presenter PC folder.
3. Delete any shortcut you created manually.

## Linux Install

### AppImage

1. Download the `.AppImage` file.
2. Make it executable:

```bash
chmod +x wireless-presenter-desktop-linux.AppImage
```

3. Run it:

```bash
./wireless-presenter-desktop-linux.AppImage
```

### ZIP

1. Download the Linux ZIP package.
2. Extract it.
3. Run:

```bash
./run.sh
```

### DEB

On Debian or Ubuntu based distributions:

```bash
sudo apt install ./wireless-presenter-desktop-linux.deb
```

Then launch Wireless Presenter PC from the applications menu, or run:

```bash
wireless-presenter-desktop
```

### RPM

On Fedora, openSUSE, or compatible distributions:

```bash
sudo dnf install ./wireless-presenter-desktop-linux.rpm
```

or:

```bash
sudo rpm -i wireless-presenter-desktop-linux.rpm
```

### Linux Uninstall

For AppImage or ZIP:

```bash
rm -f wireless-presenter-desktop-linux.AppImage
```

or delete the extracted folder.

For DEB:

```bash
sudo apt remove wireless-presenter-desktop
```

For RPM:

```bash
sudo dnf remove wireless-presenter-desktop
```

or:

```bash
sudo rpm -e wireless-presenter-desktop
```

## macOS Install

1. Download the macOS package when available.
2. Open the `.dmg` file or extracted app bundle.
3. Move Wireless Presenter PC to the `Applications` folder.
4. Open Wireless Presenter PC.

If macOS blocks the app because it was downloaded from the internet, open `System Settings`, then `Privacy & Security`, and allow the app if you trust the official release.

### macOS Uninstall

1. Close Wireless Presenter PC.
2. Open the `Applications` folder.
3. Move Wireless Presenter PC to Trash.

## Troubleshooting

### The Mobile App Cannot Connect

Check the following:

- The phone and computer are on the same Wi-Fi network.
- VPN is disabled on the phone and computer.
- Firewall allows Wireless Presenter PC to receive local network connections.
- If using a mobile hotspot, the computer is connected to the phone hotspot.
- Close and reopen the PC app to generate a fresh QR code.

### QR Code Scan Works, But Control Does Not

Try:

- Restarting the PC app.
- Restarting the mobile app.
- Checking that the computer did not switch network.
- Allowing the app through Windows Firewall, macOS firewall, or Linux firewall.

### Linux Wayland Limitation

On Linux, Air Mouse, touchpad cursor control, clicks, keyboard shortcuts, and active-window detection work best in an X11 session.

Wayland intentionally blocks many global mouse and keyboard control APIs for security reasons. If your Linux desktop is running Wayland, Wireless Presenter PC may be able to pair with the mobile app, but Air Mouse or cursor control may not work correctly.

For the best Linux experience, use an X11 session:

1. Log out of your Linux desktop session.
2. On the login screen, select your user.
3. Click the session or gear icon.
4. Choose `Ubuntu on Xorg`, `GNOME on Xorg`, or another X11 session.
5. Log in again and reopen Wireless Presenter PC.

### Air Mouse Or Laser Pointer Does Not Work Smoothly

The mobile app uses phone motion sensors. For best results:

- Hold the phone naturally.
- Tap Recenter before presenting.
- Keep the mobile app open during the presentation.

## Build From Source

Wireless Presenter PC is built with C++, Qt, and CMake.

### Requirements

- CMake 3.16 or later
- C++17 compiler
- Qt 5.15 or Qt 6 with:
  - Core
  - Gui
  - Widgets
  - Network
  - Concurrent

Linux also requires X11 development libraries:

```bash
sudo apt install build-essential cmake qtbase5-dev libx11-dev libxtst-dev libxfixes-dev
```

Package names may differ depending on your Linux distribution.

### Build On Linux

```bash
cmake -S . -B build-release -DCMAKE_BUILD_TYPE=Release
cmake --build build-release --parallel
./build-release/wireless-presenter-desktop
```

### Build On Windows

Use a terminal where Qt, CMake, and your compiler are available.

```powershell
cmake -S . -B build-release -DCMAKE_BUILD_TYPE=Release
cmake --build build-release --config Release --parallel
```

The executable is generated in `build-release`.

### Build On macOS

```bash
cmake -S . -B build-release -DCMAKE_BUILD_TYPE=Release
cmake --build build-release --parallel
```

## Tests

The desktop test suite uses CMake and CTest.

It currently checks small protocol and input helpers that are safe to test without launching the full Qt desktop UI:

- Presentation window title detection
- Mouse button mapping for left, right, and middle click
- Remote key normalization such as `Esc` to `escape`
- Supported remote key names such as `F5`, `page_down`, and `enter`

Run the tests from the `desktop` folder:

```bash
cmake -S . -B build-tests -DBUILD_TESTING=ON
cmake --build build-tests --target wireless-presenter-desktop-tests
ctest --test-dir build-tests --output-on-failure
```

You can also build the desktop app from the same test build folder:

```bash
cmake --build build-tests --target wireless-presenter-desktop
```

These tests do not replace manual testing of QR pairing, firewall/network behavior, or real mouse/keyboard control on Windows, Linux, and macOS. They provide a fast safety net for shared logic.

## Packaging

### Linux Packages

From the `desktop` folder:

```bash
./package-desktop-linux.sh
```

Then choose:

```text
1) ZIP
2) AppImage
3) DEB
4) RPM
```

Generated packages are copied to:

```text
desktop/releases
```

### Windows Package

From PowerShell:

```powershell
.\package-desktop.ps1
```

Or from Command Prompt:

```bat
package-desktop-windows.bat
```

The Windows ZIP package is copied to:

```text
desktop/releases
```

### macOS Package

From macOS:

```bash
./package-desktop.sh
```

## Notes For Releases

Do not commit generated packages into the source repository unless there is a specific reason.

Recommended release flow:

1. Build the package for each platform.
2. Create a GitHub Release.
3. Upload the generated Windows, Linux, and macOS packages to the release.
4. Keep the source repository focused on source code, scripts, and documentation.

## License Notices

This app uses Qt. License notices are included in:

```text
resources/licenses
```

When distributing packages, keep the license files included with the app.
