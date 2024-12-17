# Internationalization

UIKit supports Chinese and English as the interface display language by default.  You can use the default language pack or add other packs and pass in the ISO code of the corresponding language to the `resources` and `lng` parameters. For example, `en` for English and `zh` for Chinese.

## Set the interface language

```javascript
// Set local parameters when initializing UIKit
< UIKitProvider
 local={{
  lng: 'zh', // Set the language to be used
 }}
></UIKitProvider>
```

## Add a new language pack

UIKit uses `i18next` for internationalization. You can add new language packs according to the settings of `i18next`, set ` resources` to add new language pack resources, and set `lng` to the language to be used.

```javascript
// Introduce a language pack
import en from './en.json';

<UIKitProvider
 local={{
  lng:  'one',
  resources: {
   in: {
    translation: en, // Set the language pack
   },
  },
}}
></UIKitProvider>;
```

Set the strings to be modified in the `en.json` file. View all strings [here](https://github.com/easemob/Easemob-UIKit-web/tree/dev/local). For example:

```json
{
  "conversationTitle": "Conversation List", 
  "deleteCvs": "Delete"
   ...
}
```