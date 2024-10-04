# Contact details

You can configure the AppBar, the button in the middle, the list items, and other elements.

The contact details page provides the following customization options:

- AppBar: Customize through `appBarModel`.
- The buttons in the middle, including sending messages, searching messages, audio calls, and video calls: Customize through `actionsBuilder`. The default events will be returned through the builder, and the events to be displayed need to be returned.
- List items on the page, including the DND mode, clear chat history, and others: Customize through `itemsBuilder`. The default list items will be called back through the builder, and the list items to be displayed need to be returned.

Customize the contact details page in the following ways:
   
- Set directly in `ContactDetailsView`:

    ```dart
      ContactDetailsView(
        profile: ChatUIKitProfile.contact(
          id: "userId",
          nickname: "nickname",
        ),
        appBarModel: ChatUIKitAppBarModel(
          centerTitle: true,
          title: "Test",
        ),
        itemsBuilder: (context, profile, defaultItems) {
          return [
            ...defaultItems,
            ChatUIKitDetailsListViewItemModel.space(),
            ChatUIKitDetailsListViewItemModel(
              title: "Add content",
              onTap: () {},
            )
          ];
        },
        actionsBuilder: (context, defaultList) {
          return [
            ...defaultList ?? [],
            ChatUIKitDetailContentAction(
              iconSize: const Size(30, 35),
              icon: 'assets/images/chat.png',
              title: "Chat",
              onTap: (ctx) {},
            ),
          ];
        },
      );
    ```

- Set via `ContactDetailsViewArguments`:

    ```dart
      static RouteSettings contactDetails(RouteSettings settings) {
        ContactDetailsViewArguments arguments =
            settings.arguments as ContactDetailsViewArguments;
        arguments = arguments.copyWith(
          appBarModel: ChatUIKitAppBarModel(
            centerTitle: true,
            title: "Test",
          ),
          itemsBuilder: (context, profile, defaultItems) {
            return [
              ...defaultItems,
              ChatUIKitDetailsListViewItemModel.space(),
              ChatUIKitDetailsListViewItemModel(
                title: "Add content",
                onTap: () {},
              )
            ];
          },
          actionsBuilder: (context, defaultList) {
            return [
              ...defaultList ?? [],
              ChatUIKitDetailContentAction(
                iconSize: const Size(30, 35),
                icon: 'assets/images/chat.png',
                title: "Chat",
                onTap: (ctx) {},
              ),
            ];
          },
        );
    
        return RouteSettings(name: settings.name, arguments: arguments);
      }
    ```