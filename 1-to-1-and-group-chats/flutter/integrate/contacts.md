# Contacts

`ContactsView` is used to display the user list, including contact search, adding contacts, friend request items, group list items, and the contact list.

Nicknames can be sorted by the first letter. `ContactsView` can be used directly or through routing.
                                             
## Add a contact list

When adding a conversation list, just add `ContactsView` to the page:

```dart
@override
Widget build(BuildContext context) {
  return const ContactsView();
}
```

## Customize the contact list

If you need to customize the contacts page, you can modify the following properties:

| Property | Description |
|:---:|:---:|
| `final ContactListViewController? controller` | The contact list controller. |
| `final ChatUIKitAppBarModel? appBarModel` | Customize the message page `appBarModel`. If not set, the default one will be used. |
| `final bool enableAppBar` | Whether to enable the `appBar`. Enabled by default. If disabled, it will no longer be displayed and the `appBar` input will no longer take effect. |
| `final void Function(List<ContactItemModel> data)? onSearchTap` | The click event callback of the contact list search. If not set, there is a default implementation. |
| `final bool enableSearchBar` | Whether to display the contact search box. Enabled by default.|
| `final String? searchHideText` | The default text displayed in the search box. |
| `final List<ChatUIKitListViewMoreItem>? beforeItems` | The widget displayed before the contact list. The friend request and group list entrances will no longer be displayed after setting. |
| `final List<ChatUIKitListViewMoreItem>? afterItems` | The widget displayed after the contact list. |
| `final ChatUIKitContactItemBuilder? listViewItemBuilder` | The contact list item builder. If you want to customize the display, set it here. |
| `final void Function(BuildContext context, ContactItemModel model)? onTap` | The contact list item click event. If not set, the default is opening the contact details. |
| `final void Function(BuildContext context, ContactItemModel model)? onLongPress` | The contact list item long-press event. There is no default implementation. If you need to add a long-press event, set it here. |
| `final Widget? listViewBackground` | The background image to display when a contact is empty. If not set, a default implementation will be used. |
| `final String? loadErrorMessage` | The text message displayed when contact loading fails. If not set, there is a default implementation. |
| `final String? attributes` | The extended parameters that will be passed to the next page. |


## Customize AppBar

Set whether to display AppBar through `enableAppBar` and customize it through `appBarModel`:

```dart
ContactsView(
  appBarModel: ChatUIKitAppBarModel( 
    title: 'Title',
    subtitle: 'Subtitle',
  ),
);
```

For a detailed description of AppBar customization, see [Advanced usage](advanced-usage.md).

## Customize the message list

The message page can be customized in two ways:

- Customize directly when you need to jump to the `MessagesView` page.
- Customize `RouteSettings` when redirecting via routing. For details, see [Advanced usage](advanced-usage.md).

## Customize the widgets before and after the contact list

- `beforeItems`: The widget displayed before the contact list. After setting, the friend request and group list entrance will no longer be displayed.

- `afterItems`: The widget displayed after the contact list.

`beforeItems` and `afterItems` are in the `header` and `footer` of the list, respectively.

```dart
ContactsView(
  beforeItems: const [
    ChatUIKitListViewMoreItem(
      title: 'before',
    )
  ],
  afterItems: const [
    ChatUIKitListViewMoreItem(
      title: 'after',
    )
  ],
);
```

## Customize list items

You can customize the list items through `itemBuilder`. `model` is the `ContactItemModel` object. The returned `Widget` will replace the original display. If `null` is returned, the default style will be used:

```dart
ContactsView(
  itemBuilder: (context, model) {
    return ListTile(
      title: Text(model.profile.showName),
      leading: CircleAvatar(
        child: Text(model.profile.showName[0].toUpperCase()),
      ),
    );
  },
);
```


## More

To obtain the number of friend requests, refer to the following sample code:

```dart
try {
  int newRequestCount = await ChatUIKit.instance.contactRequestCount();
} catch (e) {
  // Error
}
```
