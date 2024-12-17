# Internationalization

UIKit supports Chinese and English as the interface display language by default. You can use the default language pack or add other packs and pass in the corresponding ISO 639-1 language code in the `languageCode` parameter. For example, `en` represents English and `zh` represents Chinese.

## Set the interface language

```kotlin
object LanguageUtil {
    fun changeLanguage(languageCode:String){
        val appLocale: LocaleListCompat = LocaleListCompat.forLanguageTags(languageCode)
        // Because Activity.restart() may be required, call this method on the main thread.
        AppCompatDelegate.setApplicationLocales(appLocale)
    }
}
```

## Add a new language pack

1. Create a language resource file.

    You can create a new resource folder for each language under the `res` directory. The naming convention for each folder is `res/values-<language_code>`.
    
    Sample folder structure:
    
    ```xml
    res/
        values/ # Default language (usually English)
            strings.xml
        values-zh/ # Chinese
            strings.xml
        values-es/ # Spanish
            strings.xml
    ```

1. Add string resources.

    In each `strings.xml` file, add string resources for the corresponding language.