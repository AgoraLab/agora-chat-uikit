# Configurable items

The `chatroom_uikit` component contains various properties that you can set according to your needs.

The main components include:

| Component Name | Component Introduction |
|:---:|:---:|
| `ChatroomLocal` | The language of all UI components can be set here. |
| `ChatRoomUIKit` | Provides the basic chat room page. |
| `ChatroomMessageListView` | The message area component, used to display sent or received messages. |
| `ChatroomParticipantsListView` | The member list component, including the management of chat room members and muted members. |
| `ChatroomGiftMessageListView` | The gift message area component, used to display gifts. |
| `ChatroomGlobalBroadcastView` | The global broadcast component, used to send messages to all chat rooms under the app. |
| `ChatInputBar` | The toolbar area at the bottom of the chat room. Can be switched with the message input component and supports adding custom buttons. |
| `ChatRoomGiftListView` | The send gift component, used to send gifts. The source of the gift is specified by the developer. |
| `ChatroomReportListView` | The chat room reporting component. |

## ChatroomLocal

UIKit supports multi-language switching. Currently, English is built-in and other languages can be expanded.

For example, if you want UIKit to display in English, you can set it as follows:

```dart
final FlutterLocalization _localization = FlutterLocalization.instance;
_localization.init(mapLocales: [
  const MapLocale('zh', ChatroomLocal.zh),
  const MapLocale('en', ChatroomLocal.en),
], initLanguageCode: 'zh');

...

return MaterialApp(
      title: 'Flutter Demo',
      supportedLocales: _localization.supportedLocales,
      localizationsDelegates: _localization.localizationsDelegates,
      ...
);
```

## ChatRoomUIKit 

ChatRoomUIKit is a chat room component that can be used to quickly create a chat room page. It provides customization of the chat room event monitoring and input components.

The sample code is as follows:

```dart
ChatroomController controller =
        ChatroomController(roomId: widget.roomId, ownerId: widget.ownerId);
...

@override
Widget build(BuildContext context) {
  return ChatRoomUIKit(
          controller: controller,
          inputBar: ChatInputBar( // Customize the input style
            actions: [
              InkWell(
                onTap: () => controller.showGiftSelectPages(),
                child: Padding(
                  padding: const EdgeInsets.all(3),
                  child: Image.asset('images/send_gift.png'),
                ),
              ),
            ],
          ),
          child: (context) {
            // Return to your widget
            return Container();
          },
        );
}
```

## ChatroomMessageListView

The chat room message area component `ChatroomMessageListView` provides message display. Text messages, emoji messages, gift messages, and successfully sent messages received in the chat room will be displayed in this area.

Users can long-press a message to perform actions on it, for example, translate, recall, and report. The reporting component supports custom options.

The sample code is as follows:

```dart
ChatroomController controller =
        ChatroomController(roomId: widget.roomId, ownerId: widget.ownerId);
...

@override
Widget build(BuildContext context) {
  return ChatRoomUIKit(
          controller: controller,
          inputBar: ChatInputBar( // Customize the input style 
            actions: [
              InkWell(
                onTap: () => controller.showGiftSelectPages(),
                child: Padding(
                  padding: const EdgeInsets.all(3),
                  child: Image.asset('images/send_gift.png'),
                ),
              ),
            ],
          ),
          child: (context) {
            return Stack(
              children: [
                const Positioned(
                  left: 16,
                  right: 78,
                  height: 204,
                  bottom: 90,
                  child: ChatroomMessageListView(), // Add the message list and set location
                )
              ],
            );
          },
        );
}
```

`ChatroomMessageListView` provides the following properties:

| Property | Required/Optional | Description |
|:---:|:---:|:---:|
| `onTap` | Optional | The message list click event. |
| `onLongPress` | Optional | The message list long-press event. |
| `itemBuilder` | Optional | The message list item builder, can be used to customize the display style. |
| `reportController` | Optional | `ChatReportController`, used to customize the report content. Used by the default `DefaultReportController`. |
| `controller` | Optional | `ChatroomMessageListController`, used to configure the default long-press event. Used by the default `DefaultMessageListController`. |

## ChatroomParticipantsListView 

The chat room members component can display and manage chat room members, chat room owners, ban lists, and administrative permissions.

Chat room owners can modify member status, for example, muting or kicking a member out of the chat room. 

This component does not support customization.

## ChatroomGiftMessageListView

The `ChatroomGiftMessageListView` component is used to display the effects of sent gifts. Gift messages can be displayed in the message list or in this component to display gifts separately.

The sample code is as follows:

```dart
ChatroomController controller =
        ChatroomController(roomId: widget.roomId, ownerId: widget.ownerId);
...

@override
Widget build(BuildContext context) {
  return ChatRoomUIKit(
          controller: controller,
          inputBar: ChatInputBar( // Customize the input style
            actions: [
              InkWell(
                onTap: () => controller.showGiftSelectPages(),
                child: Padding(
                  padding: const EdgeInsets.all(3),
                  child: Image.asset('images/send_gift.png'),
                ),
              ),
            ],
          ),
          child: (context) {
            return Stack(
              children: [
              const Positioned(
                left: 16,
                right: 100,
                height: 84,
                bottom: 300,
                child: ChatroomGiftMessageListView(), // Set up gift display area
              ),
                const Positioned(
                  left: 16,
                  right: 78,
                  height: 204,
                  bottom: 90,
                  child: ChatroomMessageListView(), // Add message list and set location
                )
              ],
            );
          },
        );
}
```

