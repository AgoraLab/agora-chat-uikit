The `ConversationList` component handles display and management of the conversation list. By default, the component provides features such as creating a new conversation, deleting a conversation, setting the DND mode, and pinning conversations to the top, specifically:

- Click **Search** to go to the search page and search for conversations.
- Click a conversation list item to jump to the conversation details page.
- Click the expand button in the navigation bar and select **New Conversation** to create a new conversation.
- Long-press a conversation list item to display the menu, where you can delete the conversation, pin the conversation, or disable messages.

- A single conversation displays the conversation name, the last message, the time of the last message, the pinned and muted status, and so on.

The displayed name of the conversation depends on its type. For one-on-one chats, the name displayed in the conversation is the nickname of the other user. If the other user has not set a nickname, the other user's ID is displayed. The conversation avatar is the other user's avatar. If not set, the default avatar is used. For group chats, the conversation name is the name of the current group and the avatar is the default avatar.

For details about the features related to the conversation list, see [Product features](../overview/product-features.md).

![Conversation list](../../assets/images/conversation_list_highlighted.jpg)

The following is a sample code using the default parameters:

```typescript
import type { NativeStackScreenProps } from "@react-navigation/native-stack";
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ConversationListScreen(props: Props) {
  const { navigation } = props;

  return (
    <SafeAreaView
      style={{
        flex: 1,
      }}
    >
      <ConversationList
        onClickedSearch={() => {
          // Jump to search page
          navigation.push("SearchConversation", {});
        }}
        onClickedItem={(data) => {
          // Jump to the conversation details page
          if (data === undefined) {
            return;
          }
          const convId = data?.convId;
          const convType = data?.convType;
          const convName = data?.convName;
          navigation.push("ConversationDetail", {
            params: {
              convId,
              convType,
              convName: convName ?? convId,
            },
          });
        }}
        onClickedNewConversation={() => {
          // Jump to the create a new conversation page
          navigation.navigate("NewConversation", {});
        }}
      />
    </SafeAreaView>
  );
}
```

## Core properties of the conversation list

The core properties provided by the `ConversationList` component are as follows:

| Property | Type | Required/Optional | Description |
|:---:|:---:|:---:|:---:|
| `containerStyle` | object | Optional | Modify the component style. |
| `onSort` | Function | Optional | Customize the list sorting strategy. |
| `onClickedNewConversation` | Function | Optional | The callback for clicking the create a conversation button. Routing may be used. |
| `onClickedNewGroup` | Function | Optional | The callback for clicking the create a group button. Routing may be used. |
| `onClickedNewContact` | Function | Optional | The callback for clicking the add a contact button. Routing may be used. |
| `ListItemRender` | Function | Optional | A component for customizing the conversation list items. You can modify the layout, style, visibility, and others. |
| `onStateChanged` | Function | Optional | The list component status notification, including the loading failure, the list is empty, and others. |
| `propsRef` | reference | Optional | The reference object of the list component that can actively add, modify, and delete conversation list items. Pay attention to the operating conditions. |
| `onInitNavigationBarMenu` | Function | Optional | Customize the list navigation bar to modify the style, event behavior, and others. |
| `onInitBottomMenu` | Function | Optional | Register menu items and customize menus. |
| `filterEmptyConversation` | Function | Optional | Whether to filter empty conversations. |
| `onChangeUnreadCount` | Function | Optional | The callback notification for changes in the total number of unread messages. |

## Customize the navigation bar

The navigation bar is a common `TopNavigationBar` component with a left-center-right layout that supports customization. The left-center-right subcomponents are optional and support customization as well, such as modifying the style, layout, behavior, color, and others.

For example, you can add a back button and avatar on the left side of the navigation bar, a title in the middle, and an extended menu on the right.

You can also use the `View` component to implement a custom navigation bar. Compared to the `TopNavigationBar` component, the `View` component is more flexible; for example, it supports setting the background color.

