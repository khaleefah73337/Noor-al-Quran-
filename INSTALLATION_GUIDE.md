# Getting Started - Setup & Installation Guide

## 🚀 Quick Start

This guide will help you set up the Noor al-Quran development environment and prepare the app for installation on your Android phone.

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

### Required Software

1. **Flutter SDK** (v3.0.0 or higher)
   - Download: https://flutter.dev/docs/get-started/install
   - Add to PATH environment variable
   - Verify: `flutter doctor`

2. **Android Studio** (Latest version)
   - Download: https://developer.android.com/studio
   - Install Android SDK (API 21+)
   - Install Android Emulator (optional)

3. **Git**
   - Download: https://git-scm.com/download
   - Verify: `git --version`

4. **Java Development Kit (JDK)**
   - Android Studio includes JDK
   - Or download separately: https://www.oracle.com/java/technologies/downloads/

## 🔧 Development Environment Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/khaleefah73337/Noor-al-Quran-.git
cd Noor-al-Quran-
```

### Step 2: Install Flutter Dependencies

```bash
flutter pub get
flutter pub upgrade
```

### Step 3: Verify Flutter Setup

```bash
flutter doctor
```

Expected output should show:
- ✓ Flutter (installed correctly)
- ✓ Android toolchain (configured)
- ✓ Android Studio (installed)
- ✓ No iOS development (if not on macOS)

### Step 4: Configure Android Project

**File: `android/app/build.gradle`**

```gradle
android {
    compileSdkVersion 34
    
    defaultConfig {
        applicationId "com.noor_al_quran.app"
        minSdkVersion 21
        targetSdkVersion 34
        versionCode 1
        versionName "1.0.0"
    }
    
    signingConfigs {
        release {
            keyAlias 'quran_key'
            keyPassword 'your_key_password'
            storeFile file('keystore.jks')
            storePassword 'your_store_password'
        }
    }
    
    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled true
            shrinkResources true
        }
    }
}
```

### Step 5: Firebase Setup

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Create new project: "Noor al-Quran"
3. Register Android app
4. Download `google-services.json`
5. Place in `android/app/google-services.json`

**File: `android/app/google-services.json`**
```json
{
  "project_info": {
    "project_number": "YOUR_PROJECT_NUMBER",
    "project_id": "your-project-id",
    "storage_bucket": "your-project-id.appspot.com"
  },
  "client": [
    {
      "client_info": {
        "mobilesdk_app_id": "1:YOUR_PROJECT_NUMBER:android:YOUR_APP_ID",
        "android_client_info": {
          "package_name": "com.noor_al_quran.app"
        }
      }
    }
  ]
}
```

## 📱 Installation on Android Phone

### Method 1: Direct Installation (Recommended)

#### Step 1: Create Release APK

```bash
# Clean previous builds
flutter clean

# Get dependencies
flutter pub get

# Build release APK
flutter build apk --release
```

Output location: `build/app/outputs/apk/release/app-release.apk`

#### Step 2: Enable Installation from Unknown Sources (Android)

1. Go to **Settings**
2. Navigate to **Security** or **Apps**
3. Enable **"Install from Unknown Sources"** or **"Unknown apps"**
4. Alternatively, enable only for specific apps like Chrome or file manager

#### Step 3: Transfer APK to Phone

**Option A: USB Transfer**
```bash
# Connect phone via USB
# Enable USB Debugging in Developer Options
adb devices  # Verify phone is connected

# Transfer APK
adb install build/app/outputs/apk/release/app-release.apk
```

**Option B: Direct Download**
1. Email the APK to yourself
2. Download on your phone
3. Tap to install

**Option C: File Manager**
1. Copy APK to phone storage
2. Open file manager
3. Navigate to APK file
4. Tap to install

#### Step 4: Verify Installation

After installation:
1. App appears on home screen
2. Tap to launch
3. Qur'an data loads
4. See surah list
5. Select reciter and play audio

### Method 2: Android Studio Installation

```bash
# Connect phone via USB
flutter run --release
```

This will automatically build and install on your connected device.

### Method 3: App Bundle for Play Store (Future)

```bash
# Build App Bundle for Google Play Store
flutter build appbundle --release
```

Output: `build/app/outputs/bundle/release/app-release.aab`

## 🔑 Generate Signing Key

If you don't have a signing key:

```bash
# Generate keystore
keytool -genkey -v -keystore ~/quran_key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias quran_key

# Follow prompts:
# - Keystore password: [create one]
# - Key password: [create one]
# - Name: Your Name
# - Organization: Noor al-Quran
# - City: Your City
# - State: Your State
# - Country: Your Country Code (e.g., NG for Nigeria)

