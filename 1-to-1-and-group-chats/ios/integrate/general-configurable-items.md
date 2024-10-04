# General configurable items

The `Appearance.swift` class that contains all configurable items. These items have default values. If you want to modify some configuration items, modify the properties before initializing the corresponding UI controls, so that the configuration items take effect.

Note that `value` in the examples below is the value to be set, which will change the UI style or data source of the corresponding configuration item. Check the [source code](https://github.com/easemob/chatuikit-ios) before use.

## Set the bottom pop-up page style

- `Appearance.pageContainerTitleBarItemWidth = value`: The width of the bottom pop-up page title bar. Configure it in `PageContainerTitleBar.swift`.
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

























