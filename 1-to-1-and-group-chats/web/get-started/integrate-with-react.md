Before using UIKit, you need to integrate it into your app. This page explains how to integrate it into a React project.

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- React 16.8.0 or above
- React DOM 16.8.0 or above

## Integrate UIKIt

Take the following steps:

1. Install UIKit

    Use npm or yarn to install:

    ```
    npm i easemob-chat-uikit --save;
    ```

1. Introduce the required components

    Import the UIKit component into your React project:

    ```javascript
    // Import components
    import {
      UIKitProvider,
      Chat,
      ConversationList,
      // ...
    } from "agora-chat-uikit";
   
    // Import styles
    import "agora-chat-uikit/style.css";
    ```

1. Initialize 

    All components used in the project need `UIKitProvider` to be used internally. When using UIKit, first configure the `UIKitProvider` parameters, as shown below.

    To implement automated login, pass in `userId` and `password` or `token` during initialization.

    Create a Chat user in Agora Console and obtain a user ID and password. If you use a token, obtain it from your app server. For more details, see [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication?platform=web).

    ```javascript
    import React from 'react';
    import { UIKitProvider } from 'agora-chat-uikit';
    import 'agora-chat-uikit/style.css';
    ReactDOM.createRoot(document.getElementById('root') as Element).render(
      <div>
        <UIKitProvider
          initConfig={{
            appKey: 'your app key', // Your app key
            userId: 'user ID', // User ID
            password: 'password', // If you use a password to log in, pass it in .
          }}
        />
      </div>
    )
    ```
   
    For more configuration information, see [UIKitProvider documentation](../core-functionality/user-information.md).

1. Log in

    When the Provider is rendered and destroyed, UIKit will automatically complete the login and logout.
    
    For more information about automated and manual login, see the [login documentation](../core-functionality/log-in.md).
    
1. Build the interface

    UIKit provides components such as a conversation list, chat, group settings, and contact list. You can use these components to build the interface. These components support customization. For details, refer to the documentation of the corresponding component.

    The container-level components provided by UIKit use a flex layout, with a default width and height of 100%, so you need to implement the layout yourself and then place the components in its container.

    The following is an example of an interface consisting of a conversation list and a chat component:

    ```javascript
   import React from "react";
   import { Chat, MessageList, TextMessage } from "agora-chat-uikit";
   import "agora-chat-uikit/style.css";
   
   const App = () => {
     return (
       <div style={{ display: "flex" }}>
         <div style={{ width: "30%", height: "100%" }}>
           <ConversationList />
         </div>
         <div style={{ width: "70%", height: "100%" }}>
           <Chat />
         </div>
       </div>
     );
   };
    ```
   
    