# Theme

UIKit has built-in light (default) and dark themes. 

- Light theme

  ![Light theme](../../assets/images/light_theme.png)

- Dark theme

  ![Dark theme](../../assets/images/dark_theme.png)

The sample code for using the default theme is as follows:

```typescript
export function App() {
  const palette = usePresetPalette();
  const dark = useDarkTheme(palette);
  const light = useLightTheme(palette);
  const theme = React.useRef("light").current;

  return (
    <Container
      options={{ appKey: "appKey" }}
      palette={palette}
      theme={theme === "light" ? light : dark}
    >
      {/* Add child components */}
    </Container>
  );
}
```

## Theme customization

The customization is done via `Palette` and `Theme`:

- `Palette`: Set the color palette of the theme.
- `Theme`: Compose light and dark default themes by selecting different colors and styles.

The interface is as follows:

| Property | Description |
|:---:|:---:|
| `style` | The theme type: Light or dark. |
| `button` | The button theme, supports custom styles and sizes. |
| `shadow` | The shadow theme, supports custom styles. |
| `cornerRadius` | The border rounding: Supports custom rounded corners for avatars, warning boxes, input boxes, and message bubbles. |

### Modify theme colors

Get the custom color palette with the `useCreatePalette` method, and then create a `Theme` object with the `useCreateTheme` method.

For example:

```typescript
export function App() {
  const { createPalette } = useCreatePalette({
    colors: {
      primary: 203,
      secondary: 155,
      error: 350,
      neutral: 203,
      neutralSpecial: 220,
    },
  });
  const palette = createPalette();
  const light = useLightTheme(p, "global");

  return (
    <Container options={{ appKey: "appKey" }} palette={palette} theme={light}>
      {/* Add child components */}
    </Container>
  );
}
```

### Modify the theme style

In addition to colors, you can also modify fonts, border radius, and other styles:

```typescript
export function App() {
  // Modify via color palette
  const palette = usePresetPalette();
  // Modify border radius
  palette.cornerRadius.large = 20;
  // Modify title style
  palette.fonts.title.large = {
    fontSize: 20,
    fontWeight: "normal",
    lineHeight: 24,
  };

  // Modified by theme
  const light = useLightTheme(palette);
  // Modify the rounded corner size of the message bubble border
  light.cornerRadius.bubble = ["extraSmall"];

  return (
    <Container
      options={{ appKey: "appKey" }}
      palette={palette}
      theme={theme === "light" ? light : dark}
    >
      {/* Add child components */}
    </Container>
  );
}
```