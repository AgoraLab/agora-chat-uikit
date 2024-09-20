`EaseContactsListFragment` is used to display the address book list, including contact search, adding contacts, friend request list entry, group list entry, and contact list.

Nicknames can be sorted by the first letter.

## Usage example

```kotlin
class ContactListActivity: AppCompactActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_contact_list)

        EaseContactsListFragment.Builder()
                        .build()?.let { fragment ->
                            supportFragmentManager.beginTransaction()
                                .replace(R.id.fl_fragment, fragment).commit()
                        }
    }
}
```

## Advanced usage

### Customize settings through `EaseContactsListFragment.Builder`

An `EaseContactsListFragment` Builder method is provided for making some custom settings. The following settings are currently provided: 

```kotlin
EaseContactsListFragment.Builder()
  .useTitleBar(true)
  .setTitleBarTitle("title")
  .enableTitleBarPressBack(true)
  .setTitleBarBackPressListener(onBackPressListener)
  .useSearchBar(false)
  .setSearchType(EaseSearchType.USER)
  .setListViewType(EaseListViewType.VIEW_TYPE_LIST_CONTACT)
  .setSideBarVisible(true)
  .setHeaderItemVisible(true)
  .setHeaderItemList(mutableListOf<EaseCustomHeaderItem>())
  .setOnHeaderItemClickListener(OnHeaderItemClickListener)
  .setOnUserListItemClickListener(OnUserListItemClickListener)
  .setOnItemLongClickListener(onItemLongClickListener)
  .setOnContactSelectedListener(OnContactSelectedListener)
  .setEmptyLayout(R.layout.layout_conversation_empty)
  .setCustomAdapter(customAdapter)
  .setCustomFragment(myContactsListFragment)
  .build()
```

| Method | Description |
|:---:|:---:|
| `useTitleBar()` | Whether to use the default title bar (`EaseTitleBar`). Set to `true` for yes, `false` (default) for no. |
| `setTitleBarTitle()` | Set the title of the title bar. |
| `enableTitleBarPressBack()` | Set whether to display the back button. Set to `true` for yes, `false` (default) for no. |
| `setTitleBarBackPressListener()` | Set the listener for clicking the back button in the title bar. |
| `useSearchBar()` | Set whether to use the search bar. Set to `true` for yes, `false` (default) for no. |
| `setSearchType()` | Set the `EaseSearchType` search type: `USER`, `SELECT_USER`, `CONVERSATION`. |
| `setListViewType()` | Set the `EaseListViewType` list type. `LIST_CONTACT`: Default contact list without checkboxes. `LIST_SELECT_CONTACT`: Contact list with checkboxes. |
| `setSideBarVisible()` | Set whether to show the alphabetical index toolbar. Set to `true` (default) for yes, `false` for no. |
| `setHeaderItemVisible()` | Set whether to display the list header layout. |
| `setHeaderItemList()` | Set the data object of the list header items. |
| `setOnHeaderItemClickListener()` | Set the click event of the list header item. |
| `setOnUserListItemClickListener()` | Set the item click event listener. |
| `setOnItemLongClickListener()` | Set the item long press event listener. |
| `setOnContactSelectedListener()` | Set the item selection event listener. |
| `setEmptyLayout()` | Set a blank page for the conversation list. |
| `setCustomAdapter()` | Set a custom adapter, the default is `EaseContactListAdapter`. |
| `setCustomFragment()` | Set a custom chat fragment, must be inherited from `EaseContactsListFragment`. |

## Customize your contact list

### Add a custom contact layout

You can inherit from `EaseContactListAdapter` to implement your own `CustomContactListAdapter` and then set it with `EaseContactsListFragment#Builder#setCustomAdapter`.

1. Inherit from `EaseContactListAdapter` to create `CustomContactListAdapter`, then override the `getViewHolder` and `getItemNotEmptyViewType` methods.

   ```kotlin
   class CustomContactListAdapter : EaseContactListAdapter() {
       override fun getItemNotEmptyViewType(position: Int): Int {
            // Set a custom itemViewType based on the message type.
            // If the default itemViewTyp is used, return super.getItemNotEmptyViewType(position).
           return CUSTOM_YOUR_CONTACT_TYPE
       }
   
       override fun getViewHolder(parent: ViewGroup, viewType: Int): ViewHolder<EaseUser> {
            // Return the corresponding ViewHolder according to the returned viewType.
            // Return a custom ViewHolder or use the default super.getViewHolder(parent, viewType)
           return CUSTOM_YOUR_VIEW_HOLDER()
       }
   }
   ```
   
1. Add `CustomContactListAdapter` to `EaseContactsListFragment#Builder`.

   ```kotlin
   builder.setCustomAdapter(CustomContactListAdapter)
   ```
   
### Set a selectable contact list

For example, if you need to add multiple users when creating a group, you can click the checkboxes next to the contacts to select them.

```kotlin
builder.setSearchType(EaseSearchType.SELECT_USER)  
```

## Event listening

```kotlin
EaseContactsListFragment.Builder()
  .setOnHeaderItemClickListener(OnHeaderItemClickListener)
  .setOnUserListItemClickListener(OnUserListItemClickListener)
  .setOnItemLongClickListener(onItemLongClickListener)
  .setOnContactSelectedListener(OnContactSelectedListener)
  .build()
```

| Method | Describe |
|:---:|:---:|
| `setOnHeaderItemClickListener()` | Set the event listener for a header item click. |
| `setOnUserListItemClickListener()` | Set the event listener for a list item click. |
| `setOnItemLongClickListener()` | Set the event listener for an item long press. |
| `setOnContactSelectedListener()` | Set the item selection event listener. |

## More

### Get the number of unread contacts' system notifications

```kotlin
val systemConversation = EaseNotificationMsgManager.getInstance().getConversation() 
systemConversation.let { cv->
    newRequestCount = cv.unreadMsgCount
}
```

