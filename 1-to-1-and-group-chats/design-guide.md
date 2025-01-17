# UIKit Mobile Design Guide

![Cover Image](/assets/images/CUIcover2.png)

## General design principles

**Universal features and behaviors**

Incorporate universal and common features and behaviors into the design to ensure consistency and ease of use.

**Customizability**

Enable easy customization of styles to allow users to tailor the interface to their preferences.

**User autonomy**

Avoid making decisions on behalf of users regarding business models or preferences, allowing them a full control over their choices.

## 1. Global style (Style)

### 1.1. UIkit color specification

#### 1.1.1. Color configuration instructions

##### 1.1.1.1. Color categories

Colors are categorized into eight types:

- **Theme colors**: Primary, secondary, error
- **Primary gradients**: Eight options
- **Transparent colors**: Alpha colors for the light and dark themes
- **Neutral colors**: Neutral and special neutral

##### 1.1.1.2. Color mode (HSLA model)

Colors are specified using the HSLA model, visualized as a cylinder:

- **Hue**: Circumference of the cylinder (0°-360°)
- **Saturation**: Radius of the cylinder (0% to 100%)
- **Lightness**: Height of the cylinder (0% to 100%)

![HSLA Model Overview](assets/images/cruk1113.png)

#### 1.1.2. Theme colors

##### 1.1.2.1. Hue value

Users can set the Hue (0-360) to adjust the theme colors. The Hue value changes the color to fit user scenarios.

![Hue Example](/assets/images/cruk11211.png)

##### 1.1.2.2. Saturation

The saturation values are fixed:

- **Primary, secondary, error**: 100%
- **Neutral**: 8%
- **Special neutral**: 36%

![Saturation Example](/assets/images/cruk1122.png)

##### 1.1.2.3. Lightness

Thirteen levels of lighting are available, ranging from 0 (0%) to 100 (100%).

![Lightness Levels](/assets/images/cruk1123.png)

##### 1.1.2.4. Example

Setting the primary hue to 203, secondary hue to 155, and error hue to 350 generates 39 theme colors.

![Example Colors](/assets/images/cruk1124.png)

#### 1.1.3. Primary gradient color

The gradient theme color is a gradient color derived from the primary color. It is a linear gradient with 8 possible directions:

![Gradient Directions](/assets/images/cruk113.png)

##### 1.1.3.1. Start color

The start color should match the primary color.

 ![Start Color](/assets/images/cruk1131.png)

##### 1.1.3.2. End color

Users can configure the end color hue. The brightness is fixed.

 ![End Color Example](/assets/images/cruk11321.png)

##### 1.1.3.3. Gradient configuration

Users can configure the end color hue for gradient effects.

##### 1.1.3.4. Example

End color hue 233 with the downward gradient direction produces the following effect:

![Gradient Example](/assets/images/cruk11341.png)

#### 1.1.4. Transparent color (Alpha)

##### 1.1.4.1. Alpha colors

Used for the modal background and the light prompt background. There are two categories: Alpha onlight and Alpha ondark.

![Alpha Colors](/assets/images/cruk1141.png)

### 1.1.5. Neutral colors

#### 1.1.5.1. Neutral

A neutral color only has hue as a configurable item. The saturation is fixed at 8%.

![Neutral Example](/assets/images/cruk1151.png)

#### 1.1.5.2. Example

If the primary hue is 203 and the neutral hue is also 203, then users get the following color options:

![Neutral Example Colors](/assets/images/cruk11521.png)

### 1.1.6. Neutral special

#### 1.1.6.1. Example

For a primary hue of 203 and a neutral special hue of 220:

![Neutral Special Colors](/assets/images/cruk1161.png)

## 1.2. Theme

There are two themes - rounded and hard. Each comes with light and dark modes.

### 1.2.1. Rounded theme

Uses larger rounded corners for a soft, light appearance.

![Rounded Theme](/assets/images/1.2.1.png)

### 1.2.2. Hard theme

Avoids large rounded corners for a tough, solid look.

![Hard Theme](/assets/images/1.2.2.png)

## 1.3. Icon

### 1.3.1. Icon template

Icons follow the Material Icon Font template with a 24-grid base and a 1.5-grid stroke.

![Icon Template](/assets/images/cruk131.png)

### 1.3.2. Icon naming

