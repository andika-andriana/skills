---
name: flutter-setup
version: 2.0.0
description: "Setup lengkap environment Flutter dari nol untuk iOS dan Android — cocok untuk pemula yang belum pernah coding. Menginstall semua tool yang dibutuhkan (Homebrew, Xcode, Android Studio, Android SDK, FVM, Flutter), dan memastikan app Flutter bisa dijalankan di simulator/emulator. Satu skill untuk semua. Tidak perlu install Java manual — Flutter pakai JDK bawaan Android Studio."
metadata:
  requires: []
---

# Flutter Complete Environment Setup — iOS & Android

> Untuk pemula yang baru mulai Flutter di macOS.
> Skill ini akan menginstall **semua yang dibutuhkan** dari nol.
> Setiap step: **cek dulu → install kalau belum ada → verifikasi → lanjut**.
> Jika ada error, **berhenti dan jelaskan** sebelum lanjut.

---

## Pembukaan — Sambut User

Sebelum mulai, sampaikan kepada user:

```
Halo! Saya akan membantu setup environment Flutter di laptop kamu.

Proses ini akan menginstall semua tool yang dibutuhkan untuk 
membuat aplikasi Flutter (iOS & Android) dari nol.

Estimasi waktu: 60-120 menit (sebagian besar adalah waktu download)

Yang akan diinstall:
  ✦ Homebrew       — package manager untuk macOS
  ✦ Git            — version control
  ✦ Xcode          — tools untuk iOS development
  ✦ CocoaPods      — dependency manager iOS
  ✦ Android Studio — IDE sekaligus bundled JDK untuk Android
  ✦ Android SDK    — tools untuk Android development
  ✦ FVM            — Flutter version manager
  ✦ Flutter        — framework utama

Sebelum mulai, ada beberapa hal yang perlu disiapkan:
  1. Koneksi internet yang stabil
  2. Storage kosong minimal 35 GB
  3. Apple ID (akun iCloud/App Store kamu)

Siap? Ketik "ya" atau "lanjut" untuk mulai.
```

Tunggu konfirmasi user sebelum lanjut.

---

## BAGIAN 1 — FONDASI (Common Tools)

### Step 1 — Cek & Install Homebrew

Homebrew adalah "toko aplikasi" untuk terminal — dipakai untuk install hampir semua tool lainnya.

**Cek:**
```bash
which brew && brew --version
```

**Jika sudah ada** → tampilkan versi, lanjut ke Step 2.

**Jika belum ada:**
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Selama proses install, terminal mungkin minta password laptop — ini normal dan aman.

Setelah install, setup PATH:
```bash
# Cek chip Mac (Apple Silicon atau Intel)
if [[ $(uname -m) == "arm64" ]]; then
  echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
  eval "$(/opt/homebrew/bin/brew shellenv)"
fi
```

**Verifikasi:**
```bash
brew --version
```

Tampilkan: `✅ Homebrew [versi] berhasil diinstall`

---

### Step 2 — Cek & Install Git

**Cek:**
```bash
git --version
```

**Jika belum ada:**
```bash
brew install git
```

**Verifikasi:**
```bash
git --version
```

---

### Step 3 — Cek & Install FVM (Flutter Version Manager)

FVM memudahkan pengelolaan versi Flutter.

**Cek:**
```bash
fvm --version 2>/dev/null
```

**Jika belum ada:**
```bash
brew tap leoafarias/fvm
brew install fvm
```

Setup PATH FVM:
```bash
grep -q 'fvm/default/bin' ~/.zshrc || echo 'export PATH="$PATH:$HOME/fvm/default/bin"' >> ~/.zshrc
source ~/.zshrc
```

**Verifikasi:**
```bash
fvm --version
```

---

### Step 4 — Cek & Install Flutter via FVM

**Cek:**
```bash
fvm list 2>/dev/null | grep -E "stable|[0-9]+\.[0-9]+"
```

**Jika belum ada:**
```bash
fvm install stable
fvm global stable
```

