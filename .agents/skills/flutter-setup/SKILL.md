---
name: flutter-setup
version: 2.0.1
description: "Complete Flutter environment setup from scratch for iOS and Android — perfect for beginners with no prior coding experience. Installs all required tools (Homebrew, Xcode, Android Studio, Android SDK, FVM, Flutter) and ensures Flutter apps can run on simulator/emulator. One skill for everything. No need to install Java manually — Flutter uses the JDK bundled with Android Studio."
metadata:
  requires: []
---

# Flutter Complete Environment Setup — iOS & Android

> For beginners starting Flutter on macOS.
> This skill will install **everything you need** from scratch.
> Every step: **check first → install if missing → verify → continue**.
> If there's an error, **stop and explain** before proceeding.

---

## Opening — Welcome the User

Before starting, say to the user:

```
Hello! I'll help you set up the Flutter environment on your laptop.

This process will install all the tools needed to build 
Flutter apps (iOS & Android) from scratch.

Estimated time: 60-120 minutes (mostly download time)

What will be installed:
  ✦ Homebrew       — package manager for macOS
  ✦ Git            — version control
  ✦ Xcode          — tools for iOS development
  ✦ CocoaPods      — iOS dependency manager
  ✦ Android Studio — IDE with bundled JDK for Android
  ✦ Android SDK    — tools for Android development
  ✦ FVM            — Flutter version manager
  ✦ Flutter        — the main framework

Before we start, a few things to prepare:
  1. A stable internet connection
  2. At least 35 GB of free storage
  3. An Apple ID (your iCloud/App Store account)

Ready? Type "yes" or "continue" to begin.
```

Wait for the user's confirmation before proceeding.

---

## PART 1 — FOUNDATION (Common Tools)

### Step 1 — Check & Install Homebrew

Homebrew is an "app store" for the terminal — used to install almost all other tools.

**Check:**
```bash
which brew && brew --version
```

**If already installed** → show version, continue to Step 2.

**If not installed:**
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

During the installation, the terminal may ask for your laptop password — this is normal and safe.

After installation, set up PATH:
```bash
# Check Mac chip (Apple Silicon or Intel)
if [[ $(uname -m) == "arm64" ]]; then
  echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
  eval "$(/opt/homebrew/bin/brew shellenv)"
fi
```

**Verify:**
```bash
brew --version
```

Show: `✅ Homebrew [version] successfully installed`

---

### Step 2 — Check & Install Git

**Check:**
```bash
git --version
```

**If not installed:**
```bash
brew install git
```

**Verify:**
```bash
git --version
```

---

### Step 3 — Check & Install FVM (Flutter Version Manager)

FVM makes it easy to manage Flutter versions.

**Check:**
```bash
fvm --version 2>/dev/null
```

**If not installed:**
```bash
brew tap leoafarias/fvm
brew install fvm
```

Set up FVM PATH:
```bash
grep -q 'fvm/default/bin' ~/.zshrc || echo 'export PATH="$PATH:$HOME/fvm/default/bin"' >> ~/.zshrc
source ~/.zshrc
```

**Verify:**
```bash
fvm --version
```

---

### Step 4 — Check & Install Flutter via FVM

**Check:**
```bash
fvm list 2>/dev/null | grep -E "stable|[0-9]+\.[0-9]+"
```

**If not installed:**
```bash
fvm install stable
fvm global stable
```

Set up Flutter PATH:
```bash
grep -q 'fvm/default/bin' ~/.zshrc || echo 'export PATH="$PATH:$HOME/fvm/default/bin"' >> ~/.zshrc
source ~/.zshrc
```

**Verify:**
```bash
fvm flutter --version
```

---

## PART 2 — iOS SETUP

Tell the user:
```
📱 Now setting up for iOS...
```

---

### Step 5 — Check & Install Xcode via xcodes CLI

`xcodes` allows installing Xcode directly from the terminal without opening the App Store.

#### Step 5a — Install xcodes

**Check:**
```bash
which xcodes && xcodes version
```

**If not installed:**
```bash
brew install xcodes
```

#### Step 5b — Ask for Apple ID

**Ask the user:**
```
To install Xcode, an Apple ID is required.
Your Apple ID is the email you use on your iPhone or iCloud.

Enter your Apple ID (email):
```

Save the answer as `APPLE_ID`.

#### Step 5c — Check if Xcode is Already Installed

**Check:**
```bash
xcodebuild -version 2>/dev/null | head -1
```

**If Xcode is already installed** → show version, skip to Step 5e.

#### Step 5d — Download & Install Xcode

Tell the user:
```
⏳ Installing Xcode (~15 GB). This process takes 20-60 minutes.

You will see 3 prompts to fill in:
  1. Your Apple ID password
  2. A 6-digit code from your phone (security verification)
  3. Your laptop password (for the installation process)

Make sure your phone is nearby to receive the verification code.
```

