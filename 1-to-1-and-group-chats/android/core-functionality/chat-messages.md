UIKit provides `EaseChatActivity` and `EaseChatFragment` to facilitate quick integration and customization of chat pages. This page describes the following features:

- Send and receive messages, including text, emoticons, pictures, voice, video, files, and business card messages.
- Copy, quote, recall, delete, edit, resend, and review messages.
- Pull roaming messages from the server.
- Clear local messages.

For details about message-related features, see [Product features](./overview/product-features.md).

## Usage examples

The `EaseChatActivity` page requests permissions, such as camera permissions, voice permissions, and others.

```kotlin
// conversationId: Peer user ID for a one-to-one conversation and group ID for a group chat
// chatType: For one-to-one chat and group chat, it is EaseChatType#SINGLE_CHAT and EaseChatType#GROUP_CHAT, respectively.
EaseChatActivity.actionStart(mContext, conversationId, chatType)
```
```kotlin
class ChatActivity: AppCompactActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_chat)
        // conversationId: Peer user ID for a one-to-one conversation and group ID for a group chat
        // chatType: For one-to-one chat and group chat, it is EaseChatType#SINGLE_CHAT and EaseChatType#GROUP_CHAT, respectively.
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
// conversationID: Peer user ID for a one-to-one conversation and group ID for a group chat
// easeChatType: SINGLE_CHAT and GROUP_CHAT for one-to-one and group chat, respectively
EaseChatFragment.Builder(conversationID, easeChatType) 
        .useTitleBar(true)
        .setTitleBarTitle("title")
        .setTitleBarSubTitle("subtitle")
        .enableTitleBarPressBack(true)
        .setTitleBarBackPressListener(onBackPressListener)
        .getHistoryMessageFromServerOrLocal(false)
        .setOnChatExtendMenuItemClickListener(onChatExtendMenuItemClickListener)
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
        .showNickname(false)
        .hideReceiverAvatar(false)
        .hideSenderAvatar(true)
        .setChatBackground(chatBackground)
        .setChatInputMenuBackground(inputMenuBackground)
        .setChatInputMenuHint(inputMenuHint)
        .sendMessageByOriginalImage(true)
        .setEmptyLayout(R.layout.layout_chat_empty)
        .setCustomAdapter(customAdapter)
        .setCustomFragment(myChatFragment)
        .build()
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

You can inherit from `EaseMessageAdapter`, `EaseChatRowViewHolder`, and `EaseChatRow` to implement your own `CustomMessageAdapter`, `CustomChatTypeViewViewHolder`, and `CustomTypeChatRow`, and then set `EaseChatFragment#Builder#setCustomAdapter` to `CustomMessageAdapter`.

1. To create a custom `CustomMessageAdapter`, inherit from `EaseMessageAdapter` and override the `getViewHolder` and `getItemNotEmptyViewType` methods:

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
    ```

1. Inherit from `EaseChatRowViewHolder` to create `CustomChatTypeViewViewHolder`:

```kotlin
class CustomChatTypeViewViewHolder(
    itemView: View
): EaseChatRowViewHolder(itemView) {

    override fun onBubbleClick(message: EaseMessage?) {
        super.onBubbleClick(message)
        // Adding a click event
    }
}
```

1. Complete `CustomMessageAdapter`:

    ```kotlin
  class CustomMessageAdapter: EaseMessagesAdapter() {
  
      override fun getItemNotEmptyViewType(position: Int): Int {
          // Set your own itemViewType according to the message type.
          mData?.get(position)?.getMessage()?.let { msg ->
              msg.getStringAttribute("type", null)?.let { type ->
                  if (type == CUSTOM_TYPE) {
                      return if (msg.direct() == ChatMessageDirection.SEND) {
                          VIEW_TYPE_MESSAGE_CUSTOM_VIEW_ME
                      } else {
                          VIEW_TYPE_MESSAGE_CUSTOM_VIEW_OTHER
                      }
                  }
              }
          }
          // If you want to use the default, return super.getItemNotEmptyViewType(position).
          return super.getItemNotEmptyViewType(position)
      }
  
      override fun getViewHolder(parent: ViewGroup, viewType: Int): ViewHolder<EaseMessage> {
          // If you want to use the default, return super.getItemNotEmptyViewType(position).
          if (viewType == VIEW_TYPE_MESSAGE_CUSTOM_VIEW_ME || viewType == VIEW_TYPE_MESSAGE_CUSTOM_VIEW_OTHER) {
              CustomChatTypeViewViewHolder(
                  CustomTypeChatRow(parent.context, isSender = viewType == VIEW_TYPE_MESSAGE_CUSTOM_VIEW_ME)
              )
          }
          // Return a custom ViewHolder or use the default super.getViewHolder(parent, viewType)
          return super.getViewHolder(parent, viewType)
      }
  
      companion object {
          private const val CUSTOM_TYPE = "custom_type"
          private const val VIEW_TYPE_MESSAGE_CUSTOM_VIEW_ME = 1000
          private const val VIEW_TYPE_MESSAGE_CUSTOM_VIEW_OTHER = 1001
      }
  }
    ```
    
1. Add `CustomMessageAdapter` to `EaseChatFragment#Builder`:

    ```kotlin
    builder.setCustomAdapter(CustomMessageAdapter())
    ```
   
