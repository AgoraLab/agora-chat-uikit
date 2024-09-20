Single group chat UIKit has built-in light and dark themes, and the default is light theme.

## Switch built-in themes

You can switch to a light or dark theme in the following way:

```javascript
import { UIKitProvider } from 'easemob-chat-uikit';

const App = () => {
  return (
    <UIKitProvider
      theme={{
        mode: 'light', // 浅色或深色主题
      }}
    ></UIKitProvider>
  );
};
```

## Customize the theme colors

In UIKit, the theme color is used for the default avatar color, message bubble color, button color, etc. The default theme color is shown in the figure below, and you can customize it to other colors.

```javascript
import { UIKitProvider } from 'agora-chat-uikit';

const App = () => {
  return (
    <UIKitProvider
      theme={{
        primaryColor: '#00CE76', // 16 进制颜色值
      }}
    ></UIKitProvider>
  );
};
```

## Set the component shape

By default, the component has large rounded corners. You can modify the rounded corners of message bubbles, avatars, and input boxes with `componentsShape`. 

```javascript
import { UIKitProvider } from 'easemob-chat-uikit';

const App = () => {
  return (
    <UIKitProvider
      theme={{
        componentsShape: 'square', // 小圆角（square）或大圆角（ground）
      }}
    ></UIKitProvider>
  );
};
```

## Set SCSS variables

