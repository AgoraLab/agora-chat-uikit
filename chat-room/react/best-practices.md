# Best practices

## Initialize UIKit

Initialization is a necessary step and must be completed before calling any interface methods.

```dart
export function App() {
  const palette = usePresetPalette();
  const dark = useDarkTheme(palette);
  const light = useLightTheme(palette);
  const [theme, setTheme] = React.useState(light);
  // const [isReady, setIsReady] = React.useState(false);
  // const fontFamily = 'Twemoji-Mozilla';
  // const [fontsLoaded] = useFonts({
  //   [fontFamily]: require('../assets/twemoji.ttf'),
  // });

  return (
    <Container
      appKey={env.appKey}
      isDevMode={env.isDevMode}
      palette={palette}
      theme={theme}
      roomOption={{
        globalBroadcast: { isVisible: true },
        messageList: {
          isVisibleTag: false,
        },
      }}
      // language={'zh-Hans'}
      languageExtensionFactory={(language) => {
        if (language === "zh-Hans") {
          return createStringSetCn();
        } else {
          return createStringSetEn();
        }
      }}
      // fontFamily={fontFamily}
      onInitialized={() => {
        // todo: Callback notification when ChatroomUIKit completes initialization.
      }}
      avatar={{ borderRadiusStyle: "large", localIcon: a_default_avatar }}
    >
      {/* todo: Adding application code*/}
    </Container>
  );
}
```

## Log in to UIKit

You can log in by using the user object in the project and complying with the `UserInfoProtocol` protocol. The sample code is as follows:

```typescript
export function LoginScreen(props: Props) {
  const {} = props;
  const im = useRoomContext();
  const [isLoading, setIsLoading] = React.useState(false);
  // ...
  const loginAction = React.useCallback(
    (onFinished: (isOk: boolean) => void) => {
      // ...
      im.login({
        userId,
        userToken: params.token!,
        userNickname: nickName,
        userAvatarURL: avatar,
        result: (params) => {
          if (params.isOk) {
            // todo: Login successful
          } else {
            // todo: Login failed
          }
        },
      });
    },
    [im]
  );

  return (
    <View style={{ flex: 1 }}>
      {/* todo: Login page code, trigger loginAction  */}
    </View>
  );
}
```
 
## Initialize the chat room view

1. Get the chat room list and join the specified chat room.

1. Loads the chat room view `Chatroom`, passing in the chat room ID, the user ID of the chat room owner, and some options.

    ```typescript
    export function ChatroomScreen(props: Props) {
      return (
        <View>
          <Chatroom roomId={room.roomId} ownerId={room.owner} {...othersProps} />
        </View>
      );
    }
    ```

## Listen to chat room events and errors

Call the `useRoomListener` method to add listeners to monitor chat room events and errors.

```typescript
useRoomListener(
  React.useMemo(() => {
    return {
      onError: (params) => {
        // todo: Handling error notifications
      },
      onFinished: (params) => {
        // todo: Operation completion notification
      },
    };
  }, [])
);

```

To learn more about the best practices above, click [here](https://github.com/AsteriskZuo/react-native-chat-room-demo). 