Icons should be named descriptively, avoiding fixed operational behaviors.

![Icon Naming](/assets/images/cruk132.png)

## 1.4. Typography

### 1.4.1. Font family

#### 1.4.1.1. iOS font family

- **Western**: SF Pro
- **Right-to-Left**: SF Arabic, SF Hebrew
- **Chinese**: PingFang (SC, TC, HK)

#### 1.4.1.2. Android font family

- **Western**: Roboto
- **Right-to-Left**: Noto Sans Arabic, Noto Sans Hebrew
- **Chinese**: Noto Sans (SC, TC, HK)

#### 1.4.1.3. Web font family

- **Western**: Roboto
- **Right-to-Left**: Noto Sans Arabic, Noto Sans Hebrew
- **Chinese**: Noto Sans (SC, TC, HK)

### 1.4.2. Font size

#### 1.4.2.1. Minimum font size

- **Mobile**: 11
- **Web**: 12

#### 1.4.2.2. Size rules

Increase the font size in increments of 2: 10, 12, 14, 16, 18, 20.

### 1.4.3. Font weight

Divided into regular (400), medium (510), and semibold (590). Use approximate values if exact weights are not supported.

### 1.4.4. Line height

Line heights are fixed per font size:

- **11**: 14
- **12**: 16
- **14**: 20
- **16**: 22
- **18**: 26
- **20**: 28

### 1.4.5. Font role

Roles include headline, title, label, and body. Use based on the component context and information importance.

### 1.4.6. Font token

Set font typesetting tokens as shown below:

![Font Token](/assets/images/cruk146a.png)

## 1.5. Effects

### 1.5.1. Background blur

Used for components with Alpha color backgrounds to reduce interference.

```css
/* Background Blur */
backdrop-filter: blur(20);
```

## 1.5.2. Shadow

Shadows are applied to alerts, pop-ups, drawers, and others, to distinguish levels and highlight components in focus.

### 1.5.2.1. Shadow size

Shadows are categorized into three types: Small, medium, and large. The general principle is the following: The smaller the component, the more recommended it is to use a small shadow, and vice versa. Additionally, the size of the rounded corners affects the shadow recommendation.

### 1.5.2.2. Shadow token

To ensure the shadow effect is natural and soft, each shadow has two layers with different offsets, blurriness, and transparency values. There are also two sets of shadows for light and dark modes.

#### Shadow on light

```css
/* shadow/onlight/large */
box-shadow: 0 24px 36px rgba(Neutral3, 0.15), 8px 0 24px rgba(Neutral1, 0.1);

/* shadow/onlight/medium */
box-shadow: 0 4px 4px rgba(Neutral3, 0.15), 2px 0 8px rgba(Neutral1, 0.1);

/* shadow/onlight/small */
box-shadow: 0 1px 3px rgba(Neutral3, 0.15), 1px 0 2px rgba(Neutral1, 0.1);
```

#### Shadow on dark

```css
/* shadow/ondark/large */
box-shadow: 0 24px 36px rgba(Neutral4, 0.15), 8px 0 24px rgba(Neutral1, 0.1);

/* shadow/ondark/medium */
box-shadow: 0 4px 4px rgba(Neutral4, 0.15), 2px 0 8px rgba(Neutral1, 0.1);

/* shadow/ondark/small */
box-shadow: 0 1px 3px rgba(Neutral4, 0.15), 1px 0 2px rgba(Neutral1, 0.1);
```

![img](/assets/images/cruk1522b.png)

## 1.6. Radius

### 1.6.1. General rounded corners

Rounded corners are categorized into six values: None (r=0), extra small (r=4), small (r=8), medium (r=12), large (r=16), and extra large (r=½ height). Typically, all four corners of a component share the same radius.

![General Rounded Corners](/assets/images/cruk161.png)

#### 1.6.1.1. Extra small (r=4)

Typically applies to the following:

- Button (small radius)
- Input (small radius)
- Float (small radius)
- Message bubble (small radius)
- Avatar (small radius)
- Popover
- Global broadcast (small radius)

#### 1.6.1.2. Small (r=8)

Typically applies to the following:

- Alert (small radius)
- Drawer (small radius)

#### 1.6.1.3. Medium (r=12)

#### 1.6.1.4. Large (r=16)

