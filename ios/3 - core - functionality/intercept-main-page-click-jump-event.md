This article introduces how to intercept click jump events of the conversation list, chat page, and contact page.

You can use the original logic or add your own extensions and implementations to it.

## Conversation list

You can inherit `ConversationListController` and register it in `ComponentsRegister.shared.ConversationsController`, then override the methods you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `createNavigationBar` | Create a navigation bar | Yes |
| `createSearchBar` | Create a search box | Yes |
| `createList` | Create a conversation list | Yes |
| `navigationClick` | Navigation click | Yes |
| `pop` | Page return  | Yes |
| `toChat` | Jump to chat  | Yes |
| `searchAction` | Сlick the search box | Yes |
| `rightActions` | Click the button on the right side of the navigation | Yes |
| `selectContact` | Jump to the contact selection page | Yes |
| `chatToContact` | Jump to the chat page to specify the contact | Yes |
| `createChat` | Create a corresponding type of conversation to start chatting | Yes |
| `addContact` | Call up the add contact pop-up | Yes |
| `createGroup` | Create a group and jump to the page for selecting group members | Yes |
| `create` | Create a group | Yes |

## Chat page

You can inherit `MessageListController` and register it in `ComponentsRegister.shared.MessageViewController`, then override the methods you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `createNavigation` | Create a navigation bar | Yes |
| `createLoading` | Create a loading page | Yes |
| `navigationClick` | All click methods of the navigation bar | Yes |
| `viewDetail` | View a contact or group details page | Yes |
| `rightItemsAction` | Click the button on the right side of the navigation | Yes |
| `pop` |  Return to the previous page  | Yes |
| `messageWillSendFillExtensionInfo` | Add extended information before sending a message | Yes |
| `filterMessageActions` | Filter pop-up menu items after long-pressing | Yes |
| `showMessageLongPressedDialog` | Display the menu after long-pressing a message | Yes |
| `processMessage` | Process the pop-up window click event after long-pressing a message | Yes |
| `editAction` | Click on the message and long-press it. The edit window will pop up. | Yes |
| `reportAction` | Click and hold the report button in the menu to call the report window | Yes |
| `messageAttachmentLoading` | Display the loading page after clicking on pictures, videos, and attachments | Yes |
| `messageBubbleClicked` | Message bubble click  | Yes |
| `viewContact` | View contacts  | Yes |
| `messageAvatarClick` | Message avatar click | Yes |
| `audioDialog` | Show the recording audio pop-up |  |
| `mentionAction` | Entering the @ symbol in the input box in the chat group  | Yes |
| `attachmentDialog` | Display a pop-up window for sending pictures, videos, and file messages | Yes |
| `selectFile` | Select a file | Yes |
| `selectPhoto` | Open the album and select a photo | Yes |
| `openCamera` | Open the camera to take video or photo | Yes |
| `selectContact` | Select a contact to send a card | Yes |
| `openFile` | Open the selected file | Yes |
| `processImagePicker是` | Click to select pictures and videos to send | Yes |
| `documentPickerOpenFile` | Open the file selector | Yes |

## Contact page

You can inherit `ContactViewController` and register it in `ComponentsRegister.shared.ContactsController`, then override the methods you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `createNavigation` | Create a navigation bar  | Yes |
| `navigationClick` | All click methods of the navigation bar | Yes |
| `viewContact` | View contact details  | Yes |
| `rightItemsAction` | Click the button on the right side of the navigation | Yes |
| `pop` | Return to the previous page | Yes |
| `setupTitle` | Set navigation titles for different types of contact pages | Yes |
| `receiveContactHeaderAction` |  | Yes |
| `searchAction` | Click the search box | Yes |
| `addContact` | Add a contact  | Yes |
| `confirmAction` | Confirm action  | Yes |
| `viewNewFriendRequest` | View the new friend requests page | Yes |
| `viewJoinedGroups` | View the group list page you have joined | Yes |

