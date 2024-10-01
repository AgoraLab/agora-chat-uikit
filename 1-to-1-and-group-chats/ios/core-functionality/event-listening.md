# Overridable methods in the ViewModel of the main page

The callback event monitoring and the monitoring of UI events are both in their respective ViewModels.

You can use the original logic or add your own extensions and implementations to it.

## Conversation list

Inherit `ConversationViewModel` and register it in `ComponentsRegister.shared.ConversationViewService`, then override the event listeners you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `loadExistLocalDataIfEmptyFetchServer` | Called when an error occurs while pulling the conversation list. This method will retrieve the conversation list again. | Yes |
| `pin` | Triggered when swiping left on the conversation list and clicking the top button. | Yes |
| `unpin` | Triggered when swiping left on the conversation list and clicking the **Unpin** button. | Yes |
| `mute` | Triggered when swiping left on the conversation list and clicking the **Mute** button. | Yes |
| `unmute` | Triggered when swiping left on the conversation list and clicking the **Unmute** button. | Yes |
| `delete` | Triggered when swiping left on the conversation list and clicking the **Delete** button. | Yes |
| `read` | Triggered when swiping left on the conversation list and clicking the **Read** button. | Yes |
| `conversationDidSelected` | Triggered when clicking a conversation list. | Yes |
| `conversationLongPressed` | Triggered when long-pressing a conversatin list. | Yes |
| `moreAction` | Trigerred when swiping right on the conversation list and clicking **...**. | Yes |
| `conversationLastMessageUpdate` | Trigerred when the last message of a conversation in the list is updated. | Yes |
| `playNewMessageSound` | Play audio for receiving a new message. | Yes |
| `conversationMessageAlreadyReadOnOtherDevice` | Trigerred when messages in a conversation have been read on other devices. | Yes |
| `conversationEventDidChanged` | Trigerred when the multi-device operation time of a conversation changes. | Yes |
| `mapper` | Maps the `ConversationInfo` object methods. | Yes |


## Message list

Inherit `MessageListViewModel` and register it in `ComponentsRegister.shared.MessagesViewModel`, then override the event listeners you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `messageDidReceived` | Triggered when a new message is received. | Yes |
| `messageDidRecalled` | Triggered when a message is recalled. | Yes |
| `onMessageDidEdited` | Triggered when a message is edited. | Yes |
| `messageStatusChanged` | Triggered when the status of a received message changes. | Yes |
| `messageAttachmentStatusChanged` | Triggered when the status of a message attachment changes. | Yes |

For details on the callbacks of UI events, see [Intercept the main page click jump event](intercept-main-page-click-jump-event.md).

## Contact list

Inherit `ContactViewModel` and register in `ComponentsRegister.shared.ContactViewService`, then override the event listeners you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `processFriendDidAgree` | Triggered when a contact has accepted the friend request. | Yes |
| `processFriendRequestDidDecline` | Triggered when a contact has rejected the friend request. | Yes |
| `processFriendshipDidRemove` | Triggered when a friend was removed. | Yes |
| `processFriendshipDidAddSuccessful` | Triggered when a friend was successfully added. | Yes |
| `processFriendRequestDidReceive` | Triggered by receiving a friend request. | Yes |
| `contactEventDidChanged` | Triggered when a multi-device event changes for a contact. | Yes |