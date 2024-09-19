# Theme

UIKit for chatroom has built-in light (default) and dark themes.

- Light

  ![Light theme](../assets/images/light_mode.png)

- Dark

  ![Dark theme](../assets/images/dark_mode_short.png)


## Customize the theme

You can customize the theme by updating the configuration items related to it.

```kotlin
@Composable
fun ChatroomUIKitTheme(
    isDarkTheme: Boolean = isSystemInDarkTheme(),
    colors: UIColors = if (!isDarkTheme) UIColors.defaultColors() else UIColors.defaultDarkColors(),
    shapes: UIShapes = UIShapes.defaultShapes(),
    dimens: UIDimens = UIDimens.defaultDimens(),
    typography: UITypography = UITypography.defaultTypography(),
    content: @Composable () -> Unit
)
```

See also [Design guide](../design-guide.md) and design resources. 