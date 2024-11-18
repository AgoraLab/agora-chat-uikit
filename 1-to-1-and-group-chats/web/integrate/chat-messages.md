# Message chat

The `Chat` component provides the following functionality:

- Send and receive messages, including text, emojis, pictures, voice, video, files, and business card messages.
- Copy, quote, reply with emoji, pin, translate, recall, delete, edit, resend, and report messages.
- Pull roaming messages from the server.

For details about message-related functions, see [Product features](../overview/product-features.md).

## Usage example

```javascript
import React from "react";
import { Chat } from "agora-chat-uikit";
import "agora-chat-uikit/style.css";

const ChatContainer = () => {
  return (
    <div style={{ width: "70%", height: "100%" }}>
      <Chat />
    </div>
  );
};
```

## Custom components

### Modify the message bubble style

Taking text messages as an example, you can modify the message bubble style as follows:

- Use the `renderMessageList` method to customize the rendered message list.
- Use the `renderMessage` method to customize the rendered message.
- Customize the text message through the properties of `TextMessage`.

```javascript
import React from "react";
import { Chat, MessageList, TextMessage } from "agora-chat-uikit";
import "agora-chat-uikit/style.css";

const ChatContainer = () => {
  const renderTxtMsg = (msg) => {
    return (
      <TextMessage
        bubbleStyle={{ background: "hsl(135.79deg 88.79% 36.46%)" }}
        shape="square"
        status={msg.status}
        avatar={<Avatar style={{ background: "pink" }}>A</Avatar>}
        textMessage={msg}
      ></TextMessage>
    );
  };

  const renderMessage = (msg) => {
    if (msg.type === "txt") {
      return renderTxtMsg(msg);
    }
  };

  return (
    <div style={{ width: "70%", height: "100%" }}>
      <Chat
        renderMessageList={() => <MessageList renderMessage={renderMessage} />}
      />
    </div>
  );
};
```

### Set the message date and time format

Use the `formatDateTime` method to set the format of the displayed message date and time:

```javascript
<Chat
  messageListProps={{
    messageProps: {
      formatDateTime: (time: number) => {
        // Custom display date and time
        return new Date(time).toLocaleString();
      },
    },
  }}
/>
```

### Set the display message operation

Use the `customAction` property of `messageProps` to set the message action button to be displayed after clicking **⋮** next to the message list item:

```javascript
<Chat
  messageListProps={{
    messageProps: {
      visible: true,
      icon: null,
      actions: [
        {
          // Display Forwarding of a single message
          content: 'FORWARD',
        },
        {
          // Display message quote
          content: 'REPLY',
        },
        {
          // Display message withdrawal
          content: 'UNSEND',
        },
        {
          // Display message editing
          content: 'Modify',
        },
        {
          // Display selection of multiple messages
          content: 'SELECT',
        },
        {
          // Display the message on top
          content: 'PIN',
        },
        {
          // Display message translation
          content: 'TRANSLATE',
        },
        {
          // Display message report
          content: 'REPORT',
        },
        {
          // Display message deletion
          content: 'DELETE',
        },
        {
          content: 'Custom button',
          // Custom icon
          icon: <Icon type="STAR"/>
          onClick: () => {},
        },
      ],
    },
  }}
/>
```

### Configure the input box features

You can configure the features of the message input box, including whether to display the voice message button, emojis, the send button, and elements:

```javascript
import React from 'react';
import { Chat, Icon, MessageInput } from 'agora-chat-uikit';
import 'agora-chat-uikit/style.css';

const ChatContainer = () => {
  return (
    <div style={{ width: '70%', height: '100%' }}>
      <Chat
        renderMessageInput={() => (
          <MessageInput
            actions={[
              {
                // Send voice function
                name: 'RECORDER',
                visible: true,
              },
              {
                //Message input box
                name: 'TEXTAREA',
                visible: true,
              },
              {
                // expression
                name: 'EMOJI',
                visible: true,
              },
              {
                // More operations
                name: 'MORE',
                visible: true,
              },
            ]}
            enabledTyping={true} // Whether to enable the typing function
            showSendButton={true} // Whether to display the send button
            sendButtonIcon={<Icon type="AIR_PLANE" />} // Icon of the Send button
            row={1} // Input row number
            placeHolder="Please enter content" // default placeholder
            enabledMention={true} // Whether to enable the group mention function
            onSendMessage={message => {}} //Callback for sending message
            onBeforeSendMessage={message => {}} // Callback before message sending. The callback returns promise. If the status of the returned promise is resolved, send the message; if the status of the returned promise is rejected, do not send the message.
          />
        )}
      />
    </div>
  );
};
```

### Add custom icons in the message input box

Add a custom icon to the message input box in the following way:

1. Use the `renderMessageInput` method to customize the rendered message input box.
1. Use custom `actions` components of `MessageInput`.

