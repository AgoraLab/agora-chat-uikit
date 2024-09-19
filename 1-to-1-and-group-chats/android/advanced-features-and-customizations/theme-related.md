UIKit has built-in light (default) and dark themes. 

## Implementation and customization

Android uses its own platform features to create `values-night` resource files in the `res` directory. The Android system will use the corresponding resource package as the system switches between light and dark colors.

UIKit defines a set of basic colors to meet the needs of the UIKit's own theme colors. If you want to customize the colors, create a new `values-night` folder in your project, copy the `chat_colors.xml` file into this folder, and then modify the basic colors in it.

## Switch to a built-in theme

To switch from the current theme to the built-in light or dark theme, use the following method:

```kotlin
// Store a Boolean type variable in sp to record whether the current theme is light or dark
 val isBlack = ChatPreferenceManager.getInstance().getBoolean("isBlack")
// Call the system API to switch the theme
 AppCompactDelegate.setDefaultNightMode(if (isBlack) AppCompactDelegate.MODE_NIGHT_NO else AppCompactDelegate.MODE_NIGHT_YES)
 ChatPreferenceManager.getInstance().putBoolean("isBlack", !isBlack)
```