# Product features

This page introduces the common UIKit features for the one-to-one and group chat.

## General

This section covers general features related to conversations, group chats, one-to-one chats, and contacts. 

### Conversation list

The conversation list presents all ongoing conversations of the logged-in user, helping them quickly find the one 
they need.

![Conversation list](../../assets/images/conversation_list.png)

### Message chat

The message chat allows users to communicate with each other in real time. This is usually carried out in the form of a 
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

A sent receipt informs the sender whether the message has been sent successfully to the server or the 
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

## Input status indication

The input status indicator helps users understand whether the other party is replying in real time.

The UI and logic structure of the input status indication are as follows:

- The `subtitle` control in `ChatNavigationBar` displays the user's status and the input status. If received, the input status is displayed first. If you disable the input status indication, only the user's status will be displayed.

- Input status-related callbacks and methods are as follows:
  - The input status is delivered as a command message. When current user typing, the input status is sent by the `MessageListViewModel#notifyTypingState()` function.
  - The other party received input state callback is `MessageListViewModel#onOtherPartyTypingText`.

The input status indication feature is enabled by default in `Appearance.chat.enableTyping`, that is, the default value of `Appearance.chat.enableTyping` is `true`. To disable, set this parameter to `false`.

The sample code is as follows:

```swift
    Appearance.chat.enableTyping = false

```

This feature is implemented using the SDK's command message transmission. Monitor the command message callback to process navigation-related effects.

## Local search

Users can search for messages within a conversation, with support for keyword matching.

The UI and logic of the local message search are as follows:

- `SearchHistoryMessagesViewController`: The search page. After the user enters a keyword, messages that include it will be displayed.
- `SearchResultMessagesController`: The search results page.
- `SearchHistoryMessageCell`: The message cell in where the historical message contains the keyword.

For local search, go to the `SearchHistoryMessagesViewController` page, pass in the conversation ID, and type the keyword. Then historical messages that contain the keyword will be displayed.

The local message search feature is enabled by default on the contact details page and group details page. `Appearance.contact.detailExtensionActionItems` contains `ContactListHeaderItem(featureIdentify: "SearchMessages", featureName: "SearchMessages".chat.localize, featureIcon: UIImage(named: "search_history_messages", in: .chatBundle, with: nil))`. If you don't need the search feature, delete this item.

The sample code is as follows:

```swift
    Appearance.contact.detailExtensionActionItems.removeAll { $0.featureIdentify == "SearchMessages" }

```

### Group mentions

Uses can directly mention specific members in a group chat using the @ symbol on the input box, and the mentioned members will receive a special notification. 

The UI and logic of the group mention feature are as follows: Enter the `@` character in `MessageInputBar` of `MessageListView` of  `MessageListController`. This will inform `ViewModel` and `Controller` that the user has entered the `@` character. After selecting the user with the `@` character, the name or nickname of the mentioned user will be displayed in the input box.

The group mention feature is enabled by default. To disable it, ignore the `MessageListController#mentionAction` method. Override this method without handling the mention event.
or
The group mention feature is enabled by default. To disable it, ignore the `MessageListController#onInputBoxEventsOccur` method. That is to say, override this method without handling the mention event.

The sample code is as follows:

```swift
    public func onInputBoxEventsOccur(action type: MessageInputBarActionType, attributeText: NSAttributedString?) {
        switch type {
        case .audio: self.audioDialog()
        case .mention: self.mentionAction()
        case .attachment: self.attachmentDialog()
        default:
            break
        }
  }
```

## Conversation-related 

This section covers specific features related to managing conversations. On the chat list page, users can swipe left or right on a single conversation to perform various actions. The left-swipe menu includes setting the DND mode, pinning a conversation, and deleting it. The right-swipe menu includes marking a conversation as read and calling up more menus.

### Conversation marked as read
Shows whether the user has read a conversation with unread messages. The user can swipe a conversation left/right it to open a context menu and mark the conversation as read. 

### Pin a conversation (sticky conversation)

The user can swipe an important conversation left/right it to open a context menu and pin it to the 

top for easy access.

### Do not disturb

The user can swipe a conversation left/right it to open a context menu and turn on the DND mode. 

### Delete a conversation

The user can swipe a conversation left/right it to open a context menu and delete the conversation.

## Message-related

This section covers specific features related to managing messages. For message quoting, translation, threads, and forwarding, you can turn the feature on or off.

The message cell contains the following display modules:

- Quoted message
- User avatar
- User nickname
- Message time
- Message thread
- Emoji reaction

If a module is not displayed, it can be hidden. The sample code is as follows: 

