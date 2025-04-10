# User information

You need to provide an avatar and nickname when using UIKit. The `ChatUIKitProvider` class is provided to facilitate this.

`ChatUIKitProfile` encapsulates the avatar and nickname information:

```dart
/// Profile type, used to distinguish between contacts and groups
enum ChatUIKitProfileType {
    /// Contact type.
    contact,
    
    /// Group type.
    group,
}

/// ChatUIKitProfile class, used to store contact or group information.
class ChatUIKitProfile {
  /// ID: The user ID for a contact, the group ID for a group
  final String id;

  /// Name: The user name for a contact, the group name for a group
  final String? showName;

  /// Avatar address: The user avatar address for a contact, the group avatar address for the group
  final String? avatarUrl;

  /// The profile type is used to distinguish between contacts and groups. For details, see [ChatUIKitProfileType]
  final ChatUIKitProfileType type;

  /// Extension fields are used to store additional information
  final Map<String, String>? extension;

  /// Timestamp, not used internally by UIKit. You can use this field to store timestamp information.
  final int timestamp;

  final String? remark;

  /// The name to be displayed. If empty, ID is displayed.
  String get contactShowName {
    if (remark != null &&
        remark!.isNotEmpty &&
        type == ChatUIKitProfileType.contact) {
      return remark!;
    }

    if (showName != null && showName!.isNotEmpty) {
      return showName!;
    }
    return id;
  }

  String get nickname {
    if (showName != null && showName!.isNotEmpty) {
      return showName!;
    }
    return id;
  }

  ChatUIKitProfile({
    required this.id,
    required this.type,
    this.showName,
    this.avatarUrl,
    this.extension,
    this.remark,
    this.timestamp = 0,
  });

  ChatUIKitProfile.contact({
    required String id,
    String? groupName,
    String? avatarUrl,
    String? remark,
    Map<String, String>? extension,
    int timestamp = 0,
  }) : this(
          id: id,
          showName: nickname,
          avatarUrl: avatarUrl,
          type: ChatUIKitProfileType.contact,
          extension: extension,
          remark: remark,
          timestamp: timestamp,
        );

  ChatUIKitProfile.group({
    required String id,
    String? groupName,
    String? avatarUrl,
    Map<String, String>? extension,
    int timestamp = 0,
  }) : this(
          id: id,
          showName: groupName,
          avatarUrl: avatarUrl,
          type: ChatUIKitProfileType.group,
          extension: extension,
          timestamp: timestamp,
        );

  /// Used to copy a new profile object. If the passed parameter is not empty, the passed parameter is used; otherwise the parameters of the current profile are used.
  ChatUIKitProfile copyWith({
    String? showName,
    String? avatarUrl,
    Map<String, String>? extension,
    String? remark,
    int? timestamp,
  }) {
    return ChatUIKitProfile(
      id: id,
      showName: showName ?? this.showName,
      avatarUrl: avatarUrl ?? this.avatarUrl,
      type: type,
      extension: extension ?? this.extension,
      timestamp: timestamp ?? this.timestamp,
      remark: remark ?? this.remark,
    );
  }

  @override
  String toString() {
    return "id: $id, nickname: $name, type: $type, avatar: $avatarUrl, remark: $remark \n";
  }
}
```

When the avatar nickname information is displayed in UIKit, it will be obtained from the data you passed in `ChatUIKitProvider`. If it is not obtained, `ChatUIKitProvider` will ask for data through `ChatUIKitProviderProfileHandler`. If you return the required `ChatUIKitProfile`, `ChatUIKitProvider` will cache your returned `ChatUIKitProfile` to reduce the number of requests. If not, `ChatUIKitProvider` will continue to ask for data the next time when data is required.

## Set user information

If you have your own app and user information, you can pass your user information to the `ChatUIKitProvider.instance.addProfiles(list` method when the app starts:

```dart
ChatUIKitProvider.instance.addProfiles(list);
```

## Implement Provider callbacks

If there is no avatar or nickname, the system will ask for data when it needs to be displayed.

The `ChatUIKitProviderProfileHandler` definition is as follows:

```dart
typedef ChatUIKitProviderProfileHandler = List<ChatUIKitProfile>? Function(
  // The default ChatUIKitProfiles generated by `ChatUIKit` also need you to return data. If you have real data, you can return it to `ChatUIKit`, and UIKit will cache your return.
  List<ChatUIKitProfile> profiles,
);
```

The profiles needed for the current chat will be called back. You need to return the `ChatUIKitProfile` data according to `ChatUIKitProfileType`. If no data is returned, you will be asked for it the next time it is used.

The `ChatUIKitProviderProfileHandler` usage is as follows:

```dart
// profiles: Default ChatUIKitProfiles generated internally by `ChatUIKit`.
// usersProfiles: The data you return.
ChatUIKitProvider.instance.profilesHandler = (profiles) {
  ...
  // You need to return the data in the database or cache. If you don't return it, you will be asked for the data next time it is used.
  return usersProfiles;
};
```

## More

### Update information based on group-related callbacks

`ChatUIKitProfile` contains not only user attributes, but also to the group avatar and display name in a conversation list. When using a group chat, listen to the callback of creating a group/modifying the group name and update `ChatUIKitProfile` to ensure that the correct group name is obtained in the conversation list.

Add listeners and update `ChatUIKitProfile` based on group-related callbacks:

```dart
// Add GroupObserver
class _UserProviderWidgetState extends State<UserProviderWidget>
    with GroupObserver {
  late SharedPreferences sharedPreferences;

  @override
  void initState() {
    super.initState();
    // Adds the current class to ChatUIKit's Observers
    ChatUIKit.instance.addObserver(this);
  }

  @override
  void dispose() {
    // When closed, the current class needs to be removed from ChatUIKit's Observers
    ChatUIKit.instance.removeObserver(this);
    super.dispose();
  }

  // Callback occurs when a group is created by yourself
  @override
  void onGroupCreatedByMyself(Group group) {
    // After receiving the callback, add the group information to profiles
    ChatUIKitProvider.instance.addProfiles(
      [ChatUIKitProfile.group(id: group.groupId, name: group.name)],
    );
  }

  // Callback for changing the group name
  @override
  void onGroupNameChangedByMeSelf(Group group) {
    // After receiving the callback, add the group information to profiles
    ChatUIKitProvider.instance.addProfiles(
      [ChatUIKitProfile.group(id: group.groupId, name: group.name)],
    );
  }
}
```