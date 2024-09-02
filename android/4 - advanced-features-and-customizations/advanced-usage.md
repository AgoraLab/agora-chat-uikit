## Activity jump path setting

If the default Activity and the configurable items it provides do not meet your needs, inherit the default Activity and add the required logic. If the Activity is a page called internally by UIKit, you can modify the jump of the Activity.

For example, if `EaseChatActivity` cannot meet the current needs, inherit `EaseChatActivity` to implement a new `ChatActivity`. When calling `EaseChatActivity.actionStart`, it will intercept `getActivityRoute()` jump direction through `ChatActivity`.

Only the Activity that implements `EaseIM.getCustomActivityRoute()?.getActivityRoute()` can be intercepted.

```kotlin
// Jump implementation in EaseChatActivity

companion object {
    private const val REQUEST_CODE_STORAGE_PICTURE = 111
    private const val REQUEST_CODE_STORAGE_VIDEO = 112
    private const val REQUEST_CODE_STORAGE_FILE = 113

    fun actionStart(context: Context, conversationId: String, chatType: EaseChatType) {
        Intent(context, EaseChatActivity::class.java).apply {
             putExtra(EaseConstant.EXTRA_CONVERSATION_ID, conversationId)
             putExtra(EaseConstant.EXTRA_CHAT_TYPE, chatType.ordinal)
             EaseIM.getCustomActivityRoute()?.getActivityRoute(this.clone() as Intent)?.let {
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
EaseIM.setCustomActivityRoute(object : EaseCustomActivityRoute {
    override fun getActivityRoute(intent: Intent): Intent {
        if (intent.component?.className == EaseChatActivity::class.java.name) {
            intent.setClass(this@DemoApplication, ChatActivity::class.java)
         }
        return intent
    }
})
```

## Global configuration

The UIKit provides some global configurations that can be set during initialization. The sample code is as follows:

```kotlin
val avatarConfig = EaseAvatarConfig()
// Set avatar to rounded corners
avatarConfig.avatarShape = EaseImageView.ShapeType.ROUND
val config = EaseIMConfig(avatarConfig = avatarConfig)
EaseIM.init(this, options, config)
```

`EaseAvatarConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `avatarShape` | There are three avatar styles: Default, circular, and rectangular. |
| `avatarRadius` | The avatar corner radius is only valid when the avatar style is set to rectangular. |
| `avatarBorderColor` | The color of the avatar border. |
| `avatarBorderWidth` | The width of the avatar border. |

`EaseChatConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `enableReplyMessage` | Whether the message reply function is available. Enabled by default. |
| `enableModifyMessageAfterSent` | Whether the message editing function is available. Enabled by default. |
| `timePeriodCanRecallMessage` | Set the time within which a message can be recalled. The default is 2 minutes. |
| `avatarBorderWidth` | The width of the avatar border. |

`EaseDateFormatConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `convTodayFormat` | The current date format of the conversation list. The default format is `HH:mm`. |
| `convOtherDayFormat` | The format of other dates in the conversation list. The default format is `MM:dd`. |
| `convOtherYearFormat` | The format of other dates with year in the conversation list. The default format is `MM:dd:yyyy`. |

`EaseSystemMsgConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `useDefaultContactInvitedSystemMsg` | Whether to enable the system message function. Enabled by default.|

`EaseMultiDeviceEventConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
|`useDefaultMultiDeviceContactEvent` |	Whether to enable default the multi-device contact event handling. |
|`useDefaultMultiDeviceGroupEvent`	| Whether to enable the default multi-device group event processing.|