## List control-related function settings

```kotlin
val chatMessageListLayout:EaseChatMessageListLayout? = binding?.layoutChat?.chatMessageListLayout
```

The following `EaseChatMessageListLayout` methods are provided:

| Method | Description |
|:---:|:---:|
| `setViewModel()` | UIKit provides a default implementation `EaseMessageListViewModel`, which you can inherit and add your own data logic. |
| `setMessagesAdapter()` | Set the adapter for the message list, which needs to be a subclass of `EaseMessagesAdapter`. |
| `getMessagesAdapter()` | An adapter that returns a list of messages. |
| `addHeaderAdapter()` | Add an adapter for the header layout of the message list. |
| `addFooterAdapter()` | Add an adapter for the footer layout of the message list. |
| `removeAdapter()` | Remove the specified adapter. |
| `addItemDecoration()` | Decorator that adds a list of messages. |
| `removeItemDecoration()` | Remove the message list decorator. |
| `setAvatarDefaultSrc()` | Sets the default avatar for an entry. |
| `setAvatarShapeType()` | Set the style of the avatar, which is divided into three styles: default style, circular style and rectangular style. |
| `showNickname()` | Whether to display the nickname of the entry. `EaseChatFragment#Builder` also provides a setting method for this feature. |
| `setItemSenderBackground()` | Set the background of the sender. `EaseChatFragment#Builder` also provides a setting method for this feature. |
| `setItemReceiverBackground()` | Set the background of the receiver. `EaseChatFragment#Builder` also provides a setting method for this feature. |
| `setItemTextSize()` | Set the font size for text messages. |
| `setItemTextColor()` | Set the font color of text messages. |
| `setTimeTextSize()` | Set the font size of the timeline text. `EaseChatFragment#Builder` also provides a setting method for this feature. |
| `setTimeTextColor()` | Set the color of the timeline text. `EaseChatFragment#Builder` also provides a setting method for this feature. |
| `setTimeBackground()` | Set the background of the timeline. |
| `hideChatReceiveAvatar()` | The recipient's avatar is not displayed. It is displayed by default. `EaseChatFragment#Builder` also provides a setting method for this feature. |
| `hideChatSendAvatar()` | The sender's avatar is not displayed. It is displayed by default. `EaseChatFragment#Builder` also provides a setting method for this feature. |
| `setOnChatErrorListener()` | Set the error callback when sending a message. `EaseChatFragment#Builder` also provides a setting method for this feature. |

## Extended function settings

```kotlin
val chatExtendMenu: IChatExtendMenu? = binding?.layoutChat?.chatInputMenu?.chatExtendMenu
```

After obtaining a `chatExtendMenu` object, you can add, remove, sort, and handle click events of extended functions.

`IChatExtendMenu` provides the following methods: 

| Method | Describe |
|:---:|:---:|
| `clear()` | Clear all extended menu items. |
| `setMenuOrder()` | Sort the specified menu items. |
| `registerMenuItem()` | Add a new menu item. |

![CMessage types](../../assets/images/message_types.png)

## Listen for extension item click events

You can use `EaseChatFragment#Builder#setOnChatExtendMenuItemClickListener` for monitoring or override the `ChatExtendMenuItemClick` method in a custom Fragment.

