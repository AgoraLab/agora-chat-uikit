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
| `type` | `ConversationDetailModelType` | Required | Component mode. Includes chat mode, search mode, create thread mode, and thread mode. |
| `convId` | string | Required | Conversation ID. |
| `convType` | ChatConversationType | Required | Conversation type. |
| `convName` | string | Optional | Conversation name. |
| `containerStyle` | object | Optional | Modify the component style. |
| `input` | object | Optional | Input component properties, reference properties, and custom components. See [Input components](#input-components) for details. |
| `list` | object | Optional | Message list component properties, reference properties, and custom components. See [Message list component](#message-list-component) for details. |
| `onClickedAvatar` | Function | Optional | Callback when the conversation avatar is clicked. |
| `NavigationBar` | Function | Optional | Customize the navigation bar component. |
| `enableNavigationBar` | boolean | Optional | Whether to activate the navigation bar. If set to `false`, the navigation bar will not be shown. |
| `selectType` | `ConversationSelectModeType` | Optional | Selection mode: regular or multi-selection. |
| `thread` | `ChatMessageThread` | Optional | Thread mode parameters. Thread object. |
| `firstMessage` | `ChatMessageThread` | Optional | Thread mode parameters. Thread message object. |
| `msgId` | string | Optional | Parameters for starting a thread mode or a search mode. The ID of the thread message or the message keyword for the search mode. |
| `parentId` | string | Optional | Create thread mode parameters. The group ID of the thread. |
| `newThreadName` | string | Optional | Create thread mode parameters. The name of the thread. |
| `onCreateThreadResult` | string | Optional | Create thread mode parameters. Create thread result callback notification. |
| `onClickedThread` | Function | Optional | Click on the message bubble to open the callback notification of the thread page. Routing may be used. |
| `onClickedVoice` | Function | Optional | Callback notification when clicking the audio button in the navigation bar. Routing may be used. |
| `onClickedVideo` | Function | Optional | Callback notification when clicking the video button in the navigation bar. Routing may be used. |
| `onThreadDestroyed` | Function | Optional | Callback notification for thread destruction. Routing may be used. |
| `onThreadKicked` | Function | Optional | Callback notification for leaving a thread. Routing may be used. |
| `onForwardMessage` | Function | Optional | Callback notification for forwarded messages. Routing may be used. |

## Input components

The `MessageInput` component mainly sends messages. By default, it supports sending text, emojis, images, audio, video, files, custom messages, and so on. It also supports sending composite messages, such as a quoted messages, modification messages, business card messages, and others.

### Custom input components

The `MessageInput` component provides a custom menu, where you can add custom items and send custom messages.
    
The core properties are as follows:

| Property | Type | Required/Optional | Description |
|:---:|:---:|:---:|:---:|
| `convId` | string | Required | Session ID. |
| `convType` | `ChatConversationType` | Required | Conversation type. |
| `selectType` | `ConversationSelectModeType` | Optional | Selection mode: Regular or multi-selection. |
| `top` | number | Optional | If the `SafeAreaView` component is used, this property needs to be set. |
| `bottom` | number | Optional | If the `SafeAreaView` component is used, this property needs to be set. |
| `numberOfLines` | number | Optional | Enter the maximum number of lines for the text component. The default is 4. |
| `onClickedSend` | Function | Optional | Callback for clicking the send button. |
| `onEditMessageFinished` | Function | Optional | Callback for editing a message. |
| `onClickedCardMenu` | Function | Optional | Callback for clicking a business card message. |
| `onInitMenu` | Function | Optional | Callback for initializing the menu. |
| `emojiList` | string | Optional | Custom emoji list. If not set, the default list is used. |
| `multiSelectCount` | number | Optional | The number of selected messages in the multi-select mode. |
| `onClickedMultiSelectDeleteButton` | Function | Optional | Callback for clicking the delete button. Internal routing. |
| `onClickedMultiSelectShareButton` | Function | Optional | Callback for clicking the forward button. Internal routing. |
| `unreadCount` | number | Optional | Message not read. |
| `onClickedUnreadCount` | Function | Optional | Callback notification for the unread messages count. |


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

- A sent message will be displayed in the message list. The message status will be modified when it is sent to the server, and the status will be modified when the other party has read it.
- After sending a message, you can cancel, edit, and reply within a specified time.
- The received messages can be set to have a read status, and messages with attachments can support downloading of attachments.
- The message menu supports updating or adding custom items.
- Message list items support modification of style, layout, and display/hiding.

### Custom message list component

The core properties are as follows:

| Property | Type | Required/Optional | Description |
|:---:|:---:|:---:|:---:|
| `convId` | string | Required | Conversation ID. |
| `convType` | `ChatConversationType` | Required | Conversation type. |
| `selectType` | `ConversationSelectModeType` | Optional | Selection mode: Regular or multi-select. |
| `containerStyle` | object | Optional | Modify the component style. |
| `thread` | `ChatMessageThread` | Optional | Thread mode parameters. Thread object. |
| `msgId` | string | Optional | Parameters for creating a thread mode or a search mode. The ID of the thread message, or the message keyword for the search mode. |
| `parentId` | string | Optional | Create thread mode parameters. The group ID of the thread. |
| `newThreadName` | string | Optional | Create thread mode parameters. The name of the thread. |
| `firstMessage` | `ChatMessageThread` | Optional | Thread mode parameters. Thread message object. |
| `onClicked` | Function | Optional | Callback for clicking the message list. |
| `onClickedItem` | Function | Optional | Callback for clicking a message. |
| `onLongPressItem` | Function | Optional | Callback for long-pressing a message. |
| `onClickedItemAvatar` | Function | Optional | Callback for clicking a message avatar. |
| `onClickedItemQuote` | Function | Optional | Callback for clicking a message reply. |
| `onQuoteMessageForInput` | Function | Optional | Callback for replying to a message. |
| `onEditMessageForInput` | Function | Optional | Callback for editing a message. |
| `reportMessageCustomList` | array | Optional | Custom list for message reporting. |
| `listItemRenderProps` | `MessageListItemRenders` | Optional | Customization of components of the message list items. Customization of internal components is also supported. |
| `onInitMenu` | Function | Optional | Callback for initializing the menu. |
| `onCopyFinished` | Function | Optional | Callback for when copying is completed. |
| `recvMessageAutoScroll` | boolean | Optional | Whether to automatically scroll the message list when a new message is received. |
| `messageLayoutType` | `MessageLayoutType` | Optional | Whether the message list is aligned to the left or the right. By default, received messages are on the left and sent messages are on the right. |
| `onNoMoreMessage` | Function | Optional | Callback for when there are no more notifications for messages. You may receive multiple notifications, so be careful to avoid duplicates. |
| `onCreateThread` | Function | Optional | Callback for requesting to create a thread. Routing may be used. |
| `onOpenThread` | Function | Optional | Callback for opening a thread. Routing may be used. |
| `onCreateThreadResult` | Function | Optional | Callback for the thread creation result. Routing may be used. |
| `onClickedEditThreadName` | Function | Optional | Callback for editing a thread name. Routing may be used. |
| `onClickedOpenThreadMemberList` | Function | Optional | Callback for checking the list of thread members. Routing may be used. |
| `onClickedLeaveThread` | Function | Optional | Callback for leaving a thread. Routing may be used. |
| `onClickedDestroyThread` | Function | Optional | Callback for destroying a thread. Routing may be used. |
| `onClickedMultiSelected` | Function | Optional | Callback for clicking the multi-select menu. |
| `onChangeMultiItems` | Function | Optional | Callback for multiple selection results. |
| `onClickedSingleSelect` | Function | Optional | Callback for selecting a single message to forward. |
| `onClickedHistoryDetail` | Function | Optional | Callback for clicking on message history. Routing may be used. |
| `onChangeUnreadCount` | Function | Optional | Callback for when the unread value changes. Routing may be used. |

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
| `cancelMultiSelected` | Cancel multi-selection. |
| `removeMultiSelected` | Remove the selected message. |
| `getMultiSelectedMessages` | Get the list of selected messages. |

## Avatar and nickname in the message list

There is no default value in the `MessageList` component for the avatar and nickname that need to be provided by the user. If not provided, the default avatar and user ID will be displayed.

Avatars and nicknames can be provided in the following ways:

- Register callbacks: Use the `onRequestMultiDataproperty` property of the `Container` component.
- Active call: Use the `ChatService.updateDataList` method. Calling this method will trigger internal event distribution. You can also customize the distribution handle and refresh the loaded component page.
- Message-carried: The avatar and nickname carried in the message will be used first.

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