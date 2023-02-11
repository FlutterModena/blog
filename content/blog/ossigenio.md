---
author: "Davide Palma"
title: "Ossigenio"
description: "Davide ci parla della sua esperienza di uso di Flutter con BLE."
tags: ["bluetooth", "ble", "iot", "esperienze"]
date: 2023-02-11
thumbnail: https://user-images.githubusercontent.com/7345120/214380344-852bb52a-08c6-4c13-a99f-b96ed6800c03.png
latitude: 40.90025460
longitude: 14.27727490
altitude: 0.0000
---

[Ossigenio](https://ossigenio.it/)
è una piattaforma per **monitorare la qualità dell'aria** nelle proprie vicinanze e per scoprire i luoghi di studio con la qualità dell'aria migliore.

È stata creata per il **progetto universitario** di IoT.
[Qui il codice sorgente.](https://github.com/ElDavoo/ossigenio/flutter_app)

## Requisiti del progetto

Essendo un progetto di IoT, deve essere composto da un dispositivo IoT embedded, come un sensore, un "bridge" che colleghi il dispositivo a Internet e ne trasmetta i dati e un server che li riceva.

Abbiamo deciso quindi di utilizzare, per soddisfare i requisiti del progetto, un sensore IoT basato su
[ESP32](https://www.espressif.com/en/products/socs/esp32)
che si vuole collegare, tramite
[Bluetooth Low Energy](https://it.wikipedia.org/wiki/Bluetooth_Low_Energy)
, a uno **smartphone**, che dovrà inviare poi i dati geolocalizzati su **cloud**.

Abbiamo quindi bisogno di una applicazione che ci gestisca:

- BLE
- HTTP (API REST)
- GPS
- UI che mostri tutto (slider, mappa, icone...)

... il tutto in maniera **cross platform**.

## Perché usare Flutter

Dopo una breve ricerca, abbiamo deciso di utilizzare Flutter per costruire l'applicazione. Ecco perché:

- Per il supporto cross platform, ovvero sviluppare una unica *codebase* per iOS e Android (ricordo che Flutter gestisce smartphone, desktop e web)
    - Anche se il progetto è stato presentato su uno smartphone Android, abbiamo voluto costruire qualcosa di cross-platform, perché non si sa mai...
    - Desktop e web potrebbero essere realizzabili, ma non era negli obiettivi del progetto
        - Anche se platform-agnostic, molti pacchetti (librerie) sono platform-specific
        - Sicuramente più facile che realizzare una app nativa per ogni piattaforma
- Tecnologia innovativa e che valeva la pena imparare
    - Conoscere come realizzare app può sempre tornare utile

I vantaggi di Flutter sono stati scoperti in corso d'opera, ci ha stupito soprattutto **la performance e la [null safety](https://dart.dev/null-safety)** (di Dart)

## Tempistiche

Partendo da **zero esperienza in sviluppo app**, il prototipo funzionante ha richiesto circa **due mesi** di sviluppo. Ciò sottolinea la **semplicità di apprendimento** di Dart e di Flutter. Lo sviluppo, chiaramente, sarebbe stato ancora più rapido se avessimo già conosciuto le logiche tipiche della programmazione **asincrona** e della **UI dichiarativa** (I due pilastri di Flutter).

## Struttura dell'app

Non essendo uno sviluppatore professionista, ho strutturato le cose seguendo *l'intuito*, il che significa che i pattern di sviluppo migliori potrebbero essere diversi.

Il codice Dart dell'applicazione è diviso in:
    - Classi Managers, che svolgono il lavoro "dietro le quinte";
    - Widgets, quindi codice che riguarda la UI
    - Varie classi di utilities

Spieghiamo nel dettaglio i Manager e la UI.

### Managers

Nell'ambito di questo progetto, i manager sono classi *singleton* che implementano vari metodi per gestire una certa funzionalità. Essi sono:
    - account\_man , gestisce la comunicazione col server e le API HTTP REST
    - ble\_man , gestisce il Bluetooth e la comunicazione con i sensori
    - gps\_man , gestisce il GPS e la posizione dell'utente
    - mqtt\_man , gestisce la comunicazione con il cloud MQTT
    - perm\_man , chiede i permessi necessari all'utente (Bluetooth e GPS)
    - pref\_man. , salva alcuni dati nella memoria permanente.

### UI

In Flutter, ogni elemento della UI è un **widget**. Ogni widget può eventualmente contenere **uno o più widget** al suo interno. Ciò struttura la UI come un **widget tree**.  
![1c088cd0fbed484534f1dfdf0b0e8caa.png](/1c088cd0fbed484534f1dfdf0b0e8caa.png)

A piacimento si può prendere una parte dell'albero desiderato e creare un custom widget. È stato deciso quindi di dividere i widget in tre categorie:

#### Pagine

![2bf1ea36b9f6271dd6426a2ef7da5da7.png](/2bf1ea36b9f6271dd6426a2ef7da5da7.png)

Le pagine rappresentano una vista completa dell'app, quindi la schermata principale, le schermate di login e registrazione e le schermate di previsione.

#### Schede

La pagina principale è composta dalla scheda di benvenuto e la scheda per visualizzare la mappa.

![e4e88008e27a7c0960ada54a616fb54e.png](/e4e88008e27a7c0960ada54a616fb54e.png)

#### Widget

I vari widget "di base", quindi le card presenti nella schermata principale.

![452db2f03d4405d527c9c99e18af9745.png](/452db2f03d4405d527c9c99e18af9745.png)

![6aaaef16550b477bf9949acf1b49c34b.png](/6aaaef16550b477bf9949acf1b49c34b.png)

### UI &lt;-&gt; Managers

I meccanismi utilizzati in larga scala per far comunicare la UI con la business logic, oltre le classiche
[Future](https://dart.dev/codelabs/async-await) 
e 
[FutureBuilder](https://api.flutter.dev/flutter/widgets/FutureBuilder-class.html), sono:

- Gli StreamController, flussi di valori
- I ValueNotifier, che conservano un valore e notificano quando esso cambia

Segue una spiegazione più dettagliata di questi meccanismi.

#### StreamController

Gli [StreamController](https://api.flutter.dev/flutter/dart-async/StreamController-class.html) incapsulano uno [Stream](https://dart.dev/tutorials/language/streams) e permettono di aggiungerci valori. Gli Stream sono utili in cui si hanno diversi valori restituiti in tempi non definiti. Per esempio, lo StreamController è stato usato per i risultati della scansione Bluetooth del dispositivo, per la posizione dell'utente dal GPS e per i messaggi ricevuti dal sensore.

```dart
/// La lista dei messaggi scambiati col dispositivo
final List<MessageWithDirection> messages = [];
late Stream<MessageWithDirection> messagesStream;
```


Lo stream dei messaggi viene definito durante l'inizializzazione della classe Device:

```dart
messagesStream = btUart.txCharacteristic.value
        .map((value) {
          // Crea un messaggio da un valore
          final Message? message = SerialComm.receive(value);
          if (message != null) {
            return MessageWithDirection(
                MessageDirection.received, DateTime.now(), message);
          }
          return null;
        })
        .where((message) => message != null)
        .cast<MessageWithDirection>()
        .asBroadcastStream();
```

Quando viene aggiunto un valore allo stream, tutti i callback registrati vengono eseguiti. Un callback si registra con il metodo **listen**:  
```dart
    messagesStream.listen((message) {
      Log.v("Message received");
      // Salva i messaggi scambiati
      messages.add(message);
      // Manda i messaggi ricevuti su MQTT
      if (message.direction == MessageDirection.received) {
        MqttManager().publish(message.message);
      }
    });

```

Non bisogna quindi effettuare alcun polling. Bisogna ricordarsi che gli Stream **non conservano** valori, quindi non sono adatti a valori che vengono restituiti di rado o solamente una volta.

#### ValueNotifier

In Flutter, per aggiornare lo stato di uno [StatefulWidget](https://docs.flutter.dev/development/ui/interactive#stateful-and-stateless-widgets), è necessario utilizzare il metodo [setState](https://api.flutter.dev/flutter/widgets/State/setState.html) all'interno della classe State. Non possiamo però chiamare questo metodo in altre classi o altri file, in quanto necessita di un BuildContext. In altre parole, se cambia il valore di una variabile che si trova in un'altra classe, la UI non verrà aggiornata. Per risolvere questo problema, c'è  [ValueNotifier](https://api.flutter.dev/flutter/foundation/ValueNotifier-class.html) e [ValueListenableBuilder](https://api.flutter.dev/flutter/widgets/ValueListenableBuilder-class.html), rispettivamente una classe che conserva un singolo valore e che avvisa i suoi Listener quando il valore cambia e un widget che ascolta un ValueNotifier.

```dart
  /// Il dispositivo a cui siamo connessi
  ValueNotifier<Device?> dvc = ValueNotifier<Device?>(null);
```

Nella parte di connessione al dispositivo, imposto il valore:

```dart
  /// Prova a connettersi a un dispositivo.
  ///
  /// Questo metodo cerca di connettersi al dispositivo.
  /// Lo imposta in [dvc] se la connessione è andata a buon fine.
  Future<void> connectToDevice(ScanResult result) async {
    stopBLEScan();

    try {
      await result.device.connect().timeout(const Duration(seconds: 3));
    } on Exception catch (e) {
      Log.l("Errore durante la connessione: $e");
      rethrow;
    }

    dvc.value = Device(result, await BTUart.fromDevice(result.device));
    return;
  }
```

Nella UI, infine, mostro la card relativa al sensore:

```dart
  ValueListenableBuilder(
     valueListenable: BLEManager().dvc,
     builder: (context, dvc, _) {
         // Se non siamo connessi a un dispositivo, ritorniamo una
         // scatola vuota e invisibile
       if (dvc == null) {
         return const SizedBox();
       }
       return UI.buildCard(AirQualityDevice(device: dvc));
     }),
```

## Conclusioni

Flutter non solo si è mostrato all'altezza delle aspettative, ma le ha ampiamente superate, dimostrando di essere un framework che non ha paura di nulla.

Tuttavia, come in ogni tecnologia, ho notato aree di potenziale miglioramento:

- I pacchetti spesso non hanno la stessa qualità del framework principale
    - Prima scelta libreria bluetooth: [flutter_blue](https://pub.dev/packages/flutter_blue) , non più aggiornata e senza indicatore di RSSI
    - Seconda scelta, usata per il progetto: [flutter\_blue\_plus](https://pub.dev/packages/flutter_blue_plus)

Entrambe le librerie supportano solo iOS e Android.

Siccome abbiamo bisogno solamente della parte Low Energy del bluetooth, se potessimo tornare indietro avremmo scelto [quick_blue](https://pub.dev/packages/quick_blue), dato che supporta anche le piattaforme desktop.

- Interagire al livello più basso del framework è complicato
    - Se si vuole avere un widget davvero particolare, è possibile che nessun pacchetto in
    [pub.dev](https://pub.dev)
     soddisfi i propri bisogni (per fortuna a noi non è capitato). In questo caso, bisogna costruire il proprio widget manualmente con le API Canvas, il che vuol dire disegnare linee e forme in maniera "grezza". La curva di apprendimento di questa funzionalità è più complicata.
