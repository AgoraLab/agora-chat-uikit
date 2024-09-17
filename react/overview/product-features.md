This page introduces the common UIKit features for the one-to-one and group chat.

## General

This section covers general features related to conversations, group chats, one-to-one chats, and contacts. 

### Conversation list

The conversation list presents all ongoing conversations of the logged-in user, helping them quickly find the one 
they need.

### Message chat

Message chat allows users to communicate with each other in real time. This is usually carried out in the form of a 
one-to-one conversation or a group chat.

### Start a conversation

A user initiates communication with one or more users by starting a conversation.

### Create group chat

A group chat is a conversation that allows multiple users to join. Users can invite other users to join the group and 
manage it.

### Manage group chat

Group chat administrators have all permissions to the group, which includes adding or deleting members, 
modifying the group name, description, and avatar, banning or deleting group members, and others.

### User list

The user list displays the logged-in user's contacts, group members, blacklist, and so on.

### File sharing

File sharing allows users to exchange documents, pictures, videos, and other files through an instant messaging 
application.

### Unread messages

Unread messages are messages that the logged-in user has received but hasn't yet viewed.

### Sent receipt

A sent receipt informs the sender whether the message has been sent successfully to the server or 
recipient.

### Message read receipt

A read receipt informs the sender that the receiver has read the message.

### Contact card

A contact card contains detailed information about a contact, usually including their profile picture and nickname. 
Users can quickly add a contact or start a conversation through the contact card.

### Voice message

Users can send and receive voice messages in addition to text ones. 

### Message reporting

Messages sent by users are examined to determine whether they comply with the platform's 
community guidelines, terms of service, and relevant laws and regulations.

### Input status indication

The input status indicator means real-time display of the input status of one party in a one-to-one chat, which enhances the real-time nature of communication interaction. This feature helps users understand whether the other party is replying.

This feature is implemented using the SDK's transparent message transmission. For details, see [the SDK documentation](https://docs.agora.io/en/agora-chat/client-api/messages/send-receive-messages?platform=web).

The input status indication feature is enabled by default. To disable this feature, set `ContainerProps.enableTyping` to `false`.

The sample code is as follows:

```typescript
export function App() {
  // Set whether to enable the input state
  const enableTypingRef = React.useRef(false);

  return (
    <UIKitContainer enableTyping={enableTypingRef.current}>
      {/* your custom component */}
      <ToastView />
    </UIKitContainer>
  );
}
```

If you need to customize the style of the typing component or the navigation bar component of the chat page, refer to `ConversationDetailNavigationBar`.

### Local message search

The local message search feature allows users to quickly search for messages within a conversation, supporting keyword matching. This helps users efficiently find the information they need, improving work efficiency and the convenience of information management.

On the message search page, enter keywords to search for messages in the current conversation. If there are any results, they will be returned in the form of a list. Click the search result to jump to the location of the message.

The `MessageSearch` component is an independent page and requires the input of necessary parameters `convId`, `convType`, and `onClickedItem`.

The sample code is as follows:

```typescript
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function MessageSearchScreen(props: Props) {
  const { route } = props;
  const navi = useStackScreenRoute(props);
  const convId = ((route.params as any)?.params as any)?.convId;
  const convType = ((route.params as any)?.params as any)?.convType;
  return (
    <SafeAreaViewFragment>
      <MessageSearch
        onCancel={(_data?: MessageSearchModel) => {
          navi.goBack();
        }}
        convId={convId}
        convType={convType}
        onClickedItem={(item) => {
          navi.push({
            to: "MessageHistory",
            props: {
              convId: convId,
              convType: convType,
              messageId: item.msg.msgId,
            },
          });
        }}
      />
    </SafeAreaViewFragment>
  );
}
```

`MessageSearch` provides the basic style and other parameter modifications. You can also implement the message search component yourself.

## Conversation-related 

This section covers specific features related to managing conversations. 

### Conversation marked as read