# Copy to project
cp ~/quran_key.jks android/app/keystore.jks
```

**File: `android/key.properties`**
```properties
storePassword=your_store_password
keyPassword=your_key_password
keyAlias=quran_key
storeFile=keystore.jks
```

**File: `android/app/build.gradle`** (Update signing config)
```gradle
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android {
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
        }
    }
}
```

## 🐛 Troubleshooting

### Issue: "Flutter not found"
```bash
# Add Flutter to PATH
export PATH="$PATH:/path/to/flutter/bin"
# Make permanent by adding to ~/.bashrc or ~/.zshrc
```

### Issue: "Android SDK not found"
```bash
# Set ANDROID_SDK_ROOT
export ANDROID_SDK_ROOT=/path/to/android/sdk
```

### Issue: "Gradle build failed"
```bash
# Clean and rebuild
flutter clean
flutter pub get
flutter build apk --release
```

### Issue: "App crashes on launch"
1. Check Logcat: `adb logcat | grep flutter`
2. Verify Firebase configuration
3. Check file permissions
4. Ensure Qur'an data is present

### Issue: "Audio not playing"
1. Check device volume is up
2. Verify audio files are downloaded
3. Check WiFi/cellular connection
4. Restart app

### Issue: "Storage permission denied"
1. Go to **Settings** > **Apps** > **Noor al-Quran**
2. Tap **Permissions**
3. Enable **Storage** access
4. Restart app

### Issue: "App won't install"
1. Ensure device has enough storage (>500MB)
2. Uninstall previous version: `adb uninstall com.noor_al_quran.app`
3. Re-install APK
4. Check Android version compatibility (API 21+)

## 📊 System Requirements

### Minimum Requirements
- **Android**: 5.0 (API 21)
- **RAM**: 2 GB
- **Storage**: 500 MB available
- **Screen**: 4.5"

### Recommended Requirements
- **Android**: 8.0+ (API 26+)
- **RAM**: 4 GB or more
- **Storage**: 2 GB available
- **Screen**: 5.5"+

## 📲 ADB Commands Reference

```bash
# List connected devices
adb devices

# Install APK
adb install path/to/app.apk

# Reinstall APK (uninstall first)
adb install -r path/to/app.apk

# Uninstall app
adb uninstall com.noor_al_quran.app

# View app logs
adb logcat | grep flutter

# Take screenshot
adb shell screencap -p /sdcard/screenshot.png
adb pull /sdcard/screenshot.png

# Record video
adb shell screenrecord /sdcard/video.mp4
adb pull /sdcard/video.mp4

# Enable USB debugging
adb shell settings put global adb_enabled 1

# Clear app data
adb shell pm clear com.noor_al_quran.app
```

## 🔍 Verify Installation

After installing, verify everything works:

1. **Launch App**
   - Tap Noor al-Quran icon
   - Should load in <3 seconds
   - See surah list

2. **Test Qur'an Browsing**
   - Scroll through surahs
   - Tap surah to view ayahs
   - Verify Arabic text displays correctly

3. **Test Audio**
   - Select reciter (default: Mishary Alafasy)
   - Tap play button
   - Should hear audio playback
   - Volume controls work

4. **Test Other Features**
   - Add bookmark
   - Add note to bookmark
   - Search surahs
   - View translations (if available)
   - Check Tasbih counter

## 📈 Development Workflow

```bash
# Start development
flutter run

# Run on specific device
flutter run -d <device_id>

# Debug mode with breakpoints
flutter run -d <device_id> --debug

# Profile app performance
flutter run --profile

# Release build (optimized)
flutter run --release

# Hot reload (during development)
# Press 'r' in terminal

# Hot restart
# Press 'R' in terminal

# Stop app
# Press 'q' in terminal
```

## 🎯 Next Steps

After successful installation:

1. **Explore Features**
   - Browse all 114 surahs
   - Listen to different reciters
   - Create bookmarks
   - Take notes

2. **Test Admin Panel**
   - Enter PIN: `khaleefah37`
   - Manage reciters
   - View statistics
   - Upload audio (requires Firebase setup)

3. **Provide Feedback**
   - Report bugs
   - Suggest improvements
   - Share usage experience

4. **Development**
   - Read PHASE files for features
   - Implement missing functionality
   - Contribute improvements
   - Follow contributing guidelines

## 📞 Support

For issues or questions:
1. Check troubleshooting section above
2. Review Flutter documentation: https://flutter.dev/docs
3. Check Android documentation: https://developer.android.com
4. Open GitHub issue: https://github.com/khaleefah73337/Noor-al-Quran-/issues

## 🔐 Security Notes

- **Never commit** `key.properties` or keystore to git
- **Never share** admin PIN
- **Secure Firebase** credentials
- **Use HTTPS** for all API calls
- **Encrypt** sensitive local data

## 📝 Useful Resources

- [Flutter Installation Guide](https://flutter.dev/docs/get-started/install)
- [Android Studio Setup](https://developer.android.com/studio/intro)
- [Firebase Setup for Flutter](https://firebase.flutter.dev/docs/overview)
- [Flutter Best Practices](https://flutter.dev/docs/testing/best-practices)
- [Android Security & Privacy](https://developer.android.com/privacy-and-security)

---

**Created**: August 25, 2026  
**Version**: 1.0  
**Status**: Production Ready  

🕌 **Noor al-Quran** - Ready for your Android device!
