# Mobile App Installation Guide

This guide will walk you through setting up a complete development environment to contribute to the app. It covers installation of Flutter, Android and iOS development tools, and Firebase configuration.

By the end of this guide, you’ll have a fully working local development setup, ready to build and run the app on Android and iOS devices.

---

## Pre-requisites

Before installing, ensure you have [Visual Studio Code (VS Code)](https://code.visualstudio.com/) installed locally.

While you may use other editors, this guide and the rest of the documentation are written for VS Code and its strongly recommended.

## 1. Install Flutter SDK

!!! note
    This guide is adapted from the [official Flutter installation guide](https://docs.flutter.dev/install). 
    Follow the official guide if this section does not work for you.

=== "VS Code Installation (Recommended)"

    1. Launch VS Code with the mobile repo open
    2. Add the Flutter extension to VS Code
    3. Once installed, open the command palette in VS Code (Go to View > Command Palette or press CTRL/Command + Shift + P)
    4. In the command palette, type flutter and Select Flutter: Run Flutter Doctor
    5. VS Code prompts you to locate the Flutter SDK on your computer. Select Download SDK.
    6. When the Select Folder for Flutter SDK dialog displays, choose where you want to install Flutter.

        === "macOS / Linux"

            Select a folder like `~/development/flutter`

        === "Windows"
            
            Select a folder like `C:/Users/{YourUser}/development/flutter`

    7. Click Clone Flutter. This download takes a few minutes. If you suspect that the download has hung, click Cancel then start the installation again.
    8. Once installed, click "Add SDK to PATH".
    9. To ensure you installed Flutter correctly, run
        ```bash 
        flutter doctor -v
        ```

=== "Manual Installation"

    1. Download the latest stable Flutter SDK from the official site:  
    [Flutter Installation](https://docs.flutter.dev/get-started/install)

    2. Extract the Flutter SDK to a permanent location on your machine where you can find it.

        === "macOS / Linux"

            Extract to a folder like `~/development/flutter`

        === "Windows"
            
            Extract to a folder like `C:/Users/{YourUser}/development/flutter`

    3. Add Flutter to your system `PATH`.

        === "macOS / Linux"

            ```bash
            export PATH="$PATH:`pwd`~/development/flutter/bin"
            ```

        === "Windows"
            ```powershell
            setx PATH "%PATH%;C:\Users\{YourUser}\development\flutter\bin"
            ```

    4. Verify installation:

        ```bash
        flutter doctor
        ```

    5. Install the VS Code Flutter and Dart extensions

---

## 2. Install Dependencies & Setup env

Run the following to install project dependencies:

```bash
flutter pub get
```

Once installed, duplicate the `lib/config.example.dart` file as `lib/config.dart` and modify the following values in the config file. 

Leave everything else as default within the file unless required during development.

```dart
...
// Replace 'yourapilink' with your prayerTimesFetch function API endpoint
static String apiLink = "yourapilink";
static String apiLinkForNextDay = "yourapilink?day=tomorrow"; 
...
```

After your flutter environment is setup, configure either an Android environment, iOS environment, or both.

---

## 3. Android Development Setup

### Install Android Studio

1. Download and install [Android Studio](https://developer.android.com/studio).
2. Open **Android Studio** and install:
    * Android SDK
    * Android SDK Platform
    * Android Virtual Device (AVD)
3. Accept the android licenses:

    ```bash
    flutter doctor --android-licenses
    ```

    If this doesn't work, ensure that you have the Android SDK added to your `PATH`.

4. Verify that android is configured properly by running `flutter doctor` and looking for a checkmark next to Android.
5. In Android Studio, create an emulator device targeting the latest Android release
6. Close Android Studio and launch VS Code. You are now ready to test on and build for Android.

---

## 4. iOS Development Setup (macOS Only)

> iOS development requires a macOS machine with Xcode installed.

### Install Xcode

1. Download from the [Mac App Store](https://apps.apple.com/us/app/xcode/id497799835).
2. Open Xcode, agree to the license, and allow Xcode to finish installing all of its development components.
3. Install command-line tools: `xcode-select --install`

### Install CocoaPods

CocoaPods manages iOS dependencies.

```bash
sudo gem install cocoapods
```

Verify installation:

```bash
pod --version
```

Close Xcode and launch VS Code. You are now ready to test on and build for iOS.

---

## 5. Firebase Setup

You now need to configure the app to connect to your firebase project. The mobile app uses FlutterFire which must be configured and run through the root of your project. This part should ideally be done in VS Code through its built in terminal.

### Step 1. Install FlutterFire CLI

The **FlutterFire CLI** configures Firebase for your app:

```bash
dart pub global activate flutterfire_cli
```

Add it to your `PATH`:

=== "macOS / Linux"

    ```bash
    export PATH="$PATH":"$HOME/.pub-cache/bin"
    ```

=== "Windows"
    ```powershell
    setx PATH "$($env:Path);$HOME\.pub-cache\bin"
    ```

### Step 2. Configure Firebase Project

From the project root, run:

```bash
flutterfire configure
```

* Select your Firebase project (`kelownaislamiccenter` if you have access to the production project).
* Enable Android and iOS platforms.
* This generates a `firebase_options.dart` file inside `lib/`. This is an environment file and is automatically ignored by git, do not commit this file.

### Step 3. Add Firebase Config Files

Download the following files from the Firebase Console:

* `google-services.json` → Place in `android/app/`.
* `GoogleService-Info.plist` → Place in `ios/Runner/`.

FlutterFire should now be configured properly and the app should function properly now.

---

## 6. Run the App

=== "VS Code (Recommended)"

    1. Open the `lib/main.dart` file in the editor
    2. Press the play button the top right. If the button does not appear then you do not have the Flutter VS Code extension installed properly.
    3. Connect a Physical Android or iOS device or select an iOS simulator/ Android emulator.
    4. Wait for the app to build and dependencies to install (can take a while on first setup) after which the app should launch.

=== "Manual"

    1. Connect a Physical Android or iOS device or launch an iOS simulator/ Android emulator.
    2. Run the following command and leave it running in the background

        ```bash
        flutter run
        ```

---

## 9. Verify Installation

Run `flutter doctor` again and ensure all checks pass:

```bash
flutter doctor
```

You should see green checkmarks for Flutter, Android toolchain (if you configured Android), iOS toolchain (if you configured iOS), and connected devices.

---

## 8. Troubleshooting

### Flutter SDK not found

Make sure you added `flutter/bin` to your system `PATH` and restart your terminal/IDE.

### Android Emulator not showing up

* In Android Studio → **Tools → Device Manager**, create a new Virtual Device (AVD).
* Ensure the emulator is started before running `flutter run`.

### CocoaPods not installed (iOS)

If `flutter doctor` reports CocoaPods missing, reinstall:

```bash
sudo gem install cocoapods
cd ios
pod install
```

### Android SDK not found

Check that `ANDROID_HOME` points to the correct directory. For most installations:

* macOS/Linux → `$HOME/Library/Android/sdk`
* Windows → `C:\Users\<username>\AppData\Local\Android\sdk`

---

You should now be ready to begin development on the KIC mobile app.
