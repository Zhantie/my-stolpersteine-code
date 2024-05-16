# my-stolpersteine-code
Readme met code snippets waarin ik code snippets weergeef


***Language model***
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';

enum Language {
  english(flag: '🇬🇧', name: 'English', code: 'en'), 
  french(flag: '🇫🇷', name: 'Français', code: 'fr'),
  dutch(flag: '🇳🇱', name: 'Nederlands', code: 'nl'),;

  const Language ({required this.flag, required this.name, required this.code});

  final String flag;
  final String name;
  final String code;
}

final languageProvider = StateProvider<Language>((ref) => Language.english);
```

***popup menu taal***
```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:stolpersteine_app/language/language.dart';

class LanguagePopupMenu extends ConsumerWidget {
  const LanguagePopupMenu({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final language = ref.watch(languageProvider);
    return Container(
      decoration: BoxDecoration(
        color: Theme.of(context).colorScheme.surface.withOpacity(0.3),
        borderRadius:
            BorderRadius.circular(10.0), // adjust the radius as needed
      ),
      padding: EdgeInsets.all(5.0),
      child: PopupMenuButton<Language>(
        color: Theme.of(context).colorScheme.surface,
        onSelected: (value) =>
            ref.read(languageProvider.notifier).update((_) => value),
        itemBuilder: (context) => [
          for (var value in Language.values)
            PopupMenuItem(
              value: value,
              child: Row(
                children: [
                  Text(value.flag),
                  const SizedBox(width: 10.0),
                  Text(value.name),
                ],
              ),
            )
        ],
        child: Text(
          "${language.flag}",
        ),
      ),
    );
  }
}
```
