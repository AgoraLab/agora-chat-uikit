# Conversation list

The `ConversationList` component is used to display all conversations of the current user in one-to-one and group chats. It provides conversation search, deletion, pinning, and do not disturb features.

- Click **Search** to go to the search page and search for conversations.
- Click a conversation list item to jump to the conversation details page.
- Click the expand button in the navigation bar and select **New Conversation** to create a new conversation.
- Click the action button of conversation list item to display the menu where you can delete the conversation, pin the conversation, or disable offline push for the conversation.

A single conversation displays the conversation name, the last message, the time of the last message, and the pinned and muted status.

- For one-on-one chats, the name displayed in the conversation is the nickname of the other user. If the other user has not set a nickname, the other party's user ID is displayed. The conversation avatar is the other party's avatar. If it is not set, the default avatar is used.
- For group chats, the conversation name is the name of the current group and the avatar is the default avatar.

For details about the features related to the conversation list, see [Product features](../overview/product-features.md).

## Usage examples

```javascript
import React from 'react';
import { ConversationList } from 'agora-chat-uikit';
import 'agora-chat-uikit/style.css';

const Conversation = () => {
  return (
    <div style={{ width: '30%', height: '100%' }}>
      <ConversationList />
    </div>
  );
};
```

## Customize the conversation list

If the default conversation list page does not meet your needs, you can customize it using the properties provided by the `ConversationList` component.

### Customize the style of the conversation list area

You can customize the background color, size, and other styles of the conversation list.

1. Add `className` for the ConversationList component and define styles: 

    ```javascript
    import React from 'react';
    import { ConversationList } from 'agora-chat-uikit';
    import 'agora-chat-uikit/style.css';
    import './index.css';
   
    const Conversation = () => {
      return (
        <div style={{ width: '30%', height: '100%' }}>
          <ConversationList className="conversation" />
        </div>
      );
    };
    ```
   
2. Define the conversation UI style in `index.css`:

    ```javascript
    .conversation {
      background-color: '#03A9F4';
      height: 100%;
      width: 100%;
    }
    ```
   
### Customize the header of the conversation list page

You can customize the header element of the `ConversationList` component, for example, change the title name.

```javascript
import React from 'react';
import { ConversationList, Header, Avatar } from 'agora-chat-uikit';
import 'agora-chat-uikit/style.css';

const Conversation = () => {
  return (
    <div style={{ width: '30%', height: '100%' }}>
      <ConversationList
        renderHeader={() => (
          <Header
            avatar={<Avatar>D</Avatar>}
            content="custom header"
            moreAction={{  
              visible: true,
              actions: [
                {
                  content: 'my info',
                  onClick: () => {
                    console.log('my info');
                  },
                },
              ],
            }}
          />
        )}
      ></ConversationList>
    </div>
  );
};
```

## Customize a conversation list item

### Set the user's nickname

- Use the `renderItem` method to render each conversation.
- Use the `ConversationItem` component's properties to customize it.

```javascript
import React from 'react';
import { ConversationList, ConversationItem, Avatar } from 'agora-chat-uikit';
import 'agora-chat-uikit/style.css';
import './index.css';

const Conversation = () => {
  // In a one-to-ne chat, the peer user's user ID is mapped to its user nickname.
  const idToName = {
    userId1: 'name1',
    zd2: 'Henry 2',
  };
  return (
    <div style={{ width: '30%', height: '100%' }}>
      <ConversationList
        className="conversation"
        renderItem={cvs => {
          return (
            <ConversationItem
              avatar={
                <Avatar
                  size="normal"
                  shape="square"
                  style={{ background: 'yellow', color: 'black' }}
                >
                  {idToName[cvs.conversationId] || cvs.conversationId}
                </Avatar>
              }
              data={{
                ...cvs,
                name: idToName[cvs.conversationId] || cvs.conversationId,
              }}
            />
          );
        }}
      ></ConversationList>
      />
    </div>
  );
};
```

### Set the date and time format

```javascript
<ConversationList
  itemProps={{
    formatDateTime: (time: number) => {
      //Format the timestamp
      return new Date(time).toLocaleString();
    },
  }}
/>
```

### Set up more actions

You can configure the `moreAction` property of `itemProps` to control which features are displayed, or add custom features.

```javascript
<ConversationList
  itemProps={{
    moreAction: {
      visible: true, // Whether to display more operations
      actions: [
        {
          content: 'DELETE', // delete the conversation
        },
        {
          content: 'PIN', // Pin the conversation
        },
        {
          content: 'SILENT', // Conversation do-not-disturb
        },
        {
          content: 'Custom function',
          onClick: () => {},
          icon: <Icon type="STAR" />,
        },
      ],
    },
  }}
/>
```

