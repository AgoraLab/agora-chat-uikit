# Configurable items

UIKit provides `Chatroom` and `ChatroomMember` components, which contain various properties that you can set according to your needs.

`Chatroom` is the entire chat interface component, which consists of `Header`, `MessageList`, `MessageInput`, and `Broadcast` subcomponents. Each component can be replaced with a custom component using the method.

## Chatroom

| Property | Required/Optional | Type | Description |
|:---:|:---:|:---:|:---:|
| `className` | Optional | `String` | The class name of the component. |
| `prefix` | Optional | `String` | The prefix for the CSS class names. |
| `style` | Optional | `React.CSSProperties` | The style of the component. |
| `chatroomId` | Required | `String` | The chat room ID. |
| `renderEmpty` | Optional | `() => ReactNode` | Customize the content for rendering when there is no conversation. |
| `renderHeader` | Optional | `(roomInfo: ChatroomInfo) => ReactNode` | Customize the header rendering. |
| `headerProps` | Optional | `HeaderProps` | Properties of the header component. |
| `renderMessageList` | Optional | `() => ReactNode` | Custom rendering of the chat room message area. |
| `renderMessageInput` | Optional | `() => ReactNode` | Custom rendering of the message input area. |
| `messageInputProps` | Optional | `MessageEditorProps` | Properties of the message input area component. |
| `messageListProps` | Optional | `MsgListProps` | Properties of the chat room message area component. |
| `renderBroadcast` | Optional | `() => ReactNode` | Custom rendering of the global broadcast component. |
| `broadcastProps` | Optional | `BroadcastProps` | Global broadcast component properties. |

For example, you can pass `className` and modify styles through the `style` and `prefix` component properties.

```javascript
import { Chatroom, Button } from "easemob-chat-uikit";

const ChatApp = () => {
  return (
    <div>
      <Chatroom className="customClass" prefix="custom" />
      <Button style={{ width: "100px" }}>Button</Button>
    </div>
  );
};
```

## ChatroomMember

ChatroomMember is used to display the chat room owner and chat room members, as well as the ban list. The chat room owner can ban or kick members out of the chat room.

| Property | Required/Optional | Type | Description |
|:---:|:---:|:---:|:---:|
| `className` | Optional | `String` | The class name of the component. |
| `prefix` | Optional | `String` | The prefix for the CSS class names. |
| `style` | Optional | `React.CSSProperties` | The style of the component. |
| `chatroomId` | Optional | `String` | The chat room ID. |
| `renderHeader` | Optional | `(roomInfo: ChatroomInfo) => ReactNode` | Customize the rendering of the header. |
| `headerProps` | Optional | `HeaderProps` | Properties of the header component. |
| `memberListProps` | Optional | `{ search?: boolean; placeholder?: string; renderEmpty?: () => ReactNode; renderItem?: (item: AppUserInfo) => ReactNode; UserItemProps?: UserItemProps; }` | Properties of the member list component. |
| `muteListProps` | Optional | `{ search?: boolean; placeholder?: string; renderEmpty?: () => ReactNode; renderItem?: (item: AppUserInfo) => ReactNode; UserItemProps?: UserItemProps; }` | Properties of the mute list component. |

```javascript
import { ChatroomMember } from "easemob-chat-uikit";

const ChatApp = () => {
  return (
    <div>
      <ChatroomMember chatroomId="chatroomId"></ChatroomMember>
    </div>
  );
};
```


