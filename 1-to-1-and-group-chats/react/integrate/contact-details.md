# Contact details

For the contact details page, you can customize the navigation bar, the contact list items, and the buttons for sending messages, audio calls, and video calls.

## Customize the navigation bar

The navigation bar component is a common component with the left-center-right layout. The customization method is similar to the conversation list. For details, see [Conversation list](conversation-list.md).

## Custom buttons

The `onInitButton` property is provided to customize the buttons. The default buttons include sending messages, audio call, video call, and others.

You can add, delete, and modify button items, as well as modifying the style, layout, events, and other contents of each button item.

```typescript
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ContactInfoScreen(props: Props) {
  const { route } = props;
  const userId = ((route.params as any)?.params as any)?.userId;

  return (
    <ContactInfo
      userId={userId}
      onInitButton = {(items) => {
        items.length = 0;
        items.push(
          <BlockButton key={"1001"} iconName="2_bars_in_circle" text="test" />
        );
        return items;
      }}
    />
  );
}
```

## Custom list items

The `customItemRender` property is provided to modify the list items. The default includes setting the DND mode, clearing the chat history, and others.

You can add, delete, and modify list items, including their style, layout, events, and other contents.

```typescript
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function ContactInfoScreen(props: Props) {
  const { route } = props;
  const userId = ((route.params as any)?.params as any)?.userId;

  return (
    <ContactInfo
      userId={userId}
      customItemRender={(list) => {
        // todo: add custom list items
        list.push(
          <View style={{ height: 100, width: 100, backgroundColor: "green" }} />
        );
        return list;
      }}
    />
  );
}
```