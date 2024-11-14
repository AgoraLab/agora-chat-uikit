# Global context

All internal state data of UIKit is stored in `rootStore`. The `rootStore` data update will drive each component to update the UI. You can use the data in `rootStore` or call the methods provided by UIKit to update the data.

`rootStore` contains the following data modules:

- `initConfig`: The UIKit initialization data.
- `client`: The Chat instance.
- `conversationStore`: The conversation list-related data.
- `messageStore`: The message-related data.
- `addressStore`: The contact list-related data.

| Store | Property | Description |
|---|---|---|
| `conversationStore` |  |  |
|  | `currentCvs` | The current conversation. |
|  | `conversationList` | All conversations. |
|  | `searchList` | The conversations retrieved after a search. |
| `messageStore` |  |  |
|  | `message` | The entire app's messages including `singleChat` and `groupChat` messages, as well as messages stored by message ID (`byId`). |
|  | `selectedMessage` | The multiple selected messages. |
|  | `repliedMessage` | The quoted messages. |
|  | `typing` | Whether input is in progress. |
|  | `unreadMessageCount` | The number of unread messages in the current conversation. |
| `addressStore` |  |  |
|  | `appUsersInfo` | The user attributes of all users, used when displaying avatar nicknames. |
|  | `Contacts` | All contacts. |
|  | `groups` | All joined groups. |
|  | `searchList` | The searched contacts or groups. |
|  | `requests` | The friend requests. |

## Usage example

```javascript
import { rootStore } from "agora-chat-uikit";
```

## Call API to change the global state

UIKit provides three custom hooks - `useChatContextstate`, `useConversationContext`, and `useAddressContext` - for changing the state.

### useConversationContext

This custom hook can return conversation-related data and data management methods.

The sample code is as follows:

```javascript
import React from "react";
import { useConversationContext } from "agora-chat-uikit";

const ChatAPP = () => {
  const { conversationList, setCurrentConversation } = useConversationContext();
  const setCurrentCvs = () => {
    setCurrentConversation({
      chatType: "singleChat",
      conversationId: "userId",
    });
  };
  return (
    <div>
      <button onClick={setCurrentCvs}>setCurrentConversation</button>
    </div>
  );
};
```

`useConversationContext` provides the following properties and methods:

| Name | Property or method | Type | Description |
|---|---|---|---|
| `currentConversation` | Property | `CurrentConversation` | The current conversation. |
| `conversationList` | Property | `Conversation[]` | All conversations. |
| `setCurrentConversation` | Method | `(currentCvs: CurrentConversation) => void` | Set the current conversation. |
| `deleteConversation` | Method | `(conversation: CurrentConversation) => void` | Delete a conversation from the conversation list. |
| `topConversation` | Method | `(conversation: Conversation) => void` | Pin a conversation. |
| `addConversation` | Method | `(conversation: Conversation) => void` | Add a conversation to the conversation list. |
| `modifyConversation` | Method | `(conversation: Conversation) => void` | Modify a conversation. |

### useChatContext

This custom hook can return message-related data and data management methods.

The sample code is as follows:

```javascript
import React from "react";
import { useChatContext, useSDK } from "agora-chat-uikit";

const ChatAPP = () => {
  const { agoraChat } = useSDK(); // Get Chat SDK
  const { messages, sendMessage } = useChatContext();
  const sendCustomMessage = () => {
    const customMsg = agoraChat.message.create({
      type: "custom",
      to: "targetId", // Peer user ID for a one-to-one chat, group ID for a group chat.
      chatType: "singleChat",
      customEvent: "CARD",
      customExts: {
        id: "userId",
      },
    });
    sendMessage(customMsg);
  };

  return (
    <div>
      <button onClick={sendCustomMessage}>sendMessage</button>
    </div>
  );
};
```

`useChatContext` provides the following properties and methods:

| Name | Property or method | Type | Description |
|---|---|---|---|
| `messages` | Property | Message | All message data in UIKit. |
| `repliedMessage` | Property | `AgoraChat.MessagesType` | The message being replied to. |
| `typing` | Property | Typing | The user object being entered. |
| `sendMessage` | Method | `(message: AgoraChat.MessageBody) => Promise<void>` | Send a message. |
| `deleteMessage` | Method |<code>deleteMessage: (cvs: CurrentConversation, messageId: string &#124; string[]) => void &#124; Promise<void></code> | Delete a message. |
| `recallMessage` | Method |`(cvs: CurrentConversation, messageId: string, isChatThread?: boolean) => Promise<void>` | Recall a message. |
| `translateMessage` |Method | `(cvs: CurrentConversation, messageId: string, language: string) => Promise<boolean>` | Translate text messages. |
| `modifyMessage` | Method |`(messageId: string, msg: AgoraChat.TextMsgBody) => Promise<void>` | Edit a message on the server. After editing, the modified message will be displayed to the peer user. This method is only valid for text messages. |
| `modifyLocalMessage` | Method |<code>(id: string, message: AgoraChat.MessageBody &#124; RecallMessage) => void</code> | Edit a local message. This method is valid for any type of message. |
| `updateMessageStatus` | Method |<code>(msgId: string, status: 'received' &#124; 'read' &#124; 'unread' &#124; 'sent' &#124; 'failed' &#124; 'sending' &#124; 'default') => void</code> | Update a message status. |
| `sendTypingCommand` | Method |`(cvs: CurrentConversation) => void` | Send the command to indicate that the peer user is typing. |
| `setRepliedMessage` | Method |<code>(message: AgoraChat.MessageBody &#124; null) => void</code> | Set a  message reply. |
| `clearMessages` | Method |`(cvs: CurrentConversation) => void` | Clear the local messages of a conversation. |

## useAddressContext

This custom hook can return contact and group-related data and data management methods.

The sample code is as follows:

```javascript
import React from "react";
import { useAddressContext } from "agoraChat-chat-uikit";

const ChatAPP = () => {
  const { appUsersInfo } = useAddressContext();
  console.log(appUsersInfo);
  return <div>ChatAPP</div>;
};
```

`useAddressContext` provides the following methods and properties:

| Name | Property or method | Type | Description |
|---|---|---|---|
| `appUsersInfo` | Property | `Record<string, AppUserInfo>` | All user information. |
| `groups` | Property | `GroupItem[]` | All groups. |
| `setAppUserInfo` | Method | `(appUsersInfo: Record<string, AppUserInfo>) => void` | Set the user information. |
| `setGroups` | Method | `(groups: GroupItem[]) => void` | Set the group data. |
| `setGroupMemberAttributes` | Method | `(groupId: string, userId: string, attributes: AgoraChat.MemberAttributes) => void` | Set the group member attributes. |



