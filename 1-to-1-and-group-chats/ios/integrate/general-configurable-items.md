# General configurable items

The `Appearance.swift` class contains all configurable items. These items have default values. If you want to modify some of them, you need to modify the properties of the related UI control before initializing it for the configurable items to take effect.

Note that `value` in the examples below indicates the configured UI style or data source of the corresponding configurable item. Check the [source code](https://github.com/AgoraIO-Usecase/AgoraChat-UIKit-ios/blob/SwiftUIKit/Sources/EaseChatUIKit/Classes/UI/Core/UIKit/Commons/Appearance.swift) before use.

## Set the bottom pop-up page style

- `Appearance.pageContainerConstraintsSize = value`: The width and height of the bottom pop-up page. Configure it in `PageContainersDialogController.swift`.

## Set the alert style

- `Appearance.alertContainerConstraintsSize = value`: The width and height of the alert pop-up window. Configure it in `AlertController.swift`.
- `Appearance.alertStyle = value`: The rounded corner style of the pop-up window, that is, large rounded corners or small rounded corners.

## Set the page color tone

- `Appearance.primaryHue = value`: The primary hue, used for the background color of buttons, input boxes, and other controls.
- `Appearance.secondaryHue = value`: The secondary hue, used for the background color of buttons, input boxes, and other controls.
- `Appearance.errorHue = value`: The error hue.
- `Appearance.neutralHue = value`: The neutral hue.
- `Appearance.neutralSpecialHue = value`: The neutral special hue.

## Set the avatar

- `Appearance.avatarRadius = value`: The avatar radius divided into four levels: Very small, small, medium, and large.
- `Appearance.avatarPlaceHolder = value`: The avatar placeholder image.

## Set the row height of the ActionSheet cell

`Appearance.actionSheetRowHeight = value`: The row height of the `ActionSheet` cell.

























