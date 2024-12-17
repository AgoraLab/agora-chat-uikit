# Internationalization

UIKit supports English and Chinese. If you need to support other languages, add language files in the same format as the supported languages in `EaseChatResource.bunlde` before releasing the version.

To set the interface language, set the `ease_chat_language` enumeration in `Appearance`. 

The sample code is as follows:

```Swift
    Appearance.ease_chat_language = .English
    Appearance.ease_chat_language = .Chinese
```