# Theme

UIKit has built-in light (default) and dark themes. 

- Light theme

  ![Light theme](../../assets/images/light_theme.png)

- Dark theme

  ![Dark theme](../../assets/images/dark_theme.png)

## Implementation and customization

Android uses its own platform features to create `values-night` resource files in the `res` directory. The Android system will use the corresponding resource package as the system switches between light and dark colors.

UIKit defines a set of basic colors to meet its theme color requirements. If you want to customize the colors, create a new `values-night` folder in your project, copy the `uikit_colors.xml` file to this folder, and then modify the basic colors in it.

## Switch to a built-in theme

To switch from the current theme to the other built-in light or dark theme, use the following method:

```kotlin
// Store a Boolean type variable to record whether the current theme is light or dark
 val isBlack = ChatUIKitPreferenceManager.getInstance().getBoolean("isBlack")
// Call the system API to switch the theme
 AppCompactDelegate.setDefaultNightMode(if (isBlack) AppCompactDelegate.MODE_NIGHT_NO else AppCompactDelegate.MODE_NIGHT_YES)
 ChatUIKitPreferenceManager.getInstance().putBoolean("isBlack", !isBlack)
```