### Set the content of the latest message in the conversation

Set the `renderMessageContent` method in `itemProps` to customize the content of the latest message:

```javascript
<ConversationList
  itemProps={{
    renderMessageContent: message => {
      return <div>Custom message content</div>;
    },
  }}
/>
```

### Set the message bubble color and avatar

Control the style of the `ConversationItem` by setting the `itemProps` property, including the bubble color and the avatar size and shape.

```javascript
<ConversationList
  itemProps={{
    badgeColor: 'red', // bubble color
    avatarSize: 50, // avatar size  
    avatarShape: 'circle', // avatar shape
  }}
/>
```

### Add and pin conversations

Use the methods provided in `conversationStore`, for example:

- Use the `topConversation` method to pin a conversation.
- Use the `addConversation` method to add a conversation.

```javascript
import React from 'react';
import { ConversationList, ConversationItem, rootStore, Button } from 'agora-chat-uikit';
import 'agora-chat-uikit/style.css';

const Conversation = () => {
  // Pin a conversation
  const topConversation = () => {
    rootStore.conversationStore.topConversation({
      chatType: 'singleChat', // Group chat is `groupChat`
      conversationId: 'userID', // Enter the conversation ID obtained from the conversation list
      lastMessage: {},
    });
  };

  // Create a new conversation
  const createConversation = () => {
    rootStore.conversationStore.addConversation({
      chatType: 'singleChat',
      conversationId: 'conversationId',
      lastMessage: {},
      unreadCount: 3,
    });
  };
  return (
    <div style={{ width: '30%', height: '100%' }}>
      <ConversationList
        renderItem={cvs => {
          return (
            <ConversationItem
              moreAction={{
                visible: true,
                actions: [
                  {
                    // The conversation deletion event is provided by default。
                    content: 'DELETE',
                  },
                  {
                    content: 'Top Conversation',
                    onClick: topConversation,
                  },
                ],
              }}
            />
          );
        }}
      ></ConversationList>
      <div>
        <Button onClick={createConversation}>create conversation</Button>
      </div>
    </div>
  );
};
```

### Modify the topic related to the conversation list

The `ConversationList` component provides variables related to the theme of the conversation list page, as shown below. For how to modify the theme, see [here](theme.md).

```javascript
// Variables used to set the topic of the conversation.
$cvs-background: $component-background;
$cvs-search-margin: $margin-xs $margin-sm;
$cvs-item-height: 74px;
$cvs-item-padding: $padding-s;
$cvs-item-border-radius: 16px;
$cvs-item-margin: $margin-xss $margin-xs;
$cvs-item-selected-bg-color: #e6f5ff;
$cvs-item-selected-name-color: $blue-6;
$cvs-item-hover-bg-color: $gray-98;
$cvs-item-active-bg-color: $gray-9;
$cvs-item-info-right: 16px;
$cvs-item-name-margin: 0 $margin-sm;
$cvs-item-name-font-size: $font-size-lg;
$cvs-item-name-font-weight: 500;
$cvs-item-name-color: $title-color;
$cvs-item-message-margin-left: $margin-sm;
$cvs-item-message-font-size: $font-size-base;
$cvs-item-message-font-weight: 400;
$cvs-item-message-color: $font-color;
$cvs-item-time-font-weight: 400;
$cvs-item-time-font-size: $font-size-sm;
$cvs-item-time-color: $gray-5;
$cvs-item-time-margin-bottom: 9px;
```

The `ConversationList` component contains the following properties:

| Property | Type | Description |
|---|---|---|
| `className` | `String` | The class name of the component. |
| `prefix` | `String` | The CSS class name prefix. |
| `headerProps` | `HeaderProps` | The header component parameters. |
| `itemProps` | `ConversationItemProps` | The `ConversationItem` component parameters. |
| `renderHeader` | `() => React.ReactNode` | Customize the rendering of the header component. |
| `renderSearch` | `() => React.ReactNode` | Customize the rendering of the search component. |
| `onItemClick` | `(data: ConversationData[0]) => void` | Callback event upon a click of a conversation in the conversation list. |
| `onSearch` | `(e: React.ChangeEvent<HTMLInputElement>) => boolean` | The change event of the search input box. When the function returns `false`, the default search behavior will be prevented and you can use your own search criteria to search. | 

