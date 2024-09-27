# Customize UIKit

This page provides several examples to show how to add your own business logic.

## Custom header 

Customize the header via the `renderHeader` method: 

```javascript
import {Chatroom, Header} from 'easemob-chat-uikit'

const ChatApp = () => {
  const CustomHeader = <Header back content="Custom Header">
  return(
    <div>
      <Chatroom renderHeader={(cvs) => CustomHeader}>
    </div>
  )
}
```

## Custom message area

Customize the message area via `renderMessageList`:

```typescript
import { Chatroom, MessageList } from "easemob-chat-uikit";

const ChatApp = () => {
  const renderMessage = (message) => {
    switch (message.type) {
      case "txt":
        return <div>{message.msg}</div>;
    }
  };
  return (
    <div>
      <Chatroom
        chatroomId="chatroomId"
        renderMessageList={() => (
          <MessageList
            conversation={{
              chatType: "chatRoom",
              conversationId: "chatroomId",
            }}
            renderMessage={renderMessage}
          />
        )}
      ></Chatroom>
    </div>
  );
};
```

## Custom gifts

Customize gifts via the `giftConfig` property of `messageEditorProps`:

```typescript
import { Chatroom, MessageList } from "easemob-chat-uikit";

const ChatApp = () => {
  const renderMessage = (message) => {
    switch (message.type) {
      case "txt":
        return <div>{message.msg}</div>;
    }
  };
  return (
    <div>
      <Chatroom
        chatroomId="chatroomId"
        messageEditorProps={{
          giftKeyboardProps: {
            giftConfig: {
              gifts: [
                {
                  giftId: "2665752a-e273-427c-ac5a-4b2a9c82b255",
                  giftIcon:
                    "https://fullapp.oss-cn-beijing.aliyuncs.com/uikit/pictures/gift/AUIKitGift1.png",
                  giftName: "Heart",
                  giftPrice: "1",
                },
              ],
            },
          },
        }}
      ></Chatroom>
    </div>
  );
};
```

## Context

UIKit uses React Context to manage global data. You can use `useChatroomContext` to manage chat room-related data:

```javascript
import React from "react";
import { useChatroomContext, Button } from "easemob-chat-uikit";

const ChatAPP = () => {
  const {
    chatroom,
    muteChatRoomMember,
    unmuteChatRoomMember,
    removerChatroomMember,
  } = useChatroomContext();

  const muteMember = () => {
    muteChatRoomMember("chatroomId", "userId");
  };
  return <Button onClick={muteMember}>mute</Button>;
};
```

`useChatroomContext` includes the following methods/properties: 

| Property/Method | Type | Description |
|---|---|---|
| `chatroom` | `ChatroomInfo` | Chat room information. |
| `muteChatRoomMember` | `(chatroomId: string, userId: string, muteDuration?: number) => Promise<void>` | Mute members. |
| `unmuteChatRoomMember` | `(chatroomId: string, userId: string) => Promise<void>` | Unban a member. |
| `removerChatroomMember` | `(chatroomId: string, userId: string) => void` | Remove members. |





