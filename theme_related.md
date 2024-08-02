# Theme-related

- Switch to the light or dark theme that comes with `EaseChatUIKit`. Switch the theme before initializing the single group chat `UIKit` view. You can change the default theme by switching the theme. You can also switch the theme while the view is in use. The developer can determine the theme to switch to based on the current system theme.

```swift
Theme.switchTheme(style: .dark)
// or
Theme.switchTheme(style: .light)
```

- Switch to a custom theme.

```swift
/**
How to customize the theme

To customize the theme, you need to refer to the design document for the theme color to define the following five theme color hue values.

All colors in EaseChatUIKit are defined using the HSLA color model, which is a way of representing colors using hue, saturation, brightness, and alpha.

H（Hue)：Hue, the fundamental property of a color, is a number of degrees on the color wheel from 0 to 360. 0 is red, 120 is green, and 240 is blue.

S（Saturation)：Saturation is the intensity and purity of a color. The higher the saturation, the more vivid the color; the lower the saturation, the closer the color is to gray. Saturation is expressed as a percentage value ranging from 0% to 100%. 0% means grayscale, 100% means full color.

L（Lightness)：Lightness is the brightness or darkness of a color. The higher the luminance, the brighter the color; the lower the luminance, the darker the color. Luminance is expressed as a percentage value ranging from 0% to 100%. 0% means black and 100% means white.

A（Alpha)：Alpha is the transparency of the color. A value of 1 means completely opaque, 0 means completely transparent.

By adjusting the value of each component of the HSLA model, you can achieve precise color control.
 */
Appearance.primaryHue = 191/360.0
Appearance.secondaryHue = 210/360.0
Appearance.errorHue = 189/360.0
Appearance.neutralHue = 191/360.0
Appearance.neutralSpecialHue = 199/360.0
Theme.switchTheme(style: .custom)
```