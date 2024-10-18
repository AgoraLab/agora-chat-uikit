# Advanced usage

## Activity route setting

If the default Activity and its configurable items do not meet your needs, you can inherit the default Activity and add additional logics. If the Activity is an internally called page in the UIKit, you can modify its route.

For example, if `EaseChatActivity` fails to meet your needs, you can implement `ChatActivity` by inheriting `EaseChatActivity`. In this case, when calling `EaseChatActivity.actionStart` for activity routing, the UIKit, via `getActivityRoute()`, intercepts the default route and redirects the activity to `ChatActivity`.

Only the Activity that implements `EaseIM.getCustomActivityRoute()?.getActivityRoute()` can be intercepted.

```kotlin
// Implement getActivityRoute for the EaseChatActivity page

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


// Implementation of the routing interception for the application
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

UIKit provides some global configurations that can be set during initialization. The sample code is as follows:

```kotlin
val avatarConfig = EaseAvatarConfig()
// Set avatar with rounded corners
avatarConfig.avatarShape = EaseImageView.ShapeType.ROUND
val config = EaseIMConfig(avatarConfig = avatarConfig)
EaseIM.init(this, options, config)
```

`EaseAvatarConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `avatarShape` | Avatar styles: Default, round, and rectangular. |
| `avatarRadius` | The avatar corner radius. Only valid when the avatar style is set to rectangular. |
| `avatarBorderColor` | The color of the avatar border. |
| `avatarBorderWidth` | The width of the avatar border. |

`EaseChatConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `enableReplyMessage` | Whether the message reply feature is available. Enabled by default. |
| `enableModifyMessageAfterSent` | Whether the message editing feature is available. Enabled by default. |
| `timePeriodCanRecallMessage` | Set the time within which a message can be recalled. The default is 2 minutes. |
| `avatarBorderWidth` | The width of the avatar border. |

`EaseDateFormatConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `convTodayFormat` | The current date format of the conversation list. The default format is `HH:mm`. |
| `convOtherDayFormat` | The format for dates other than the current date. The default format is `MM:dd`. |
| `convOtherYearFormat` | The format for years other than the current year. The default format is `MM:dd:yyyy`. |

`EaseSystemMsgConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
| `useDefaultContactInvitedSystemMsg` | Whether to enable the system message feature. Enabled by default.|

`EaseMultiDeviceEventConfig` provides the following configuration items:

| Property | Description |
|:---:|:---:|
|`useDefaultMultiDeviceContactEvent` |	Whether to use the default multi-device contact event handling. |
|`useDefaultMultiDeviceGroupEvent`	| Whether to use the default multi-device group event processing.|
