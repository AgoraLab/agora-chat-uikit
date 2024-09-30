# Overridable methods in the ViewModel of the main page

The callback event monitoring and the monitoring of UI trigger events are both in their respective ViewModels.

You can use the original logic or add your own extensions and implementations to it.

## Conversation list

You can inherit `ConversationViewModel` and register it in `ComponentsRegister.shared.ConversationViewService`, then override the event listeners you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `loadExistLocalDataIfEmptyFetchServer` | Called when an error occurs while pulling the conversation list. This method will retrieve the conversation list again. | Yes |
| `pin` | Triggered after swiping left on the conversation list and clicking the top button. | Yes |
| `unpin` | Triggered after swiping left on the conversation list and clicking the **Unpin** button. | Yes |
| `mute` | Triggered after swiping left on the conversation list and clicking the **Mute** button. | Yes |
| `unmute` | Triggered after swiping left on the conversation list and clicking the **Unmute** button. | Yes |
| `delete` | Triggered after swiping left on the conversation list and clicking the **Delete** button. | Yes |
| `read` | Triggered after swiping left on the conversation list and clicking the **Read** button. | Yes |
| `conversationDidSelected` | Triggered when a conversation list is clicked. | Yes |
| `conversationLongPressed` | Triggered when a conversatin list is long-pressed. | Yes |
| `moreAction` | Trigerred on swiping right on the conversation list and tapping **...**. | Yes |
| `conversationLastMessageUpdate` | Trigerred when the last message of a conversation in the list is updated. | Yes |
| `playNewMessageSound` | Play audio method when a new message is received. | Yes |
| `conversationMessageAlreadyReadOnOtherDevice` | Called when messages in a conversation have been read on other devices. | Yes |
| `conversationEventDidChanged` | Called when the multi-device operation time of a conversation changes. | Yes |
| `mapper` | Map `ConversationInfoobject` methods. | Yes |


## Message list

You can inherit `MessageListViewModel` and register it in `ComponentsRegister.shared.MessagesViewModel`, then override the event listeners you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `messageDidReceived` | Triggered when a new message is received. | Yes |
| `messageDidRecalled` | Triggered when a weceived message is recalled. | Yes |
| `onMessageDidEdited` | Triggered when a message is edited. | Yes |
| `messageStatusChanged` | Triggered when the status of a received message changes. | Yes |
| `messageAttachmentStatusChanged` | Triggered when the status of a message attachment changes. | Yes |

For details on the callback of UI events, see [Intercept the main page click jump event](intercept-main-page-click-jump-event.md).

## Contact list

You can inherit `ContactViewModel` and register in `ComponentsRegister.shared.ContactViewService`, then override the event listeners you want to intercept:

| Method name | Usage | Overridable? (Yes/No) |
|:---:|:---:|:---:|
| `processFriendDidAgree` | Triggered when the contact has agreed to add a contact. | yes |
| `processFriendRequestDidDecline` | Triggered when a contact was rejected. | yes |
| `processFriendshipDidRemove` | Triggered when a friend was removed. | yes |
| `processFriendshipDidAddSuccessful` | Triggered when a friend was successfully added. | yes |
| `processFriendRequestDidReceive` | Triggered by a friend request. | yes |
| `contactEventDidChanged` | Triggered when a multi-device event changes for a contact. | yes |