Run:
```bash
xcodes install --latest --experimental-unxip
```

If `xcodes` asks for an Apple ID, use the `$APPLE_ID` entered by the user.

**Verify after installation:**
```bash
xcodebuild -version
```

#### Step 5e — Configure Xcode

```bash
# Set Xcode as the active developer directory
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer

# Accept license agreement
sudo xcodebuild -license accept

# Install additional components
sudo xcodebuild -runFirstLaunch
```

**Verify:**
```bash
xcode-select --print-path
```

Output should be: `/Applications/Xcode.app/Contents/Developer`

---

### Step 6 — Check iOS Simulator

**Check available runtimes:**
```bash
xcrun simctl list runtimes | grep iOS | head -3
```

**If no iOS runtime is available**, install:
```bash
xcodes runtimes install "iOS 18"
```

**Check available simulator devices:**
```bash
xcrun simctl list devices available | grep iPhone | head -5
```

---

### Step 7 — Check & Install CocoaPods

**Check:**
```bash
pod --version 2>/dev/null
```

**If not installed:**
```bash
brew install cocoapods
```

**Verify:**
```bash
pod --version
```

---

## PART 3 — ANDROID SETUP

Tell the user:
```
🤖 Now setting up for Android...
```

> Flutter 3.10+ uses the JDK bundled inside Android Studio.
> No need to install Java separately.

---

### Step 8 — Check & Install Android Studio

Android Studio is the IDE for writing Flutter code and also provides the bundled JDK needed for Android builds.

**Check:**
```bash
ls /Applications/Android\ Studio.app 2>/dev/null && echo "FOUND" || echo "NOT FOUND"
```

**If already installed** → show it, continue to Step 9.

**If not installed:**
```bash
brew install --cask android-studio
```

> Installing via Homebrew automatically downloads and installs to Applications (~1.5 GB).

**Verify:**
```bash
ls /Applications/Android\ Studio.app && echo "✅ Android Studio installed"
```

---

### Step 9 — Check & Install Android Command Line Tools

Command Line Tools are required to install SDK components and create emulators via the terminal.

**Check:**
```bash
ls ~/Library/Android/sdk/cmdline-tools/latest/bin/sdkmanager 2>/dev/null && echo "FOUND" || echo "NOT FOUND"
```

**If not installed:**
```bash
brew install --cask android-commandlinetools
```

If the brew cask fails, install manually:
```bash
CMDLINE_URL="https://dl.google.com/android/repository/commandlinetools-mac-11076708_latest.zip"
mkdir -p ~/Library/Android/sdk/cmdline-tools
cd /tmp && curl -O "$CMDLINE_URL"
unzip -q commandlinetools-mac-*.zip -d /tmp/cmdtools
mkdir -p ~/Library/Android/sdk/cmdline-tools/latest
cp -r /tmp/cmdtools/cmdline-tools/* ~/Library/Android/sdk/cmdline-tools/latest/
rm -rf /tmp/cmdtools /tmp/commandlinetools-mac-*.zip
```

**Set up Android environment variables** (append to `~/.zshrc` if not already present):
```bash
grep -q 'ANDROID_HOME' ~/.zshrc || cat >> ~/.zshrc << 'EOF'

# Android SDK
export ANDROID_HOME="$HOME/Library/Android/sdk"
export ANDROID_SDK_ROOT="$HOME/Library/Android/sdk"
export PATH="$ANDROID_HOME/emulator:$PATH"
export PATH="$ANDROID_HOME/platform-tools:$PATH"
export PATH="$ANDROID_HOME/cmdline-tools/latest/bin:$PATH"
EOF
source ~/.zshrc
```

**Verify:**
```bash
sdkmanager --version
```

---

### Step 10 — Install Android SDK Components

Detect the Mac chip to determine the correct ABI:
```bash
CHIP=$(uname -m)
if [ "$CHIP" = "arm64" ]; then
  SYS_IMG="system-images;android-34;google_apis_playstore;arm64-v8a"
else
  SYS_IMG="system-images;android-34;google_apis_playstore;x86_64"
fi
echo "Chip: $CHIP → System image: $SYS_IMG"
```

Install all components:
```bash
sdkmanager \
  "platform-tools" \
  "emulator" \
  "build-tools;34.0.0" \
  "platforms;android-34" \
  "cmdline-tools;latest" \
  "$SYS_IMG"
```

Accept all licenses:
```bash
yes | sdkmanager --licenses
```

**Verify:**
```bash
sdkmanager --list_installed 2>/dev/null | grep -E "platform-tools|emulator|build-tools"
```

---

### Step 11 — Create Android Emulator (Virtual Device)

An emulator is a virtual Android phone on your laptop for testing apps without a physical device.

**Check if emulator already exists:**
```bash
$ANDROID_HOME/cmdline-tools/latest/bin/avdmanager list avd 2>/dev/null | grep "Name:"
```

