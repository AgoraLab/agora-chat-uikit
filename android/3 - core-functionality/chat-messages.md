UIKit provides `EaseChatActivity` and `EaseChatFragment` to facilitate quick integration and customization of chat pages. This page describes the following features:

- Send and receive messages, including text, emoticons, pictures, voice, video, files, and business card messages.
- Copy, quote, recall, delete, edit, resend, and review messages.
- Pull roaming messages from the server.
- Clear local messages.

For details about message-related features, see [Product features](./overview/product-features.md).

## Usage examples

The `EaseChatActivity` page mainly requests permissions, such as camera permissions, voice permissions, and others.

```kotlin
// conversationId: Peer user ID for a one-to-one conversation and group ID for a chat group
// chatType: For one-to-one chat and chat group, it is EaseChatType#SINGLE_CHAT and EaseChatType#GROUP_CHAT, respectively.
EaseChatActivity.actionStart(mContext, conversationId, chatType)
```
```kotlin
class ChatActivity: AppCompactActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_chat)
        // conversationId: Peer user ID for a one-to-one conversation and group ID for a chat group
        // chatType: For one-to-one chat and chat group, it is EaseChatType#SINGLE_CHAT and EaseChatType#GROUP_CHAT, respectively.
        EaseChatFragment.Builder(conversationId, chatType)
            .build()?.let { fragment ->
                supportFragmentManager.beginTransaction()
                    .replace(R.id.fl_fragment, fragment).commit()
                }
    }
}
```

## Advanced usage

### Customize settings through EaseChatFragment.Builder

An `EaseChatFragment` Builder construction method is provided to facilitate customization of settings. The currently provided settings are as follows:

```kotlin
// conversationID: Peer user ID for a one-to-one conversation and group ID for a chat group
// easeChatType: SINGLE_CHAT and GROUP_CHAT for one-to-one and chat group, respectively
EaseChatFragment.Builder(conversationID, easeChatType) 
        .useTitleBar(true) 
        .setTitleBarTitle("title") 
        .setTitleBarSubTitle("subtitle") 
        .enableTitleBarPressBack(true) 
        .setTitleBarBackPressListener(onBackPressListener) 
        .getHistoryMessageFromServerOrLocal(false) 
        .setOnChatExtendMenuItemClick Listener(onChatExtendMenuItemClickListener)
        .setOnChatInputChangeListener(onChatInputChangeListener) 
        .setOnMessageItemClickListener(onMessageItemClickListener) 
        .setOnMessageSendCallBack(onMessageSendCallBack) 
        .setOnWillSendMessageListener(willSendMessageListener) 
        .setOnChatRecordTouchListener(onChatRecordTouchListener) 
        .setOnModifyMessageListener(onModifyMessageListener) 
        .setOnReportMessageListener(onReportMessageListener) 
        .setMsgTimeTextColor(msgTimeTextColor) 
        .setMsgTimeTextSize(msgTimeTextSize) 
        .setReceivedMsgBubbleBackground(receivedMsgBubbleBackground) 
        .setSentBubbleBackground(sentBubbleBackground) 
        .showNick name(false)
        .hideReceiverAvatar(false) 
        .hideSenderAvatar(true) 
        .setChatBackground(chatBackground) 
        .setChatInputMenuBackground(inputMenuBackground) 
        .setChatInputMenuHint(inputMenuHint) 
        .sendMessageByOriginalImage(true) 
        .setEmptyLayout(R.layout.layout_chat_empty) 
        .setCustomAdapter(customAdapter) 
        .setCustomFragment(myChatFragment) .build()
```

`EaseChatFragment#Builder` provides the following methods:

