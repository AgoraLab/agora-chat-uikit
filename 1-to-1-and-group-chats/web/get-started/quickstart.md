# Quickstart

With UIKit, you can easily implement messaging in one-to-one chats and group chats. This page explains how do this for a one-to-one chat.

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- React 16.8.0 or above;
- React DOM 16.8.0 or above;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Supported browsers

|      Browser      | Supported versions |
|:-----------------:|:------------------:|
| Internet Explorer |    11 or above     |
|       Edge        |    43 or above     |
|      Firefox      |     10 or more     |
|      Chrome       |    54 or above     |
|      Safari       |    11 or above     |

## Implementation

Take the following steps to implement the project:

1. Create a project:

    ```
    # Install CLI tools
    npm install create-react-app
    # Build a my-app project
    npx create-react-app my-app
    cd my-app
    ```
    
    ```
    Project directory:
    ├── package.json
    ├── public # Webpack’s static directory
    │ ├── favicon.ico
    │ ├── index.html # Default single-page application
    │ └── manifest.json
    ├── src
    │ ├── App.css # CSS for the app root component
    │ ├── App.js # App component code
    │ ├── App.test.js
    │ ├── index.css # Startup file style
    │ ├── index.js # Startup file
    │ ├── logo.svg
    │ └── serviceWorker.js
    └── yarn.lock
    ```

1. Integrate UIKIt.

   1. Install UIKIt.

       - Install via npm:
    
           ```
           npm install easemob-chat-uikit --save
           ```
    
       - Install via yarn:
    
           ```
           yarn add easemob-chat-uikit
           ```
   
   1. Build an app using UIKit components,
    
      Import the `easemob-chat-uikit` library:
    
       ```javascript
          // App.js
          import React, { Component, useEffect } from "react";
          import {
            Provider,
            Chat,
            ConversationList,
            useClient,
            rootStore,
          } from "easemob-chat-uikit";
          import "easemob-chat-uikit/style.css";
    
          const ChatApp = () => {
            const client = useClient();
            useEffect(() => {
              client &&
                client
                  .open({
                    user: "",
                    token: "",
                  })
                  .then((res) => {
                    // Create a conversation
                    rootStore.conversationStore.addConversation({
                      chatType: "singleChat", // One-to-onne chat and group chat are 'singleChat' and 'groupChat', respectively.
                      conversationId: "userId", // Peer user ID for a one-to-one chat, group ID for a group chat.
                      name: "User 1", // Peer user nickname for a one-to-one chat, group name for a group chat.
                      lastMessage: {},
                    });
                  });
            }, [client]);
    
            return (
              <div>
                <div>
                  <ConversationList />
                </div>
                <div>
                  <Chat />
                </div>
              </div>
            );
          };
    
          class App extends Component {
            render() {
              return (
                <Provider
                  initConfig={{
                    appKey: "your app key",
                  }}
          >
                  <ChatApp />
                </Provider>
              );
            }
          }
    
          export default App;
       ```

3. Run the project and send your first message.

   1. Run the project:

       ```
       npm run start
       ```
   
       Your application is now visible in the browser.

   1. Type your first message and send it. Note that if you are using a custom app key, since there are no contacts, you need to add friends first.




