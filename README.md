# Mijn Stolpersteine werkzaamheden
Readme met codesnippets waar ik tijdens het groeps project mee bezig ben geweest, daarnaast ook voorbeelden van het resultaat

De werkzaamheden waaraan ik gewerkt heb zijn:
* Map clustering
* Victim Card & List
* Translation text
* Material 3 design
* Reminder uploade Photo
* Fullscreen image
* Changed Victim detail screen
* In-app Notification

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

### Grid view
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
## Reminder uploade Photo

https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/6e826dfc-a949-463f-820d-9d94cb0f40d6

### Popup
Deze popup moet ervoor zorgen dat gebruikers na elke 7 dagen een reminder krijgt dat hij of zij een nieuwe foto kan doorsturen met onze ingebouwde foto functie, mocht er geen foto aanwezig zijn of als de huidige foto outdated is.

```dart
void showCustomDialog(BuildContext context) {
    showDialog(
      context: context,
      builder: (BuildContext context) {
        return GestureDetector(
          onTap: () {
            Navigator.of(context).pop();
          },
          child: AlertDialog(
            insetPadding: const EdgeInsets.all(0.0),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(0.0),
            ),
            backgroundColor:
                Theme.of(context).colorScheme.primary.withOpacity(0.6),
            content: SafeArea(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.end,
                children: [
                  Padding(
                    padding: const EdgeInsets.symmetric(vertical: 20.0),
                    child: Image.asset(
                      "assets/images/arrow_camera.png",
                    ),
                  ),
                  Padding(
                    padding: const EdgeInsets.symmetric(vertical: 15.0),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Padding(
                          padding: const EdgeInsets.symmetric(
                            vertical: 10.0,
                          ),
                          child: Text(
                            AppLocalizations.of(context)!.popup_title,
                            style: TextStyle(
                              color: Theme.of(context).colorScheme.surface,
                              fontWeight: FontWeight.bold,
                              fontSize: 28.0,
                            ),
                          ),
                        ),
                        Text(
                          AppLocalizations.of(context)!.popup_text,
                          style: TextStyle(
                            color: Theme.of(context).colorScheme.surface,
                            fontWeight: FontWeight.normal,
                            fontSize: 18.0,
                          ),
                        ),
                      ],
                    ),
                  ),
                ],
              ),
            ),
          ),
        );
      },
    );
  }
```
### Timer Popup
Deze timer zorgt ervoor dat de popup zodra deze de eerste keer weergeven is na 7 dagen week opnieuw zal verschijnen.
```dart
// timer for showing the dialog only once a week
  Future<bool> timerShowDialog() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    int lastShown = prefs.getInt('lastShown') ?? 0;
    int now = DateTime.now().millisecondsSinceEpoch;
    int oneWeekInMillis = 7 * 24 * 60 * 60 * 1000;

    if (now - lastShown >= oneWeekInMillis) {
      // Update the last shown time
      await prefs.setInt('lastShown', now);
      return true;
    }
    return false;
  }
```
## Fullscreen image

https://github.com/Zhantie/my-stolpersteine-code/assets/74553048/90f89b63-5232-43b1-b1b2-c482d485d29b

### Fullscreen widget
Een belangerijke functie wat geraliseerd moest worden was de optie om de foto van de stenen die in de carousel weergeven zijn op volledig scherm te zien zijn, en dat gebruikers kunnen inzoomen om bepaalde details te zien.
Ik heb gebruik gemaakt van de package `photo_view: ^0.15.0 `. Door middel van `loadingBuilder` word de volgende image als deze nog niet geladen is door een loadingindicator word weergeven.

```dart
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.primary,
        centerTitle: true,
        title: Text(AppLocalizations.of(context)!.gallery,
            style: Theme.of(context).textTheme.bodyLarge?.copyWith(
                  color: Theme.of(context).colorScheme.secondary,
                )),
        iconTheme: IconThemeData(
          color: Theme.of(context).colorScheme.secondary,
        ),
      ),
      body: SafeArea(
        child: Stack(
          children: [
            PhotoViewGallery.builder(
              scrollPhysics: const BouncingScrollPhysics(),
              builder: (BuildContext context, int index) {
                return PhotoViewGalleryPageOptions(
                  imageProvider:
                      NetworkImage(widget.imageUrls[index].toString()),
                  initialScale: PhotoViewComputedScale.contained * 0.8,
                  heroAttributes: PhotoViewHeroAttributes(tag: index),
                );
              },
              itemCount: widget.imageUrls.length,
              loadingBuilder: (context, event) => Center(
                child: CircularProgressIndicator(
                  value: event == null || event.expectedTotalBytes == null
                      ? null
                      : event.cumulativeBytesLoaded / event.expectedTotalBytes!,
                ),
              ),
              backgroundDecoration: BoxDecoration(
                color: Theme.of(context).canvasColor,
              ),
              pageController: _pageController,
            ),
            Positioned(
              top: 16,
              right: 16,
              child: ValueListenableBuilder<int>(
                valueListenable: _pageNumberNotifier,
                builder: (context, page, child) {
                  return Text(
                    "${page + 1}/${widget.imageUrls.length}",
                    style: Theme.of(context).textTheme.bodyLarge!.copyWith(
                          color: Theme.of(context).colorScheme.primary,
                        ),
                  );
                },
              ),
            ),
          ],
        ),
      ),
    );
  }
```

### Image indicator
De `_FullScreenImageGalleryState` klasse bevat een PageController en een `ValueNotifier<int>`. De `PageController` wordt gebruikt om door de afbeeldingen in de galerij te navigeren, terwijl de `ValueNotifier<int>` wordt gebruikt om de huidige pagina bij te houden. De `initState` methode wordt aangeroepen wanneer de widget voor het eerst wordt gecreëerd. Hier wordt de `PageController` geïnitialiseerd en een `listener` toegevoegd die de huidige pagina bijhoudt.
De `dispose` methode wordt aangeroepen wanneer de widget wordt verwijderd. Hier worden de `PageController` en `ValueNotifier` opgeruimd om ervoor te zorgen dat het geen geheugen gebruikt.

```dart
  late final PageController _pageController;
  late final ValueNotifier<int> _pageNumberNotifier;

  @override
  void initState() {
    super.initState();
    _pageController = PageController(initialPage: widget.startIndex);
    _pageNumberNotifier = ValueNotifier<int>(widget.startIndex);
    _pageController.addListener(() {
      _pageNumberNotifier.value = _pageController.page!.round();
    });
  }

  @override
  void dispose() {
    _pageController.dispose();
    _pageNumberNotifier.dispose();
    super.dispose();
  }
```

## Changed Victim detail screen


