# Quickstart

With UIKit, you can easily implement user interaction in a chat room. This page describes how to implement sending chat room messages.

## Prerequisites

- React 16.8.0 or above;
- React DOM 16.8.0 or above;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Supported browsers

| Browser | Supported Versions |
|:---:|:---:|
| Internet Explorer | 11 or above |
| Edge | 43 or above |
| Firefox | 10 or more |
| Chrome | 54 or above |
| Safari | 11 or above |

## Implementation

Take the following steps to implement message sending:

1. Create a project:

    ```
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

1. Install UIKit into your project.

   - To install via npm, run the following command:

    ```
    npm install agora-chat-uikit
    ```
   
   - To install via yarn, run the following command:

    ```
    yarn add agora-chat-uikit
    ```

  - To install dependencies, run the following command:

    ```
    npm install mobx-react-lite
    ```
   
1. Build an application.

   Import the `agora-chat-uikit` library into your code:

    ```javascript
    // App.js
    import React, { useState, useEffect, Component } from "react";
    import {
      Provider,
      Chatroom,
      useClient,
      rootStore,
      ChatroomMember,
    } from "agora-chat-uikit";
    import "agora-chat-uikit/style.css";
    import { observer } from "mobx-react-lite";
    
    const ChatroomApp = observer(() => {
      const client = useClient();
      const chatroomId = ""; // Chat room to join
      const appKey = ""; // Your app key
    
      useEffect(() => {
        if (client.addEventHandler) {
          client.addEventHandler("chatroom", {
            onConnected: () => {
              console.log("Login successful");
            },
          });
        }
      }, [client]);
    
      const [userId, setUserId] = useState("");
      const [accessToken, setAccessToken] = useState("");
      const login = () => {
        client
          .open({
            user: userId,
            accessToken: accessToken,
          })
          .then((res) => {
            console.log("Get token successfully");
          });
      };
      return (
        <div style={{
          display: 'flex',
          height: '100vh',
          gap: '1px',
          width: '100%',
          position: 'fixed',
          top: '0'
        }}>
          <Provider
            theme={{
              mode: "dark",
            }}
            initConfig={{
              appKey: appKey,
            }}
          >
            <div style={{ flex: "1" }}>
              <div>
                <label>userID</label>
                <input
                  onChange={(e) => {
                    setUserId(e.target.value);
                  }}
                ></input>
              </div>
              <div>
                <label>AccessToken</label>
                <input
                  onChange={(e) => {
                    setAccessToken(e.target.value);
                  }}
                ></input>
              </div>
              <div>
                <button onClick={login}>login</button>
              </div>
            </div>
    
            <div style={{ flex: "1" }}>
              <Chatroom chatroomId={chatroomId}></Chatroom>
            </div>
            <div style={{ flex: "1" }}>
              <ChatroomMember chatroomId={chatroomId}></ChatroomMember>
            </div>
          </Provider>
        </div>
      );
    });

    class App extends Component {
      render() {
        return (
          <ChatroomApp />
        );
      }
    }

    export default App;
    ```

1. Run the project:

    ```
    npm run dev
    ```

1. Send a message

   Enter the message content at the bottom of the screen and click **Send** to send the message.
