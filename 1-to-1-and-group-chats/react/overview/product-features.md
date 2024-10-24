# Product features

This page introduces the common UIKit features for the one-to-one and group chat.

## General

This section covers general features related to conversations, group chats, one-to-one chats, and contacts. 

### Conversation list

The conversation list presents all ongoing conversations of the logged-in user, helping them quickly find the one 
they need.

![Conversation list](../../assets/images/conversation_list.png)

### Message chat

Message chat allows users to communicate with each other in real time. This is usually carried out in the form of a 
one-to-one conversation or a group chat.

![Group chat](../../assets/images/group_chat.png)

### Start a conversation

A user initiates communication with one or more users by starting a conversation.

![Create conversation](../../assets/images/create_conversation.png)

### Create a group chat

A group chat is a conversation that allows multiple users to join. Users can invite other users to join the group and 
manage it.

![Create group chat](../../assets/images/create_group_chat.png)

### Manage a group chat

Group chat administrators have all permissions to the group, which includes adding or deleting members, 
modifying the group name, description, and avatar, banning or deleting group members, and others.

![Manage group chat](../../assets/images/manage_group_chat.png)

### User list

The user list displays the logged-in user's contacts, group members, blacklist, and so on.

![Contacts](../../assets/images/contacts.png)

### File sharing

File sharing allows users to exchange documents, pictures, videos, and other files through an instant messaging 
application.

![File sharing](../../assets/images/file_sharing.png)

### Unread messages

Unread messages are messages that the logged-in user has received but hasn't yet viewed.

![Unread messages](../../assets/images/unread_messages.png)

### Sent receipt

A sent receipt informs the sender whether the message has been sent successfully to the server or 
recipient.

![Sent and read receipt](../../assets/images/sent_receipt.png)

### Message read receipt

A read receipt informs the sender that the receiver has read the message.

![Sent and read receipt](../../assets/images/sent_receipt.png)

### Contact card

A contact card contains detailed information about a contact, usually including their avatar and nickname. 
Users can quickly add a contact or start a conversation through the contact card.

![Contact card](../../assets/images/contact_card.png)

### Voice message

Users can send and receive voice messages in addition to text ones. 

![Voice message](../../assets/images/voice_message.png)

### Message reporting

Messages sent by users are examined to determine whether they comply with the platform's 
community guidelines, terms of service, and relevant laws and regulations.

![Message reporting](../../assets/images/message-reporting.png)

### Local message search

The local message search feature allows users to quickly search for messages within a conversation, supporting keyword matching. This helps users efficiently find the information they need, improving work efficiency and the convenience of information management.

![Search](../../assets/images/search.png)

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

![Conversation swipe menu](../../assets/images/conversation_swipe.png)

![Conversation swipe menu](../../assets/images/conversation_swipe_2.png)

### Conversation marked as read

Shows whether the user has read a conversation with unread messages. The user can swipe a conversation left/right or
long-press it to open a context menu and mark the conversation as read.

### Pin a conversation (sticky conversation)

The user can swipe an important conversation left/right or long-press it to open a context menu and pin it to the 
top for easy access.

### Do not disturb

The user can swipe a conversation left/right or long-press it to open a context menu and turn on the DND 
mode. 

### Delete a conversation

The user can swipe a conversation left/right or long-press it to open a context menu and delete the conversation.

## Message-related

This section covers specific features related to managing messages.

### Copy a message

Users can copy a message to the clipboard to save it somewhere else or paste it into other applications.

![Copy message](../../assets/images/copy_message.png)

### Delete a message

Users can delete messages that they do not want to keep.

![Delete message](../../assets/images/delete_message.png)

### Recall a message

Users can recall messages that were sent by mistake. 

![Recall message](../../assets/images/recall_message.png)

### Edit a sent message

Users can edit sent messages to correct mistakes. 

![Edit message](../../assets/images/edit_message.png)

### Quote a message

Users can quote a specific message to reply to it or emphasize its importance. 

![Quote a message](../../assets/images/message_quote.png)

The message quoting UI and logic structure are as follows:

- `MessageQuoteBubble`: A custom View for the quoted message of the message bubble.

The message quoting feature is enabled by default, that is, the default value of `ContainerProps.enableMessageQuote` is `true`. To disable, set this to `false`.

### Translate a message

Users can translate messages into other languages for easier communication. 

The UI layout of message translation is in `MessageText`.

1. Enable message translation.

   The `ContainerProps` object of UIKit provides a `ContainerProps.enableTranslate` setting to enable the message translation feature. The default value is `false`. To enable this feature, you need to set this parameter to `true`.

1. Set the target language.

  The `ContainerProps` object of UIKit provides a `translateLanguage` setting to enable the message translation feature. If the target language for the translation is not set, English is used by default. For more translation target languages, refer to [Translation Language Support](https://learn.microsoft.com/zh-cn/azure/ai-services/translator/language-support).
 
### Reply with emoji

Users can long-press a single message to open the context menu and reply with an emoji. Emoji replies 
(reactions) can help express emotions or attitudes, conduct surveys or votes. 

![Emoji reply](../../assets/images/emoji_reply.png)

The structure of the reaction UI and logic is as follows:

- `MessageReaction`: Implements a custom UI layout in the message list. 
- `BottomSheetEmojiList`: A pop-up window showing the reaction list.

The `ContainerProps` object provides an `enableReaction` property to enable reactions. This feature is disabled by default. To enable, set it to `true`.

### Message thread

Users can create a message thread based on a message in a group chat, to have a topic-specific discussion.

UIKit provides a `ContainerProps.enableThread` switch. The message thread feature is disabled by default. To enable this feature, set it to `true`. 

### Forward messages

Users can forward single or multiple combined messages to other users. 

The UI and logic structure are as follows:

- `MessageForwardSelector`: Select the recipients for forwarding a message.

The message forwarding feature is enabled by default. To disable, set `ContainerProps.enableMessageMultiSelect` to `false`. 

### Pin a message

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

### Input status indication

The input status indicator helps users understand whether the other party is replying in real time.

![Message status indication](../../assets/images/message_status_indication.png)

The input status indication feature is enabled by default. To disable this feature, set `ContainerProps.enableTyping` to `false`.

The sample code is as follows:

```typescript
export function App() {
  // Set whether to enable the input status indication
  const enableTypingRef = React.useRef(false);

  return (
    <UIKitContainer enableTyping={enableTypingRef.current}>
      {/* your custom component */}
      <ToastView />
    </UIKitContainer>
  );
}
```

If you need to customize the style of the typing component or the navigation bar component of the chat page, refer to the `ConversationDetailNavigationBar` component.
