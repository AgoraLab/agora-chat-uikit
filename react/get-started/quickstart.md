With UIKit, you can easily implement messaging in one-to-one chats and group chats. This page explains how do this for a one-to-one chat.

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- MacOS 12 or above;
- React Native 0.71 or above;
- NodeJs 16.18 or above;
- For iOS, Xcode 14 or above;
- For Android, Android Studio 2022 or above.

## Implementation

1. Create a new project 
   
   Run the following command to create a project: 

   ```
   npx react-native --version 0.73.2 init ProjectName
   ```
   
   You may be prompted to install the latest version of React.

   Once the creation is completed, the project will be initialized by default, `node_modules` dependencies will be installed, and `package-lock.json` file will be generated. If you use yarn initialization, `yarn.lock` file will be generated.
   
1. Add dependencies

   UIKit requires additional dependencies. Add them to the `package.json` file.

   ```json
   {
     "dependencies": {
       "@react-native-async-storage/async-storage": "^1.17.11",
       "@react-native-camera-roll/camera-roll": "^5.6.0",
       "@react-native-clipboard/clipboard": "^1.13.2",
       "date-fns": "^2.30.0",
       "pinyin-pro": "^3.18.3",
       "pure-uuid": "^1.6.3",
       "react": "18.2.0",
       "react-native": "0.73.2",
       "react-native-agora": "^4.2.6",
       "react-native-chat-uikit": "2.1.0",
       "react-native-chat-sdk": "1.3.1",
       "react-native-audio-recorder-player": "^3.5.3",
       "@easemob/react-native-create-thumbnail": "^1.6.6",
       "react-native-device-info": "^10.6.0",
       "react-native-document-picker": "^9.0.1",
       "react-native-fast-image": "^8.6.3",
       "react-native-file-access": "^3.0.4",
       "react-native-gesture-handler": "~2.9.0",
       "react-native-get-random-values": "~1.8.0",
       "react-native-image-picker": "^7.0.3",
       "react-native-permissions": "^3.8.0",
       "react-native-safe-area-context": "4.5.0",
       "react-native-screens": "^3.20.0",
       "react-native-video": "^5.2.1",
       "react-native-web": "~0.19.6",
       "react-native-webview": "13.2.2",
       "twemoji": ">=14.0.2"
     }
   }
    ```
   
    - iOS 

      Update the `ProjectName/Info.plist` file to add the following permissions:

      ```xml
      <dict>
        <!-- Start of append section -->
              <key>NSCameraUsageDescription</key>
              <string></string>
              <key>NSMicrophoneUsageDescription</key>
              <string></string>
              <key>NSPhotoLibraryUsageDescription</key>
              <string></string>
        <!-- End of additional section -->
      </dict>
      ```
      
    - Android

     Update the `AndroidManifest.xml` file:

    ```xml
    <manifest xmlns:android="http://schemas.android.com/apk/res/android">
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.CAMERA" />
        <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.RECORD_AUDIO" />
    </manifest>
    ```
   
1. Add code

   The main code added includes logging in, logging out, and sending messages.

   ```typescript
   import * as React from "react";
   import { Pressable, SafeAreaView, Text, View } from "react-native";
   import {
     Container,
     ConversationDetail,
     TextInput,
     useChatContext,
   } from "react-native-chat-uikit";
   
   const appKey = "easemob#easeim";
   const userId = "du004";
   const userPs = "1";
   const peerId = "du005";
   
   function SendMessage() {
     const [page, setPage] = React.useState(0);
     const [appkey, setAppkey] = React.useState(appKey);
     const [id, setId] = React.useState(userId);
     const [ps, setPs] = React.useState(userPs);
     const [peer, setPeer] = React.useState(peerId);
     const im = useChatContext();
   
     if (page === 0) {
       return (
         <SafeAreaView style={{ flex: 1 }}>
           <TextInput
             placeholder="Please App Key."
             value={appkey}
             onChangeText={setAppkey}
           />
           <TextInput
             placeholder="Please Login ID."
             value={id}
             onChangeText={setId}
           />
           <TextInput
             placeholder="Please Login token or password."
             value={ps}
             onChangeText={setPs}
           />
           <TextInput
             placeholder="Please peer ID."
             value={peer}
             onChangeText={setPeer}
           />
           <Pressable
             onPress={() => {
               console.log("test:zuoyu:login", id, ps);
               im.login({
                 userId: id,
                 userToken: ps,
                 usePassword: true,
                 result: (res) => {
                   console.log("login result", res);
                   console.log("test:zuoyu:error", res);
                   if (res.isOk === true) {
                     setPage(1);
                   }
                 },
               });
             }}
           >
             <Text>{"Login"}</Text>
           </Pressable>
           <Pressable
             onPress={() => {
               im.logout({
                 result: () => {},
               });
             }}
           >
             <Text>{"Logout"}</Text>
           </Pressable>
         </SafeAreaView>
       );
     } else if (page === 1) {
       return (
         <SafeAreaView style={{ flex: 1 }}>
           <ConversationDetail
             convId={peer}
             convType={0}
             onBack={() => {
               setPage(0);
               im.logout({
                 result: () => {},
               });
             }}
             type={"chat"}
           />
         </SafeAreaView>
       );
     } else {
       return <View />;
     }
   }
   
   function App(): React.JSX.Element {
     return (
       <Container options={{ appKey: appKey, autoLogin: false }}>
         <SendMessage />
       </Container>
     );
   }
   
   export default App;
   ```

1. Compile and run

   - For iOS: Run `yarn run ios`;
   - For Android: Run `yarn run android`.
    #Step 5: Send the first message


1. Send the first message 

   Click **Login** to enter the chat page, enter the text content, and click **Send**.

## Sample project

See [UIKit sample project](https://github.com/easemob/easemob-uikit-reactnative).