```kotlin
override fun onChatExtendMenuItemClick(view: View?, itemId: Int): Boolean {
    if(itemId == CUSTOM_YOUR_EXTEND_MENU_ID) {
        // Handle your own click event logic
        // If you want to customize the click event, return `true`
        return true
    }
    return super.onChatExtendMenuItemClick(view, itemId)
}
```

## Long press menu function settings

- Add custom menu items

    ```kotlin
  binding?.let {
      it.layoutChat.addItemMenu(menuId, menuOrder, menuTile)
  }
    ```
  
    `EaseChatLayout` provides the following long-press menu methods: 

    | Method | Description |
    |:---:|:---:|
    | `clearMenu()` | Clear a menu item. |
    | `addItemMenu()` | Add a new menu item. |
    | `findItemVisible()` | Set the visibility of a menu item by specifying `itemId`. |
    | `setOnMenuChangeListener()` | Set the click event listener for the menu item. This listener is already set in `EaseChatFragment`.|

- Handle menu events

    Override the following method in your custom Fragment:

        ```kotlin
        override fun onPreMenu(helper: EaseChatMenuHelper?, message: ChatMessage?) {
          // Callback event before menu is displayed. You can use the helper object to set whether the menu item is displayed.
        }
      
        override fun onMenuItemClick(item: EaseMenuItem?, message: ChatMessage?): Boolean {
          // If you want to intercept a click event, you need to set it to return `true`.
        return false
        }
      
        override fun onDismiss() {
          // You can handle the hiding event of the shortcut menu here.
        }
        ```
  
## Set properties related to the input menu 

- Get an `EaseChatInputMenu` object: 

  ```kotlin
  val chatInputMenu: EaseChatInputMenu? = binding?.layoutChat?.chatInputMenu
  ```
  
  `EaseChatInputMenu` provides the following methods: 

    | method | describe |
    |:---:|:---:|
    | `setCustomPrimaryMenu()` | Set custom menu items, supports View and Fragment. |
    | `setCustomEmojiconMenu()` | Set a custom emoji menu, supports View and Fragment. |
    | `setCustomExtendMenu()` | Set custom extended menu items, supports View, Dialog, and Fragment. |
    | `setCustomTopExtendMenu()` | Set a custom extended top menu layout, supports View and Fragment. |
    | `hideExtendContainer()` | Hide the extended area, including the emoji and extended menu areas. |
    | `hideInputMenu()` | Hide all areas except the top area of the menu. |
    | `showEmojiconMenu()` | Display the emoji area. |
    | `showExtendMenu()` | Display the extended menu. |
    | `showTopExtendMenu()` | Show the top of the extended menu. |
    | `setChatInputMenuListener()` | Set the input menu listener. |
    | `chatPrimaryMenu` | Get the menu item interface. |
    | `chatEmojiMenu` | Get the emoji menu interface. |
    | `chatExtendMenu` | Get the extended menu interface. |
    | `chatTopExtendMenu` | Get the top extended menu interface. |

- Get a menu item object:

  ```kotlin
  val primaryMenu: IChatPrimaryMenu? = binding?.layoutChat?.chatInputMenu?.chatPrimaryMenu
  ```

  `IChatPrimaryMenu` provides the following methods:
    
    | Method | Description|
    |:---:|:---:|
    | `onTextInsert()` | Insert text at the cursor. |
    | `editText` | Get the menu input box object. |
    | `setMenuBackground()` | Set the menu background. |

- Get an emoji menu object:

  ```kotlin
  val emojiconMenu: IChatEmojiconMenu? = binding?.layoutChat?.chatInputMenu?.chatEmojiMenu
  ```
  
  `IChatEmojiconMenu` provides the following methods:

    | Method | Description |
    |:---:|:---:|
    | `addEmojiconGroup()` | Add custom emojis. |
    | `removeEmojiconGroup()` | Remove the specified emoji group. |
    | `setTabBarVisibility()` | Set the `TabBar` visibility. |

    Add a custom emoji in the following way:

    ```kotlin
    binding?.let {
           it.layoutChat.chatInputMenu?.chatEmojiMenu?.addEmojiconGroup(EmojiconExampleGroupData.getData())
       }
    ```
  
