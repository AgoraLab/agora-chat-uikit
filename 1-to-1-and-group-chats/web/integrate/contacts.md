# Contacts

The `ContactList` component is used to display the address book, including the contact list, group list, and friend request list. Nicknames can be sorted by the first letter.

## Usage examples

```javascript
import React, { useEffect, useState } from "react";
import { ContactList } from "easemob-chat-uikit";
import "easemob-chat-uikit/style.css";

const ContactList = () => {
  return (
    <div style={{ width: "30%", height: "100%" }}>
      <ContactList />
    </div>
  );
};
```

## Customize the contact list

### Customize the contact list header

For example, change the default title name of the contact list page. The sample code is as follows:

```javascript
import React, { useEffect, useState } from "react";
import { ContactList } from "easemob-chat-uikit";
import "easemob-chat-uikit/style.css";

const ContactList = () => {
  return (
    <div style={{ width: "30%", height: "100%" }}>
      <ContactList header={<div>自定义 Header</div>} />
    </div>
  );
};
```

### Add a blacklist 

Add a contact blacklist in the contact list. The sample code is as follows:

```javascript
import React, { useEffect, useState } from "react";
import { ContactList } from "easemob-chat-uikit";
import "easemob-chat-uikit/style.css";

const ContactList = () => {
  return (
    <div style={{ width: "30%", height: "100%" }}>
      <ContactList
        menu={[
          "contacts",
          "groups",
          "requests",
          {
            title: "Block list",
            data: [
              {
                remark: "Nickname",
                userId: "userId",
              },
            ],
          },
        ]}
        onItemClick={(data) => {
          console.log("data", data);
        }}
      />
    </div>
  );
};
```

### Set as a selectable contact list

For example, if a user needs to add multiple users when creating a group, they can click the checkboxes corresponding to the contacts to select them.

```javascript
import React, { useEffect, useState } from "react";
import { ContactList } from "easemob-chat-uikit";
import "easemob-chat-uikit/style.css";

const ContactListContainer = () => {
  return (
    <div style={{ width: "30%", height: "100%" }}>
      <ContactList
        onCheckboxChange={handleSelect}
        checkable
        menu={["contacts"]}
        hasMenu={false} // Valid when there is only one menu item
        header={<></>}
        checkedList={checkedList}
        defaultCheckedList={defaultCheckedUsers || []}
      />
    </div>
  );
};
```

## ContactList properties

`ContactList` contains the following properties:

| Property | Type | Description |
|---|---|---|
| `className` | `String` | The class name of the component. |
| `prefix` | `String` | The prefix for the CSS class names. |
| `style` | `React.CSSProperties` | The styles for the `ContactList` component. |
| `onItemClick` | <code>(info: { id: string; type: 'contact' &#124; 'group' &#124; 'request'; name: string }) => void;</code> | The click callback of each item. |
| `hasMenu` | `boolean` | Whether to display the category menu item, the default value is `true`. It can be set to `false` when there is only one menu item in the contact list. |
| `checkable` | `boolean` | Whether to display a checkbox after the contact. |
| `onCheckboxChange` | `(checked: boolean, data: UserInfoData) => void;` | The callback that displays a checkbox after clicking a contact. |
| `header` | `React.ReactNode;` | The component's `Header`. |
| `checkedList` | `{ id: string; type: 'contact' &#124; 'group'; name?: string }[]` | For a selectable contact list, set the item to be selected. |
| `defaultCheckedList` | `{ id: string; type: 'contact' &#124; 'group'; name?: string }[]` | For a selectable contact list, set the default selected item. |
| `menu` | <code>( &#124; 'contacts' &#124; 'groups' &#124; 'requests' &#124; { title: string; data: ({ remark?: string; userId: string } &#124; { groupname: string; groupid: string })[]; } )[] ;</code> | Customize the menu items of `ContactList`. |
