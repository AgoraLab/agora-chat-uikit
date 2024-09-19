This page introduces the common UIKit features for the one-to-one and group chat.

## General

This section covers general features related to conversations, group chats, one-to-one chats, and contacts. 

### Conversation list

The conversation list presents all ongoing conversations of the logged-in user, helping them quickly find the one 
they need.

![Conversation list](../../assets/images/conversation_list.png)

### Message chat

Message chat allows users to communicate with each other in real time. This is usually carried out in the form of a 
one-to-one or group chat.

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

### Message sent receipt

A sent receipt informs the sender whether the message has been sent successfully to the server or 
recipient.

### Message read receipt

A read receipt informs the sender that the receiver has read the message.

![Sent and read receipt](../../assets/images/sent_receipt.png)

### Contact card

A contact card contains detailed information about a contact, usually including their profile picture and nickname. 
Users can quickly add a contact or start a conversation through the contact card.

![Contact card](../../assets/images/contact_card.png)

### Voice message

Users can send and receive voice messages in addition to text ones. 

![Voice message](../../assets/images/voice_message.png)

### Message reporting

Messages sent by users are examined to determine whether they comply with the platform's 
community guidelines, terms of service, and relevant laws and regulations.

![Message reporting](../../assets/images/message-reporting.png)

### Input status indication

The input status indicator helps users understand whether the other party is replying in real time.

![Message status indication](../../assets/images/message_status_indication.png)

The UI and logic structure of the input status indication are as follows:

- The `subtitle` control in `ChatTitleBar` displays the user's status and input status. After receiving the input status, the input status will be displayed first. After the user cancels the input status, the user's status will be displayed and the input status will disappear.

- Input-status-related callbacks and methods:
  - The input status is delivered as a transparent message. After receiving the transparent message, the input status of the other party is monitored through the `setOnPeerTypingListener` method provided in `AgoraChatFragment.Builder`.
  - The input state callback is `onPeerTyping(action: String?)`, where `action` represents the state `AgoraChatLayout.ACTION_TYPING_BEGI| AgoraChatLayout.ACTION_TYPING_END`.

The input status indication feature is enabled by default in `ChatIM.getConfig()?.chatConfig?.enableChatTyping`. That is, the default value of `enableChatTyping` is `true`. To disable this feature, set this parameter to `false`.

This feature can also be set via the `builder.turnOnTypingMonitor(true|false)` API provided in `AgoraChatFragment.Builder`, which has a higher priority.

The sample code is as follows:

```kotlin
ChatIM.getConfig()?.chatConfig?.enableChatTyping = true
```

### Local search

Users can search contacts (with or without a selection box), conversations, message history, and blacklists, with support for keyword matching. 

![Search](../../assets/images/search.png)

UIKit provides an encapsulated `ChatSearchActivity` search page. After the user enters keywords, the search data is searched according to `ChatSearchType` and the search results are displayed.

Jump to the `ChatSearchActivity` page and enter the required parameters according to the type of search you need. UIKit will match the keywords and display the search results according to `ChatSearchType`: `USER`, `SELECT_USER`, `CONVERSATION`, `MESSAGE`, and `BLOCK_USER`.

For example, the sample code for searching the blacklist is as follows:

```kotlin
    
    private val returnSearchClickResult: ActivityResultLauncher<Intent> = registerForActivityResult(
        ActivityResultContracts.StartActivityForResult()
    ) { result -> onClickResult(result) }

    returnSearchClickResult.launch(
        ChatSearchActivity.createIntent(
            context = mContext,
            searchType = ChatSearchType.BLOCK_USER
        )
    )
    private fun onClickResult(result: ActivityResult) {
        if (result.resultCode == Activity.RESULT_OK) {
            result.data?.getSerializableExtra("user")?.let {
                if (it is ChatUser) {
                    // Search result
                }
            }
        }
    }

```