| Method | Description |
|:---:|:---:|
| `useTitleBar()` | Set whether to use the default title bar (`EaseTitleBar`). Set to `true` for yes, `false` (default) for no. |
| `setTitleBarTitle()` | Set the title of the title bar. |
| `setTitleBarSubTitle()` | Set the subtitle of the title bar. |
| `enableTitleBarPressBack()` | Set whether to display the back button. Set to `true` for yes, `false` (default) for no. |
| `setTitleBarBackPressListener()` | Set the event listener for clicking the back button in the title bar. |
| `getHistoryMessageFromServerOrLocal()` | Set whether to get messages from the server or locally first. |
| `setOnChatExtendMenuItemClickListener()` | Set the item click event listener for the extended function. |
| `setOnChatInputChangeListener()` | Set the listener for text changes in the menu. |
| `setOnMessageItemClickListener()` | Set the click event listener for the message item, including click and long press events of the bubble area and avatar. |
| `setOnMessageSendCallBack()` | Set the result callback listener for sending messages. |
| `setOnWillSendMessageListener()` | Set the callback for adding extended message attributes before sending the message. |
| `setOnChatRecordTouchListener()` | Set the touch event callback of the recording button. |
| `setOnModifyMessageListener()` | Set the result callback listener for editing a message. |
| `setOnReportMessageListener()` | Set the result callback listener for reporting a message. |
| `setMsgTimeTextColor()` | Set the color of the timeline text. |
| `setMsgTimeTextSize()` | Set the font size of the timeline text. |
| `setReceivedMsgBubbleBackground()` | Set the background of the receiving message bubble area. |
| `setSentBubbleBackground()` | Set the background of the send message bubble area. |
| `showNickname()` | Set whether to display the nickname. Set to `true` for yes, `false` (default) for no. |
| `hideReceiverAvatar()` | Set to hide the recipient's profile picture, displayed by default. |
| `hideSenderAvatar()` | Set to hide the sender's profile picture, displayed by default. |
| `setChatBackground()` | Set the background of the chat list area. |
| `setChatInputMenuBackground()` | Set the background of the menu area. |
| `setChatInputMenuHint()` | Set the prompt text of the input text box in the menu area. |
| `sendMessageByOriginalImage()` | Set whether to send the original image when sending picture messages. Set to `true` for yes, `false` (default) for no. |
| `setEmptyLayout()` | Set a blank page for the chat list. |
| `setCustomAdapter()` | Set a custom adapter, the default is `EaseMessageAdapter`. |
| `setCustomFragment()` | Set a custom chat Fragment. Must be inherited from `EaseChatFragment`. |

### Add a custom message layout

Developers can inherit `EaseMessageAdapter`, `EaseChatRowViewHolder`, and `EaseChatRow` to implement their own `CustomMessageAdapter`, `CustomChatTypeViewViewHolder`, and `CustomTypeChatRow`, and then set `EaseChatFragment#Builder#setCustomAdapter` to `CustomMessageAdapter`.

1. To create a custom `CustomMessageAdapter`, inherit from `EaseMessageAdapter` and override the `getViewHolder` and `getItemNotEmptyViewType` methods.

    ```kotlin
    class CustomMessageAdapter: EaseMessagesAdapter() {
    
    override fun getItemNotEmptyViewType(position: Int): Int {
    // Set your own itemViewType according to the message type
    // If you want to use the default, return super.getItemNotEmptyViewType(position)
    return CUSTOM_YOUR_MESSAGE_TYPE
    }
    
    override fun getViewHolder(parent: ViewGroup, viewType: Int): ViewHolder<EaseMessage> {
    // Return the corresponding ViewHolder according to the returned viewType
    // Return a custom ViewHolder or use the default super.getViewHolder(parent, viewType)
    return CUSTOM_VIEW_HOLDER()
    }
    }
    ```
   
1. Inherit from `EaseChatRow` to create `CustomTypeChatRow`:

    ```kotlin
    class CustomTypeChatRow(
    private val context: Context,
    private val attrs: AttributeSet? = null,
    private val defStyle: Int = 0,
    isSender: Boolean = false
    ): EaseChatRow(context, attrs, defStyle, isSender) {
    
    override fun onInflateView() {
    inflater.inflate(if (!isSender) R.layout.layout_row_received_custom_type
    else R.layout.layout_row_sent_custom_type,
    this)
    }
    
    override fun onSetUpView() {
    (message?.getMessage()?.body as? ChatTextMessageBody)?.let { txtBody ->
    contentView.text = txtBody.message
    }
    }
    }
    ```kotlin