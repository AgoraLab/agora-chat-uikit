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

### Create a group chat

A group chat is a conversation that allows multiple users to join. Users can invite other users to join the group and
manage it.

### Manage a group chat

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

This functionality is in the UIKit `Typing` component.

The input status indicator feature is enabled by default. To disable it in the global configuration, you can set it as follows:

```javascript
features.chat.messageInput.typing = false;
```

This feature is implemented using the SDK's transparent message transmission. For details, see [the SDK documentation](https://docs.agora.io/en/agora-chat/client-api/messages/send-receive-messages?platform=web).

### Group mention

The group mention feature allows users to directly mention specific members in a group chat using the `@` symbol, and the mentioned members will receive a special notification. This feature facilitates the efficient delivery of important information and ensures that key messages receive timely attention and response.

This feature is available in the `MessageInput`, `TextMessage`, and `ConversationItem` components.

The group mention feature is enabled by default. Disable it in the global configuration in the following way:

```javascript
features.chat.messageInput.mention = false;
```

## Conversation-related

This section covers specific features related to managing conversations.

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

This section covers specific features related to managing messages, including message deletion, recall, editing, quoting, translation, emoji reply, topic, and forwarding. You can turn these features on or off.

### Copy a message

Users can copy a message to the clipboard to save it somewhere else or paste it into other applications.

### Delete a message

Users can delete messages that they do not want to keep. This feature is included in the message components such as `TextMessage`, `AudioMessage`, `FileMessage`, and so on.

The message deletion feature is enabled by default. You can disable it in the global configuration:

```javascript
features.chat.message.delete = false;
```

### Recall a message

Users can recall messages that were sent by mistake. This feature is included in the message components such as `TextMessage`, `AudioMessage`, `FileMessage`, and so on.

The message recall feature is enabled by default. You can disable it in the global configuration:

```javascript
features.chat.message.recall = false;
```

### Edit a sent message

Users can edit sent messages to correct mistakes. This feature is in the UIKit `TextMessage` component. 

The message editing feature is enabled by default. You can disable it in the global configuration:
                                                  
```javascript
features.chat.message.edit = false;
```

### Quote a message

Users can quote a specific message to reply to it or emphasize its importance. This feature is in the message components in UIKit, such as `TextMessage`, `AudioMessage`, `FileMessage`, and others. 

The quoting feature is enabled by default. You can disable it in the global configuration:

```javascript
features.chat.message.reply = false;
```

### Translate a message

Users can translate messages into other languages for easier communication. This feature is in the UIKit `TextMessage` component.

1. Enable message translation

    The message translation feature is enabled by default. You can disable it in the global configuration: 
    
    ```javascript
   features.chat.message.translate = false;
    ```
   
1. Set the target language

Initialize UIKit configuration `initConfig.translationTargetLanguage` and set it to the target language for translation. If the target language is not set, English is used by default. For more translation target languages, refer to [Translation Language Support](https://learn.microsoft.com/zh-cn/azure/ai-services/translator/language-support).

### Reply with emoji

Users can long-press a single message to open the context menu and reply with an emoji. Emoji replies
(reactions) can help express emotions or attitudes, conduct surveys or votes. 

This feature is in the message components in UIKit, such as `TextMessage`, `AudioMessage`, `FileMessage`, and so on.

The emoji reply feature is enabled by default. You can disable it in the global configuration:

```javascript
features.chat.message.reaction = false;
```

### Message thread

Users can create a message thread based on a message in a group chat, to have a topic-specific discussion.

The thread page is implemented in the UIKit `EaseChatThreadActivity`. You only need to call `EaseChatThreadActivity.actionStart` to start the page and pass in the required parameters. This feature is in the UIKit `TextMessage` component.

The message thread feature is enabled by default. Disable it in the global configuration:

```javascript
features.chat.message.thread = false;
```

Introduce the Thread component from UIKit and display this component when the listener `rootStore.threadStore.showThreadPanel` is set to `true`.

### Forward multiple messages

Users can combine and forward multiple messages to other users.

This feature is in the message components in UIKit, such as `TextMessage`, `AudioMessage`, `FileMessage`, and so on.

Multiple message forwarding is enabled by default. You can disable it in the global configuration: 

```javascript
features.chat.message.select = false;
```

The logic is as follows: Listen for the `onSendMessage` event in the Chat component, determine if it is a combined message, display the contact component, select the target user to forward the message to, and then send the message.

Sample code:

```javascript
// ...
<Chat
  messageInputProps={{
    onSendMessage: (message) => {
      if (message.type == "combine") {
        forwardedMessages = message
        setContactListVisible(true); // Show the contact component
      }
    },
  }}
></Chat>

//...
<ContactList
    onItemClick={(data) => {
        forwardedMessages.to = data.id;
        forwardedMessages.chatType =
        data.type == "contact" ? "singleChat" : "groupChat";
        rootStore.messageStore.sendMessage(forwardedMessages); // Send a message

        // Set up a new conversation
        rootStore.conversationStore.setCurrentCvs({
            chatType: data.type == "contact" ? "singleChat" : "groupChat",
            conversationId: data.id,
            lastMessage: forwardedMessages,
        });

         // Set the selection status of the last message in the current conversartion  
        rootStore.messageStore.setSelectedMessage(currentConversation, {
            selectable: false,
            selectedMessage: [],
        });
    }}
></ContactList>
```

### Forward a single message

Users can forward a single message. This feature is in the message components in UIKit, such as `TextMessage`, `AudioMessage`, `FileMessage`, and so on.

The single message forwarding feature is enabled by default. You can disable it in the global configuration:

```javascript
features.chat.message.forward = false;
```

The logic is as follows: Listen for `onForwardMessage` events in the `Chat` component, display the contact component, select the target user to forward the message to, and then send the message.

Sample code:

```javascript
// ...
<Chat
  messageListProps={{
    messageProps: {
        onForwardMessage: (msg) => {
            forwardedMessages = {...msg}
            forwardedMessages.id = Date.now() + ""; // Set a new message ID
            forwardedMessages.from = rootStore.client.user; // Set to your own user ID
            setContactListVisible(true); // Show the contact component
        }
  }
}}
></Chat>

// The contact component is the same as that of combined forwarding
```

### Pin a message

Users can pin important messages to the top of a conversation. This feature is particularly useful for handling urgent matters or ongoing projects, helping to efficiently manage important matters.

This feature is in UIKit's message components, such as `TextMessage`, `AudioMessage`, `FileMessage`, and so on. The pinned message list feature is in the `PinnedMessage` component.

This feature is enabled by default. You can disable it in the global configuration:

```javascript
features.chat.header.pinMessage = false;
features.chat.message.pin = false;
```