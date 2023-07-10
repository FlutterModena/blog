---
author: "Dario Varriale"
title: "Flutter CI con le GitHub Actions"
description: "Dario ci mostra come creare workflow di test per app Flutter con GitHub Actions."
tags: ["github", "test", "ci/cd", "tutorial"]
date: 2023-07-10
thumbnail: /ghactions.png
---

La Continuous Integration (**CI**) è una pratica essenziale nello sviluppo software moderno, che consente di automatizzare il processo di integrazione del codice e di eseguire test automatici per garantire la stabilità del progetto. 

In questo articolo, ti guiderò passo passo nell'integrazione dei test all'interno di un progetto Flutter utilizzando le **GitHub Actions**. 

Creeremo un *workflow* che: 
- controllerà che la versione del progetto Flutter sia effettivamente aumentata
- eseguirà i test 
- creerà la build per Android e per iOS per verificare che tutto funzioni correttamente.

## Preparazione del progetto Flutter

⚠️ Assicurati di avere Flutter installato nel tuo sistema e che il tuo ambiente di sviluppo sia correttamente configurato. 

Puoi verificare l'installazione di Flutter eseguendo il comando: 

```bash
flutter doctor
```

Se il comando restituisce avvisi o errori, segui le istruzioni fornite per risolverli.

Inizia un nuovo progetto Flutter eseguendo il comando 

```bash
flutter create nome_progetto
```

nella tua riga di comando, sostituendo `nome_progetto` con il nome che desideri assegnare al tuo progetto.

## Aggiunta di un test di esempio

Per integrare i test nel tuo progetto Flutter, iniziamo con un esempio semplice. All'interno della cartella test del tuo progetto, crea un nuovo file chiamato `example_test.dart`. Questo file conterrà un test di esempio.

```dart
import 'package:flutter_test/flutter_test.dart';

void main() {
  test('Example test', () {
    // Inserisci qui il codice del tuo test
    expect(2 + 2, 4); // Esempio di asserzione
  });
}
```

Nell'esempio sopra, abbiamo creato una funzione `main()` che conterrà il nostro test. All'interno della funzione `main()`, abbiamo definito un test utilizzando la funzione `test()` fornita dalla libreria `flutter_test`. Il primo argomento della funzione `test()` è una descrizione del test, mentre il secondo argomento è una funzione anonima che contiene il codice del test stesso.

All'interno del test di esempio, abbiamo utilizzato la funzione `expect()` per stabilire un'asserzione. L'asserzione verifica se il risultato dell'espressione `2 + 2` è uguale a `4`. Se l'asserzione fallisce, il test riporterà un errore.

Questo è solo un esempio di base per mostrare come definire un test. Puoi aggiungere ulteriori test nel file `example_test.dart` o creare nuovi file di test per coprire diverse parti del tuo progetto Flutter.

## Creazione di un repository GitHub

Accedi a *GitHub* e crea un nuovo repository vuoto. Puoi dare al repository un nome significativo e aggiungere una breve descrizione.

## Configurazione delle GitHub Actions

Le *GitHub Actions* consentono di automatizzare varie attività all'interno del tuo repository. Creeremo un file di configurazione per il workflow delle Actions.

All'interno del tuo progetto Flutter, crea una nuova cartella chiamata `.github` nella radice del progetto. All'interno di questa cartella, crea un'altra cartella chiamata `workflows`. 

**Questa struttura di cartelle è richiesta per la corretta configurazione delle GitHub Actions.**

Ora, crea un file YAML chiamato `ci.yaml` (il nome del file è indicativo) all'interno della cartella workflows. Questo file conterrà la configurazione per il tuo workflow di Continuous Integration.

## Definizione del workflow di CI

Apri il file `ci.yaml` appena creato con un editor di testo o un IDE e inizia a definire il tuo workflow di CI utilizzando la sintassi YAML. Il file YAML deve avere una struttura chiave-valore.

Andiamo a inserire il contenuto del file `ci.yaml`:

```yaml
name: Flutter CI

on:
  pull_request:
    branches: [ "master" ]
    
jobs:

  version-check:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Fetch latest changes
        run: git fetch origin master:master > /dev/null 2>&1

      - name: Check Flutter project version
        run: |
          current_version=$(grep 'version:' pubspec.yaml | awk '{print $2}' | tr -d "'")
          previous_version=$(git show "origin/master:pubspec.yaml" | grep 'version:' | awk '{print $2}' | tr -d "'")

          IFS='.' read -r -a current_components <<< "${current_version%+*}"
          IFS='.' read -r -a previous_components <<< "${previous_version%+*}"

          current_build=${current_version##*+}
          previous_build=${previous_version##*+}

          is_major=$((current_components[0] > previous_components[0]))
          is_minor=$((current_components[1] > previous_components[1]))
          is_patch=$((current_components[2] > previous_components[2]))
          is_build=$((current_build > previous_build))

          if [ "$is_build" -eq 0 ]; then
            echo "Error: Flutter project build number must be incremented."
            exit 1
          fi

          if [ "$is_major" -eq 0 ] && [ "$is_minor" -eq 0 ] && [ "$is_patch" -eq 0 ]; then
            echo "Error: Flutter project version has not been incremented."
            exit 1
          fi

          echo "✅ Flutter version updated"

  test:
    needs: version-check
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          channel: "stable"
          cache: true

      - name: Install dependencies
        run: flutter pub get

      - name: Run tests
        run: flutter test

  build-android:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
      
      - name: Setup Java
        uses: actions/setup-java@v2
        with:
          distribution: 'zulu'
          java-version: '11'
      
      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          channel: "stable"
          cache: true
      
      - name: Install dependencies
        run: flutter pub get
      
      - name: Build APK
        run: flutter build apk
      
      - name: Build App Bundle
        run: flutter build appbundle

  build-ios:
    needs: test
    runs-on: macos-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
      
      - name: Setup Flutter
        uses: subosito/flutter-action@v2
        with:
          channel: "stable"
          cache: true
          architecture: x64
      
      - name: Install dependencies
        run: flutter pub get
      
      - name: Build iOS
        run: flutter build ios --release --no-codesign
```