```javascript
import React from "react";
import { Chat, Icon, MessageInput } from "agora-chat-uikit";
import "agora-chat-uikit/style.css";

const ChatContainer = () => {
  // Customize an icon to add in the message input box
  const CustomIcon = {
    visible: true,
    name: "CUSTOM",
    icon: (
      <Icon
        type="DOC"
        onClick={() => {
          console.log("click custom icon");
        }}
      ></Icon>
    ),
  };

  const actions = [...MessageInput.defaultActions];
  // Add a custom icon in the message input box
  actions.splice(2, 0, CustomIcon);
  return (
    <div style={{ width: "70%", height: "100%" }}>
      <Chat renderMessageInput={() => <MessageInput actions={actions} />} />
    </div>
  );
};
```

### Send custom messages

1. Use the `sendMessage` method provided in `messageStore` to send a custom message.
1. Use `renderMessage` to render a custom message.

To ensure that the message is displayed in the current conversation, the `to` field in the message must be the other party's user ID or group ID.

```javascript
import React from "react";
import {
  Chat,
  MessageList,
  TextMessage,
  rootStore,
  MessageInput,
  Icon,
} from "agora-chat-uikit";
import "agora-chat-uikit/style.css";

const ChatContainer = () => {
  // Display a custom message
  const renderCustomMsg = (msg) => {
    return (
      <div>
        <h1>Business Card </h1>
        <div>{msg.customExts.id}</div>
      </div>
    );
  };
  const renderMessage = (msg) => {
    if (msg.type === "custom") {
      return renderCustomMsg(msg);
    }
  };

  // Add an icon in the message input box
  const CustomIcon = {
    visible: true,
    name: "CUSTOM",
    icon: (
      <Icon
        type="DOC"
        onClick={() => {
          sendCustomMessage();
        }}
      ></Icon>
    ),
  };
  const actions = [...MessageInput.defaultActions];
  actions.splice(2, 0, CustomIcon);

  // Implement sending custom messages
  const sendCustomMessage = () => {
    const customMsg = ChatSDK.message.create({
      type: "custom",
      to: "targetId", // Message recipient: Peer user ID for a one-to-one chat and group ID for a group chat.
      chatType: "singleChat",
      customEvent: "CARD",
      customExts: {
        id: "userId3",
      },
    });
    rootStore.messageStore.sendMessage(customMsg).then(() => {
      console.log("send success");
    });
  };
  return (
    <div style={{ width: "70%", height: "100%" }}>
      <Chat
        renderMessageList={() => <MessageList renderMessage={renderMessage} />}
        renderMessageInput={() => <MessageInput actions={actions} />}
      />
    </div>
  );
};
```

### Modify the chat-related topics

The `Chat` component provides variables related to the chat page theme to support customization, as shown below. For more information on how to modify the theme, see [theme documentation](theme.md).

```javascript
$chat-bg: $component-background;
$msg-base-font-size: $font-size-lg;
$msg-base-color: $font-color;
$msg-base-margin: $margin-xs 0;
$msg-base-padding: 0 $padding-lg;
$msg-bubble-border-radius-left: 12px 16px 16px 4px;
$msg-bubble-border-radius-right: 16px 12px 4px 16px;
$msg-bubble-arrow-border-size: 6px;
$msg-bubble-arrow-bottom: 8px;
$msg-bubble-arrow-left: -11px;
$msg-bubble-arrow-right: -11px;
$msg-bubble-color-secondly: $blue-95;
$msg-bubble-color-primary: $blue-5;
$msg-bubble-font-color-secondly: $font-color;
$msg-bubble-font-color-primary: $gray-98;
$msg-base-content-margin: 0 $margin-xs 0 $margin-sm;
$msg-base-content-padding: $padding-xs $padding-sm;
$msg-base-content-minheight: 24px;
$msg-bubble-none-bg: transparent;
$msg-bubble-none-color: $font-color;
$msg-bubble-square-border-radius: 4px;
$msg-info-margin-left: $margin-sm;
$msg-nickname-font-size: $font-size-sm;
$msg-nickname-font-weight: 500;
$msg-nickname-font-color: #5270ad;
$msg-nickname-height: 16px;
$msg-time-font-size: $font-size-sm;
$msg-time-font-weight: 400;
$msg-time-font-color: $gray-7;
$msg-time-height: 16px;
$msg-time-margin: 0 $margin-xss;
$msg-time-width: 106px;
```

## Chat component properties

The `Chat` component contains the following properties:

| Property | Type | Description |
|---|---|---|
| `className` | `String` | The class name of the component. |
| `prefix` | `String` | The CSS class name prefix. |
| `headerProps` | `HeaderProps` | The properties in the header component. |
| `messageListProps` | `MsgListProps` | The properties of the `MessageList` component. |
| `messageInputProps` | `MessageInputProps` | The properties of the `MessageInput` component. |
| `renderHeader` | `(cvs: CurrentCvs) => React.ReactNode` | Method for customizing the rendering of the header component. |
| `renderMessageList` | `() => ReactNode;` | Method for customizing the rendering of the `MessageList` component. |
| `renderMessageInput` | `() => ReactNode;` | Method for customizing the rendering of the `MessageInput` component. |
| `renderEmpty` | `() => ReactNode;` | Method for customizing the rendering of the empty content components. |