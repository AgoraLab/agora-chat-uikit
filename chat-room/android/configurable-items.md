# Configurable items

You can modify the following:

- Configurable items in `UiOptions`. For example, set whether to display gifts on the configuration message list with `useGiftsInList`.
    
    ```kotlin
    val chatroomUIKitOptions = ChatroomUIKitOptions(
          uiOptions = UiOptions(
          targetLanguageList = listOf(GlobalConfig.targetLanguage.code),
          useGiftsInList = false,
        )
    )
    ```

- Configurable items in `ViewModel`. For example, set whether to display the time and avatar in `MessageListViewModel`.

    ```kotlin
    class MessageListViewModel(
      private val isDarkTheme: Boolean? = false,
      private val showDateSeparators: Boolean = true,
      private val showLabel: Boolean = true,
      private val showAvatar: Boolean = true,
      private val roomId: String,
      private val chatService: UIChatroomService,
      private val composeChatListController: ComposeChatListController
    )
    ```