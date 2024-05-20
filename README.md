# my-stolpersteine-code
Readme met code snippets waarin ik code snippets weergeef

# Victim Card & List
<div style="display: flex; justify-content: space-around;">
    <img src="https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/6d0647ed-0bd9-45d0-9ef7-b51cce3f53ff" alt="shared image 2" style="width: 30%;">
    <img src="https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/1371c8c1-93ad-477e-81be-d69aac1d6e83" alt="shared image" style="width: 30%;">
    <img src="https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/c31d3eef-b2be-4eb2-bc17-3e9b13b23a90" alt="shared image 3" style="width: 30%;">
</div>


# Translation text
https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/ceb7c635-56b6-488d-938f-1c3db5247f93
## Language model
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

## popup menu taal
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

# Material 3 design


https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/bbb9ffe8-c254-4c25-b3d4-cdfb06354358