Typically applies to the following:

- Input area (large radius)
- Alert (large radius)
- Drawer (large radius)
- Float (large radius)

#### 1.6.1.5. Extra large (r=½ height)

Typically applies to the following:

- Input area (large radius)
- Alert (large radius)
- Drawer (large radius)
- Message bubble (large radius)

### 1.6.2. Special rounded corners

Special rounded corners are applied to chat message components with background color:

- Message bubble (large radius)

![Special Rounded Corners](/assets/images/cruk162.png)

## 2. Widgets

Widgets are basic visual interaction modules.

### 2.1. Button (Bottom)

Button components come in three types: Regular button, text button, and icon button. Each button has five states: Enabled, hovered (only for web), pressed, loading, and disabled. There are also three button sizes: Large, medium, and small to fit different containers.

### 2.1.1. Regular button

Regular buttons are divided into two types: Primary and secondary.

#### 2.1.1.1. Primary button

Used for recommended actions. Typically, the background color is the theme color (Primary5/Primary6) or a gradient theme color. It appears grayed out when disabled. Rounded corners can be configured, and icons can be added as needed.

![Primary Button](/assets/images/cruk2111.png)

#### 2.1.1.2. Secondary button

Used to assist primary operations. They usually do not appear alone. The background color is typically light (Neutral98) or dark (Neutral1) and includes a stroke. They appear grayed out when disabled. Rounded corners can be configured, and icons can be added as needed.

![Secondary Button](/assets/images/cruk2112.png)

### 2.1.2. Text button (Text)

Text buttons only have foreground color and are divided into primary and secondary. They are used for more frequent routine actions (for example, form steps, message display) or as secondary actions when a regular button is present on the page.

![Text Button](/assets/images/cruk212.png)

### 2.1.3. Icon button (Icon)

Icon buttons are used when space is limited but buttons are necessary, such as for keyboard switching, top bar operations, form actions, or clearing inputs.

![Icon Button](/assets/images/cruk213.png)

Note: On the web, icon buttons should be used with a popover to explain their specific function clearly.

![Icon Button with Popover](/assets/images/cruk213b.png)

### 2.2. Input box

Used for entering short text. It comes in three sizes: Large, medium, and small. The style includes options for background color, stroke color, rounded corners, and six status types: 

- Out of focus and unfilled,
- Out of focus and filled, 
- Focused and unfilled, 
- Focused and filled, 
- Disabled and filled, 
- Disabled and unfilled.

![Input Box](/assets/images/cruk22.png)

### 2.3. Input area

Used for entering larger amounts of text, such as in forms or content publishing. The style includes options for background color, stroke color, rounded corners, and maximum character count display. The status is divided into six types: 

- Out of focus and unfilled,
- Out of focus and filled, 
- Focused and unfilled, 
- Focused and filled, 
- Disabled and filled, 
- Disabled and unfilled.

![Input Area](/assets/images/cruk23.png)

### 2.4. Checkboxes and radio buttons

Single and multiple selectors allow users to choose one or more items from a list. There are four states: 

- Selected,
- Unselected,
- Selected and disabled,
- Unselected and disabled.

### 2.5. Switch

A switch allows users to toggle a list item on and off. There are four states: 

- Closed, 
- Open, 
- Closed and disabled, 
- Open and disabled. 

Switches should not be automatically toggled; user action is required.

The style uses the iOS or Material default style, with the enabled color matching the KeyColor value of Primary5/Primary6.

### 2.6. Slider

Sliders enable users to set numerical values precisely. Configurable items include icons on both sides or values on one side, and the slider can be in either a disabled or enabled state.

### 2.7. PopMenu

The PopMenu displays non-recurring options or lists. It supports configuration for whether an icon appears on the left of the operation item.

### 2.8. Avatar

Avatars display user or operation item information. They are often placed on personal info pages or related list items. The corner radius can be extra small (r=4) or extra large (r=½ height). The size of the avatar can be configured, but the ratio remains 1:1.

#### 2.8.1. Picture avatar

Displays a picture when user avatar information is available.

![Picture Avatar](/assets/images/cruk241.png)

#### 2.8.2. Character avatar

Displayed when the user has not uploaded an avatar. Character avatars can be single-character or double-character.

#### 2.8.3. Combined avatars

