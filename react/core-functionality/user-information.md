The data categories of UIKit include the following: Conversation list, contact list, group list, group member list, new request list, and chat page. These UI components need to display avatars and nicknames but do not have this data by default. The user provides data, and the UI component library uses the provided data to display avatars and nicknames. If the user does not provide this data, the default avatar and ID are used.

`DataProfileProvider` can be used to implement the initialization, update, and acquisition of avatars and nicknames.

There are two main implementation methods: Registering data to provide callbacks and actively updating data.

- Registering data to provide callback: Mainly during app initialization, the callback interface provided by the user registration data is automatically called when the data needs to be loaded.

- Actively updating data: Actively update user data when needed and refresh the corresponding UI components. At the same time, this data will be cached and used directly next time.

## Register data to provide callback

Use the `Container` component to implement registration. For example:

```typescript
export function App() {
  const onUsersHandler = React.useCallback(
    async (data: Map<string, DataModel>) => {
      if (data.size === 0) return data;
      const userIds = Array.from(data.keys());
      const ret = new Promise<Map<string, DataModel>>((resolve, reject) => {
        im.getUsersInfo({
          userIds: userIds,
          onResult: (res) => {
            if (res.isOk && res.value) {
              const finalUsers = [] as DataModel[];
              for (const user of res.value) {
                finalUsers.push({
                  id: user.userId,
                  type: "user",
                  name: user.userName,
                  avatar: user.avatarURL,
                  remark: user.remark,
                } as DataModel);
              }
              resolve(DataProfileProvider.toMap(finalUsers));
            } else {
              reject(data);
            }
          },
        });
      });
      return ret;
    },
    [im]
  );
  const onGroupsHandler = React.useCallback(
    async (data: Map<string, DataModel>) => {
      if (data.size === 0) return data;
      const ret = new Promise<Map<string, DataModel>>((resolve, reject) => {
        im.getJoinedGroups({
          onResult: (res) => {
            if (res.isOk && res.value) {
              const finalGroups = res.value.map<DataModel>((v) => {
                // !!! Not recommended: only for demo
                const g = v as ChatGroup;
                const avatar = g.options?.ext?.includes("http")
                  ? g.options.ext
                  : undefined;
                return {
                  id: v.groupId,
                  name: v.groupName,
                  avatar: v.groupAvatar ?? avatar,
                  type: "group",
                } as DataModel;
              });
              resolve(DataProfileProvider.toMap(finalGroups));
            } else {
              reject(data);
            }
          },
        });
      });
      return ret;
    },
    [im]
  );

  return (
    <Container
      options={{ appKey: "appKey" }}
      onGroupsHandler={onGroupsHandler}
      onUsersHandler={onUsersHandler}
    >
      {/* Add child components */}
    </Container>
  );
}
```

## Actively update data

Data is updated by using the `ChatService.updateDataList` interface.

For example:

```javascript
// Suppose the group name is updated
export function SomeComponent() {
  const im = useChatContext();
  const updatedRef = React.useRef<boolean>(false);
  const updateData = React.useCallback(() => {
    if (updatedRef.current) {
      return;
    }
    updatedRef.current = true;
    im.getJoinedGroups({
      onResult: (r) => {
        if (r.value) {
          const groups: DataModel[] = [];
          r.value.forEach((conv) => {
            const avatar =
              conv.options?.ext && conv.options?.ext.includes("http")
                ? conv.options.ext
                : undefined;
            groups.push({
              id: conv.groupId,
              type: "group",
              name: conv.groupName,
              avatar: avatar,
            });
          });
          im.updateDataList({
            dataList: DataProfileProvider.toMap(groups),
            isUpdateNotExisted: true,
            dispatchHandler: (data) => {
              const items = [];
              for (const d of data) {
                const item = {
                  groupId: d[0],
                  groupName: d[1].name,
                  groupAvatar: d[1].avatar,
                } as GroupModel;
                items.push(item);
              }
              im.sendUIEvent(UIListenerType.Group, "onUpdatedListEvent", items);
              return false;
            },
          });
        }
      },
    });
  }, [im]);

  React.useEffect(() => {
    const listener: EventServiceListener = {
      onFinished: (params) => {
        if (params.event === "getAllConversations") {
          timeoutTask(500, updateData);
        }
      },
    };
    im.addListener(listener);
    return () => {
      im.removeListener(listener);
    };
  }, [im, updateData]);

  return <View>{/* Add child components */}</View>;
}
```

There is also a public `ChatService.getDataModel` interface, through which you can obtain the avatar and nickname of a specified user. It is convenient for usage in custom components.

## Conversation list page and group list page

Both components have list items of the group type, and groups have names. Therefore, users need to pay attention to notifications of group name changes to ensure data consistency.

## Chat page

In addition to using `DataProfileProvider` to obtain avatars and nicknames, the chat page can also carry avatars and nicknames through message extension fields, and the avatars and nicknames in the message are used first.

## Avatar and nickname usage rules

- If provided in `DataProfileProvider`, use the provided data. If not, use the default avatar and ID for a user and the default avatar and name for a group.
- For a chat page, use the avatar and nickname carried in the message first, otherwise use `DataProfileProvider`.