```typescript
type MyConversationListScreenProps = {};
function MyConversationListScreen(props: MyConversationListScreenProps) {
  const {} = props;
  const convRef = React.useRef<ConversationListRef>({} as any);
  const { tr } = useI18nContext();

  return (
    <ConversationList
      propsRef={convRef}
      customNavigationBar={
        <TopNavigationBar
          Left={
            <StatusAvatar
              url={
                'https://cdn3.iconfinder.com/data/icons/vol-2/128/dog-128.png'
              }
              size={32}
              onClicked={() => {
                convRef.current?.showStatusActions?.();
              }}
              userId={'userId'}
            />
          }
          Right={TopNavigationBarRight}
          RightProps={{
            onClicked: () => {
              convRef.current?.showMoreActions?.();
            },
            iconName: 'plus_in_circle',
          }}
          Title={TopNavigationBarTitle({
            text: tr('_uikit_navi_title_chat'),
          })}
        />
      }
    />
  );
}
```

## Configure the conversation list item

Use the `ListItemRender` property to modify the style and layout of list items, such as the height, width, background color, top and bottom margins, left and right margins, and the custom click and long-press behaviors:

```typescript
type MyConversationListScreenProps = {};
function MyConversationListScreen(props: MyConversationListScreenProps) {
  const {} = props;
  const convRef = React.useRef<ConversationListRef>({} as any);

  return (
    <ConversationList
      propsRef={convRef}
      ListItemRender = {() => {
        // todo: custom list item style
        return (
          <Pressable
            style={{
              height: 40,
              width: '100%',
              marginVertical: 10,
              backgroundColor: 'red',
            }}
            onPress={() => {
              // todo: custom click behavior
            }}
            onLongPress={() => {
              // todo: custom long press behavior
            }}
          />
        );
      }}
    />
  );
}
```

The default component does not support side sliding. For this, you will need to customize the list item component, for example, add buttons to the left and right slide menus. The `SlideListItem` component is provided:

```typescript
type MyConversationListScreenProps = {};
function MyConversationListScreen(props: MyConversationListScreenProps) {
  const {} = props;
  const convRef = React.useRef<ConversationListRef>({} as any);

  return (
    <ConversationList
      propsRef={convRef}
      ListItemRender = {() => {
        const { data } = props;
        return (
          <SlideListItem    
            height={100}
            leftExtraWidth={100}
            rightExtraWidth={100}
            data={data}
            key={data.convId}
            containerStyle={{
              backgroundColor: 'orange',
            }}
            onPress={() => {
              console.log('test:zuoyu: onPress');
            }}
            onLongPress={() => {
              console.log('test:zuoyu: onLongPress');
            }}
          >
            <View
              style={{
                width: Dimensions.get('window').width + 200,
                height: '100%',
                backgroundColor: 'orange',
                flexDirection: 'row',
              }}
            >
              <View
                style={{
                  backgroundColor: 'yellow',
                  height: '100%',
                  width: 100,
                }}
              />
              <View
                style={{
                  backgroundColor: 'blue',
                  height: '100%',
                  width: Dimensions.get('window').width,
                }}
              />
              <View />
            </View>
          </SlideListItem>
        );
      }}
    />
  );
}
```

## Configure the avatar and nickname

`ConversationList` items are divided into users and groups. For avatars and nicknames, the priority is given to the data provided by the user. Otherwise, default avatars and IDs are used. For groups, the default avatars and IDs are used by default. 

Avatars and nicknames can be provided in the following ways:

- Register callbacks: Use the `onUsersHandler` and `onGroupsHandler` properties of the `Container` component.
- Active call: Use the `ChatService.updateDataList` method. Calling this method will trigger internal event distribution. You can also customize the distribution handle and refresh the loaded component page.

Regardless of the update method, the cached data will be updated, and active updates will also trigger UI component refreshes.

## Event notifications

Event notifications have been implemented in the list, and the list will be updated when the corresponding event is received. 