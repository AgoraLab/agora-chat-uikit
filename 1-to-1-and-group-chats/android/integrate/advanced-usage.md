# Advanced usage

## Activity jump path setting   // TODO： Activity routing setting?

If the default Activity and the configurable items it provides do not meet your needs, inherit the default Activity and add the required logic. If the Activity is a page called internally by UIKit, you can modify its jump. // TODO： 最后一句：you can modify its route.

For example, if `EaseChatActivity` cannot meet the current needs, inherit `EaseChatActivity` to implement a new `EaseActivity`. When calling `EaseChatActivity.actionStart`, it will intercept `getActivityRoute()` jump direction through `EaseActivity`.

// TODO：第二行的 `EaseActivity` 是否改为 `ChatActivity`
// TODO:第二句 When calling `EaseChatActivity.actionStart` for page redirection, it will intercept the redirection to `EaseActivity` via `getActivityRoute()`.

Only the Activity that implements `EaseIM.getCustomActivityRoute()?.getActivityRoute()` can be intercepted.

```kotlin
// Jump implementation in EaseChatActivity     // TODO: Routing implementation in EaseChatActivity

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