Automatically generated avatars for group chats when no user avatar data is available.

![Combined Avatars](/assets/images/cruk243.png)

#### 2.8.4. Icon avatar

Used for empty states when no user avatar information is available or for form items with icons.

![Icon Avatar](/assets/images/cruk244.png)

#### 2.8.5. Avatar badge

Avatars can include badges to reflect online and offline status. Badges can be placed in the lower right or upper right corner.

![Avatar Badge](/assets/images/cruk245.png)

### 2.9. Badge

Badges are used in navigation items, list items, and avatars to display status, notifications, and counts. Configuration options include count visibility, size (standard or small), and the presence of an icon.

### 2.10. Emojis

#### 2.10.1. Twemoji [↗](https://github.com/twitter/twemoji)

Twemoji, an open-source emoji set free for commercial use, is used as the base for emojis. By default, 52 emojis are built in. Users can replace, add, or remove emojis from the 3,245 available in Twemoji.

![Twemoji Examples](/assets/images/cruk291.png)

#### 2.10.2. Emoji template

For custom emojis or replacing Twemoji, use the provided templates.

![Emoji Template](/assets/images/cruk292.png)

#### 2.10.3. Expression component state

Expression components have four states: 

- Enabled, 
- Hovered (web only), 
- Pressed, 
- Focused. 

On hover, the background color darkens; when clicked, it lightens; and on focus, it changes to the Key Color.

![Expression Component State](/assets/images/cruk293.png)

### 2.11. Toast

Provides simple feedback on the current operation. Toasts come in two types: with and without an icon.

### 2.12. Modal background (modal)

Modals are temporary popups with critical information requiring user action to exit. The modal background color can be any AlphaColor, and background blur effects can be set.

### 2.13. Index

The index is used on contact pages to quickly find contacts by category.

#### 2.13.1. Section sndex

The section index is located on the far right of the contact or member list and acts as a scroll bar to locate related contacts by letter.

#### 2.13.2. Index in list

Used to separate contacts into alphabetical categories. Configuration options include whether to display a bottom separator line.

### 2.14. Bottom tab bar (BottomTabs)

The bottom navigation bar allows switching between different views on mobile devices, with at least two items. If there are more than five items, they are displayed in pages.

### 2.15. Top tab bar (TopTabs)

The top navigation bar is used to switch between different views on mobile devices, with a minimum of one item and paged display when there are more than five items.

## 3. Components

### 3.1. Top bar

The top bar displays the current view title and provides overall control over the current page. 

- **Left side:** Configurable for return operations, avatar (with/without), and subtitle (with/without).
- **Right side:** Supports 1 to 3 actions.

### 3.2. Search bar

The search bar allows users to search for items on the current page. Configurable items include the following:

- Return icon on the left
- Display of the cancel button

### 3.3. Bottom bars

#### 3.3.1. Bottom operation bar (navigation_bar)

Used for operations on the current view. Supports the following:

- At least one item, and at most three items.
- Configurable as a text button or an icon button.

#### 3.3.2. Input bar

The input bar is used for message publishing, such as on the conversation details page. Configurable items include:

- Action buttons on the left (Action1, Action2, Action3).
- Send button on the right (configurable as a text button or an icon button).
- Top dividing line.
- Input box rounded corner style.

#### 3.3.3. Recording model

The recording popup window controls recording and sending voice messages.

### 3.4. EmojisPick

The emoji keyboard allows sending emojis built into the app. It supports the following:

- Increasing or decreasing the number of emojis
- Send and backspace buttons with configurable rounded corners
- Access to third-party emoji/sticker libraries

**Note:** Emojis entered through this component will not synchronize with the system's emojis but will display as built-in app emojis. Use emojis that are open source and free for commercial use to meet legal requirements.

![EmojisPick](/assets/images/cruk34.png)

### 3.5. List item

#### 3.5.1. Form list item (FromItem)

Used on contact/group details pages or app settings pages. Supports the following:

- Click events
- Configurable buttons on the right
- Data display, switches, sliders, single/multi-selectors
- Configurable left avatar, subtitle, category title (headline), and annotation (postil)

#### 3.5.2. ConversationItem

The conversation list item provides access to conversation details and displays the following:

