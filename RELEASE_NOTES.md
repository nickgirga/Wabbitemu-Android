# Wabbitemu Android - Release Build Instructions

## Release APK Status: ✅ READY FOR INSTALLATION

The release APK has been successfully built, signed, and aligned for installation on modern Android devices.

### APK Details

**Location:** `app/build/outputs/apk/release/app-release.apk`

**Signing:** Android Debug Certificate (suitable for development/testing)
- Certificate DN: C=US, O=Android, CN=Android Debug
- SHA-256: e6734af6c6f17983f9a0c49106723655b043d1da81a47f4924c255925a118ea2

**Alignment:** ✅ Verified (4-byte aligned)

**Architecture Support:**
- ✅ arm64-v8a (383 KB)
- ✅ armeabi-v7a (297 KB)
- ✅ x86 (374 KB)
- ✅ x86_64 (399 KB)

**Version Information:**
- Package: io.github.angelsl.wabbitemu
- Version Code: 39
- Version Name: 1.06.0
- Min SDK: 21 (Android 5.0 Lollipop)
- Target SDK: 34 (Android 14)

**Build Features:**
- R8 code shrinking enabled
- ProGuard obfuscation enabled
- Optimized native libraries

## Installation Instructions

### On Physical Device
```bash
adb install app/build/outputs/apk/release/app-release.apk
```

### On Emulator
```bash
adb -e install app/build/outputs/apk/release/app-release.apk
```

### Via File Manager
1. Copy `app-release.apk` to your Android device
2. Enable "Install from Unknown Sources" in Settings
3. Open the APK file and tap Install

## Production Release (Play Store)

For production release to Google Play Store, you'll need to:

1. **Create a Release Keystore:**
```bash
keytool -genkey -v -keystore release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias wabbitemu
```

2. **Update app/build.gradle:**
```gradle
android {
    signingConfigs {
        release {
            storeFile file('release-key.jks')
            storePassword 'your-password'
            keyAlias 'wabbitemu'
            keyPassword 'your-password'
        }
    }
    
    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
```

3. **Build Release APK:**
```bash
./gradlew assembleRelease
```

4. **Or Build AAB (Android App Bundle) for Play Store:**
```bash
./gradlew bundleRelease
```

The AAB will be at: `app/build/outputs/bundle/release/app-release.aab`

## Testing Checklist

Before releasing, verify:
- [ ] App installs successfully on arm64-v8a devices
- [ ] Calculator buttons respond accurately to touch
- [ ] Emulation runs smoothly without freezing
- [ ] File picker works for loading ROM files
- [ ] All calculator models work (TI-73, TI-83+, TI-84+, etc.)
- [ ] App doesn't crash on startup
- [ ] ROM state saves and loads correctly

## Build Configuration Summary

All issues fixed in this release:
1. ✅ ARM64-v8a architecture support
2. ✅ Button hitbox alignment
3. ✅ Emulation core stability
4. ✅ Modern file picker APIs
5. ✅ Build system modernization
6. ✅ Release build lint configuration

See `FIXES_SUMMARY.md` for detailed technical information about all fixes.
