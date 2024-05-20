# Mijn Stolpersteine werkzaamheden
Readme met codesnippets waar ik tijdens het groeps project mee bezig ben geweest, daarnaast ook voorbeelden van het resultaat

De werkzaamheden waaraan ik gewerkt heb zijn:
* Map clustering
* Victim Card & List
* Translation text
* Material 3 design

## Map clustering

https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/3fb0b820-45c6-4a25-ba66-5b8b00b67049

Moet nog geschreven worden...

## Victim Card & List
https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/bbb9ffe8-c254-4c25-b3d4-cdfb06354358

## Translation text
https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/ceb7c635-56b6-488d-938f-1c3db5247f93

### Taalkeuze met Riverpod
Deze code stelt een systeem in waarmee gebruikers de taal van de app kunnen kiezen. Er wordt gebruik gemaakt van de `flutter_riverpod` package om de toestand van de geselecteerde taal te beheren. De `Language` enum definieert drie talen: Engels, Frans en Nederlands. Elke taal heeft een vlag, naam en taalcode. De constructor van deze enum neemt drie vereiste parameters: `flag`, `name` en `code`. Daarnaast is er een `StateProvider` genaamd `languageProvider` die de huidige taal bijhoudt. De Standaard is de taal ingesteld op Engels.

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

### Load Languge en save language

Deze code zorgt ervoor dat de taalinstelling wordt geladen tijdens de splashscreen van de app, zodat de gebruiker niet opnieuw de taal hoeft te selecteren bij het opstarten. In de `initState` methode wordt de `loadLanguage` functie aangeroepen. Deze functie haalt de opgeslagen taal op uit `SharedPreferences` onder de naam `languageName`.

```dart
class _SplashScreenState extends State<SplashScreen> {
  @override
  void initState() {
    super.initState();
    loadLanguage();
    load();
  }

  void loadLanguage() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    String? languageName = prefs.getString('language');

    if (languageName != null) {
      Language language = Provider.of<LanguageModel>(context, listen: false)
          .getLanguageByName(languageName);
      Provider.of<LanguageModel>(context, listen: false)
          .changeLanguage(language);
    }
  }

```

### popup menu taal

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
            BorderRadius.circular(10.0), 
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

## Material 3 design
*Oude design*
<div style="display: flex; justify-content: space-around;">
    <img width="174" alt="voorbeeld ouder versie app 2" src="https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/e59ce908-2d34-4dc8-a1c9-a95204647c52" alt="voorbeeld ouder versie app 2" style="width: 20%;">
    <img width="173" alt="voorbeeld ouder versie app 1" src="https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/326fc046-5445-47e5-98cf-5109111237be" alt="voorbeeld ouder versie app 1" style="width: 20%;">
</div>

*Material 3 Design*
<div style="display: flex; justify-content: space-around;">
    <img src="https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/6d0647ed-0bd9-45d0-9ef7-b51cce3f53ff" alt="shared image 2" style="width: 20%;">
    <img src="https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/1371c8c1-93ad-477e-81be-d69aac1d6e83" alt="shared image" style="width: 20%;">
    <img src="https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/c31d3eef-b2be-4eb2-bc17-3e9b13b23a90" alt="shared image 3" style="width: 20%;">
</div>






