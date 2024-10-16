# User information

UIKit provides a `Provider` component for data management. It does not render any UI and is only used to provide global context for other components. `Provider` automatically listens to SDK events and passes data down in the component tree to drive updates. Other components in UIKit must be wrapped with `UIKitProvider`.

## Usage examples

```javascript
import React from 'react';
import { UIKitProvider } from 'agora-chat-uikit';
import 'agora-chat-uikit/style.css';
import ChatApp from './ChatApp'
ReactDOM.createRoot(document.getElementById('root') as Element).render(
  <div>
    <UIKitProvider
      initConfig={{
        appKey: 'your app key',
        userId: 'userId',
        token: 'token',
        translationTargetLanguage: 'es', // Translation target language
        useUserInfo: true, // Whether to use the user attribute feature to display the avatar nickname (UIKit will obtain user attributes internally and must be set by the user)
      }}
      local={{
        fallbackLng: 'en',
        lng: 'en',
        resources: {
          en: {
            translation: {
              'conversationTitle': 'Conversation List',
              'deleteCvs': 'Delete Conversation',
              // ...
            },
          },
        },
      }}
      theme={{
        primaryColor: '#33ffaa',
        mode: 'light',
        componentsShape: 'square'
      }}
      reactionConfig={{
        map: {
            'emoji_1': <img src={'customIcon'} alt={'emoji_1'} />,
            'emoji_2': <img src={'customIcon'} alt={'emoji_2'} />,
        }
      }}

      features={{
        conversationList: {
          // search: false,
          item: {
            moreAction: false,
            deleteConversation: false,
          },
        },
        chat: {
          header: {
            threadList: true,
            moreAction: true,
            clearMessage: true,
            deleteConversation: false,
            audioCall: false,
          },
          message: {
            status: false,
            reaction: true,
            thread: true,
            recall: true,
            translate: false,
            edit: false,
          },
          messageEditor: {
            mention: false,
            typing: false,
            record: true,
            emoji: false,
            moreAction: true,
            picture: true,
          },
        },
      }}
    >
      <ChatApp></ChatApp>
    </UIKitProvider>
  </div>,
);
```

## Avatar and nickname

### Contact

When UIKit is used directly without any settings, the user ID is displayed by default, and the avatar defaults to the first two letters of the user ID. UIKit provides two ways to set the user's avatar and nickname:

- If the user's avatar and nickname are stored on Agora server, UIKit uses the user attribute function to obtain the avatar and nickname by default. When a user logs in for the first time, they can call the SDK API to set their own avatar and nickname.

    The sample code is as follows:

    ```javascript
    rootStore.client.updateUserInfo({
      nickname: 'nickname',
      avatarurl: 'https://example.com/image',
    });
    ```
  
    For the contact list, conversation list, conversation detail, group members, and other locations, UIKit automatically obtains other users' personal information to display avatars and nicknames.

- If the user's avatar and nickname are not stored on the Agora server, configure not to use the user attribute function during initialization:

    ```javascript
    // ...
    
    <UIKitProvider initConfig={{
        useUserInfo: false // Turn off automated use of user attributes
    }}>
        <ChatApp>
    </UIKitProvider>
    ```
    Then listen to the contact data, get each user's avatar and nickname, and set them in UIKit. The sample code is as follows:

    ```javascript
    import { useEffect } from 'react';
    
    useEffect(() => {
      if (rootStore.loginState) {
        // Filter out user IDs that have no user information.
        const userIds = rootStore.addressStore.contacts
          .filter(item => !rootStore.addressStore.appUsersInfo[item.id])
          .map(item => {
            return item.id;
          });
        // Get user avatar nickname from your own server
        getUserInfo(userIds).then(usersInfo => {
          //usersInfo: {[userId]: {avatarurl: '', nickname: '', userId: ''}}
          rootStore.addressStore.setAppUserInfo(usersInfo);
        });
      }
    }, [rootStore.loginState, rootStore.addressStore.contacts.length]);
    ```
  
In a group conversation, when UIKit sends a message internally, it will carry the sender's avatar and nickname in the message extension. The members who receive the message will display the avatar and nickname according to the information in the message, and UIKit will also store this information in `appUsersInfo`. When you need to view the list of group members, UIKit will first obtain the personal information in `appUsersInfo`. You need to check which group members' information is not in `appUsersInfo`, obtain the personal information of these members, and set it in `appUsersInfo`.

### Group avatar

UIKit does not have a way to obtain group avatars. Users need to set them in UIKit. The format and settings are similar to personal information.

The sample code is as follows:

```javascript
useEffect(() => {
  if (rootStore.loginState) {
    const groupIds =
      rootStore.addressStore.groups
        .filter(item => !item.avatarUrl)
        .map(item => {
          return item.groupid;
        }) || [];
    // Get group avatar
    getGroupAvatar(groupIds).then(res => {
      // res: {[groupId]: 'avatarurl'}
      for (let groupId in res) {
        rootStore.addressStore.updateGroupAvatar(groupId, res[groupId]);
      }
    });
  }
}, [rootStore.loginState, rootStore.addressStore.groups.length]);
```

## UIKitProvider property overview

| Property         | Type                            | Description                                                                                                                                                                                                            |
|------------------|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `initConfig`     | ProviderProps['initConfig']     | Initialization parameters, such as setting `appKey`, `userId`, `token`, whether to use user attributes, and the target language for translation.                                                                                  |
| `local`          | ProviderProps['local']          | Internationalization configuration parameters, you can configure i18next parameters during initialization.                                                                                                             |
| `features`       | ProviderProps['features']       | Configure the features you need globally. UIKit displays all features by default. If the required features are also configured in the corresponding component, the configuration in the component takes precedence. |
| `reactionConfig` | ProviderProps['reactionConfig'] | Globally configure the emojis for the message emoji reply feature. If this parameter is also set in the message component, the setting in the message component will have higher priority.                                         |
| `theme`          | ProviderProps['theme']          | Theme-related configurations, such as the primary color, light and dark themes, and component rounded corners.                                                                                                             |