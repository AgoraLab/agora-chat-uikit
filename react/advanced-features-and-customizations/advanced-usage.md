The UI component library `Chat UIKit SDK` provides themes, internationalization, common UI components, and other elements. In addition to providing default usage, the component library supports custom component styles and behaviors.

## Entry component Container

`Container` provides global configuration and initialization. If this component is not used, other UI components may not work properly. Its custom parameters are as follows:

| Property | Type | Required/Optional | Description |
|:---:|:---:|:---:|:---:|
| `options` | `ChatOptionsType` | Required | A composite parameter set, consistent with `ChatOptions`, with `appKey` as the only required parameter. |
| `language` | `LanguageCode` | Optional | Language code. Specifies the language in which the UI component displays content. For example, English. |
| `translateLanguage` | `LanguageCode` | Optional | Language code. Specifies the target language for message translation. For example, English. |
| `palette` | palette | Optional | Theme color palette. The theme will select colors and styles from the palette to form the theme. |
| `theme` | theme | Optional | Theme. Light and dark are provided by default. |
| `fontFamily` | string | Optional | Customize the font style of UI components. |
| `emojiFontFamily` | string | Optional | Customize the emoji font style. |
| `headerFontFamily` | string | Optional | Customize the font style of the navigation bar. |
| `releaseArea` | `ReleaseArea` | Optional | Set the release region. For example, Global. |
| `formatTime` | object | Optional | Time formatting callback notification for the conversation list and chat page. If not provided, the default formatting is used. |
| `recallTimeout` | number | Optional | Callback timeout. The unit is milliseconds. The default value is 120. |
| `group` | object | Optional | The maximum number of members to select when creating a group. Default is 1000. |
| `conversationDetail` | `ConversationDetailType` | Optional | A collection of chat page configurations. See the corresponding definition for details. |
| `avatar` | object | Optional | Global avatar style settings. Supports border rounding settings. |
| `input` | object | Optional | Global input component style settings. Supports border radius settings. |
| `alert` | object | Optional | Global warning box component style settings. Supports border radius settings. |
| `onInitLanguageSet` | function | Optional | Register callback notification for the language pack. You can customize the language pack. |
| `onInitialized` | function | Optional | Register the callback notification when initialization is completed. |
| `onUsersHandler` | function | Optional | Register the callback notification for obtaining user data. If the user provides this interface, the user's avatar and nickname are obtained through this interface. If not provided, the default is used. |
| `onGroupsHandler` | function | Optional | Register the callback notification for obtaining group data. If the user provides this interface, the group's avatar and nickname are obtained through this interface. If not provided, the default is used. |
| `onChangeStatus` | function | Optional | Register the presence status callback notification. If the user provides this interface, the user's status change notification will be received. |
| `enableTranslate` | boolean | Optional | Whether to enable the translation feature. If enabled, it also needs to be turned on in the background. |
| `enableThread` | boolean | Optional | Whether to enable the thread feature. If enabled, it also needs to be enabled in the background. |
| `enableReaction` | boolean | Optional | Whether to enable the emoji reply feature. If enabled, it also needs to be turned on in the background. |
| `enablePresence` | boolean | Optional | Whether to enable the status subscription feature. If enabled, it also needs to be enabled in the background. |
| `enableAVMeeting` | boolean | Optional | Whether to enable the audio and video call feature. If enabled, it also needs to be enabled in the background. Enabled by default. |

## Chat SDK component ChatService

`ChatServiceComponents` are encapsulations of Chat SDK, which can simplify calling logic, return standardized results, trigger UI change events, and so on.

Commonly used interfaces include login, logout, adding and deleting event listeners, adding and deleting Chat SDK event listeners, and others.

The core common methods are as follows:

| Method | Type | Description |  
|:---:|:---:|:---:|
| `login` | Function | Log in. |  
| `logout` | Function | Log out. |  
| `autoLogin` | Function | Automatically log in. |  
| `loginState` | Function | Current login status. |  
| `userId` | get | Get the ID of the currently logged in user. |  
| `updateDataList` | Function | Actively update the avatar and nickname of the specified data. Will trigger the refresh of the loaded UI components. |  