`ChatroomGiftMessageListView` provides the following properties:

| Property | Required/Optional | Description |
|:---:|:---:|:---:|
| `giftWidgetBuilder` | Optional | Builder for a single gift display. |
| `placeholder` | Optional | The default image placeholder used as a placeholder before the gift image is downloaded successfully |

## ChatroomGlobalBroadcastView

The global broadcast notification component `ChatroomGlobalBroadcastView` receives and displays global broadcasts, as well as adding messages to the queue for display.

The sample code is as follows:

```dart
ChatroomController controller =
        ChatroomController(roomId: widget.roomId, ownerId: widget.ownerId);
...

@override
Widget build(BuildContext context) {
  return ChatRoomUIKit(
          controller: controller,
          inputBar: ChatInputBar( // Customize the input style
            actions: [
              InkWell(
                onTap: () => controller.showGiftSelectPages(),
                child: Padding(
                  padding: const EdgeInsets.all(3),
                  child: Image.asset('images/send_gift.png'),
                ),
              ),
            ],
          ),
          child: (context) {
            return Stack(
              children: [
              Positioned(
                top: MediaQuery.of(context).viewInsets.top + 10,
                height: 20,
                left: 20,
                right: 20,
                child: ChatroomGlobalBroadcastView(),  // Used to display broadcast messages
              ),
              const Positioned(
                left: 16,
                right: 100,
                height: 84,
                bottom: 300,
                child: ChatroomGiftMessageListView(), // Set up the gift display area
              ),
                const Positioned(
                  left: 16,
                  right: 78,
                  height: 204,
                  bottom: 90,
                  child: ChatroomMessageListView(), // Add the message list and set location
                )
              ],
            );
          },
        );
}
```

`ChatroomGlobalBroadcastView` provides the following properties:

| Property | Required/Optional | Description |
|:---:|:---:|:---:|
| `Icon` | Optional | Sets the icon for when a broadcast is received. |
| `textStyle` | Optional | Sets the broadcast content font. |
| `backgroundColor` | Optional | Sets the broadcast background color. |

## ChatInputBar

`ChatInputBar` can send messages such as text and emojis, and can be dynamically switched with the toolbar area at the bottom of the chat room. When a user clicks the toolbar area component at the bottom of the chat room, it switches to the input state. When a user sends a message or closes the input box, it switches to the toolbar area component at the bottom of the chat room.

The sample code is as follows:

```dart
 Widget build(BuildContext context) {
    Widget content = ChatRoomUIKit(
      controller: controller,
      inputBar: ChatInputBar( // Add a gift button
        actions: [
          InkWell(
            onTap: () => controller.showGiftSelectPages(),
            child: Padding(
              padding: const EdgeInsets.all(3),
              child: Image.asset('assets/images/send_gift.png'),
            ),
          ),
        ],
      ),
    );
 }
```

`ChatInputBar` provides the following properties:

| Property | Required/Optional | Description |
|:---:|:---:|:---:|
| `inputIcon` | no | The display icons. |
| `inputHint` | no | The placeholder character when no input is given. |
| `leading` | no | The left input area widget. |
| `actions` | no | The widget array for the right input area. A maximum of 3 widgets is allowed. |
| `textDirection` | no | The direction of the input area. |
| `onSend` | no | The callback for clicking the **Send** button. |

## ChatRoomGiftListView

The gift list component `ChatRoomGiftListView` provides a custom gift list. The content can be filled in through the `ChatroomController#giftControllers` configuration.

The gift list component is an independent component that requires application developers to implement operations such as display and loading by themselves.

The sample code is as follows:

```dart
ChatroomController controller = ChatroomController(
      roomId: widget.room.id,
      ownerId: widget.room.ownerId,
      giftControllers: () async {
        List<DefaultGiftPageController> service = [];
        final value = await rootBundle.loadString('assets/data/Gifts_cn.json'); // Parse JSON and assign it to giftControllers.
        Map<String, dynamic> map = json.decode(value);
        for (var element in map.keys.toList()) {
          service.add(
            DefaultGiftPageController(
              title: element,
              gifts: () {
                List<GiftEntityProtocol> list = [];
                map[element].forEach((element) {
                  GiftEntityProtocol? gift = ChatroomUIKitClient
                      .instance.giftService
                      .giftFromJson(element);
                  if (gift != null) {
                    list.add(gift);
                  }
                });
                return list;
              }(),
            ),
          );
        }
        return service;
      }(),
    );
```


## ChatroomReportListView

The message reporting component `ChatroomReportListView` does not provide a custom part in the default interface, but the content can be modified through configuration.

The sample code is as follows:

```dart
// Modify the report content:
ChatRoomSettings.reportMap = {'tag': 'reason'};
```

