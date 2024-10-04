# Group details

You can configure the navigation bar, the custom buttons, and the list items on the group details page.

## Customize the navigation bar

The navigation bar is a common component. It includes the back button on the left and the extended menu on the right. The customization method is similar to the conversation list.

```typescript
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function GroupParticipantListScreen(props: Props) {
  const { route } = props;
  const groupId = ((route.params as any)?.params as any)?.groupId;
  return (
    <GroupParticipantList
      groupId={groupId}
      customNavigationBar={
        <TopNavigationBar
          Left={
            <TopNavigationBarLeft onBack={() => {}} content={"participant"} />
          }
          Right={
            isOwner === true ? (
              <View style={{ flexDirection: "row" }}>
                <Pressable style={{ padding: 6 }}>
                  <IconButton
                    iconName={"person_add"}
                    style={{ width: 24, height: 24 }}
                    onPress={() => {}}
                  />
                </Pressable>
                <View style={{ width: 4 }} />
                <Pressable style={{ padding: 6 }}>
                  <IconButton
                    iconName={"person_minus"}
                    style={{ width: 24, height: 24, padding: 6 }}
                    onPress={() => {}}
                  />
                </Pressable>
              </View>
            ) : null
          }
        />
      }
    />
  );
}
```

## Custom buttons

The `onInitButton` property is provided to customize the buttons. The default buttons include sending messages, audio calls, video calls, and others.

You can add, delete, and modify button items, as well as modifying their style, layout, events, and other contents.

```typescript
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function GroupInfoScreen(props: Props) {
  const { route } = props;
  const groupId = ((route.params as any)?.params as any)?.groupId;
  const ownerId = ((route.params as any)?.params as any)?.ownerId;

  return (
    <GroupInfo
      groupId={groupId}
      ownerId={ownerId}
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

The `customItemRender` property is provided to modify the list items. By default, it includes the DND mode, clearing chat history, and others.

You can add, delete, and modify list items, including their style, layout, events, and other contents.

```typescript
type Props = NativeStackScreenProps<RootScreenParamsList>;
export function GroupInfoScreen(props: Props) {
  const { route } = props;
  const groupId = ((route.params as any)?.params as any)?.groupId;
  const ownerId = ((route.params as any)?.params as any)?.ownerId;

  return (
    <GroupInfo
      groupId={groupId}
      ownerId={ownerId}
      customItemRender={(items) => {
        items.push(
          <View style={{ height: 100, width: 100, backgroundColor: "green" }} />
        );
        return items;
      }}
    />
  );
}
```