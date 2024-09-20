Single group chat UIKit has built-in light and dark themes, and the default is light theme.

## Switch built-in themes

You can switch to a light or dark theme in the following way:

```javascript
import { UIKitProvider } from 'easemob-chat-uikit';

const App = () => {
  return (
    <UIKitProvider
      theme={{
        mode: 'light', // Light or dark theme
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
        primaryColor: '#00CE76', // 16 binary color value
      }}
    ></UIKitProvider>
  );
};
```

## Set the component shape

By default, components have large rounded corners. You can modify the rounded corners of message bubbles, avatars, and input boxes with `componentsShape`. 

```javascript
import { UIKitProvider } from 'easemob-chat-uikit';

const App = () => {
  return (
    <UIKitProvider
      theme={{
        componentsShape: 'square', // Small rounded corners (square) or large rounded corners (ground)
      }}
    ></UIKitProvider>
  );
};
```

## Set SCSS variables

UIKit uses SCSS internally and defines some global variables. If your project also uses SCSS, you can modify the theme by overwriting these global variables. This is not recommended.

View the defined variables [here](https://github.com/easemob/Easemob-UIKit-web/blob/dev/common/style/themes/default.scss).

The following sections describe how to modify these variables.

### Modify SCSS variables when Create React App projects

In a project created with Create React App, you can create a SCSS file to override the default variables. In the following example, we name the created file `your-theme.scss` and then import the files in the following order.

```scss
@import 'easemob-chat-uikit/style.scss'; // easemob-chat-uikit theme
@import 'your-theme.scss'; // Your theme files
@import 'easemob-chat-uikit/components.scss'; // UIKit component styles
```

### Override SCSS variables by modifying the Webpack config

By configuring the SCSS loader to automatically import custom `style.scss` files, you can override the SCSS variables inside UIKit.

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: [
          'style-loader',
          'css-loader',
          {
            loader: 'sass-loader',
            options: {
              additionalData: `@import "@/styles/index.scss";`,
            },
          },
        ],
      },
    ],
  },
};
```


