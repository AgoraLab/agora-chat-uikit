# Quickstart

With UIKit, you can easily implement messaging in one-to-one chats and group chats. This page explains how do this for a one-to-one chat.

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- MacOS 12 or above;
- React Native 0.71 or above;
- NodeJs 18.16 or above;
- For iOS: Xcode 14 or above;
- For Android: Android Studio 2022 or above;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details.

## Implementation

1. Create a new project.

   Run the following command to create a project:

   ```sh
   # old cli
   npx react-native --version 0.73.2 init ProjectName

   # latest cli
   npx @react-native-community/cli init ProjectName
   ```

   You may be prompted to install the latest version of React.

   Once the creation is completed, the project will be initialized by default, `node_modules` dependencies will be installed, and `package-lock.json` file will be generated. If you use yarn initialization, `yarn.lock` file will be generated.

1. Add dependencies.

   ```bash
    yarn add @react-native-async-storage/async-storage \
    @react-native-camera-roll/camera-roll \
    @react-native-clipboard/clipboard \
    date-fns \
    pinyin-pro \
    pure-uuid \
    react-native-agora-chat-uikit \
    react-native-agora-chat-sdk \
    react-native-audio-recorder-player \
    react-native-create-thumbnail \
    react-native-device-info \
    react-native-document-picker \
    react-native-fast-image \
    react-native-file-access \
    react-native-gesture-handler \
    react-native-get-random-values \
    react-native-image-picker \
    react-native-permissions \
    react-native-safe-area-context \
    react-native-screens \
    react-native-video \
    react-native-web \
    react-native-webview \
    twemoji
   ```

   - For iOS, update the `ProjectName/Info.plist` file to add the following permissions:

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

   - For Android, update the `AndroidManifest.xml` file:

     ```xml
     <manifest xmlns:android="http://schemas.android.com/apk/res/android">
         <uses-permission android:name="android.permission.INTERNET"/>
         <uses-permission android:name="android.permission.CAMERA" />
         <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
         <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
         <uses-permission android:name="android.permission.RECORD_AUDIO" />
     </manifest>
     ```

1. Add code.

   The main code added includes logging in, logging out, and sending messages:

   ```typescript
   import * as React from "react";
   import { Pressable, SafeAreaView, Text, View } from "react-native";

   // Components used by UIKIT in quickstart.
   import {
     Container,
     ConversationDetail,
     TextInput,
     useChatContext,
   } from "react-native-agora-chat-uikit";

   const appKey = "<your app key>";
   const userId = "<current login id>";
   const userPs = "<current login password or token>";
   const peerId = "<chat peer id>";

   function SendMessage() {
     const [page, setPage] = React.useState(0);
     const [appkey, setAppkey] = React.useState(appKey);
     const [id, setId] = React.useState(userId);
     const [ps, setPs] = React.useState(userPs);
     const [peer, setPeer] = React.useState(peerId);
     const im = useChatContext();

     if (page === 0) {
       // Load the login page.
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
               // Login to im server
               im.login({
                 userId: id,
                 userToken: ps,
                 usePassword: true,
                 result: (res) => {
                   console.log("login result", res);
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
               // Logout from im server
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
       // Load the chat page component, which can perform chat operations.
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
     // Initialize UIKIT at the entry root node.
     return (
       <Container options={{ appKey: appKey, autoLogin: false }}>
         <SendMessage />
       </Container>
     );
   }

   export default App;
   ```

1. Compile and run the project.

   - For iOS: Run `yarn run ios`.
   - For Android: Run `yarn run android`.

2. Send the first message.

   Click **Login** to enter the chat page, enter the text, and click **Send**.


## Q&A

#### yarn vs npm in react-native ?

For `react-native` developers, `yarn` tool is usually used for management. The main basis is as follows:

1. Use the command recommended by the official website to create an application project. The project management configuration uses `yarn` by default. You can verify it by creating a project `npx @react-native-community/cli init xxx-app`
2. Check the help document of the cli recommended by the official website to create an application `npx @react-native-community/cli init --help `
3. Use the official recommended command to create a library project. The project management configuration uses `yarn` by default. You can verify it by creating a project `npx create-react-native-library@0.34.3 --react-native-version 0.72.17 react-native-xxx-lib`
4. After creating a project, some scripts are completed through `yarn`, which may automatically execute actions or behaviors, while `npm` cannot trigger automatic execution.