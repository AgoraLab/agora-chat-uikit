# Best practices

## Initialize UIKit

The `Chatroom` and `ChatroomMember` components need to be wrapped in the `UIKitProvider` component for use. If you use `useClient` to obtain the SDK inside the `UIKitProvider` component before initialization is completed, the acquisition will fail, so it is recommended to put `UIKitProvider` in the parent component that uses the `Chatroom` or `ChatroomMember` component.

```javascript
// App.ts
// ...
const App = () => {
    return <Chatroom />
}

// index.ts
// ...
ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
    <UIKitProvider>
        <App>
    </UIKitProvider>
)
```

## Log in to UIKit

UIKit provides two login methods:

- Specify `userId` and `token` for automatic login during initialization.
- Use `useClient` to get an SDK instance to log in manually.

```typescript
// Manual login
// ...
const App = () => {
    const client = useClient()
    const login = () => {
        client.open({
            user: 'userId',
            token: 'token'
        })
    }
    return <Button onClick={login}>Log in</Button>
}

// Automatic login
// ...
ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
    <UIKitProvider initConfig={{
        userId: 'userId',
        token: 'token'
    }}>
        <App>
    </UIKitProvider>
)
```
 
## Listen to chat room events

UIKit provides a chatroom event listening interface. You can register a chatroom listener to obtain chatroom events and handle them accordingly.

The event listeners for actively calling API in UIKit are as follows:

```javascript
import { eventHandler } from "agora-chat-uikit";

eventHandler.addEventHandler("chatroom", {
  onError: (error) => {
    // All API call failures will call back the onError event in addition to the corresponding event.
  },
  joinChatRoom: {
    error: (err) => {},
    success: () => {},
  },
  recallMessage: {
    error: (err) => {},
    success: () => {},
  },
  reportMessage: {
    // ...
  },
  sendMessage: {
    // ...
  },
  getChatroomMuteList: {
    // ...
  },
  removeUserFromMuteList: {
    // ...
  },
  unmuteChatRoomMember: {
    // ...
  },
  removerChatroomMember: {
    // ...
  },
});
```

Get the Chat SDK instance from UIKit to listen for incoming chat room events:

```javascript
import React, { useEffect } from "react";
import { useClient } from "agora-chat-uikit";

const ChatroomApp = () => {
  const client = useClient();

  useEffect(() => {
    client.addEventHandler("chatroom", {
      onChatroomEvent: (event) => {
        if (event.operation === "muteMember") {
          // console.log('You have been banned')
        }
      },
    });
  }, []);
};
```