For more methods, see the [corresponding definitions](https://github.com/easemob/react-native-chat-library).

### Listeners

`ChatService` provides `EventServiceListener` and `ConnectServiceListener`.

- `ConnectServiceListener`: Receive notifications of changes in the connection status between the SDK and the server.

   | Method | Description |
   |:---:|:---:|
   | `onConnected` | Connected. |
   | `onDisconnected` | Disconnected. See `DisconnectReasonType`. |

- `EventServiceListener`: Receive notifications of event changes called by the specified interface.

   | Method | Description |
   |:---:|:---:|
   | `onBefore` | Notification before the interface is called. |
   | `onFinished` | Notification of the interface call completion. |
   | `onError` | Notification of an error in an interface call. |

## Custom hooks

This section introduces common hooks.

### useColors

Usually `usePaletteContext` is used with custom color objects to change component colors.

For example:

```typescript
export function SomeView() {
  const { colors } = usePaletteContext();
  const { getColor } = useColors({
    bg: {
      light: colors.neutral[98],
      dark: colors.neutral[1],
    },
  });
  return (
    <View
      style={{
        backgroundColor: getColor("bg"),
      }}
    />
  );
}
```

### useDelayExecTask

Delays the execution of a task. If called again before the timeout occurs, the delay continues until timeout while executing the task.

For example:

```typescript
export function SomeView() {
  const [value, setValue] = React.useState("");
  const { delayExecTask: deferSearch } = useDelayExecTask(
    500,
    React.useCallback((keyword: string) => {
      // Perform a search
    }, [])
  );
  return (
    <Search
      onCancel={onCancel}
      onChangeText={(v) => {
        setValue(v);
        deferSearch?.(v);
      }}
      value={value}
    />
  );
}
```

### useForceUpdate

`useForceUpdate` provides forced updates. This hook can be used if the component has no state or needs to be updated manually.

For example:

```typescript
export function SomeView() {
  const count = React.useRef(0);
  const { updater } = useForceUpdate();
  return (
    <View style={{ paddingTop: 100, flexGrow: 1 }}>
      <Pressable
        onPress={() => {
          count.current += 1;
          updater();
        }}
      >
        <Text>{"test updater"}</Text>
      </Pressable>
      <Text>{`${count.current}`}</Text>
    </View>
  );
}
```

### useGetStyleProps

`useGetStyleProps` provides access to component style properties. Usually used in conjunction with `getStyleSize`.

For example:

```typescript
export function SomeView(props) {
  const { containerStyle } = props;
  const { getStyleSize } = useGetStyleProps();
  const { width: propsWidth } = getStyleSize(containerStyle);
  const { checkType } = useCheckType();
  if (propsWidth) {
    checkType(propsWidth, "number");
  }
  return <View style={{ width: propsWidth }} />;
}
```

### useKeyboardHeight

Gets the user's keyboard height. The keyboard needs to be popped up at least once to get the height.

### usePermissions

Request the permissions required by the UI component library. For example:

```javascript
export function SomeView() {
  const { getPermission } = usePermissions();
  React.useEffect(() => {
    getPermission({
      onResult: (isSuccess: boolean) => {
        // Permission obtained successfully
      },
    });
  }, [getPermission]);
  return <View />;
}
```

## Event notifications

Currently, the UI component library mainly provides two event notification methods:

- SDK event notifications: Forwarded Chat SDK events, focusing on data changes: 

    - `ConnectServiceListener`: Listens for notifications of changes in the connection between the SDK and the server.
    - `MessageServiceListener`: Listens for message-related notifications.
    - `ConversationListener`: Listens for conversation-related notifications.
    - `GroupServiceListener`: Listens for group-related notifications.
    - `ContactServiceListener`: Listens for contact-related notifications.
    - `PresenceServiceListener`: Listens for notifications of user status subscriptions.
    - `CustomServiceListener`: Listens for custom notifications.
    - `MultiDeviceStateListener`: Listens for notifications related to multiple devices.
    - `EventServiceListener`: Listens for event notifications, such as notifications before adding a friend, adding a friend successfully, and failure to add a friend.

- UI event notifications: Application behavior may cause addition, deletion, and modification of list items, list refresh, and list reload. For example, if a group name is modified on the group details page, the page of the conversation list component will also be updated. In many cases, a single interface line may cause changes in multiple UI components. See [UIListener](#listeners) for details.
  
    - `UIConversationListListener`: Listens for notifications of changes to the conversation list.
    - `UIContactListListener`: Listen for notifications of changes to the contact list.
    - `UIGroupListListener`: Listens for notifications of changes to group lists.
    - `UIGroupParticipantListListener`: Listens for notifications of changes to the group member list.
    - `UINewRequestListListener`: Listens for notifications of changes to the friend request list.