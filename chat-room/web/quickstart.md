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

1. Install UIKit into your project.

   - To install via npm, run the following command:

    ```
    npm install easemob-chat-uikit --save
    ```
   
   - To install via yarn, run the following command:

    ```
    yarn add easemob-chat-uikit
    ```
   
1. Build an application.

   Import the `easemob-chat-uikit` library into your code:

    ```javascript
    // App.js
    import React, { Component, useEffect } from "react";
    import {
      Provider,
      Chatroom,
      useClient,
      rootStore,
      ChatroomMember,
    } from "easemob-chat-uikit";
    import "easemob-chat-uikit/style.css";
    
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
      const [password, setPassword] = useState("");
      const login = () => {
        client
          .open({
            user: userId,
            pwd: password,
            //accessToken: '',
          })
          .then((res) => {
            console.log("Get token successfully");
          });
      };
      return (
        <>
          <Provider
            theme={{
              mode: "dark",
            }}
            initConfig={{
              appKey: appKey,
            }}
          >
            <div>
              <div>
                <label>userID</label>
                <input
                  onChange={(e) => {
                    setUserId(e.target.value);
                  }}
                ></input>
              </div>
              <div>
                <label>password</label>
                <input
                  onChange={(e) => {
                    setPassword(e.target.value);
                  }}
                ></input>
              </div>
              <div>
                <button onClick={login}>login</button>
              </div>
            </div>
    
            <div style={{ width: "350px" }}>
              <Chatroom chatroomId={chatroomId}></Chatroom>
            </div>
            <div style={{ width: "350px" }}>
              <ChatroomMember chatroomId={chatroomId}></ChatroomMember>
            </div>
          </Provider>
        </>
      );
    });
    export default ChatroomApp;
    ```

1. Run the project:

    ```
    npm run start
    ```

1. Send a message

   Enter the message content at the bottom of the screen and click **Send** to send the message.