Setup PATH Flutter:
```bash
grep -q 'fvm/default/bin' ~/.zshrc || echo 'export PATH="$PATH:$HOME/fvm/default/bin"' >> ~/.zshrc
source ~/.zshrc
```

**Verifikasi:**
```bash
fvm flutter --version
```

---

## BAGIAN 2 — iOS SETUP

Sampaikan ke user:
```
📱 Sekarang kita setup untuk iOS...
```

---

### Step 5 — Cek & Install Xcode via xcodes CLI

`xcodes` memungkinkan install Xcode langsung dari terminal tanpa buka App Store.

#### Step 5a — Install xcodes

**Cek:**
```bash
which xcodes && xcodes version
```

**Jika belum ada:**
```bash
brew install xcodes
```

#### Step 5b — Tanya Apple ID

**Tanya user:**
```
Untuk menginstall Xcode, dibutuhkan Apple ID.
Apple ID adalah email yang kamu pakai di iPhone atau iCloud.

Masukkan Apple ID kamu (email):
```

Simpan jawaban sebagai `APPLE_ID`.

#### Step 5c — Cek Xcode Sudah Terinstall

**Cek:**
```bash
xcodebuild -version 2>/dev/null | head -1
```

**Jika Xcode sudah ada** → tampilkan versi, skip ke Step 5e.

#### Step 5d — Download & Install Xcode

Sampaikan ke user:
```
⏳ Menginstall Xcode (~15 GB). Proses ini membutuhkan 20-60 menit.

Nanti akan ada 3 prompt yang perlu kamu isi:
  1. Password Apple ID kamu
  2. Kode 6 angka dari HP kamu (verifikasi keamanan)
  3. Password laptop kamu (untuk proses install)

Pastikan HP kamu ada di dekat kamu untuk menerima kode verifikasi.
```

Jalankan:
```bash
xcodes install --latest --experimental-unxip
```

Jika `xcodes` meminta Apple ID, gunakan `$APPLE_ID` yang sudah diinput user.

**Verifikasi setelah install:**
```bash
xcodebuild -version
```

#### Step 5e — Konfigurasi Xcode

```bash
# Set Xcode sebagai active developer directory
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer

# Accept license agreement
sudo xcodebuild -license accept

# Install komponen tambahan
sudo xcodebuild -runFirstLaunch
```

**Verifikasi:**
```bash
xcode-select --print-path
```

Output harus: `/Applications/Xcode.app/Contents/Developer`

---

### Step 6 — Cek iOS Simulator

**Cek runtime tersedia:**
```bash
xcrun simctl list runtimes | grep iOS | head -3
```

**Jika tidak ada iOS runtime**, install:
```bash
xcodes runtimes install "iOS 18"
```

**Cek device simulator tersedia:**
```bash
xcrun simctl list devices available | grep iPhone | head -5
```

---

### Step 7 — Cek & Install CocoaPods

**Cek:**
```bash
pod --version 2>/dev/null
```

**Jika belum ada:**
```bash
brew install cocoapods
```

**Verifikasi:**
```bash
pod --version
```

---

## BAGIAN 3 — ANDROID SETUP

Sampaikan ke user:
```
🤖 Sekarang kita setup untuk Android...
```

> Flutter 3.10+ menggunakan JDK yang sudah dibundel di dalam Android Studio.
> Tidak perlu install Java secara terpisah.

---

### Step 8 — Cek & Install Android Studio

Android Studio adalah IDE untuk menulis kode Flutter sekaligus menyediakan JDK bawaan yang dibutuhkan untuk build Android.

**Cek:**
```bash
ls /Applications/Android\ Studio.app 2>/dev/null && echo "FOUND" || echo "NOT FOUND"
```

**Jika sudah ada** → tampilkan, lanjut ke Step 9.

**Jika belum ada:**
```bash
brew install --cask android-studio
```

> Install via Homebrew otomatis download dan install ke Applications (~1.5 GB).

