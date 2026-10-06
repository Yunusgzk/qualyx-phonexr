# APK build

GitHub Actions workflow: `.github/workflows/android.yml`.

It installs Android API 35, CMake 3.22.1 and NDK 27.2.12479018, then builds an arm64-v8a debug APK.

The APK packages the reference HLVR native payload under `lib/arm64-v8a/` and loads `libphonexr_runtime.so` before `libhlvr_host_real.so`.

This is a bring-up APK, not yet a claim of full Alyx compatibility. Device validation should collect `adb logcat | grep -E 'PhoneXR|DEBUG|AndroidRuntime'`.
