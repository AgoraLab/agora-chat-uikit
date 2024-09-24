# Best practices

## Initialize UIKit

Initialization is a necessary step and must be completed before calling any interface methods.

```dart
void main() async {
  assert(appKey.isNotEmpty, 'appKey is empty');
  await ChatroomUIKitClient.instance.initWithAppkey(
    appKey,
  );
  runApp(const MyApp());
}
```

## Log in to UIKit

You can log in by using the user object in the project and complying with the `UserInfoProtocol` protocol. UIKIt provides a default `UserEntity` implementation, which you can inherit if you need to add your own properties.

   - Log in with `userId` and `password`:

     ```dart
         // ...
            //Set the current user's avatar and nickname
             UserEntity user = UserEntity(userId, nickname: nickname, avatarURL: avatarURL);
            try {
              // Log in with password
              await ChatroomUIKitClient.instance.loginWithPassword(
                userId: userId!,
                password: password,
                userInfo: user,
              );
            
            } on ChatError catch (e) {
              // error.
            }
         ```

   - Log in with `userId` and `token`:
  
     ```dart
         //Set the current user's avatar and nickname
          UserEntity user = UserEntity(userId, nickname: nickname, avatarURL: avatarURL);
         try {
           await ChatroomUIKitClient.instance.loginWithToken(
             userId: userId,
             token: token!,
             userInfo: user,
           );
         
         } on ChatError catch (e) {
           // error.
         }
         ```

## Initialize the chat room view

1. Configure the theme to ensure that the chat room can be loaded into the theme as required.

    ```dart
    @override
      Widget build(BuildContext context) {
        return MaterialApp(
          builder: (context, child) {
            return ChatUIKitTheme(child: child!);
          },
          home: const MyHomePage(title: 'Flutter Demo Home Page'),
          ...
        );
      }
    ```


1. Get the chat room list and join the specified chat room.

1. Create `ChatroomController` and make `ChatRoomUIKit` the root node of the current page. Make other components child nodes of `ChatRoomUIKit`.

    ```dart
    // roomId: ID of the chat room that needs to be joined.
    // ownerId: User ID of the chat room owner.
    ChatroomController controller = ChatroomController(roomId: roomId, ownerId: ownerId);
    
    @override
    Widget build(BuildContext context) {
      Widget content = Scaffold(
        resizeToAvoidBottomInset: false,
        appBar: AppBar(),
        body: ChatRoomUIKit(
          controller: controller,
          child: (context) {
            // Build pages in sub-components, such as gift pop-ups, message lists, etc.
            return ...;
          },
        ),
      );
    
      return content;
    }
    ```

## Listen to ChatroomUIKit events and errors

You can set a `ChatroomEventListener` for chat room events during `ChatroomController` initialization, and events related to the current chat room will be returned through this callback.

```dart
controller = ChatroomController(
  listener: ChatroomEventListener((type, error) {
    // catch errors.
  }),
  roomId: widget.room.id,
  ownerId: widget.room.ownerId,
);
```

If you need to listen to global events, you can use `ChatroomResponse` and `ChatroomEventResponse`.

```dart

class _ChatroomViewState extends State<ChatroomView>
    with ChatroomResponse, ChatroomEventResponse {
  @override
  void initState() {
    super.initState();

    ChatroomUIKitClient.instance.roomService.bindResponse(this);
    ChatroomUIKitClient.instance.bindRoomEventResponse(this);
  }

  @override
  void dispose() {
    ChatroomUIKitClient.instance.roomService.unbindResponse(this);
    ChatroomUIKitClient.instance.unbindRoomEventResponse(this);
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return const Placeholder();
  }
}
```

See more best practices [here](https://github.com/easemob/ChatroomDemo/tree/dev/flutter/chatroom_uikit_demo). 