**Verifikasi:**
```bash
ls /Applications/Android\ Studio.app && echo "✅ Android Studio terinstall"
```

---

### Step 9 — Cek & Install Android Command Line Tools

Command Line Tools dibutuhkan untuk install SDK komponen dan membuat emulator via terminal.

**Cek:**
```bash
ls ~/Library/Android/sdk/cmdline-tools/latest/bin/sdkmanager 2>/dev/null && echo "FOUND" || echo "NOT FOUND"
```

**Jika belum ada:**
```bash
brew install --cask android-commandlinetools
```

Jika brew cask gagal, install manual:
```bash
CMDLINE_URL="https://dl.google.com/android/repository/commandlinetools-mac-11076708_latest.zip"
mkdir -p ~/Library/Android/sdk/cmdline-tools
cd /tmp && curl -O "$CMDLINE_URL"
unzip -q commandlinetools-mac-*.zip -d /tmp/cmdtools
mkdir -p ~/Library/Android/sdk/cmdline-tools/latest
cp -r /tmp/cmdtools/cmdline-tools/* ~/Library/Android/sdk/cmdline-tools/latest/
rm -rf /tmp/cmdtools /tmp/commandlinetools-mac-*.zip
```

**Setup Android environment variables** (tambahkan ke `~/.zshrc` jika belum ada):
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

**Verifikasi:**
```bash
sdkmanager --version
```

---

### Step 10 — Install Android SDK Components

Deteksi chip Mac untuk menentukan ABI yang tepat:
```bash
CHIP=$(uname -m)
if [ "$CHIP" = "arm64" ]; then
  SYS_IMG="system-images;android-34;google_apis_playstore;arm64-v8a"
else
  SYS_IMG="system-images;android-34;google_apis_playstore;x86_64"
fi
echo "Chip: $CHIP → System image: $SYS_IMG"
```

Install semua komponen:
```bash
sdkmanager \
  "platform-tools" \
  "emulator" \
  "build-tools;34.0.0" \
  "platforms;android-34" \
  "cmdline-tools;latest" \
  "$SYS_IMG"
```

Accept semua license:
```bash
yes | sdkmanager --licenses
```

**Verifikasi:**
```bash
sdkmanager --list_installed 2>/dev/null | grep -E "platform-tools|emulator|build-tools"
```

---

### Step 11 — Buat Android Emulator (Virtual Device)

Emulator = HP Android virtual di laptop untuk test app tanpa HP fisik.

**Cek emulator sudah ada:**
```bash
$ANDROID_HOME/cmdline-tools/latest/bin/avdmanager list avd 2>/dev/null | grep "Name:"
```

**Jika sudah ada** → tampilkan nama, lanjut ke Step 12.

**Jika belum ada**, buat emulator menggunakan JDK bawaan Android Studio:

```bash
CHIP=$(uname -m)
if [ "$CHIP" = "arm64" ]; then
  SYS_IMG_ABI="google_apis_playstore/arm64-v8a"
  PKG="system-images;android-34;google_apis_playstore;arm64-v8a"
else
  SYS_IMG_ABI="google_apis_playstore/x86_64"
  PKG="system-images;android-34;google_apis_playstore;x86_64"
fi

# Gunakan JDK bawaan Android Studio untuk avdmanager
AS_JDK="/Applications/Android Studio.app/Contents/jbr/Contents/Home"

JAVA_HOME="$AS_JDK" \
SKIP_JDK_VERSION_CHECK=1 \
echo "no" | $ANDROID_HOME/cmdline-tools/latest/bin/avdmanager create avd \
  --name "Flutter_Android" \
  --abi "$SYS_IMG_ABI" \
  --package "$PKG" \
  --device "pixel_8"
```

**Jika muncul error JAXB** (`NoClassDefFoundError: javax/xml/bind`), gunakan fallback ini:

```bash
# Fallback: buka Android Studio Device Manager
open -a "Android Studio"
```

