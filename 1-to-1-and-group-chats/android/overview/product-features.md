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

The user list displays the logged-in user's contacts, group members, block list, and so on. 

![Contacts](../../assets/images/contacts.png)

### File sharing

File sharing allows users to exchange documents, pictures, videos, and other files through an instant messaging 
application.

![File sharing](../../assets/images/file_sharing.png)

### Unread messages

Unread messages are messages that the logged-in user has received but hasn't yet viewed.

![Unread messages](../../assets/images/unread_messages.png)

### Message delivery receipt 

A delivery receipt informs the sender whether the message has been sent successfully to the server or 
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

### Local search

Users can search contacts (with or without a selection box), conversations, message history, and blacklists, with support for keyword matching. 

![Search](../../assets/images/search.png)

UIKit provides an encapsulated `ChatUIKitSearchActivity` search page. After the user enters keywords, the search data is searched according to `ChatUIKitSearchType` and the search results are displayed.

Jump to the `ChatUIKitSearchActivity` page and enter the required parameters according to the type of search you need. UIKit will match the keywords and display the search results according to `ChatUIKitSearchType`: `USER`, `SELECT_USER`, `CONVERSATION`, `MESSAGE`, and `BLOCK_USER`.

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

UIKit also provides a search base class `ChatUIKitBaseSearchFragment`, which you can inherit and extend for a different implementation. Use the `initAdapter()` method in `ChatUIKitBaseSearchFragment` to implement your own adapters for data processing and display. 

### Group mention

The group mention feature allows users to directly mention specific members in a group chat using the `@` symbol, and the mentioned members will receive a special notification. This feature facilitates efficient delivery of important information and ensures that key messages receive timely attention and response.

Group mentions are enabled by default. To disable them, set `enableMention` to `false`.

The sample code is as follows:

``` kotlin
ChatUIKitClient .getConfig()?.chatConfig?.enableMention ==  false
```

## Conversation-related 

This section covers specific features related to managing conversations. 

![Conversation swipe menu](../../assets/images/conversation_swipe.png)

![Conversation swipe menu](../../assets/images/conversation_swipe_2.png)

### Conversation marked as read

Shows whether the user has read a conversation with unread messages. The user can long-press a conversation to open a context menu and mark the conversation as read.

### Pin a conversation (sticky conversation)

The user can long-press a conversation to open a context menu and pin it to the top for easy access.

### Do not disturb

### Delete a conversation

The user can long-press a conversation to open a context menu and delete the conversation.

## Message-related

This section covers specific features related to managing messages.

### Copy a message

Users can copy a message to the clipboard to save it somewhere else or paste it into other applications.

![Copy message](../../assets/images/copy_message.png)

### Delete a message

Users can delete messages that they do not want to keep.

![Delete message](../../assets/images/delete_message.png)

### Recall a message

Users can recall messages that have been sent by mistake.

![Recall message](../../assets/images/recall_message.png)

### Edit a sent message

Users can edit sent messages to correct mistakes. 

![Edit message](../../assets/images/edit_message.png)

### Quote a message

Users can quote a specific message to reply to it or emphasize its importance. 

![Quote a message](../../assets/images/message_quote.png)

The message quoting UI and logic structure are as follows:

- `ChatUIKitMessageReplyView`: The custom view for the quoted message.
- `ChatUIKitExtendMessageReplyView`: The custom view for the quoted message displayed above the bottom input box. 
  component.
- `ChatUIKitMessageReplyController`: Control the display, hiding, scrolling, and other logic of the quoting feature.

The quoting feature is enabled by default in `ChatUIKitConfig`, that is, the default value of `enableReplyMessage` 
is `true`. To disable this feature, set it to `false`.

The sample code is as follows:

```kotlin
ChatUIKitClient.getConfig()?.chatConfig?.enableReplyMessage
```

### Translate a message

Users can translate messages into other languages for easier communication. The UI and logic structure are as follows:

- The UI layout of message translation is a custom `ChatUIKitMessageTranslationView` layout.
- The logic for adding views to the message bubble and showing and hiding the translation layout is in the 
  `addTranslationViewToMessage` method in `ChatUIKitAddExtendFunctionViewController`.
