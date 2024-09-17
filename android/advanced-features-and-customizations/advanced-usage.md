## Activity jump path setting

If the default Activity and the configurable items it provides do not meet your needs, inherit the default Activity and add the required logic. If the Activity is a page called internally by UIKit, you can modify the jump of the Activity.

For example, if `AgoraChatActivity` cannot meet the current needs, inherit `AgoraChatActivity` to implement a new `ChatActivity`. When calling `AgoraChatActivity.actionStart`, it will intercept `getActivityRoute()` jump direction through `ChatActivity`.

Only the Activity that implements `ChatIM.getCustomActivityRoute()?.getActivityRoute()` can be intercepted.

```kotlin
// Jump implementation in AgoraChatActivity

companion object {
    private const val REQUEST_CODE_STORAGE_PICTURE = 111
    private const val REQUEST_CODE_STORAGE_VIDEO = 112
    private const val REQUEST_CODE_STORAGE_FILE = 113

    fun actionStart(context: Context, conversationId: String, chatType: AgoraChatType) {
        Intent(context, AgoraChatActivity::class.java).apply {
             putExtra(ChatConstant.EXTRA_CONVERSATION_ID, conversationId)
             putExtra(ChatConstant.EXTRA_CHAT_TYPE, chatType.ordinal)
             ChatIM.getCustomActivityRoute()?.getActivityRoute(this.clone() as Intent)?.let {
                    if (it.hasRoute()) {
                    context.startActivity(it)
                    return
                }
            }
            context.startActivity(this)
        }
    }
}


// Implementation of the routing interception 
ChatIM.setCustomActivityRoute(object : ChatCustomActivityRoute {
    override fun getActivityRoute(intent: Intent): Intent {
        if (intent.component?.className == AgoraChatActivity::class.java.name) {
            intent.setClass(this@DemoApplication, ChatActivity::class.java)
         }
        return intent
    }
})
```

## Global configuration

The UIKit provides some global configurations that can be set during initialization. The sample code is as follows:

```kotlin
val avatarConfig = ChatAvatarConfig()
// Set avatar to rounded corners
avatarConfig.avatarShape = ChatImageView.ShapeType.ROUND
val config = ChatIMConfig(avatarConfig = avatarConfig)
ChatIM.init(this, options, config)
```

`ChatAvatarConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `avatarShape` | There are three avatar styles: Default, circular, and rectangular. |
| `avatarRadius` | The avatar corner radius is only valid when the avatar style is set to rectangular. |
| `avatarBorderColor` | The color of the avatar border. |
| `avatarBorderWidth` | The width of the avatar border. |

`ChatChatConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `enableReplyMessage` | Whether the message reply function is available. Enabled by default. |
| `enableModifyMessageAfterSent` | Whether the message editing function is available. Enabled by default. |
| `timePeriodCanRecallMessage` | Set the time within which a message can be recalled. The default is 2 minutes. |
| `avatarBorderWidth` | The width of the avatar border. |

`ChatDateFormatConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `convTodayFormat` | The current date format of the conversation list. The default format is `HH:mm`. |
| `convOtherDayFormat` | The format of other dates in the conversation list. The default format is `MM:dd`. |
| `convOtherYearFormat` | The format of other dates with year in the conversation list. The default format is `MM:dd:yyyy`. |

`ChatSystemMsgConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `useDefaultContactInvitedSystemMsg` | Whether to enable the system message function. Enabled by default.|

`ChatMultiDeviceEventConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
|`useDefaultMultiDeviceContactEvent` |	Whether to enable default the multi-device contact event handling. |
|`useDefaultMultiDeviceGroupEvent`	| Whether to enable the default multi-device group event processing.|
