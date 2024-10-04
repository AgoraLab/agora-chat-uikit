# Chat messages

The `ConversationDetail` component consists of the input component, message list component, menu component, and the navigation bar component. It supports the following features:

- Send and receive messages, including text, emojis, pictures, voice, video, files, and business card messages.
- Copy, quote, recall, delete, edit, resend, and moderate messages.
- Pull roaming messages from the server.
- Clear local messages.
- Message translation.
- Message emoji reply (reaction).
- Message thread.
- Message forwarding.

This component supports multiple modes: The chat mode, the search result display mode, the thread creation mode, and the thread mode. They are defined by the `ConversationDetailProps.type` attribute. See `ConversationDetailModelType` for details.

For details about the message-related features, see [Product features](../overview/product-features.md).

The sample code is as follows:

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

  // Multi-select mode?
  const selectType = ((route.params as any)?.params as any)?.selectType;

  // Thread mode parameters
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

The core properties of the `ConversationDetail` component are as follows:

| Property | Type | Required/Optional | Description |
|:---:|:---:|:---:|:---:|
| `type` | `ConversationDetailModelType` | Required | The component mode. Includes the chat mode, search mode, create thread mode, and thread mode. |
| `convId` | string | Required | The conversation ID. |
| `convType` | ChatConversationType | Required | The conversation type. |
| `convName` | string | Optional | The conversation name. |
| `containerStyle` | object | Optional | Modify the component style. |
| `input` | object | Optional | The input component properties, reference properties, and custom components. See [Input components](#input-components) for details. |
| `list` | object | Optional | The message list component properties, reference properties, and custom components. See [Message list component](#message-list-component) for details. |
| `onClickedAvatar` | Function | Optional | The callback for clicking the conversation avatar. |
| `NavigationBar` | Function | Optional | Customize the navigation bar component. |
| `enableNavigationBar` | boolean | Optional | Whether to activate the navigation bar. If set to `false`, the navigation bar will not be shown. |
| `selectType` | `ConversationSelectModeType` | Optional | The selection mode: Regular or multi-selection. |
| `thread` | `ChatMessageThread` | Optional | The thread mode parameters: The thread object. |
| `firstMessage` | `ChatMessageThread` | Optional | The thread mode parameters: The thread message object. |
| `msgId` | string | Optional | The parameters for starting the thread mode or the search mode: The ID of the thread message or the message keyword for the search mode. |
| `parentId` | string | Optional | The create a thread mode parameter: The group ID of the thread. |
| `newThreadName` | string | Optional | The create a thread mode parameter: The name of the thread. |
| `onCreateThreadResult` | string | Optional | The create a thread mode parameters: The create a thread result callback. |
| `onClickedThread` | Function | Optional | A click on the message bubble to open the callback notification of the thread page. Routing may be used. |
| `onClickedVoice` | Function | Optional | The callback for clicking the audio button in the navigation bar. Routing may be used. |
| `onClickedVideo` | Function | Optional | The callback for clicking the video button in the navigation bar. Routing may be used. |
| `onThreadDestroyed` | Function | Optional | The callback for thread deletion. Routing may be used. |
| `onThreadKicked` | Function | Optional | The callback for leaving a thread. Routing may be used. |
| `onForwardMessage` | Function | Optional | The callback for forwarding a message. Routing may be used. |

## Customize the navigation bar

The navigation bar component is a common component with the left-center-right layout. The customization method is similar to the conversation list. See [Conversation list](conversation-list.md).

## Input components

The `MessageInput` component sends messages. By default, it supports sending text, emojis, images, audio, video, files, custom messages, and so on. It also supports sending composite messages, such as quoted messages, edited messages, business card messages, and others.

### Custom input components

The `MessageInput` component provides a custom menu where you can add custom items and send custom messages.
    
The core properties are as follows:

| Property | Type | Required/Optional | Description |
|:---:|:---:|:---:|:---:|
| `convId` | string | Required | The conversation ID. |
| `convType` | `ChatConversationType` | Required | The conversation type. |
| `selectType` | `ConversationSelectModeType` | Optional | The selection mode: Regular or multi-selection. |
| `top` | number | Optional | If the `SafeAreaView` component is used, set this property. |
| `bottom` | number | Optional | If the `SafeAreaView` component is used, set this property. |
| `numberOfLines` | number | Optional | Enter the maximum number of lines for the text component. The default is `4`. |
| `onClickedSend` | Function | Optional | The callback for clicking the send button. |
| `onEditMessageFinished` | Function | Optional | The callback for editing a message. |
| `onClickedCardMenu` | Function | Optional | The callback for clicking a business card message. |
| `onInitMenu` | Function | Optional | The callback for initializing the menu. |
| `emojiList` | string | Optional | The custom emoji list. If not set, the default list is used. |
| `multiSelectCount` | number | Optional | The number of selected messages in the multi-select mode. |
| `onClickedMultiSelectDeleteButton` | Function | Optional | The callback for clicking the delete button. Internal routing. |
| `onClickedMultiSelectShareButton` | Function | Optional | The callback for clicking the forward button. Internal routing. |
| `unreadCount` | number | Optional | The number of unread messages. |
| `onClickedUnreadCount` | Function | Optional | The callback for the unread message count. |

The core methods of the reference objects are as follows:

| Method | Description |
|:---:|:---:|
| `close` | Close the keyboard input component and switch to the regular mode. |
| `quoteMessage` | Reply to a message. |
| `editMessage` | Edit a message. |
| `showMultiSelect` | Turn on the multi-select mode and show checkboxes in the message list. |
| `hideMultiSelect` | Cancel the multi-select mode and hide checkboxes in the message list. |

## Message list component

`MessageList` displays and manages message lists and supports adding, editing, and deleting message list items.

- A sent message will be displayed in the message list. The message status will be modified when it is sent to the server and then when the other party has read it.
- After sending a message, you can cancel, edit, and reply within a specified time.
- The received messages can be set to have a read status, and messages with attachments can support downloading of attachments.
- The message menu supports updating or adding custom items.
- The message list items support modification of style, layout, and display/hiding.

### Custom message list components

The core properties are as follows:

| Property | Type | Required/Optional | Description |
|:---:|:---:|:---:|:---:|
| `convId` | string | Required | The conversation ID. |
| `convType` | `ChatConversationType` | Required | The conversation type. |
| `selectType` | `ConversationSelectModeType` | Optional | The selection mode: Regular or multi-select. |
| `containerStyle` | object | Optional | Modify the component style. |
| `thread` | `ChatMessageThread` | Optional | The thread mode parameters: Thread object. |
| `msgId` | string | Optional | Parameters for creating a thread mode or a search mode: The ID of the thread message or the message keyword for the search mode. |
| `parentId` | string | Optional | The create a thread mode parameters: The group ID of the thread. |
| `newThreadName` | string | Optional | The create a thread mode parameters: The name of the thread. |
| `firstMessage` | `ChatMessageThread` | Optional | The thread mode parameters: The thread message object. |
| `onClicked` | Function | Optional | The callback for clicking the message list. |
| `onClickedItem` | Function | Optional | The callback for clicking a message. |
| `onLongPressItem` | Function | Optional | The callback for long-pressing a message. |
| `onClickedItemAvatar` | Function | Optional | The callback for clicking a message avatar. |
| `onClickedItemQuote` | Function | Optional | The callback for clicking a message reply. |
| `onQuoteMessageForInput` | Function | Optional | The callback for replying to a message. |
| `onEditMessageForInput` | Function | Optional | The callback for editing a message. |
| `reportMessageCustomList` | array | Optional | The custom list for message reporting. |
| `listItemRenderProps` | `MessageListItemRenders` | Optional | Customization of components of the message list items. Customization of internal components is also supported. |
| `onInitMenu` | Function | Optional | The callback for initializing the menu. |
| `onCopyFinished` | Function | Optional | The callback for when copying is completed. |
| `recvMessageAutoScroll` | boolean | Optional | Whether to automatically scroll the message list when a new message is received. |
| `messageLayoutType` | `MessageLayoutType` | Optional | Whether the message list is aligned to the left or the right. By default, received messages are on the left and sent messages are on the right. |
| `onNoMoreMessage` | Function | Optional | The callback for when there are no more notifications for messages. You may receive multiple notifications, so be careful to avoid duplicates. |
| `onCreateThread` | Function | Optional | The callback for requesting to create a thread. Routing may be used. |
| `onOpenThread` | Function | Optional | The callback for opening a thread. Routing may be used. |
| `onCreateThreadResult` | Function | Optional | The callback for the thread creation result. Routing may be used. |
| `onClickedEditThreadName` | Function | Optional | The callback for editing a thread name. Routing may be used. |
| `onClickedOpenThreadMemberList` | Function | Optional | The callback for checking the list of thread members. Routing may be used. |
| `onClickedLeaveThread` | Function | Optional | The callback for leaving a thread. Routing may be used. |
| `onClickedDestroyThread` | Function | Optional | The callback for deleting a thread. Routing may be used. |
| `onClickedMultiSelected` | Function | Optional | The callback for clicking the multi-select menu. |
| `onChangeMultiItems` | Function | Optional | The callback for multiple selection results. |
| `onClickedSingleSelect` | Function | Optional | The callback for selecting a single message to forward. |
| `onClickedHistoryDetail` | Function | Optional | The callback for clicking on message history. Routing may be used. |
| `onChangeUnreadCount` | Function | Optional | The callback for when the unread value changes. Routing may be used. |

The methods of object reference are as follows:

| Method | Description |
|:---:|:---:|
| `addSendMessage` | Send a message. |
| `addSendMessageToUI` | Send a message to the UI, not to the server. |
| `sendMessageToServer` | Send a message to the server. |
| `removeMessage` | Delete a message. |
| `recallMessage` | Recall a message. |
| `updateMessage` | Update a message. |
| `loadHistoryMessage` | Load message history. |
| `onInputHeightChange` | The height change of the input component. |
| `editMessageFinished` | Editing a message is complete. |
| `scrollToBottom` | Scroll to the bottom of the message. |
| `startShowThreadMoreMenu` | Show the bottom thread menu. |
| `cancelMultiSelected` | Cancel multi-select. |
| `removeMultiSelected` | Remove the selected message. |
| `getMultiSelectedMessages` | Get the list of selected messages. |

## Avatar and nickname in the message list

There is no default value in the `MessageList` component for the avatar and nickname that need to be provided by the user. If not provided, the default avatar and user ID will be displayed.

Avatars and nicknames can be provided in the following ways:

- Register callbacks: Use the `onRequestMultiDataproperty` property of the `Container` component.
- Active call: Use the `ChatService.updateDataList` method. Calling this method will trigger internal event distribution. You can also customize the distribution handle and refresh the loaded component page.
- Message-carried: The avatar and nickname carried in the message will be used first.

### Set the background color of the message list

```typescript
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ConversationDetailScreen(props: Props) {
  const { route } = props;
  const convId = ((route.params as any)?.params as any)?.convId;
  const convType = ((route.params as any)?.params as any)?.convType;

  return (
    <ConversationDetail
      type={'chat'}
      convId={convId}
      convType={convType}
      list={{
        props: {
          containerStyle: { backgroundColor: 'red' },
        },
      }}
    />
  );
}
```

### Set the background image of the message list

```typescript
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ConversationDetailScreen(props: Props) {
  const { route } = props;
  const convId = ((route.params as any)?.params as any)?.convId;
  const convType = ((route.params as any)?.params as any)?.convType;

  return (
    <ConversationDetail
      type={'chat'}
      convId={convId}
      convType={convType}
      list={{
        props: {
          backgroundImage: 'https://img.yzcdn.cn/vant/cat.jpeg',
        },
      }}
    />
  );
}
```

### Custom message timestamp

Set the timestamp below the message bubble during initialization. The sample code is as follows:

```typescript
export function App() {
  const { getOptions } = useApp();

  return (
    <UIKitContainer
      options={getOptions()}
      formatTime={{
        locale: enAU,
        conversationDetailCallback(timestamp, enAU) {
          return format(timestamp, 'yyyy-MM-dd HH:mm:ss', { locale: enAU });
        },
      }}
    >
      {/* sub component */}
    </UIKitContainer>
  );
}
```

### Customize the message list item style

For message list items, you can set the avatar, nickname, bubble layout, style, events, and other elements.

```typescript
export function MyMessageContent(props: MessageContentProps) {
  const { msg, layoutType, isSupport, contentMaxWidth } = props;
  if (msg.body.type === ChatMessageType.TXT) {
    // todo: If it is a text type message, this style will be used for display.
    return (
      <MessageText
        msg={msg}
        layoutType={layoutType}
        isSupport={isSupport}
        maxWidth={contentMaxWidth}
      />
    );
  }
  return <MessageContent {...props} />;
}

type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ConversationDetailScreen(props: Props) {
  const { route } = props;
  const convId = ((route.params as any)?.params as any)?.convId;
  const convType = ((route.params as any)?.params as any)?.convType;

  return (
    <ConversationDetail
      type={'chat'}
      convId={convId}
      convType={convType}
      list={{
        props: {
          listItemRenderProps: {
            MessageContent: MyMessageContent,
          },
        },
      }}
    />
  );
}
```

You can customize the message list items, for example, hide the avatar. For other customized content, refer to the `MessageViewProps` property.

```typescript
export function MyMessageView(props: MessageViewProps) {
  if (props.model.layoutType === 'left') {
    // todo: If it is the message on the left, the avatar will not be displayed
    return <MessageView {...props} avatarIsVisible={false} />;
  }
  return MessageView(props);
}

type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ConversationDetailScreen(props: Props) {
  const { route } = props;
  const convId = ((route.params as any)?.params as any)?.convId;
  const convType = ((route.params as any)?.params as any)?.convType;

  return (
    <ConversationDetail
      type={'chat'}
      convId={convId}
      convType={convType}
      list={{
        props: {
          listItemRenderProps: {
            MessageView: MyMessageView,
          },
        },
      }}
    />
  );
}
```

## Event notification

Event notifications have been implemented in the list, and the list will be updated when the corresponding event is received. 

## Sample application

The core of the implementation is to customize the menu of the input component, click the customized item to send a customized message, and customize the rendering component to display it.

For example:

```javascript
export function MyMessageContent(props: MessageContentProps) {
  const { msg } = props;
  if (msg.body.type === ChatMessageType.CUSTOM) {
    return (
      <View>
        <Text>{(msg.body as ChatCustomMessageBody).params?.test}</Text>
      </View>
    );
  }
  return <MessageContent {...props} />;
}
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ConversationDetailScreen(props: Props) {
  const { navigation, route } = props;
  const convId = ((route.params as any)?.params as any)?.convId;
  const convType = ((route.params as any)?.params as any)?.convType;
  const listRef = React.useRef<MessageListRef>({} as any);
  const inputRef = React.useRef<MessageInputRef>({} as any);
  const { top, bottom } = useSafeAreaInsets();
  const im = useChatContext();

  return (
    <SafeAreaView
      style={{
        flex: 1,
      }}
    >
      <ConversationDetail
        type={"chat"}
        convId={convId}
        convType={convType}
        input={{
          ref: inputRef,
          props: {
            top,
            bottom,
            onInitMenu: (menu) => {
              return [
                ...menu,
                {
                  name: "test",
                  isHigh: false,
                  icon: "bell",
                  onClicked: () => {
                    listRef.current?.addSendMessage({
                      type: "custom",
                      msg: ChatMessage.createCustomMessage(convId, "test", 1, {
                        params: { test: "111" },
                      }),
                    });
                  },
                },
              ];
            },
          },
        }}
        list={{
          ref: listRef,
          props: {
            listItemRenderProps: {
              MessageContent: MyMessageContent,
            },
          },
        }}
      />
    </SafeAreaView>
  );
}
```