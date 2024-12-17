# Integrate UIKit

Before using UIKit, you need to integrate it into your app. This page explains the necessary steps. 

Unlike the procedure described in the quickstart, UIKit requires the use of routing in the application, to achieve switching and jumping between pages. `react-native` does not have routing features and needs to use a third-party library. It is recommended to use `react-navigation`, since it is more popular than `react-native-navigation`. 

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- MacOS 12 or above;
- React Native 0.71 or above;
- NodeJs 18.16 or above;
- For iOS: Xcode 14 or above;
- For Android: Android Studio 2022 or above;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Existing projects

Existing projects must meet certain conditions. If the React Native, Xcode, or Android Studio version is too old, compilation or running problems may occur. Tools and components of the same period are usually recommended. 

Compatibility is also a common problem of React Native.

## Integrate UIKit npm package

The UIKit npm package requires dependent packages, which can be installed using the following code. You can view dependencies by using the `npm ls` or `yarn list` command.

```
yarn add @react-native-async-storage/async-storage@^1.17.11 \
@react-native-camera-roll/camera-roll@^5.6.0 \
@react-native-clipboard/clipboard@^1.11.2 \
date-fns@^2.30.0 \
pinyin-pro@^3.18.3 \
pure-uuid@^1.6.3 \
react@18.2.0 \
react-native@0.73.2 \
react-native-agora@^4.2.6 \
react-native-chat-uikit@2.3.0 \
react-native-chat-sdk@1.5.1 \
react-native-audio-recorder-player@^3.5.3 \
@easemob/react-native-create-thumbnail@^1.6.6 \
react-native-device-info@^10.6.0 \
react-native-document-picker@^9.0.1 \
react-native-fast-image@^8.6.3 \
react-native-file-access@^3.0.4 \
react-native-gesture-handler@~2.9.0 \
react-native-get-random-values@~1.8.0 \
react-native-image-picker@^7.0.3 \
react-native-permissions@^3.8.0 \
react-native-safe-area-context@4.5.0 \
react-native-screens@^3.20.0 \
react-native-video@^5.2.1 \
react-native-web@~0.19.6 \
react-native-webview@13.2.2 \
twemoji@>=14.0.2
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

## Integrate UI components

The component library provides themes, internationalization, contact list, conversation list, and conversation details pages. These pages support default and customized settings.

1. Initialize the components.

   Before using any UI component, it must be initialized. Components can complete global configuration and initialization.

   For example: 

   ```typescript
   function App(): React.JSX.Element {
     return (
       <Container options={{ appKey: "appKey" }}>
         {/* Set the components that need to be added. For example, conversation list. */}
       </Container>
     );
   }
   ```
   
1. Set the theme.

   Set the theme through the `Container` properties provided by the component. The sample code is as follows:

   ```typescript
   function App(): React.JSX.Element {
     const palette = usePresetPalette();
     const light = useLightTheme(palette);
     return (
       <Container options={{ appKey: "appKey" }} palette={palette} theme={light}>
         {/* Set the components that need to be added. For example, conversation list. */}
       </Container>
     );
   }
   ```
   
## Set up internationalization

Set internationalization through the `Container` properties provided by the component. The sample code is as follows:

```typescript
function App(): React.JSX.Element {
  return (
    <Container options={{ appKey: "appKey" }} language={"zh-Hans"}>
      {/* Set the components that need to be added. For example, conversation list. */}
    </Container>
  );
}
```

## Use routes

To jump between pages, set up the routing component on the root page. Put the pages you need to jump to in the component.

For example:

```typescript
function App(): React.JSX.Element {
  return (
    <Container options={{ appKey: "appKey" }}>
      <NavigationContainer>
        <Root.Navigator initialRouteName={"Login"}>
          <Root.Screen name={"Login"} component={LoginScreen} />
          {/* Add more page components here */}
        </Root.Navigator>
      </NavigationContainer>
    </Container>
  );
}
```

## Use the Safe Area Component

It is recommended to use the `SafeAreaView` component. This component ensures that the required page is displayed correctly and does not overlap with the status bar or the bottom gesture area.

## Integrate the conversation list component

The `ConversationList` component can manage the conversation list and provide recent chat history. If you want to add search, use the `SearchConversation` component, for example:

```typescript
import type { NativeStackScreenProps } from "@react-navigation/native-stack";

type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ConversationListScreen(props: Props) {
  const { navigation } = props;

  return (
    <SafeAreaView
      style={{
        flex: 1,
      }}
    >
      <ConversationList
        onClickedSearch={() => {
          // Jump to the search page
          navigation.push("SearchConversation", {});
        }}
        onClickedItem={(data) => {
          // Jump to the conversation details page
          if (data === undefined) {
            return;
          }
          const convId = data?.convId;
          const convType = data?.convType;
          const convName = data?.convName;
          navigation.push("ConversationDetail", {
            params: {
              convId,
              convType,
              convName: convName ?? convId,
            },
          });
        }}
      />
    </SafeAreaView>
  );
}
```

## Integrate the contact list component

Contacts can be used in many scenarios. In addition to displaying the contact list, they can be used in scenarios such as creating groups and adding group members.

The sample code is as follows:

```
import type { NativeStackScreenProps } from "@react-navigation/native-stack";

type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ContactListScreen(props: Props) {
  const { navigation } = props;

  return (
    <SafeAreaView
      style={{
        flex: 1,
      }}
    >
      <ContactList
        contactType={"contact-list"}
        onClickedSearch={() => {
          navigation.navigate("SearchContact", {
            params: { searchType: "contact-list" },
          });
        }}
        onClickedItem={(data) => {
          if (data?.userId) {
            navigation.push("ContactInfo", { params: { userId: data.userId } });
          }
        }}
      />
    </SafeAreaView>
  );
}
```

## Integrate the chat page

The `ConversationDetail` component can display, send, and receive messages, as well as loading message history.

For example:

```typescript
import type { NativeStackScreenProps } from "@react-navigation/native-stack";
import type { RootParamsName, RootScreenParamsList } from "../routes";
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ConversationDetailScreen(props: Props) {
  const { route } = props;
  const name = route.name as RootParamsName;

  // Required parameters
  const convId = ((route.params as any)?.params as any)?.convId;
  const convType = ((route.params as any)?.params as any)?.convType;

  // Search mode
  const messageId = ((route.params as any)?.params as any)?.messageId;

  // Is it the multi-selection mode?
  const selectType = ((route.params as any)?.params as any)?.selectType;

  // Topic mode parameters
  const thread = ((route.params as any)?.params as any)?.thread;
  const firstMessage = ((route.params as any)?.params as any)?.firstMessage;

  // Component pattern
  const comType = React.useRef<ConversationDetailModelType>(
    name === "ConversationDetail"
      ? "chat"
      : name === "MessageThreadDetail"
      ? "thread"
      : name === "MessageHistory"
      ? "search"
      : "create_thread"
  ).current;

  return (
    <SafeAreaView
      style={{
        flex: 1,
      }}
    >
      <ConversationDetail
        type={comType}
        convId={convId}
        convType={convType}
        msgId={messageId}
        thread={thread}
        firstMessage={firstMessage}
        selectType={selectType}
      />
    </SafeAreaView>
  );
}
```