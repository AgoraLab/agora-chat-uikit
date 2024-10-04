# Internationalization

UI provides internationalization services, with Chinese and English language packs provided by default.

For example:

```typescript
export function App() {
  // If no language is specified, UIKIt obtains the default language from the system
  const [language] = React.useState<LanguageCode>("zh-Hans");

  return (
    <Container options={{ appKey: "appKey" }} translateLanguage={'zh-Hans'}>
      {/* Add child components */}
    </Container>
  );
}
```

## Set up the language pack

If you set a language to other than Chinese or English, provide a language pack for the specified language. The language pack is registered through the `onInitLanguageSet` callback.

For example: 

```typescript
// Create a French language pack
export function createStringSetFr(): StringSet {
  return {
    "this is test.": "c'est un test.",
    "This is test with ${0} and ${1}": (a: string, b: number) => {
      return `Ceci est un test avec ${a} et ${b}`;
    },
  };
}

export function App() {
  // Set French
  const [language] = React.useState<LanguageCode>("fr");
  const onInitLanguageSet = React.useCallback(() => {
    const ret = (language: LanguageCode): StringSet => {
      const d = createDefaultStringSet(language);
      if (language === "fr") {
        return createStringSetFr();
      }
      return d;
    };
    return ret;
  }, []);

  return (
    <Container
      options={{ appKey: "appKey" }}
      language={language}
      onInitLanguageSet={onInitLanguageSet}
    >
      {/* Add child components */}
    </Container>
  );
}
```