- Contact avatar (Avatar)
- Contact nickname (Title)
- Latest message (Subtitle)
- New message notification (Badge)
- New message timestamp (Time)

Style options include large/small rounded corners for avatars and dividing lines for list items.

#### 3.5.3. ContactsItem

The contact list item provides access to contact details and displays the following:

- Contact avatar (Avatar)
- Contact nickname (Title)
- Contact status (Subtitle)

Style options include large/small rounded corners for avatars and dividing lines for list items.

### 3.6. Popup window (Alert)

Popup notifications are modal prompts for key information or user actions. Configurable items include the following:

- Description
- Input box
- Up to three operation items

Style options include matching rounded corners of the input box and operation buttons with the popup window.

![Popup Window](/assets/images/cruk27.png)

### 3.7. Action sheet

The action sheet displays multiple operation items in a modal form. Each item has four states: Enabled, pressed, disabled, destructive, and a special cancel type. Configurable items include the following:

- Display of icons
- Presence of dividing lines (strokes)

This component is available only on mobile devices.

![Action Sheet](/assets/images/cruk25.png)

## 4. Message bubble

### 4.1. Text messages (TextsMsg)

Text messages use a bubble style for sending characters and emoji expressions.

### 4.2. Audio message (AudioMsg)

Audio messages use a bubble style for sending voice messages. The width of the bubble changes with the length of the voice message and supports click-to-play with animation during playback.

### 4.3. File message (FileMsg)

File messages use a bubble style for sending files. Displayed fields include the following:

- File icon
- File name (Title)
- File size (Subtitle)

### 4.4. Contact message (ContactMsg)

Contact messages use a bubble style to display contacts and support click events. Displayed fields include the following:

- Contact avatar
- Contact nickname (Title)

### 4.5. Thumbnail message (ImgMsg)

Thumbnail messages use a bubble style for sending picture and video messages.

### 4.6. Additional message at the top (DescantMsg)

Top additional messages display attached information, such as replies to messages. Supports click events.

### 4.7. Message long-press action list (MsgActionSheet)

Supports operations such as copying, editing, recalling, and replying to the current message.

## 5. Module view

### 5.1. Conversation view

#### 5.1.1. Conversation list

The conversation view includes conversation search and conversation list items.

#### 5.1.2. New conversation (NewMsg)

Options in the upper right corner of the conversation for initiating a conversation, creating a group, and adding contacts.

##### 5.1.2.1. Start conversation

Calls up the contact list. Users can click or search for a contact to enter the conversation details page and initiate a conversation by sending a message.

##### 5.1.2.2. Create group (CreateGroup)

Calls up a contact list in the multi-select state. Users select group members and click **Create** to enter the group conversation details page.

##### 5.1.2.3. Add contact (Addcontact)

Calls up a contact addition popup window. Users enter the contact ID to be added.

### 5.2. Contacts view

The contact view includes contact search and contact list items.

#### 5.2.2. New request list (NewRequest)

Displays requests to add contacts. Users can process requests through the add operation.

#### 5.2.3. Group list (GroupList)

Displays all groups the user has joined. Users can process group addition requests through the add operation.

### 5.3. Conversation detail view (ConversationDetailView)

Displays details of group or one-to-one chats. The page is divided into the following:

- **Header:** Displays conversation title information, contact/group avatar, nickname/group name, online/offline status, and conversation-related operations.
- **Body:** Arranges messages chronologically from top to bottom.
- **Footer:** Used to send text, voice, attachments, and custom messages.

#### 5.3.1. Top navigation bar of the conversation (Header)

Displays key conversation information and conversation-related operations.

#### 5.3.2. Message bubble list (Body)

Displays messages in the chronological order.

#### 5.3.3. Sending messages (Footer)

Used for sending different types of messages.

### 5.4. Contact detail view (ContactDetailView)

Displays contact details, including contact avatar, nickname, and ID. Supports operations for contact do-not-disturb and contact deletion. Allows entering message details and other functions.

### 5.5. Group detail view (GroupDetailView)

Displays group details, including group avatar, name, and ID. Supports group do-not-disturb, exiting the group, and modifying group information. Group owners can transfer ownership and disband groups. Allows entering message details and other functions.

## 6. Design Resources

For design resources, please see the [Figma project](https://www.figma.com/community/file/1327193019424263350/chat-uikit-for-mobile).
