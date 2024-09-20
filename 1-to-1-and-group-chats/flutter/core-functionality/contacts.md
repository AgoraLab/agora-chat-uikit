`ContactsView` is used to display the user list, including contact search, adding contacts, friend request list entry, group list entry, and contact list.

Nicknames can be sorted by the first letter. `ContactsView` can be used directly or through routing.
                                             


## Add a contact list

When adding a conversation list, just add `ContactsView` to the page.

```dart
@override
Widget build(BuildContext context) {
  return const ContactsView();
}
```

## Customize your contact list

If you need to customize the contacts page, you can modify the following parameters:

| Parameter | Description |
|:---:|:---:|
| `final ContactListViewController? controller` | The contact list controller. |
| `final ChatUIKitAppBar? appBar` | Customize the message page `appBar`. If not set, the default one will be used. |
| `final bool enableAppBar` | Whether to enable the `appBar`. Enabled by default. If disabled, it will no longer be displayed and the `appBar` input will no longer take effect. |
| `final String? title` | The default `appBar` title information. If you use a custom `appBaror` or disable it, this parameter will not take effect. |
| `final void Function(List<ContactItemModel> data)? onSearchTap` | The click event callback of the contact list search. If not set, there will be a default implementation. |
| `final bool enableSearchBar` | Whether to display the contact search box. Enabled by default.|
| `final String? searchHideText` | The default text displayed in the search box. |
| `final List<ChatUIKitListViewMoreItem>? beforeItems` | The widget displayed in front of the contact list will no longer display the friend request and group list entrances after setting. |
| `final List<ChatUIKitListViewMoreItem>? afterItems` | The widget displayed behind the contact list. |
| `final ChatUIKitContactItemBuilder? listViewItemBuilder` | The contact list item builder. If you want to customize the display, set it here. |
| `final void Function(BuildContext context, ContactItemModel model)? onTap` | The contact list item click event. If not set, the default is to enter the contact details. |
| `final void Function(BuildContext context, ContactItemModel model)? onLongPress` | The contact list item long-press event. There is no default implementation. If you need to add a long-press event, set it here. |
| `final Widget? listViewBackground` | The background image to display when a contact is empty. If not set, a default implementation will be used. |
| `final String? loadErrorMessage` | The text message displayed when contact loading fails. If not set, there will be a default implementation. |
| `final String? attributes` | The extended parameters that will be passed to the next page. |


## More

To obtain the number of friend requests, refer to the following sample code:

```dart
try {
  int newRequestCount = await ChatUIKit.instance.contactRequestCount();
} catch (e) {
  // Error
}
```
