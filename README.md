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

### Victimcard
In mijn Victimcard haal ik de personen om door middel van de `person model` waarin de gegevens van de personen gedefineerd staan om opgehaald te worden, zoals `id`, `name`, `dateOfBirth`, `dateOfPassing`, `placeOfPassing` enzovoort.

```dart
...
children: [
          AutoSizeText(
             person.name ?? "",
            style: Theme.of(context).textTheme.bodyMedium?.copyWith(
                  color: Theme.of(context).colorScheme.secondary,
                ),
            maxFontSize: 14.0,
            maxLines: 1,
          ),
          AutoSizeText(
            // implement the date of placement
            person.dateOfBirth ?? "",
            style: Theme.of(context).textTheme.bodySmall?.copyWith(
                  color: Theme.of(context)
                      .colorScheme
                      .secondary
                      .withAlpha(
                        150,
                      ),
                ),
            maxFontSize: 12.0,
            maxLines: 1,
          ),
        ],
...
```

### GridView
De victimcard word weergeven in een gridView met een `crossAxisCount: 2`

```dart
GridView.count(
                  padding: const EdgeInsets.only(bottom: 20.0),
                  physics: const ScrollPhysics(),
                  crossAxisCount: 2,
                  shrinkWrap: true,
                  mainAxisSpacing: 10,
                  childAspectRatio: 1 / 1.5,
                  crossAxisSpacing: 10,
                  children: [
                    for (final Person person in persons)
                      VictimCard(
                        person: person,
                      ),
                  ],
                )
```

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
Deze code creëert een pop-up menu voor taalkeuze in een Flutter-app, waarbij `flutter_riverpod` wordt gebruikt om de geselecteerde taal te beheren. Het `LanguagePopupMenu` is een `ConsumerWidget` dat betekent dat het providers observeert en automatisch opnieuw opbouwd wanneer wanneer een provider veranderd word. Deze consumer widget zorgt ervoor dat de huidige een taal getoont word en gebruikers in staat stelt een andere taal te selecteren.

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
De vertalingen moeten in `.arb` file terecht komen. in het geval van Stolpersteine heb ik dus 3 verschillende files genaamd `app_en.arb`, `app_fr.arb` en `app_nl.arb`. In deze files geef je de vertalingen mee die hardcoded zijn binnen de app.

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
</div>

### Navigation bar
In een van de eerste versies van de app hebben wij als groep geen gebruik gemaakt van material design. aangezien het oude design niet meer up to date was zijn we verder gegaan met het design van Kyllian. Dit design is met Material 3 design ontworpen.
Ik heb wat veranderingen toegepast door Material 3 design toe te passen aan de naviagtie balk. Een belangrijk onderdeel hiervan was dat doormiddel van Material 3 design gebruikers feedback hadden in de navigatie. De naam en icon word belicht waanneer deze pagina actief is.

```dart
@override
  Widget build(BuildContext context) {
    return Theme(
      data: Theme.of(context).copyWith(
        navigationBarTheme: NavigationBarThemeData(
          labelTextStyle: MaterialStateProperty.resolveWith<TextStyle>(
            (states) =>
                _getTextStyle(context, states.contains(MaterialState.selected)),
          ),
          iconTheme: MaterialStateProperty.resolveWith<IconThemeData>(
            (states) => _getIconThemeData(
                context, states.contains(MaterialState.selected)),
          ),
        ),
      ),
      child: NavigationBar(
        backgroundColor: Theme.of(context).colorScheme.primary,
        indicatorColor: Color.fromRGBO(39, 56, 55, 1.0),
        destinations: <Widget>[
          NavigationDestination(
            icon: Icon(Icons.home),
            label: AppLocalizations.of(context)!.home,
          ),
          NavigationDestination(
            icon: Icon(Icons.map_outlined),
            label: AppLocalizations.of(context)!.map,
          ),
          NavigationDestination(
            icon: Icon(Icons.settings),
            label: AppLocalizations.of(context)!.settings,
          ),
        ],
        selectedIndex: getRouteIndex() ?? 0,
        onDestinationSelected: attemptPush,
      ),
    );
  }
```

### Navigation bar State 

Een belangerijk functie voor user feedback. Er word gekeken of de icon en tekst binnen de navigatie geselcteerd zijn, als dit niet het geval is dan word de icon en tekst door middel van een grijstint als niet actief weergeven, zo niet dat word deze als actief in kaart gebracht voor de gebruiker.

```dart
TextStyle _getTextStyle(BuildContext context, bool isSelected) {
  final color = isSelected
      ? Theme.of(context).colorScheme.secondary
      : Colors.grey;
  return (Theme.of(context).textTheme.bodySmall ?? const TextStyle())
      .copyWith(color: color);
}

IconThemeData _getIconThemeData(BuildContext context, bool isSelected) {
  final color = isSelected
      ? Theme.of(context).colorScheme.secondary
      : Colors.grey;
  return IconThemeData(color: color);
}
```