Lalu panduan ke user:
```
Buka Android Studio yang baru muncul, lalu:
1. Klik "More Actions" (ikon titik tiga) → "Virtual Device Manager"
2. Klik tombol "+" di pojok kiri atas
3. Pilih "Pixel 8" → klik Next
4. Pilih "Android 14 (API 34)" → klik Next → Finish
5. Ketik "done" kalau sudah selesai
```

**Verifikasi:**
```bash
$ANDROID_HOME/cmdline-tools/latest/bin/avdmanager list avd 2>/dev/null | grep "Name:"
```

---

## BAGIAN 4 — VERIFIKASI FINAL

### Step 13 — Konfigurasi Flutter

```bash
fvm flutter precache --ios --android
fvm flutter config --no-analytics
fvm flutter config --android-sdk "$ANDROID_HOME"
```

### Step 14 — Flutter Doctor

```bash
fvm flutter doctor -v
```

**Yang harus ✓ (hijau) semua:**
- `[✓] Flutter`
- `[✓] Xcode`
- `[✓] CocoaPods`
- `[✓] Android toolchain`
- `[✓] Android Studio`

**Tabel troubleshooting — perbaiki semua ✗ sebelum lanjut:**

| Error | Solusi |
|---|---|
| `Xcode license not accepted` | `sudo xcodebuild -license accept` |
| `Android licenses not accepted` | `yes \| sdkmanager --licenses` |
| `Android SDK not found` | `source ~/.zshrc` lalu cek `echo $ANDROID_HOME` |
| `cmdline-tools component is missing` | `sdkmanager "cmdline-tools;latest"` |
| `CocoaPods not installed` | `brew install cocoapods` |
| `Android Studio not installed` | `brew install --cask android-studio` |
| `command not found` setelah install | Tutup & buka ulang terminal |

Jalankan ulang `fvm flutter doctor` sampai semua ✓.

---

### Step 15 — Test: Buat & Jalankan App Flutter Pertama

Boot iOS Simulator:
```bash
open -a Simulator
```

Boot Android Emulator:
```bash
emulator -avd Flutter_Android &
```

Tunggu kedua device siap, lalu:
```bash
cd ~/Desktop
fvm flutter create hello_flutter
cd hello_flutter
fvm flutter run
```

Jika muncul pilihan device, pilih salah satu (iOS atau Android).
App demo Flutter akan muncul di layar (counter app berwarna biru).

---

### Step 16 — Report Status Final

Setelah semua berhasil, tampilkan ringkasan ini:

```
🎉 Setup Flutter Selesai! Kamu siap coding!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  iOS
  ────────────────────────────
  Xcode          : ✅ [versi]
  CocoaPods      : ✅ [versi]
  iOS Simulator  : ✅ siap

  Android
  ────────────────────────────
  Android Studio : ✅ terinstall (bundled JDK [versi])
  Android SDK    : ✅ [path]
  Emulator       : ✅ Flutter_Android

  Flutter
  ────────────────────────────
  FVM            : ✅ [versi]
  Flutter        : ✅ [versi]
  Flutter Doctor : ✅ semua hijau

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Perintah yang sering dipakai:
  fvm flutter create nama_app  → buat project baru
  fvm flutter run              → jalankan app
  fvm flutter doctor           → cek environment

IDE yang bisa dipakai:
  Android Studio  → cocok untuk Flutter & Android
  VS Code         → ringan, populer untuk Flutter
                    (install extension "Flutter" & "Dart")
```

---

## Catatan Penting

- **Download Xcode ~15 GB** — pastikan koneksi internet stabil dan storage cukup
- **2FA Apple:** Saat install Xcode, Apple kirim kode 6 digit ke HP — ini normal dan aman
- **Emulator pertama kali boot** bisa lambat (2-3 menit) — tunggu sampai muncul layar home Android
- **Gradle build pertama kali** bisa 5-10 menit — ini normal, selanjutnya akan lebih cepat
- Jika ada "command not found" setelah install tool → tutup dan buka ulang terminal
