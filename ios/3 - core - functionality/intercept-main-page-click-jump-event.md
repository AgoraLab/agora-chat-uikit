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
| `searchAction` | How to click the search box | Yes |
| `rightActions` | How to click the button on the right side of the navigation | Yes |
| `selectContact` | How to jump to the contact selection page | Yes |
| `chatToContact` | Jump to the chat page to specify the contact chat method | Yes |
| `createChat` | Create a corresponding type of session according to the type to start the chat method | Yes |
| `addContact` | Method to call up the add contact pop-up window | Yes |
| `createGroup` | How to create a group and jump to the page for selecting group members | Yes |
| `create` | How to create a group | Yes |

## Chat page

You can inherit `MessageListController` and register it in `ComponentsRegister.shared.MessageViewController`, then override the methods you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `createNavigation` | Creating a navigation bar method | Yes |
| `createLoading` | Create a Loading page method | Yes |
| `navigationClick` | All click methods of the navigation bar | Yes |
| `viewDetail` | View a contact or group details page | Yes |
| `rightItemsAction` | How to click the button on the right side of the navigation | Yes |
| `pop` | yes | Page return to the previous level method |
| `messageWillSendFillExtensionInfo` | Method to add extended information before sending a message | Yes |
| `filterMessageActions` | Method for filtering menu items on the pop-up menu after long pressing | Yes |
| `showMessageLongPressedDialog` | Display the menu after long pressing the message | Yes |
| `processMessage` | Process the pop-up window click event after long pressing the message | Yes |
| `editAction` | Click on the message and long press it, then the edit pop-up window will pop up. | Yes |
| `reportAction` | How to click and hold the report button in the menu to pop up the report pop-up window | Yes |
| `messageAttachmentLoading` | Whether to display the loading page after clicking on pictures, videos and attachments | Yes |
| `messageBubbleClicked` | Message bubble click method | Yes |
| `viewContact` | View Contacts Page | Yes |
| `messageAvatarClick` | Message avatar click | Yes |
| `audioDialog` | Show recording audio pop-up |  |
| `mentionAction` | Entering the @ symbol in the input box in the group chat triggers an event | Yes |
| `attachmentDialog` | Display a pop-up window for sending pictures, videos, and file messages | Yes |
| `selectFile` | Select File | Yes |
| `selectPhoto` | Open the album and select a photo | Yes |
| `openCamera` | Open the camera to take video or photo | Yes |
| `selectContact` | Select a contact to send a card | Yes |
| `openFile` | Open Select File | Yes |
| `processImagePicker是` | Processing click to select pictures and videos to send messages | Yes |
| `documentPickerOpenFile` | How to open the file chooser | Yes |

## Contact page

You can inherit `ContactViewController` and register it in `ComponentsRegister.shared.ContactsController`, then override the methods you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `createNavigation` | Creating a navigation bar method | Yes |
| `navigationClick` | All click methods of the navigation bar | Yes |
| `viewContact` | View Contact Details Page | Yes |
| `rightItemsAction` | How to click the button on the right side of the navigation | Yes |
| `pop` | Page return to the previous level method | Yes |
| `setupTitle` | Set navigation titles for different types of contact pages | Yes |
| `receiveContactHeaderAction` |  | Yes |
| `searchAction` | Click the search box | Yes |
| `addContact` | Add contact pop-up window | Yes |
| `confirmAction` | Navigation right text button click event | Yes |
| `viewNewFriendRequest` | View new friend requests page | Yes |
| `viewJoinedGroups` | View the group list page you have joined | Yes |

