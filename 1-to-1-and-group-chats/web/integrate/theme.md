# Theme

UIKit has built-in light (default) and dark themes.

## Switch to a built-in theme

You can switch to a light or dark theme in the following way:

```javascript
import { UIKitProvider } from 'agora-chat-uikit';

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

In UIKit, the theme color is used for the default avatar color, message bubble color, button color, and other elements. You can customize it in the following way:

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

By default, components have large rounded corners. You can modify the rounded corners of the message bubbles, avatars, and input boxes with `componentsShape`:

```javascript
import { UIKitProvider } from 'agora-chat-uikit';

const App = () => {
  return (
    <UIKitProvider
      theme={{
        componentsShape: 'square', // Small rounded corners (square) or large rounded corners (round)
      }}
    ></UIKitProvider>
  );
};
```

## Set SCSS variables

UIKit uses SCSS internally and defines some global variables. If your project also uses SCSS, you can modify the theme by overwriting these global variables. This is not recommended.

View the defined variables [here](https://github.com/AgoraIO-Usecase/AgoraChat-UIKit-web/blob/main/common/style/themes/default.scss).

Modify these variables in the following way:

- Modify SCSS variables in Create React App projects.

    In a project created with Create React App, you can create a SCSS file to override the default variables. In the example, we call the created file `your-theme.scss` and then import the files in the following order:
    
    ```scss
    @import 'round-chat-uikit/style.scss'; // round-chat-uikit theme
    @import 'your-theme.scss'; // Your theme files
    @import 'round-chat-uikit/components.scss'; // UIKit component styles
    ```

- Override SCSS variables by modifying the Webpack config.

    Configure the SCSS loader to automatically import custom `style.scss` files:
    
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


