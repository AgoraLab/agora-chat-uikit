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

    ```shell
    # Install the CLI tools.
    npm install -g create-vite
    # Build a my-app project.
    create-vite my-app --template react 
    cd my-app
    # Install dependencies
    npm install
    ```

    ```
    Project directory:
    ├── package.json
    ├── vite.config.js  
    ├── public                  # Vite's static directory.
    ├── src
    │   ├── assets
    │   ├── App.css             # CSS for the App root component.
    │   ├── App.js              # App component code.
    │   ├── index.css           # Startup file style.
    │   └── main.jsx            # Startup file.
    ├── index.html
    └── yarn.lock
    ```

1. Integrate UIKIt.

   1. Install UIKIt.

       - Install via npm:
    
           ```
           npm install agora-chat-uikit
           ```
    
       - Install via yarn:
    
           ```
           yarn add agora-chat-uikit
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
          } from "agora-chat-uikit";
          import "agora-chat-uikit/style.css";
    
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
              <div style={{
                width: '100%',
                position: 'fixed',
                top: '0',
                display: 'flex',
                height: '100vh',
                gap: '1px'
              }}>
                <div style={{
                  width: '350px'
                }}>
                  <ConversationList />
                </div>
                <div style={{
                  flex: '1'
                }}>
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
       npm run dev
       ```
   
       Your application is now visible in the browser.

   1. Type your first message and send it. Note that if you are using a custom app key, since there are no contacts, you need to add friends first.