UIKit also provides a search base class `ChatBaseSearchFragment`, which you can inherit and extend for a different implementation. Use the `initAdapter()implement` method in `ChatBaseSearchFragment` to implement your own adapters for data processing and display.

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

- `AgoraChatMessageReplyView`: A custom View for the quoted message of the message bubble.
- `AgoraChatExtendMessageReplyView`: A custom View for the reference message displayed above the bottom input box 
  component.
- `AgoraChatMessageReplyController`: Controls the display, hiding, scrolling, and other logic of reference functions.

The quoting feature is enabled by default in `AgoraChatConfig`, that is, the default value of `enableReplyMessage` 
is `true`. To disable this feature, set it to `false`.

The sample code is as follows:

```kotlin
ChatIM.getConfig()?.chatConfig?.enableReplyMessage
```

### Translate a message

Users can translate messages into other languages for easier communication. The UI and logic structure are as follows:

- The UI layout of message translation is a custom `AgoraChatMessageTranslationView` layout.
- The logic for adding views to the message bubble and showing and hiding the translation layout is in the 
  `addTranslationViewToMessage` method in `AgoraChatAddExtendFunctionViewController`.
- The logic for showing and hiding the translation menu that pops up when long-pressing a message bubble is in 
  `AgoraChatMessageTranslationController`.

1. Enable message translation

  The message translation feature is disabled by default, that is, the default value of 
     `enableTranslationMessage` in 
  `AgoraChatConfig` is `false`. To enable this feature, set is to `true`. The sample code is as follows:
  
  ```kotlin
  ChatIM.getConfig()?.chatConfig?.enableTranslationMessage
  ```