In questo file `ci.yaml`, abbiamo definito quattro job distinti: `version-check`, `test`, `build-android` e `build-ios`.

Il job `version-check` viene eseguito su `ubuntu-latest` e avrà il compito di controllare che la versione e il numero di build dell'applicazione Flutter siano stati aggiornati e aumentati correttamente. **Se questo job fallisce, impedisce l'esecuzione di tutti gli altri jobs.**

Il job `test` viene eseguito su `ubuntu-latest` e comprende i passaggi per eseguire i test del progetto Flutter.

Il job `build-android` viene eseguito su `ubuntu-latest` e **dipende dal job `test`**, quindi aspetterà che il job `test` sia completato prima di iniziare. Andrà ad eseguire la build dell'app-bundle per Android.

Il job `build-ios` viene eseguito su `macos-latest` e **dipende dal job `test`**. Anche lui aspetterà che il job `test` sia completato prima di iniziare per eseguire la build dell'app per iOS.

Assicurati di includere questo file `ci.yaml` nella cartella `.github/workflows` del tuo repository GitHub per attivare e configurare correttamente i workflow.

## Regole di protezione

Per evitare che il codice non testato venga unito al branch `master`, è possibile configurare delle regole di protezione. Queste regole possono essere configurate all'interno delle impostazioni del repository GitHub.

Per configurare le regole di protezione, vai su *Settings* > *Branches* > *Branch protection rules* e crea una nuova regola per il branch `master`.

Assicurati di selezionare le seguenti opzioni:

- Require a pull request before merging
- Require approvals
- Require status checks to pass before merging
- Require branches to be up to date before merging

In questo modo, ogni volta che verrà aperta una pull request, i test verranno eseguiti automaticamente e la pull request non potrà essere unita al branch `master` se i test falliscono.

## Commit delle modifiche e apertura di una pull request

Ora che hai definito il tuo workflow di CI nel file `ci.yaml`, è necessario effettuare il commit delle modifiche e aprire una pull request per includere le tue modifiche nel ramo `master` del repository.

Esegui i seguenti passaggi per effettuare il commit e aprire una pull request:

1. Assicurati di essere nel branch corretto del tuo repository. Puoi verificare il branch attuale utilizzando il comando `git branch` nella tua riga di comando. Se non sei nel branch desiderato, crea un nuovo branch utilizzando il comando `git checkout -b nome_branch`, sostituendo `nome_branch` con il nome appropriato per il tuo branch.

2. Aggiungi i file modificati al commit utilizzando il comando `git add .` per includere tutti i file modificati o specifica i file specifici utilizzando il comando `git add nome_file`.

3. Esegui il commit delle modifiche utilizzando il comando `git commit -m "Descrizione del commit"`, sostituendo `"Descrizione del commit"` con una descrizione significativa delle modifiche apportate.

4. Esegui il push del tuo branch utilizzando il comando `git push origin nome_branch`, dove `nome_branch` è il nome del tuo branch.

5. Vai al repository GitHub e apri la pagina del tuo branch. Dovresti vedere un avviso che ti consiglia di aprire una pull request per includere le tue modifiche nel ramo `master`.

6. Clicca sul pulsante "Compare & pull request" per aprire una nuova pull request. Assicurati di fornire una descrizione accurata dei cambiamenti apportati nella pull request.

7. Una volta aperta la pull request, il tuo workflow di CI sarà attivato automaticamente e i test verranno eseguiti. Potrai visualizzare lo stato della build all'interno della scheda "Checks" o "Actions" della tua pull request. Sarà possibile vedere se i test hanno avuto successo o se sono state rilevate problematiche.

## Conclusioni

Hai appena effettuato il commit delle modifiche e aperto una pull request per includere le tue modifiche nel ramo `master` del repository 🥳

Il tuo workflow di CI verrà attivato automaticamente e i test verranno eseguiti per garantire che il tuo codice sia **stabile**. 

Ora puoi visualizzare lo stato della build e le eventuali problematiche all'interno della tua pull request su GitHub. 

Questo approccio ti permette di mantenere il tuo progetto Flutter stabile e affidabile, garantendo che le modifiche siano testate prima di essere integrate nel ramo principale.