**If already exists** → show the name, continue to Step 12.

**If not**, create an emulator using the JDK bundled with Android Studio:

```bash
CHIP=$(uname -m)
if [ "$CHIP" = "arm64" ]; then
  SYS_IMG_ABI="google_apis_playstore/arm64-v8a"
  PKG="system-images;android-34;google_apis_playstore;arm64-v8a"
else
  SYS_IMG_ABI="google_apis_playstore/x86_64"
  PKG="system-images;android-34;google_apis_playstore;x86_64"
fi

# Use the JDK bundled with Android Studio for avdmanager
AS_JDK="/Applications/Android Studio.app/Contents/jbr/Contents/Home"

JAVA_HOME="$AS_JDK" \
SKIP_JDK_VERSION_CHECK=1 \
echo "no" | $ANDROID_HOME/cmdline-tools/latest/bin/avdmanager create avd \
  --name "Flutter_Android" \
  --abi "$SYS_IMG_ABI" \
  --package "$PKG" \
  --device "pixel_8"
```

**If a JAXB error appears** (`NoClassDefFoundError: javax/xml/bind`), use this fallback:

```bash
# Fallback: open Android Studio Device Manager
open -a "Android Studio"
```

Then guide the user:
```
Open Android Studio, then:
1. Click "More Actions" (three-dot icon) → "Virtual Device Manager"
2. Click the "+" button in the top left
3. Select "Pixel 8" → click Next
4. Select "Android 14 (API 34)" → click Next → Finish
5. Type "done" when finished
```

**Verify:**
```bash
$ANDROID_HOME/cmdline-tools/latest/bin/avdmanager list avd 2>/dev/null | grep "Name:"
```

---

## PART 4 — FINAL VERIFICATION

### Step 13 — Configure Flutter

```bash
fvm flutter precache --ios --android
fvm flutter config --no-analytics
fvm flutter config --android-sdk "$ANDROID_HOME"
```

### Step 14 — Flutter Doctor

```bash
fvm flutter doctor -v
```

**All of the following must show ✓ (green):**
- `[✓] Flutter`
- `[✓] Xcode`
- `[✓] CocoaPods`
- `[✓] Android toolchain`
- `[✓] Android Studio`

**Troubleshooting table — fix all ✗ before continuing:**

| Error | Solution |
|---|---|
| `Xcode license not accepted` | `sudo xcodebuild -license accept` |
| `Android licenses not accepted` | `yes \| sdkmanager --licenses` |
| `Android SDK not found` | `source ~/.zshrc` then check `echo $ANDROID_HOME` |
| `cmdline-tools component is missing` | `sdkmanager "cmdline-tools;latest"` |
| `CocoaPods not installed` | `brew install cocoapods` |
| `Android Studio not installed` | `brew install --cask android-studio` |
| `command not found` after install | Close & reopen terminal |

Re-run `fvm flutter doctor` until all show ✓.

---

### Step 15 — Test: Create & Run Your First Flutter App

Boot iOS Simulator:
```bash
open -a Simulator
```

Boot Android Emulator:
```bash
emulator -avd Flutter_Android &
```

Wait for both devices to be ready, then:
```bash
cd ~/Desktop
fvm flutter create hello_flutter
cd hello_flutter
fvm flutter run
```

If a device selection prompt appears, choose one (iOS or Android).
The Flutter demo app will appear on screen (a blue counter app).

---

### Step 16 — Final Status Report

After everything succeeds, display this summary:

```
🎉 Flutter Setup Complete! You're ready to code!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  iOS
  ────────────────────────────
  Xcode          : ✅ [version]
  CocoaPods      : ✅ [version]
  iOS Simulator  : ✅ ready

  Android
  ────────────────────────────
  Android Studio : ✅ installed (bundled JDK [version])
  Android SDK    : ✅ [path]
  Emulator       : ✅ Flutter_Android

  Flutter
  ────────────────────────────
  FVM            : ✅ [version]
  Flutter        : ✅ [version]
  Flutter Doctor : ✅ all green

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Commonly used commands:
  fvm flutter create app_name  → create a new project
  fvm flutter run              → run the app
  fvm flutter doctor           → check environment

Recommended IDEs:
  Android Studio  → great for Flutter & Android
  VS Code         → lightweight, popular for Flutter
                    (install "Flutter" & "Dart" extensions)
```

---

## Important Notes

- **Xcode download is ~15 GB** — ensure a stable internet connection and sufficient storage
- **Apple 2FA:** When installing Xcode, Apple sends a 6-digit code to your phone — this is normal and safe
- **First emulator boot** can be slow (2-3 minutes) — wait until the Android home screen appears
- **First Gradle build** can take 5-10 minutes — this is normal, subsequent builds will be faster
- If you see "command not found" after installing a tool → close and reopen the terminal