1. Set the target language

  The `AgoraChatFragment.Builder` object provides the `setTargetTranslation` method. If the target language is not set, 
  English is used by default. For more translation target languages, refer to [Translation Language Support](https://learn.microsoft.com/zh-cn/azure/ai-services/translator/language-support).
  
  The sample code is as follows: 

  ```kotlin
  val builder = AgoraChatFragment.Builder
  builder.setTargetTranslation(ChatTranslationLanguageType.English)
  ```

### Reply with emoji

Users can long-press a single message to open the context menu and reply with an emoji. Emoji replies 
(reactions) can help express emotions or attitudes, conduct surveys or votes. 

![Emoji reply](../../assets/images/emoji_reply.png)

The structure of the reaction UI and logic is as follows:

- Reaction implements a custom layout `AgoraChatMessageReactionView` in the message list. 
- Reaction implements a custom layout `ChatMessageMenuReactionView` in the message long-press menu `RecyclerView`.
- The reaction popup window `AgoraChatReactionsDialog` is inherited from `ChatBaseSheetFragmentDialog`.
- Reaction member list is `ChatReactionUserListFragment`.

The logic for adding views to message bubbles and showing and hiding React layouts is in the 
`addReactionViewToMessage` method in `AgoraChatAddExtendFunctionViewController`.

The emoji reply feature is disabled by default. That is, the default value of `enableMessageReaction` in `AgoraChatConfig` is `false`. To enable this feature, set it to `true`. The sample code is as follows:

```kotlin
ChatIM.getConfig()?.chatConfig?.enableMessageReaction
```

### Message thread

Users can create a message thread based on a message in a group chat, to have a topic-specific discussion.

The thread page is implemented in `AgoraChatThreadActivity`. Call `AgoraChatThreadActivity.actionStart` and pass in the required parameters.

The message thread feature is disabled by default. That is, the default value of `enableChatThreadMessage` in 
`AgoraChatConfig` is `false`. To enable this feature, set it to `true`. The sample code is as follows:

```kotlin
ChatIM.getConfig()?.chatConfig?.enableChatThreadMessage
```

You can add your own logic by inheriting `AgoraChatThreadActivity`. For example:

```kotlin

class ChatThreadActivity:AgoraChatThreadActivity() {
    override fun setChildSettings(builder: AgoraChatFragment.Builder) {
        super.setChildSettings(builder)
    }
}
```

### Forward a message

Users can forward a single or multiple combined messages to other users. 

The UI and logic structure are as follows:

- `Forward AgoraChatMultipleSelectMenuView`: Bottom menu view.
- `Forward AgoraChatMessageMultipleSelectController`: Handles UI layout changes (hiding/showing `AgoraChatInputMenu` in `AgoraChatLayout`) and logic for forwarding and deleting.
- `Forward AgoraChatMessageMultiSelectHelper`: The message selection helper class used to record the selected message information and provide acquisition methods.

The message forwarding feature is enabled by default. That is, the default value of `enableSendCombineMessage` in
`AgoraChatConfig` is `true`. To disable this feature, set it to `false`. The sample code is as follows:

```kotlin
ChatIM.getConfig()?.chatConfig?.enableSendCombineMessage
```

### Pin a message

Users can pin important messages to the top of a conversation. This feature is particularly useful for handling urgent matters or ongoing projects, helping to efficiently manage important matters.

The UI and logic structure are as follows:

- `AgoraChatPinMessageListViewGroup`: A custom View for the message pinning area.
- `AgoraChatPinMessageController`: Controls the display, hiding, scrolling, and other logic of the pinned message.
- `AgoraChatPinMessageListAdapter`: The message pinned list adapter.
- `AgoraChatPinDefaultViewHolder`: The default display style of pinned messages.
- `AgoraChatPinTextMessageViewHolder`: The text type display style of the pinned message.
- `AgoraChatPinImageMessageViewHolder`: The display style of the pinned message image type.

The message pinning feature is enabled by default. That is, the default value of `enableChatPingMessage` in
`AgoraChatConfig` is `true`. To disable this feature, set it to `false`. The sample code is as follows:

```kotlin
ChatIM.getConfig()?.chatConfig?.enableChatPingMessage

// Define the controller of the pin message
val chatPinMessageController:AgoraChatPinMessageController by lazy {
  AgoraChatPinMessageController(mContext,this@AgoraChatLayout, conversationId, viewModel)
}
// Initialize the controller that contains pin list entries and built-in click event listening callbacks
// When the original message exists, the list scrolls to the original message position by default
chatPinMessageController.initPinInfoView()
// Display pin message list
// Get the pin message data from the server
chatPinMessageController.fetchPinnedMessagesFromServer()
// After successful acquisition, call the setData method to set the data source for the controller value: MutableList<ChatMessage>?
override fun onFetchPinMessageFromServerSuccess(value: MutableList<ChatMessage>?) {
  if (value.isNullOrEmpty()){
    chatPinMessageController.hidePinInfoView()
  }else{
    chatPinMessageController.setData(value)
  }
}
// Actively operate pin messages
// Setting isPinned to true pins the message to the top, setting to false cancels the pin
chatPinMessageController.pinMessage(message,true)

// Update the pinned message
// Add the message listening callback
private val chatMessageListener = object : ChatMessageListener() {
  // Pinned message change callback
  override fun onMessagePinChanged(
          messageId: String?,
          conversationId: String?,
          pinOperation: ChatMessagePinOperation?,
          pinInfo: ChatMessagePinInfo?
  ) {
    // Get the local message object based on messageId. If there is no local message object, get it from the server
    val message = ChatClient.getInstance().chatManager().getMessage(messageId)
    message?.let{
      // Update the pinned message list 
      // pinInfo?.operatorId() manages the ID of the pinned message
      chatPinMessageController.updatePinMessage(it,pinInfo?.operatorId())
    }?:kotlin.run{
      chatPinMessageController.fetchPinnedMessagesFromServer()
    }
  }
}

ChatIM.addChatMessageListener(chatMessageListener)

// Show pin view
chatPinMessageController.showPinInfoView()
// Hide pin view
chatPinMessageController.hidePinInfoView()
```