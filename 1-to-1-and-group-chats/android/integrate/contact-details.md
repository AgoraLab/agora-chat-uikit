# Contact details

You can configure the title bar, buttons, and other elements of the contact details page. 

## Set the title bar

The title bars of the contact list page, chat page, conversation list page, group details page, and contact details page use `EaseTitleBar`. If the title bar does not meet your requirements, customize it. For details about the title, background color, button image, and avatar, see [Conversation list](conversation-list.md).

## Custom buttons

The button data source can be configured with items and button click events. For example, you can add the audio and video call buttons. By default, `super.getDetailItem()` includes chat and search.

```kotlin
    // Implement MyContactDetailActivity to inherit EaseContactDetailsActivity and override the following methods

    override fun getDetailItem(): MutableList<EaseMenuItem>? {
        val list = super.getDetailItem()
        val audioItem = EaseMenuItem(
            title = getString(R.string.detail_item_audio),
            resourceId = R.drawable.ease_phone_pick,
            menuId = R.id.contact_item_audio_call,
            titleColor = ContextCompat.getColor(this, com.hyphenate.easeui.R.color.ease_color_primary),
            order = 2
        )

        val videoItem = EaseMenuItem(
            title = getString(R.string.detail_item_video),
            resourceId = R.drawable.ease_video_camera,
            menuId = R.id.contact_item_video_call,
            titleColor = ContextCompat.getColor(this, com.hyphenate.easeui.R.color.ease_color_primary),
            order = 3
        )
        list?.add(audioItem)
        list?.add(videoItem)
        return list
    }

    override fun onMenuItemClick(item: EaseMenuItem?, position: Int): Boolean {
        item?.let {
            when(item.menuId){
                R.id.contact_item_audio_call -> {
                    CallKitManager.startSingleAudioCall(user?.userId)
                    return true
                }
                R.id.contact_item_video_call -> {
                    CallKitManager.startSingleVideoCall(user?.userId)
                    return true
                }
                else -> {
                    return super.onMenuItemClick(item, position)
                }
            }
        }
        return false
    }

```


 