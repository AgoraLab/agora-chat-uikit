All internal state data of UIKit is stored in `rootStore`. The `rootStore` data update will drive each component to update the UI. You can use the data in `rootStore` or call the methods provided by UIKit to update the data.

`rootStore` contains the following data modules:

`initConfig`: UIKit initialization data.
`client`: Chat instance.
`conversationStore`: Conversation list-related data.
`messageStore`: Message-related data.
`addressStore`: Address book-related data.

| Store | Property | Description |
|---|---|---|
| `conversationStore` |  |  |
|  | `currentCvs` | The current conversation. |
|  | `conversationList` | All conversations. |
|  | `searchList` | Searched conversations. |
| `messageStore` |  |  |
|  | `message` | The entire app's messages including `singleChat` and `groupChat` messages, as well as messages stored by message ID (`byId`). |
|  | `selectedMessage` | Multiple selected messages. |
|  | `repliedMessage` | Quoted messages. |
|  | `typing` | Whether input is in progress. |
|  | `unreadMessageCount` | The number of unread messages in the current conversation. |
| `addressStore` |  |  |
|  | `appUsersInfo` | User attributes of all users, used when displaying avatar nicknames. |
|  | `Contacts` | All contacts. |
|  | `groups` | All joined groups. |
|  | `searchList` | Searched contacts or groups` |
|  | `requests` | Friend requests. |

## Usage examples

```javascript
import { rootStore } from "easemob-chat-uikit";
```

## Calling the API to change the global state

UIKit provides three custom hooks - `useChatContextstate`, `useConversationContext`, and `useAddressContext`, for changing the state.

## useConversationContext

This custom hook can return conversation-related data and data management methods.

### Usage examples

```javascript
import React from "react";
import { useConversationContext } from "easemob-chat-uikit";

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

### useConversationContext overview

| Name | Property or method | Type | Description |
|---|---|---|---|
| `currentConversation` | Property | `CurrentConversation` | Current conversation. |
| `conversationList` | Property | `Conversation[]` | All conversations. |
| `setCurrentConversation` | Method | `(currentCvs: CurrentConversation) => void` | Set the current conversation. |
| `deleteConversation` | Method | `(conversation: CurrentConversation) => void` | Delete a conversation from the conversation list. |
| `topConversation` | Method | `(conversation: Conversation) => void` | Pin a conversation. |
| `addConversation` | Method | `(conversation: Conversation) => void` | Add a conversation to the conversation list. |
| `modifyConversation` | Method | `(conversation: Conversation) => void` | Modify a conversation. |

## useChatContext

This custom hook can return message-related data and data management methods.

### Usage example

```javascript
import React from "react";
import { useChatContext, useSDK } from "easemob-chat-uikit";

const ChatAPP = () => {
  const { easemobChat } = useSDK(); // Get Chat SDK
  const { messages, sendMessage } = useChatContext();
  const sendCustomMessage = () => {
    const customMsg = easemobChat.message.create({
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

### useChatContext overview

| Name | Property or method | Type | Description |
|---|---|---|---|
| `messages` | Property | Message | All message data in UIKit. |
| `repliedMessage` | Property | `easemobChat.MessagesType` | Replying message. |
| `typing` | Property | Typing | The user object being entered. |
| `sendMessage` | Method | `(message: easemobChat.MessageBody) => Promise<void>` | Send a message. |
| `deleteMessage` | Method |<code>deleteMessage: (cvs: CurrentConversation, messageId: string &#124; string[]) => void &#124; Promise<void></code> | Delete a message. |
| `recallMessage` | Method |`(cvs: CurrentConversation, messageId: string, isChatThread?: boolean) => Promise<void>` | Recall a message. |
| `translateMessage` |Method | `(cvs: CurrentConversation, messageId: string, language: string) => Promise<boolean>` | Translate text messages. |
| `modifyMessage` | Method |`(messageId: string, msg: easemobChat.TextMsgBody) => Promise<void>` | Edit a message on the server. After editing, the modified message will be displayed to the peer user. This method is only valid for text messages. |
| `modifyLocalMessage` | Method |<code>(id: string, message: easemobChat.MessageBody &#124; RecallMessage) => void</code> | Edit a local message. This method is valid for any type of message. |
| `updateMessageStatus` | Method |<code>(msgId: string, status: 'received' &#124; 'read' &#124; 'unread' &#124; 'sent' &#124; 'failed' &#124; 'sending' &#124; 'default') => void</code> | Update a message status. |
| `sendTypingCommand` | Method |`(cvs: CurrentConversation) => void` | Send the command being entered. |
| `setRepliedMessage` | Method |<code>(message: easemobChat.MessageBody &#124; null) => void</code> | Set a reply message. |
| `clearMessages` | Method |`(cvs: CurrentConversation) => void` | Clear the local messages of a conversation. |

## useAddressContext

This custom hook can return contact and group-related data and data management methods.

### Usage examples

```javascript
import React from "react";
import { useAddressContext } from "easemob-chat-uikit";

const ChatAPP = () => {
  const { appUsersInfo } = useAddressContext();
  console.log(appUsersInfo);
  return <div>ChatAPP</div>;
};
```

### useAddressContext overview

| Name | Property or method | Type | Description |
|---|---|---|---|
| `appUsersInfo` | Property | `Record<string, AppUserInfo>` | All user information. |
| `groups` | Property | `GroupItem[]` | All groups. |
| `setAppUserInfo` | Method | `(appUsersInfo: Record<string, AppUserInfo>) => void` | Set user information. |
| `setGroups` | Method | `(groups: GroupItem[]) => void` | Set group data. |
| `setGroupMemberAttributes` | Method | `(groupId: string, userId: string, attributes: easemobChat.MemberAttributes) => void` | Set group member attributes. |



