GNUstep Android Toolchain
=========================

This project comprises a collection of scripts to build a GNUstep toolchain for Android. The toolchain can then be used in an Android project to compile and run Objective-C code using the Foundation and CoreFoundation libraries.

The toolchain is built using the Android NDK (installed e.g. via [Android Studio](https://developer.android.com/studio)), and is set up to target Android API level 21 (5.0 / Lollipop) and all common Android ABIs (armeabi-v7a, arm64-v8a, x86, x86_64).

Libraries
---------

The toolchain currently compiles the following libraries for Android:

* [GNUstep Base Library](https://github.com/gnustep/libs-base) (Foundation)
* [GNUstep CoreBase Library](https://github.com/gnustep/libs-corebase) (CoreFoundation)
* [libobjc2](https://github.com/gnustep/libobjc2) (using gnustep-2.0 runtime)
* [libdispatch](https://github.com/apple/swift-corelibs-libdispatch) (official Apple release from the Swift Core Libraries)
* [libffi](https://github.com/libffi/libffi)
* [libxml2](https://github.com/GNOME/libxml2)
* [libxslt](https://github.com/GNOME/libxslt)
* [ICU](https://github.com/unicode-org/icu)

Requirements
------------

Supported host platforms are macOS and Linux.

You must have the Android NDK installed. The toolchain assumes the following version to be installed via [Android Studio](https://developer.android.com/studio)’s SDK Manager:

* NDK (Side by side) _– version 21.1.6352462_

A different NDK version or location can be provided using the `--ndk` flag (see below). Please note that NDK r21 (or later) is required, as earlier NDK releases contain Clang versions with bugs which prevent usage of the gnustep-2.0 Objective-C runtime.

Additionally, the following packages are required depending on your system.

**macOS**

Install required packages via [Homebrew](https://brew.sh):

```
brew install git-lfs cmake autoconf automake libtool pkg-config
git lfs install
```

**Linux**

Install required packages via APT:

```
sudo apt install git git-lfs curl cmake make autoconf libtool pkg-config texinfo python3-distutils
git lfs install
```

Please note that you need to have CMake version 3.15.1 or later ([for libdispatch](https://github.com/apple/swift-corelibs-libdispatch/blob/master/CMakeLists.txt#L2)).

Usage
-----

Run the [build.sh](build.sh) script to build the toolchain:

```
Usage: ./build.sh
  -n, --ndk NDK_PATH         Path to existing Android NDK (default: ~/Library/Android/sdk/ndk/21.1.6352462)
  -a, --abis ABI_NAMES       ABIs being targeted (default: "armeabi-v7a arm64-v8a x86 x86_64")
  -l, --level API_LEVEL      Android API level being targeted (default: 21)
  -b, --build BUILD_TYPE     Build type "Debug" or "Release" or "RelWithDebInfo" (default: RelWithDebInfo)
  -u, --no-update            Don't update projects to latest version from GitHub
  -c, --no-clean             Don't clean projects during build (e.g. for building local changes, only applies to first ABI being built)
  -p, --patches DIR          Apply additional patches from given directory
  -o, --only PHASE           Build only the given phase (e.g. "gnustep-base", requires previous build)
  -h, --help                 Print usage information and exit
```

The toolchain automatically downloads and installs the NDK and prebuilt Clang release (via [install-ndk.sh](install-ndk.sh)), and builds and installs the GNUstep toolchain into the following location (`$GNUSTEP_HOME`):

* macOS: `~/Library/Android/GNUstep`
* Linux: `~/Android/GNUstep`

The build for each supported ABI is installed into its separate subfolder at that location (both libraries and header files differ per ABI).

To use the toolchain from an Android project, you can use `$GNUSTEP_HOME/$ABI_NAME/bin/gnustep-config` to obtain various flags that should be used to compile and link Objective-C files, e.g.

* `gnustep-config --variable=CC`
* `gnustep-config --objc-flags` (or `--debug-flags`)
* `gnustep-config --base-libs`

Call `gnustep-config --help` to obtain the full list of available variables.

You may also want to configure your app’s build environment to use the custom NDK (e.g. in Android Studio), which is installed into the following location:

* macOS: `~/Library/Android/android-ndk-<NDK_REVISION>-clang-<CLANG_VERSION>`
* Linux: `~/Android/android-ndk-<NDK_REVISION>-clang-<CLANG_VERSION>`

Examples
--------

The [android-examples](https://github.com/gnustep/android-examples) repository contains example projects using this project.

Acknowledgements
----------------

Based on original work by Ivan Vučica.
