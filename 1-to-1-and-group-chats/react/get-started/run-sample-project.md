Agora provides an open source sample project that demonstrates how to quickly build a chat page with UIKit. This page explains how to compile and run it. 

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- MacOS 12 or above;
- React Native 0.71 or above;
- NodeJs 16.18 or above;
- For iOS, Xcode 14 or above;
- For Android, Android Studio 2022 or above;
- You have a valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## AppServer

The example project needs to configure AppServer to run. Deploy the AppServer service on the server side and implement the RESTful API on the client side.

In this sample project, the server address is configured in `RestApi.setServer`. Interfaces for obtaining mobile phone verification codes, logging in with mobile phone numbers, uploading avatars, and obtaining `rtcToken`, `rtcMap`, and group owner avatars are provided.

See details in `example/src/demo/common/rest.api.ts`.

## Implementation

Take the following steps to download and run the sample project:

1. Download the sample code from [GitHub](https://github.com/easemob/react-native-chat-library).

1. Initialize the project

   1. After the download is complete, open the file directory and run the following command to initialize the project:
      
      ```yarn && yarn uikit-prepack```

   1. Add the dependencies:

      - For iOS, Run `pod install` to initialize the native dependency configuration:
        
        ```
        cd example/ios && pod install
        ```

     - For Android, use the Android Studio to open the example/android directory abd sync the sample project.

1. Log in to Agora Console and get an App ID. Then fill in your user ID, App ID, and token in `example/src/env.ts`:

   ```javascript
   export const useSendBox = false;
   export const isDevMode = true;
   export const appKey = "xxx";
   export const accountType = "easemob"; 
   export const agoraAppId = "xxx";
   export const fcmSenderId = "xxx";
   export const account = [{ id: "xxx", token: "xxx" }];
   ```
   
   Manually set the region in `packages/react-native-chat-uikit/src/config.local.ts`. The currently supported ones are domestic and overseas. 

   ```javascript
   export const language = "zh-Hans"; // 'en' or 'zh-Hans'
   export const release_area = "china"; // 'china' or 'global'
   ```.

1. Compile and run

Run the local service in the debug mode. During debugging, you can modify the JavaScript file and view the effect of the modification.

```
yarn run ios
```

or 

```
yarn run android
```

## FAQ

Q: Why won't the app compile on Android? 
A: You may need a `cmake 3.10.2` plugin for Android Studio.