```swift
// Display module contained in message cell
  @objc public enum MessageContentDisplayStyle: UInt {
  case withReply = 1
  case withAvatar = 2
  case withNickName = 4
  case withDateAndTime = 8
  case withMessageThread = 16
  case withMessageReaction = 32
}
// If it is not displayed, it can be hidden.
Appearance.chat.contentStyle: [MessageContentDisplayStyle] = [.withReply,.withAvatar,.withNickName,.withDateAndTime,.withMessageThread,.withMessageReaction]

    if hiddenTopic {
    Appearance.chat.contentStyle.removeAll { $0 == .withMessageThread }
    }
    if hiddenReaction {
    Appearance.chat.contentStyle.removeAll { $0 == .withMessageReaction }
    }
```

### Copy a message

Users can copy a message to the clipboard to save it somewhere else or paste it into other applications.

![Copy message](../../assets/images/copy_message.png)

### Delete a message

Users can delete messages that they do not want to keep.

![Delete message](../../assets/images/delete_message.png)

### Recall a message

Users can recall messages that were sent by mistake.Recall message limited time `default` is 2minus.It's unit is second.

What if you will modify it to you want.Some sample code as follows:

```
Appearance.chat.recallExpiredTime = UInt(120)
```

![Recall message](../../assets/images/recall_message.png)

### Edit a sent message

Users can edit sent messages to correct mistakes. 

![Edit message](../../assets/images/edit_message.png)

### Quote a message

Users can quote a specific message to reply to it or emphasize its importance. The quoting feature is enabled by default.

![Quote a message](../../assets/images/message_quote.png)

The sample code is as follows:

```swift
Appearance.chat.contentStyle: [MessageContentDisplayStyle] = [.withReply,.withAvatar,.withNickName,.withDateAndTime,.withMessageThread,.withMessageReaction]

    if hiddenTopic {
      Appearance.chat.contentStyle.removeAll { $0 == .withReply }
    }
```

### Translate a message

Users can translate messages into other languages for easier communication.

The premise is that you have enabled the translation function in Agora Chat Console. If you only set `enableTranslation` translation related interfaces, an error will be reported.

The UI and logic structure are in 
`Appearance.swift`:

1. Enable message translation

  The message translation feature is disabled by default, that is, the default value of `Appearance.chat.enableTranslation` in 
  `Appearance.swift` is `false`. To enable this feature, set is to `true`. 

1. Set the target language

  `Appearance.swift` provides the `Appearance.chat.targetLanguage` function to set the target language. If the target 
  language is not set, English is used by default. For more translation target languages, refer to [Translation Language Support](https://learn.microsoft.com/en-us/azure/ai-services/translator/language-support).
  
  The sample code is as follows: 

  ```swift
  Appearance.chat.enableTranslation = true
  Appearance.chat.targetLanguage = .English
  ```

### Reply with emoji

Users can long-press a single message to open the context menu and reply with an emoji. Emoji replies 
(reactions) can help express emotions or attitudes, conduct surveys or votes. Which can be turned on and off in `Appearance.swift`.


![Emoji reply](../../assets/images/emoji_reply.png)

The emoji reply feature is disabled by default in `Appearance.swift`. That is, `.withMessageReaction` is not 
included in `Appearance.chat.contentStyle` by default. Make sure **not to include it repeatedly** when enabling.

```swift
Appearance.chat.contentStyle.append(.withMessageReaction)
```

### Message thread

Users can create a message thread based on a message in a group chat, to have a topic-specific discussion.

The message thread feature is disabled by default in `Appearance.swift`. That is, `.withMessageThread` is not
included in `Appearance.chat.contentStyle` by default. Make sure **not to include it repeatedly** when enabling. 
The sample code is as follows:

```swift
Appearance.chat.contentStyle.append(.withMessageThread)
```

### Forward a message

Users can forward a single message to other users or forward multiple messages by sending a combined messages. 

The UI and logic structure are as follows:

- `MessageMultiSelectedBottomBar.swift`: Bottom menu View.
- `MessageListController.swift`: Handles UI layout changes and logic for forwarding and deleting.
- `MessageListController.swift`: The message selection helper class used to record the selected message information and provide acquisition methods.

### Pin a message

Users can pin important messages to the top of a conversation. This feature is particularly useful for handling urgent matters or ongoing projects, helping to efficiently manage important matters.

The UI and logic structure are as follows:

- `Appearance.chat.enablePinMessage`: Controls the display and hiding of the message pin feature.
- `Appearance.chat.messageLongPressedActions`: Contains all items in the long-press menu of the message. This message pin feature is displayed on the menu by default. If this feature is unnecessary, remove the menu item. If this feature is disabled, the menu item is hidden.
- `MessageListController#showPinnedMessages`: Display the pinned message list.

The message pinning feature is enabled by default in `Appearance.chat`. That is, the default value of 
`enablePinMessage` is set to `true`. To disable this feature, set it to `false`. The sample code is as follows:

```swift
Appearance.chat.enablePinMessage = false
Appearance.chat.messageLongPressedActions.removeAll { $0.tag == "Pin" }
```
