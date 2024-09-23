# Run a sample project

Agora provides an open-source sample project, which demonstrates the use of UIKit for building a chat room and implementing the logic. This page explains how to compile and run it. 

## Prerequisites

Before you start, make sure your development environment meets the following requirements:

- Xcode 14.0 or above;
- iOS 13.0 or above;
- A project with a valid developer signature;
- A valid Agora account;
- An app key and a user token generated on your app server. See [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication).


## Implementation

Take the following steps to compile and run the sample project:

1. Download [the sample code](https://github.com/easemob/UIKit_Chatroom_ios).

1. Execute the pod command:

    1. Click to open the `UIKit_Chatroom_ios` folder.
    1. Drag the **Example** folder to the terminal.
    1. Run the following command:

    ```
    pod install --repo-update
    ```
   
1. Compile the project

   Before compiling, you need to create a chat room.

   1. Open the `.xcworkspace` project file in Xcode.
   1. Press **cmd + B** on the keyboard to compile. The compilation will fail with an error.
   1. Fill in your app key, user token, and user ID into the `appKey`, `chatToken`, and `userId` fields, respectively.
   1. Press **cmd + R** on the keyboard, select the device to run the project, and view the results.
      
1. Test the project

  - To test only the UI, click **UIComponent**. In this case, the `appKey`, `chatToken`, and `userId` fields in the third step of the compilation process can be filled with empty strings. During the test, you can choose whether to display certain configuration items of the UI.
  - For UI and business experiences (for example, sending gifts, chat room member management, or message operations), click **UI&Business**. In this case, you need to pass the `appKey`, `chatToken`, and `userId` fields.

Note that the sample project is only used to quickly launch the chat room. Multi-member interaction testing is currently not available.

## Compilation FAQ

If the following error appears when compiling with Xcode 15: `Sandbox: rsync.samba(47334) deny(1) file-write-create...` , you can search for `ENABLE_USER_SCRIPT_SANDBOXING` in **Build Settings** and change the **User Script Sandboxing** setting to **NO**.