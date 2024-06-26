---
id: installation
title: Installation
sidebar_label: Installation
---

`$ npm install react-native-google-cast --save`

or

`$ yarn add react-native-google-cast`

## Expo

Since Expo SDK 42, you can use this library in a custom-built Expo app.
There is a config plugin included to auto-configure `react-native-google-cast` when the native code is generated (`npx expo prebuild`).

> This package cannot be used in the "Expo Go" app because [it requires custom native code](https://docs.expo.io/workflow/customizing/).

Add the [config plugin](https://docs.expo.io/guides/config-plugins/) to the [`plugins`](https://docs.expo.io/versions/latest/config/app/#plugins) array of your `app.json` or `app.config.js`:

```json
{
  "expo": {
    "plugins": ["react-native-google-cast"]
  }
}
```

Next, rebuild your app as described in the ["Adding custom native code"](https://docs.expo.io/workflow/customizing/) guide.

Then ignore the rest of this page and jump straight to [Usage](usage).

## iOS

#### a. Autolinking (recommended)

By default, the package installs the `NoBluetoothArm` dependency, which is compatible with ARM Macs and doesn't enable Guest Mode (which requires an extra set of Bluetooth permissions that are not needed for many use cases).

`cd ios && pod install`

Note that the latest Google Cast SDK (currently [4.8.0](https://developers.google.com/cast/docs/release-notes#july-20,-2023)), as well as this package, only supports iOS 13 or newer. If you need to support older iOS versions, please use an older version of the library but note that some features might not be available.

#### b. Custom version or guest mode

However, if you'd like to install a different version of the Google Cast SDK:

1. In your `ios/Podfile`, add **one** of these snippets:

   - i. If you [don't need guest mode](https://developers.google.com/cast/docs/ios_sender/ios_permissions_changes#need_to_remove_guest_mode_support) and want to develop on an ARM Mac (this is the default option), add

     ```
     pod 'react-native-google-cast/NoBluetoothArm', path: '../node_modules/react-native-google-cast/'
     ```

   - ii. If you [want to support guest mode](https://developers.google.com/cast/docs/ios_sender/ios_permissions_changes#need_to_keep_guest_mode_support), add (and don't forget step 5 in the [Setup](setup#ios)):

     ```
     pod 'react-native-google-cast/GuestModeArm', path: '../node_modules/react-native-google-cast/'
     ```

   - iii. If you don't need to build on an ARM Mac, and you're having trouble installing the Arm version, you can use the `NoBluetooth` or `GuestMode` dependency instead.

     ```
     pod 'react-native-google-cast/NoBluetooth', path: '../node_modules/react-native-google-cast/'
     ```

     or for Guest Mode (don't forget step 5 in the [Setup](setup#ios)):

     ```
     pod 'react-native-google-cast/GuestMode', path: '../node_modules/react-native-google-cast/'
     ```

2. If your `ios/Podfile` contains `use_native_modules!` (it's the default), you'll need to disable autolinking for this package, otherwise the dependency you added in the previous step will conflict with the autolinked one. To do so, create `react-native.config.js` in the root of your project with this content:

   ```js
   module.exports = {
     dependencies: {
       'react-native-google-cast': {
         platforms: {
           ios: null, // this will disable autolinking for this package on iOS
         },
       },
     },
   }
   ```

3. Finally, run `pod install`.

## Android

Insert the following into `android/app/build.gradle`:

```java
dependencies {
  // ...
  implementation "com.google.android.gms:play-services-cast-framework:${safeExtGet('castFrameworkVersion', '+')}"
}

def safeExtGet(prop, fallback) {
  rootProject.ext.has(prop) ? rootProject.ext.get(prop) : fallback
}
```

By default, the latest version (`+`) of the Cast SDK is used. To use a specific version, set it in the root `android/build.gradle`:

```java
buildscript {
  ext {
    buildToolsVersion = "34.0.0"
    minSdkVersion = 22
    compileSdkVersion = 34
    targetSdkVersion = 34
    castFrameworkVersion = "21.4.0" // <-- Cast SDK version
  }
}
```

## Chrome

Not supported yet
