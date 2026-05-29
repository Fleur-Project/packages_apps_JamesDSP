# JamesDSP for LineageOS / AOSP

This repository provides the JamesDSP audio effect engine and manager app structured for AOSP/LineageOS builds.

## Integration Guide

### 1. Clone the Repository
```bash
git clone https://github.com/Fleur-Project/packages_apps_JamesDSP packages/apps/JamesDSP
```
*Optional - You can clone the [early](https://github.com/Fleur-Project/packages_apps_JamesDSP/tree/early) branch for the newest upstream features, taken from github actions.*
```bash
git clone -b early https://github.com/Fleur-Project/packages_apps_JamesDSP packages/apps/JamesDSP
```
### 2. Inherit the Product
Add the following to your device's makefile (e.g., `device.mk` or `device-common.mk`):
```makefile
# JamesDSP
$(call inherit-product, packages/apps/JamesDSP/config.mk)
```

### 3. Configure Audio Effects (`audio_effects.xml`)
You need to patch your device's `audio_effects.xml` (usually found in `device/<oem>/<device>/audio/` or similar) to declare the JamesDSP library and effect.

Add the library under `<libraries>`:
```xml
<library name="jdsp" path="libjamesdsp.so"/>
```

Add the effect under `<effects>`:
```xml
<effect name="jamesdsp" library="jdsp" uuid="f27317f4-c984-4de6-9a90-545759495bf2"/>
```

### 4. Add SELinux policy
If your device tree already has an `audioserver.te`, add:

```te
get_prop(audioserver, vendor_audio_prop)

allow audioserver unlabeled:file {
    getattr
    open
    read
    write
};

allow hal_audio_default hal_audio_default:process execmem;
```

If not, create a new file. For example:

```text
device/<vendor>/<device>/sepolicy/vendor/audioserver_jamesdsp.te
```

For Google or MTK devices, skip `get_prop(audioserver, vendor_audio_prop)` if the device tree already grants the required vendor audio property access.

## Credits
- **james34602** (Original Creator of JamesDSP)
- **ThePBone** (RootlessJamesDSP - APK & Engine enhancements)
- **ShadoV90** (JamesDSP Magisk/Root Modules)

## Source Credits
### Terms and Conditions / License
The engine frame is based on Antti S. Lankila's DSPManager.

### Credit
- **Joseph Young** (Provider of dynamic range compander logic and varies impulse responses)
- **Christopher Blomeyer** (Very patient app tester and inspiring me bit depth issue)
- **ahrion** (Making installation tools)
- **Zackptg5** (Making installation tools)
