# VAC iOS Keyboard Extension

A custom iOS keyboard that shows 4 AI suggestion chips above every keyboard in every app — WhatsApp, iMessage, Instagram, Gmail, Notes, and more.

## What it does

- Shows a horizontal scroll of 4 AI suggestions (Natural / Thoughtful / Smart / Warm)
- Tap any chip to replace what you've typed with that suggestion
- Self-learning — the more you use it, the more it learns your preferred style
- Connects to the VAC server running on your Mac via local Wi-Fi

## Requirements

- Mac running VAC (port 8787)
- iPhone on the same Wi-Fi network
- Xcode 15+
- Apple Developer account (free works for personal device)

## Build Steps

### 1. Open the project

The Xcode project is already set up in this folder: open `VACKeyboard.xcodeproj`. It has two targets,
the `VACKeyboard` app (sources in `VACKeyboard/`) and the `VACKeyboardExt` keyboard extension
(sources in `VACKeyboardExt/`). If you change `project.yml`, regenerate the project with
[XcodeGen](https://github.com/yonaskolb/XcodeGen): `xcodegen generate`.

### 2. Configure App Groups (for config sharing)

Both the main app and keyboard extension need the same App Group:

1. Select the main app target → Signing & Capabilities → + Capability → App Groups
2. Add group: `group.com.yourname.vackeyboard`
3. Repeat for the `VACKeyboardExt` target
4. In `VACKeyboard/VACConfig.swift` and `VACKeyboardExt/VACConfig.swift`, update `suiteName` to match

### 3. Set the server URL

Open `VACKeyboardExt/VACConfig.swift` (and the copy in `VACKeyboard/`) and change the default URL to your Mac's local IP:

```swift
var serverURL: String {
    get { defaults.string(forKey: "vacServerURL") ?? "http://YOUR_MAC_IP:8787" }
```

Your Mac IP is shown in the VAC app → Settings → Phone & Extension section.

### 4. Allow HTTP (local network)

In the main app's `Info.plist`, add:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

And add `NSLocalNetworkUsageDescription` with reason: "VAC needs local network access to connect to your Mac."

### 5. Build and run on device

1. Connect your iPhone
2. Select your device as the build target
3. ⌘R to build and run
4. Trust the developer certificate on device: Settings → General → VPN & Device Management

### 6. Enable the keyboard on iPhone

1. Settings → General → Keyboard → Keyboards → Add New Keyboard
2. Select **VACKeyboard**
3. Enable "Allow Full Access" (required for network requests)
4. Done ✓

### 7. Use VAC keyboard

In any app:
1. Tap any text field
2. Long-press the 🌐 globe icon on your keyboard
3. Select "VACKeyboard"
4. Start typing — suggestions appear automatically
5. Tap a chip to use that suggestion

## Switching back to system keyboard

Tap the 🌐 globe icon to cycle through keyboards, or long-press to pick one.

## Troubleshooting

**"VAC offline" in the suggestion bar:**
- Make sure the VAC app is running on your Mac
- Make sure your iPhone and Mac are on the same Wi-Fi network
- Check the IP address in `VACConfig.swift` matches your Mac's current IP (shown in VAC Settings)

**Suggestions not appearing:**
- Make sure "Allow Full Access" is enabled for the keyboard in iOS Settings
- Without full access, the keyboard extension cannot make network requests
