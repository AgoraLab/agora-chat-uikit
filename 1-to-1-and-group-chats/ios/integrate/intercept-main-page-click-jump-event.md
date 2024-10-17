# Intercept the main page click jump event

This page explains how to intercept click jump events of the conversation list, chat page, and contact page.

You can use the original logic or add your own extensions and implementations to it.

## Conversation list

Inherit `ConversationListController` and register it in `ComponentsRegister.shared.ConversationsController`, then override the methods you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `createNavigationBar` | Create a navigation bar. | Yes |
| `createSearchBar` | Create a search box. | Yes |
| `createList` | Create a conversation list. | Yes |
| `navigationClick` | The navigation click. | Yes |
| `pop` | Return to the previous page.  | Yes |
| `toChat` | Jump to chat.  | Yes |
| `searchAction` | Сlick the search box. | Yes |
| `rightActions` | Click the button on the right side of the navigation. | Yes |
| `selectContact` | Jump to the contact selection page. | Yes |
| `chatToContact` | Jump to the chat page to specify the contact. | Yes |
| `createChat` | Create a corresponding type of the conversation to start chatting. | Yes |
| `addContact` | Call the add contact pop-up. | Yes |
| `createGroup` | Create a group and jump to the page for selecting group members. | Yes |
| `create` | Create a group. | Yes |

## Chat page

Inherit `MessageListController` and register it in `ComponentsRegister.shared.MessageViewController`, then override the methods you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `createNavigation` | Create a navigation bar. | Yes |
| `createLoading` | Create a loading page. | Yes |
| `navigationClick` | All click methods of the navigation bar. | Yes |
| `viewDetail` | View a contact or group details page. | Yes |
| `rightItemsAction` | Click the button on the right side of the navigation. | Yes |
| `pop` |  Return to the previous page.  | Yes |
| `messageWillSendFillExtensionInfo` | Add extended information before sending a message. | Yes |
| `filterMessageActions` | Filter pop-up menu items after long-pressing. | Yes |
| `showMessageLongPressedDialog` | Display the menu after long-pressing a message. | Yes |
| `processMessage` | Process the pop-up window click event after long-pressing a message. | Yes |
| `editAction` | Display the message edit window when a click `Edit` of the message long-pressing menu.| Yes |
| `reportAction` | Display the message edit window when a user long-press a message and click `Edit`. | Yes |
| `messageAttachmentLoading` | Display the loading page after clicking on pictures, videos, and attachments. | Yes |
| `messageBubbleClicked` | Message bubble click.  | Yes |
| `viewContact` | View contacts.  | Yes |
| `messageAvatarClick` | Message avatar click. | Yes |
| `audioDialog` | Show the recording audio pop-up. |  |
| `mentionAction` | Enter the `@` symbol in the input box in the group chat.  | Yes |
| `attachmentDialog` | Display a pop-up window for sending pictures, videos, and file messages. | Yes |
| `selectFile` | Select a file. | Yes |
| `selectPhoto` | Open the album and select an image. | Yes |
| `openCamera` | Open the camera to take a video or photo. | Yes |
| `selectContact` | Select a contact to send a card. | Yes |
| `openFile` | Open the selected file. | Yes |
| `processImagePicker` | Click to select images and videos to send. | Yes |
| `documentPickerOpenFile` | Open the file selector. | Yes |

## Contact page

Inherit `ContactViewController` and register it in `ComponentsRegister.shared.ContactsController`, then override the methods you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `createNavigation` | Create a navigation bar.  | Yes |
| `navigationClick` | All click methods of the navigation bar. | Yes |
| `viewContact` | View contact details.  | Yes |
| `rightItemsAction` | Click the button on the right side of the navigation. | Yes |
| `pop` | Return to the previous page. | Yes |
| `setupTitle` | Set navigation titles for different types of contact pages. | Yes |
| `receiveContactHeaderAction` | The header cell click. | Yes |
| `searchAction` | Click the search box. | Yes |
| `addContact` | Add a contact.  | Yes |
| `confirmAction` | Confirm action.  | Yes |
| `viewNewFriendRequest` | View the new friend requests page. | Yes |
| `viewJoinedGroups` | View the list of joined groups. | Yes |

