This page introduces the common UIKit features for the one-to-one and group chat.

## General

This section covers general features related to conversations, chat groups, one-to-one chats, and contacts. 

### Conversation list

The conversation list presents all ongoing conversations of the logged-in user, helping them quickly find the one 
they need.

### Message chat

Message chat allows users to communicate with each other in real time. This is usually carried out in the form of a 
one-to-one conversation or a chat group.

### Start one-to-one chat

A user initiates communication with another user by starting a conversation.

### Create chat group

A chat group is a conversation that allows multiple users to join. Users can invite other users to join the group and 
manage it.

### Manage chat group

Chat group administrators have all permissions to the group, which includes adding or deleting members, 
modifying the group name, description, and avatar, banning or kicking out group members, and others.

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

### Content moderation

Messages sent by users are examined to determine whether they comply with the platform's 
community guidelines, terms of service, and relevant laws and regulations.

## Conversation-related 

This section covers specific features related to managing conversations. 

### Conversation read receipt

Conversation read receipt shows whether the user has read a conversation with unread messages. The user can swipe a conversation left/right or long-press it to open a context menu and set the conversation as read. 

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

- `AgoraChatMessageReplyView`: A custom view for the quoted message of the message bubble.
- `AgoraChatExtendMessageReplyView`: A custom view for the reference message displayed above the bottom input box 
  component.
- `AgoraChatMessageReplyController`: Controls the display, hiding, scrolling, and other logic of reference functions.

The quoting feature is enabled by default in `AgoraChatConfig`, that is, the default value of `enableReplyMessage` 
is `true`. To disable this feature, set it to `false`.

The sample code is as follows:

```kotlin
	 ChatIM.getConfig()?.chatConfig?.enableReplyMessage
```

### Translate message

Users can translate messages into other languages for easier communication. The UI and logic structure are as follows:

- The UI layout of message translation is a custom `AgoraChatMessageTranslationView` layout.
- The logic for adding views to the message bubble and showing and hiding the translation layout is in the 
  `addTranslationViewToMessage` method in `AgoraChatAddExtendFunctionViewController`.
- The logic for showing and hiding the translation menu that pops up when long-pressing a message bubble is in 
  `EaseChatMessageTranslationController`.



### Reply with emoji

### Message topic

### Forward message

### Pin message





