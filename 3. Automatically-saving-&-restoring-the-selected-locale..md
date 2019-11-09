[Flutter Translate](https://github.com/bratan/flutter_translate) can automatically save the selected locale and restore it after application restart.

This can be done by passing an implementation of [ITranslatePreferences](https://github.com/bratan/flutter_translate/blob/master/lib/translate_preferences.dart) during delegate creation:

```dart
 var delegate = await LocalizationDelegate.create(
      ...
        preferences: TranslatePreferences()
      ...
```

Example implementation using the [shared_preferences](https://pub.dev/packages/shared_preferences) package:

```dart
import 'dart:ui';
import 'package:flutter_translate/flutter_translate.dart';
import 'package:shared_preferences/shared_preferences.dart';

class TranslatePreferences implements ITranslatePreferences
{
    static const String _selectedLocaleKey = 'selected_locale';

    @override
    Future<Locale> getPreferredLocale() async
    {
        final preferences = await SharedPreferences.getInstance();

        if(!preferences.containsKey(_selectedLocaleKey)) return null;

        var locale = preferences.getString(_selectedLocaleKey);

        return localeFromString(locale);
    }

    @override
    Future savePreferredLocale(Locale locale) async
    {
        final preferences = await SharedPreferences.getInstance();

        await preferences.setString(_selectedLocaleKey, localeToString(locale));
    }
}
```

And don't forget to reference the shared_preferences package in pubspec.yaml

```dart
dependencies:
  shared_preferences: <latest version>
```