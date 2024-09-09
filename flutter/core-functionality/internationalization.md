`ChatUIKit` provides internationalization features, supporting English by default.

## Add internationalization files
   
```dart
class MyApp extends StatelessWidget {
  MyApp({super.key});

  final ChatUIKitLocalizations _localization = ChatUIKitLocalizations();

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      localizationsDelegates: _localization.localizationsDelegates,
      supportedLocales: _localization.supportedLocales,
    );
  }
}
```

When the language used by the app exceeds the language range provided, you can set the default language:

```dart
_localization.displayLanguageWhenNotSupported = const Locale('en');
```

## Add a new language

You can use the `ChatUIKitLocalizations.addLocales` method to expand the languages supported by internationalization. The corresponding text constants are defined in the `ChatUIKitLocal` file. You can add your own `ChatLocalobject` to `ChatUIKitLocalizations` to implement internationalization.

For example, you can add support for French in the following way:

```dart
class _MyAppState extends State<MyApp> {
  final ChatUIKitLocalizations _localization =
      ChatUIKitLocalizations();

  @override
  void initState() {
    super.initState();
    _localization.addLocales(locales: const [
      ChatLocal('fr', {
        ChatUIKitLocal.conversationsViewSearchHint: 'Recherche',
        // The text needs to be completed according to the definition in ChatUIKitLocal.
        ...
      })
    ]);
    // After adding a language, you need to resetLocales
    _localization.resetLocales();
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      localizationsDelegates: _localization.localizationsDelegates,
      supportedLocales: _localization.supportedLocales,
      localeResolutionCallback:_localization.localeResolutionCallback,
      ...
    );
  }
}
```