- The logic for showing and hiding the translation menu that pops up when long-pressing a message bubble is in 
  `ChatUIKitMessageTranslationController`.

1. Enable message translation

  The message translation feature is disabled by default, that is, the default value of 
     `enableTranslationMessage` in 
  `ChatUIKitConfig` is `false`. To enable this feature, set it to `true`. The sample code is as follows:
  
  ```kotlin
  ChatUIKitClient.getConfig()?.chatConfig?.enableTranslationMessage
  ```

1. Set the target language

  The `UIKitChatFragment.Builder` object provides the `setTargetTranslation` method. If the target language is not set, 
  English is used by default. For more translation target languages, refer to [Translation Language Support](https://learn.microsoft.com/en-us/azure/ai-services/translator/language-support).
  
  The sample code is as follows: 

  ```kotlin
  val builder = UIKitChatFragment.Builder
  builder.setTargetTranslation(ChatTranslationLanguageType.English)
  ```

### Reply with emoji

Users can long-press a single message to open the context menu and reply with an emoji. Emoji replies 
(reactions) can help express emotions or attitudes, conduct surveys or votes. 

![Emoji reply](../../assets/images/emoji_reply.png)

The structure of the reaction UI and logic is as follows:

- `ChatUIKitMessageReactionView` implements a custom reaction layout in the message item.
- `ChatUIKitMessageMenuReactionView` implements a custom layout in the message long-press menu `RecyclerView`. 
- The reaction popup window `ChatUIKitReactionsDialog` is inherited from `ChatUIKitBaseSheetFragmentDialog`.
- `ChatUIKitReactionUserListFragment` implements the list of users that add or remove a reaction for a message. 

The logic for adding views to message bubbles and showing and hiding React layouts is in the 
`addReactionViewToMessage` method in `ChatUIKitAddExtendFunctionViewController`.

The reaction feature is disabled by default. That is, the default value of `enableMessageReaction` in `ChatUIKitConfig` is `false`. To enable this feature, set it to `true`. The sample code is as follows:

```kotlin
ChatUIKitClient.getConfig()?.chatConfig?.enableMessageReaction
```

### Message thread

Users can create a message thread based on a message in a group chat, to have a topic-specific discussion.

The thread page is implemented in `ChatUIKitThreadActivity`. Call `ChatUIKitThreadActivity.actionStart` and pass in the required parameters.

The message thread feature is disabled by default. That is, the default value of `enableChatThreadMessage` in 
`ChatUIKitConfig` is `false`. To enable this feature, set it to `true`. The sample code is as follows:

```kotlin
ChatUIKitClient.getConfig()?.chatConfig?.enableChatThreadMessage
```

You can add your own logic by inheriting `ChatUIKitThreadActivity`. For example:

```kotlin
class ChatThreadActivity:ChatUIKitThreadActivity() {
    override fun setChildSettings(builder: UIKitChatFragment.Builder) {
        super.setChildSettings(builder)
    }
}
```

### Forward a message

Users can forward a single or multiple combined messages to other users. 

The UI and logic structure are as follows:

- `Forward ChatUIKitMultipleSelectMenuView`: The bottom menu view.
- `Forward ChatUIKitMessageMultipleSelectController`: Handles the UI layout changes (hiding/showing `ChatUIKitInputMenu` in `ChatUIKitLayout`) and logic for forwarding and deleting.
- `Forward ChatUIKitMessageMultiSelectHelper`: The message selection helper class used to record the selected message information and provide acquisition methods.

The message forwarding feature is enabled by default. That is, the default value of `enableSendCombineMessage` in
`ChatUIKitConfig` is `true`. To disable, set it to `false`. The sample code is as follows:

```kotlin
ChatUIKitClient.getConfig()?.chatConfig?.enableSendCombineMessage
```

### Pin a message

Users can pin important messages to the top of a conversation. This feature is particularly useful for handling urgent matters or ongoing projects, helping to efficiently manage important matters.

The UI and logic structure are as follows:

- `ChatUIKitPinMessageListViewGroup`: A custom View for the message pinning area.
- `ChatUIKitPinMessageController`: Controls the display, hiding, scrolling, and other logic of the pinned message.
- `ChatUIKitPinMessageListAdapter`: The adapter of the list of pinned messages.
- `ChatUIKitPinDefaultViewHolder`: The default display style of pinned messages.
- `ChatUIKitPinTextMessageViewHolder`: The display style of the pinned text message.
- `ChatUIKitPinImageMessageViewHolder`: The display style of the pinned image message.

The message pinning feature is enabled by default. That is, the default value of `enableChatPingMessage` in
`ChatUIKitConfig` is `true`. To disable this feature, set it to `false`. The sample code is as follows:

```kotlin
ChatUIKitClient.getConfig()?.chatConfig?.enableChatPingMessage

// Define the message pinning controller
val chatPinMessageController:ChatUIKitPinMessageController by lazy {
  ChatUIKitPinMessageController(mContext,this@ChatUIKitLayout, conversationId, viewModel)
}
// Initialize the controller.
chatPinMessageController.initPinInfoView()
// Display the list of pinned messages.
// Get the message pinning data from the server
chatPinMessageController.fetchPinnedMessagesFromServer() 
// After successful acquisition, call the setData method to set the data source for the controller: MutableList<ChatMessage>? 
override fun onFetchPinMessageFromServerSuccess(value: MutableList<ChatMessage>?) {
  if (value.isNullOrEmpty()){
    chatPinMessageController.hidePinInfoView()
  }else{
    chatPinMessageController.setData(value)
  }
}
// Pin or unpin messages
// Setting isPinned to true pins the message or false unpins the message.
chatPinMessageController.pinMessage(message,true)

// Add a listener for listening for the message pinning state changes.
private val chatMessageListener = object : ChatUIKitMessageListener() {
  // Message pinning status change event
  override fun onMessagePinChanged(
          messageId: String?,
          conversationId: String?,
          pinOperation: ChatMessagePinOperation?,
          pinInfo: ChatMessagePinInfo?
  ) {
    // Get the local message object based on messageId. If there is no local message object, get it from the server.
    val message = ChatClient.getInstance().chatManager().getMessage(messageId)
    message?.let{
      Update the list of pinned messages
      // pinInfo?.operatorId(): The user ID of the operator that pins or unpins the message. 
      chatPinMessageController.updatePinMessage(it,pinInfo?.operatorId())
    }?:kotlin.run{
      chatPinMessageController.fetchPinnedMessagesFromServer()
    }
  }
}

ChatUIKitClient.addChatMessageListener(chatMessageListener)

// Show pin view
chatPinMessageController.showPinInfoView()
// Hide pin view
chatPinMessageController.hidePinInfoView()
```

### Input status indication

The input status indicator helps users understand whether the other party is replying in real time.

![Message status indication](../../assets/images/message_status_indication.png)

The UI and logic structure of the input status indication are as follows:

- The `subtitle` control in `ChatUIKitTitleBar` displays the user's status and the input status. If received, the input status is displayed first. If you disable the input status indication, only the user's status will be displayed.

- The input-status-related callbacks and methods are as follows:

  - The input status is delivered as a transparent message. After receiving the transparent message, the input status of the other party is monitored through the `setOnPeerTypingListener` method provided in `UIKitChatFragment.Builder`.
  - The input status callback is `onPeerTyping(action: String?)`, where `action` represents the `ChatUIKitLayout.ACTION_TYPING_BEGI| ChatUIKitLayout.ACTION_TYPING_END` state .

The input status indication feature is enabled by default in `ChatUIKitClient.getConfig()?.chatConfig?.enableChatTyping`. That is, the default value of `enableChatTyping` is `true`. To disable, set this parameter to `false`.

This feature can also be set via the `builder.turnOnTypingMonitor(true|false)` API provided in `UIKitChatFragment.Builder`, which has a higher priority.

The sample code is as follows:

```kotlin
ChatUIKitClient.getConfig()?.chatConfig?.enableChatTyping = true
```