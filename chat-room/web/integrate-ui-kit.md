# Integrate UIKit 

Before using UIKit for chat rooms, you need to integrate it into your application.

## Prerequisites

Before you start, make sure your development environment meets the following requirements:

- React 16.8.0 or above;
- React DOM 16.8.0 or above;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details.

## Implementation

1. Create a project:

    ```shell
    # Install the CLI tools.
    npm install create-react-app
    # Build a my-app project.
    npx create-react-app my-app
    cd my-app
    ```

    ```
    Project directory:
    ├── package.json
    ├── public                  # Webpack's static directory.
    │   ├── favicon.ico
    │   ├── index.html          # The default single page application.
    │   └── manifest.json
    ├── src
    │   ├── App.css             # CSS for the App root component.
    │   ├── App.js              # App component code.
    │   ├── App.test.js
    │   ├── index.css           # Startup file style.
    │   ├── index.js            # Startup file.
    │   ├── logo.svg
    │   └── serviceWorker.js
    └── yarn.lock
    ```
1. Install UIKit into your project:

   - To install via npm, run the following command:

    ```shell
    npm install agora-chat-uikit
    ```
   
   - To install via yarn, run the following command:

    ```shell
    yarn add agora-chat-uikit
    ```