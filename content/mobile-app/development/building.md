# Building & Distributing to App Stores

This guide explains how to build and distribute the **Kelowna Islamic Center Mobile App** for both **Android** and **iOS**. The app is built with **Flutter** and connects to Firebase services (Authentication, Firestore, Cloud Messaging, and Cloud Functions).

---

## Prerequisites

- You fully followed the [installation guide](./installation.md) and have all the SDKs and development environemnts setup.
- Have **developer access** to a Play Store Developer account (for Android)
- Have **developer access** to App Store Connect (for iOS)
- Have connected the application to the production firebase project. You can follow the [Installation Guide Firebase Guide](./installation.md#5-firebase-setup) again if unsure.
    - `google-services.json` (Android)  
    - `GoogleService-Info.plist` (iOS)

    !!! warning
        If these files are misconfigured or not created, Firebase features (authentication, messaging, Firestore, etc.) will not function.

---

## Building for Android

Before beginning this part, ensure you have read and understood the [official Flutter Android deployment guide](https://docs.flutter.dev/deployment/android) as this is just a short reference adapted from the official guide.

!!! warning
    It advised that you use the official guide for your first deployment and only follow this as a future reference once you are familiar with the full process.

1. **Signing**

    1. Create a `key.properties` file in the root of the `android` folder.
    2. Add your store password, key passowrd, alias, and path to the keystore file to the `key.properties` file (you should have all of these provided if you are deploying to the official KIC Play Store listing). This should look something like this:

        ```env
        storePassword=myStorePassword
        keyPassword=myKeyPassword
        keyAlias=upload
        storeFile=/Users/MyUser/android-keys/upload-keystore.jks
        ```

    3. Ensure `build.gradle` is updated with the signing configuration. This should already be the case and is not required unless you modified the gradle file.

2. **Create a Release Build**

    Increment the version name and number in `pubspec.yaml`. Not doing this will cause Google Play to deny you from releasing the app as the version number will already be existing. This should look something like this if the previous version was `4.1.0+26`:

    ```yaml
    version: 4.1.1+27
    ```

    and then build the release APK file:

    ```bash
    flutter build apk --release
    ```

    or, the app bundle (this is recommended for Play Store):

    ```bash
    flutter build appbundle --release
    ```

3. **Distribution**
    * Create a new release in the **Google Play Console**
    * Upload the generated **AAB** or **APK** file.
    * For testing before release, use Internal Testing.

---

## Building for iOS

Before beginning this part, ensure you have read and understood the [official Flutter iOS deployment guide](https://docs.flutter.dev/deployment/ios) as this is just a short reference adapted from the official guide.

!!! warning
    It advised that you use the official guide for your first deployment and only follow this as a future reference once you are familiar with the full process.

1. **Xcode Configuration**

    * Open the generated `ios/Runner.xcworkspace` in Xcode.
    * Ensure the correct Apple Developer Team is selected.
    * Configure signing and capabilities.

2. **Create a Release Build**

    ```bash
    flutter build ios --release
    ```

4. **Distribution**

   * Use Xcode’s **Archive & Distribute** option.
   * Distribute via **TestFlight** for internal/external testing.
   * Submit to the **App Store** for public distribution.

---

## CI/CD (Future Work)

Currently, builds are performed manually. In the future, **CI/CD pipelines** should be added through Github Actions to automate: 

* Test runs before builds
* Automated distribution to Play Store/TestFlight
* Version bumping
