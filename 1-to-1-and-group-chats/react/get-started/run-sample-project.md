# Run the sample project

Agora provides an open-source sample project that demonstrates how to quickly build a chat page with UIKit. This page explains how to compile and run it. 

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- MacOS 12 or above;
- React Native 0.71 or above;
- NodeJs 18.16 or above;
- For iOS: Xcode 14 or above;
- For Android: Android Studio 2022 or above;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## AppServer

Configure AppServer for the sample project to run. Deploy AppServer on the server side and implement the RESTful API on the client side.

In this sample project, the server address is configured in `RestApi.setServer`. Interfaces for obtaining mobile phone verification codes, logging in with mobile phone numbers, uploading avatars, and obtaining `rtcToken`, `rtcMap`, and group owner avatars are provided.

See details in `example/src/demo/common/rest.api.ts`.

## Implementation

Take the following steps to download and run the sample project:

1. Download the sample code from [GitHub](https://github.com/AgoraIO/Agora-Chat-API-Examples).

1. Initialize the project.

   1. After the download is complete, open the file directory and run the following command:
      
      ```
      yarn && yarn yarn-prepack
      ```

   1. Add the dependencies.

      - For iOS, Run `pod install` to initialize the native dependency configuration:
        
         ```
         cd examples/uikit-example/ios && pod install
         ```

      - For Android, use the Android Studio to open the `example/android` directory and sync the sample project.

1. Configure locally. 

   Fill in your user ID, app ID, and token in `example/src/env.ts`:

      ```typescript
      export const appKey = "<your app key>";
      export const account = [{ id: "<your user id>", token: "<your user token>" }];
      ```

2. Compile and run.

    Run the local service in the debug mode. During debugging, you can modify the JavaScript file and view the effect of the modification.
    
    ```
    yarn run ios
    ```
    
    or 
    
    ```
    yarn run android
    ```