Shows whether the user has read a conversation with unread messages. The user can swipe a conversation left/right or
long-press it to open a context menu and mark the conversation as read.

### Pin a conversation

The user can swipe an important conversation left/right or long-press it to open a context menu and pin it to the 
top for easy access.

### Do not disturb

The user can swipe a conversation left/right or long-press it to open a context menu and turn on the DND 
mode. 

### Delete conversation

The user can swipe a conversation left/right or long-press it to open a context menu and delete the conversation.

## Message-related

This section covers specific features related to managing messages.

### Copy message

Users can copy a message to the clipboard to save it somewhere else or paste it into other applications.

### Delete message

Users can delete messages that they do not want to keep.

### Recall message

Users can recall messages that were sent by mistake. 

### Edit sent message

Users can edit sent messages to correct mistakes. 

### Quote message

Users can quote a specific message to reply to it or emphasize its importance. The message quoting UI and logic structure are as follows:

- `MessageQuoteBubble`: A custom View for the quoted message of the message bubble.

The message quoting feature is enabled by default, that is, the default value of `ContainerProps.enableMessageQuote` is `true`. To disable, set this to `false`.

### Translate message

Users can translate messages into other languages for easier communication. 

The UI layout of message translation is in `MessageText`.

Before using this feature, enable it in Agora Console.

1. Enable message translation

   The `ContainerProps` object of UIKit provides a `ContainerProps.enableTranslate` setting to enable the message translation feature. The default value is `false`. To enable this feature, you need to set this parameter to `true`.

1. Set the target language

  The `ContainerProps` object of UIKit provides a `translateLanguage` setting to enable the message translation feature. If the target language for the translation is not set, English is used by default. For more translation target languages, refer to [Translation Language Support](https://learn.microsoft.com/zh-cn/azure/ai-services/translator/language-support).
 

### Reply with emoji

Users can long-press a single message to open the context menu and reply with an emoji. Emoji replies 
(reactions) can help express emotions or attitudes, conduct surveys or votes. 

The structure of the reaction UI and logic is as follows:

- `MessageReaction`: Implements a custom UI layout in the message list. 
- `BottomSheetEmojiList`: A pop-up window showing the reaction list.

Before using this feature, enable it in Agora Console.

The `ContainerProps` object provides an `enableReaction` property to enable reactions. This feature is disabled by default. To enable, set it to `true`. The sample code is as follows:

### Message thread

Users can create a message thread based on a message in a group chat, to have a topic-specific discussion.

Before using this feature, enable it in Agora Console.

UIKit provides a `ContainerProps.enableThread` switch. The message thread feature is disabled by default. To enable this feature, set it to `true`. 

### Forward messages

Users can forward single or multiple combined messages to other users. 

The UI and logic structure are as follows:

- `MessageForwardSelector`: Select the forward message recipient page.

The message forwarding feature is enabled by default. To disable, set `ContainerProps.enableMessageMultiSelect` to `false`. 

### Pin message

Users can pin important messages to the top of a conversation. This feature is particularly useful for handling urgent matters or ongoing projects, helping to efficiently manage important matters.

Use `ContainerProps.enableMessagePin` to control whether pinning messages is enabled. This feature is enabled by default. In the `ConversationDetail` component, a built-in message pinning listener `MessageList` is used to dynamically update the pinned message list. In the component, call the context menu to pin messages.

To pin/unpin a message on the chat page, call the context menu on the message bubble and select the pin/unpin option. After successful execution, the pinned message list will be updated.

The following sample code shows how to enable this feature:

```typescript
import { NavigationContainer } from "@react-navigation/native";
import { Container as UIKitContainer } from "react-native-chat-uikit";
export function App() {
  // The default is on. To turn it off, set it to false.
  const enableMessagePin = true;
  return (
    <UIKitContainer
      options={{
        appKey: gAppKey,
      }}
      enableMessagePin={enableMessagePin}
    >
      {/** Other sample codes */}
    </UIKitContainer>
  );
}
```