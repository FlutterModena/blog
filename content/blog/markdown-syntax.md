---
author: "Carmine Zaccagnino"
title: "Benvenuti!"
description: "Post che presenta e fornisce le prime indicazioni su come contribuire al blog."
tags: ["community", "blog", "presentazione", "flutter modena"]
date: 2022-12-31
thumbnail: /fluttermodena.png
---

Ecco qua il blog di Flutter Modena!

In questo primo post presenterò il blog e come abbiamo intenzione di organizzarci per fare in modo tale che possa allo stesso tempo permettere a tutti di sentirsi parte integrante di esso e di realizzare contenuti che il mondo sia interessato a leggere.

# L'idea

La quantità e qualità di risorse su Flutter in italiano presenti online è molto scarsa, e noi siamo una community di persone interessate a Flutter, e nel primo meetup è emersa tanta voglia di contribuire in qualche modo alla community. Viste queste premesse, sembra anche fin troppo ovvio che far partire un blog in cui i membri della nostra community possano condividere qualcosa con il mondo sia un'ottima idea!

Ognuno dei membri della community sarà, quindi, in grado di contribuire al blog in due modi:

* proponendo idee per post che vorrebbe vedere sul blog;
* scrivendo post da pubblicare, basati su idee nuove o su quelle che sono già state proposte e su cui non è ancora stato scritto un post.

# L'organizzazione

Questo non è il blog personale di uno di noi, quindi è necessario che ci sia qualche forma di coordinazione tra i membri, e che ci sia qualche processo mediante il quale si assicura che i post che finiscono sul blog sono di qualità sufficiente che chiunque legga i post del blog possa fidarsi che i contenuti sono corretti e scritti abbastanza bene.

Il proesso qui descritto è il primo che tenteremo di usare, poi valuteremo se renderlo più o meno formale in base ad eventuali difficoltà che incontreremo in futuro.

La struttura assomiglierà a quella che viene utilizzata da un blog un po' più grande del nostro, ovvero [Fedora Magazine](https://fedoramagazine.org/), che è il blog ufficiale della community Fedora, anche se con un po' di variazioni per adattarla alla nostra community.

Al fine di coordinare il tutto, sarà necessario avere degli editor che fanno in modo che l'afflusso di idee e di post sia ordinato. Inizialmente questo gruppo include gli organizzatori della community, ma il nostro obiettivo è includere in questo gruppo chiunque mostri un interesse e una propensione a contribuire attivamente alla community, e in particolare al blog.

Visto che ci sono due modi per contribuire, questa sezione è divisa a seconda del tipo di contributo.

## Richiesta post su un argomento

Per richiedere un post su un argomento, basta [aprire una issue sulla pagina GitHub del blog](https://github.com/FlutterModena/blog/issues), descrivendo (il template è un suggerimento) l'argomento e il post che si vorrebbe vedere sul sito.

A questo punto, se l'idea è ritenuta interessante e coerente con i contenuti del blog, verrà spostata sulla [board del progetto GitHub del blog](https://github.com/orgs/FlutterModena/projects/1), che è il punto di arrivo per le idee e il punto di partenza per i post, come vedremo nella prossima sezione!

## Realizzazione post per il blog

Chiunque voglia scrivere un post per il blog, può iniziare guardando [la sezione contenente gli articoli da scrivere della board che ho appena menzionato](https://github.com/orgs/FlutterModena/projects/1), e chiedere di poter scrivere un post. Tranne che in casi eccezionali, questo non vuol dire prendersi un impegno particolare di scrivere il post con un certo livello di qualità o in un certo periodo di tempo, ma semplicemente esprimere l'intenzione di farlo e quindi poter "prenotare" un argomento.

Se dovessero esserci molte più persone che vogliono contribuire che idee di post e dovessero esserci molte idee occupate da molto tempo senza segni di progressi significativi, allora sembra abbastanza ovvio che si renderebbe necessario di liberare una di quelle idee o di sollecitare affinché l'idea diventi un post.

Per ulteriori dettagli sul formato in cui scrivere il post e su come "consegnarlo", consulta la sezione [dedicata a questo tema](#aggiungere-un-post-al-blog) più avanti nei dettagli tecnici.

Una volta realizzato un post, questo verrà spostato nella fase "In revisione", in cui gli editor si prendono cura di assicurarsi che tutti i contenuti previsti siano inclusi e che il post sia di qualità sufficiente. Potrà essere richiesta una modifica all'autore del post, oppure potranno essere fatte piccole modifiche (ad esempio per correggere errori ortografici e grammaticali) direttamente dagli editor prima di pubblicare il post.

# Dettagli tecnici

Il blog è realizzato con Hugo, che è un generatore di siti statici. Il codice sulla base del quale è generato il sito è ospitato [in questa repository](https://github.com/FlutterModena/blog), ed è fatto (al netto della configurazione) di file Markdown nella cartella `content`, ognuno dei quali rappresenta una pagina del sito finale.

## Aggiungere un post al blog

Come ho appena detto, ogni post è un file Markdown. Per chi non avesse familiarità con questo formato, è un formato di markup molto semplice da imparare e molto meno "ingombrante" dell'HTML. Per imparare ad usarlo, potete seguire una delle tante [guide online](https://www.markdownguide.org/tools/hugo/), oppure semplicemente guardare nella repository o online qualche esempio di post. Potete anche guardare il codice sorgente dei README di qualunque progetto GitHub.

### Formato dei post

I file Markdown dei post del blog [sono in content/blog nella repository del blog](https://github.com/FlutterModena/blog/tree/main/content/blog), e quindi lì dovrete aggiungere eventuali post che volete contribuire.

Ogni post inizia con un'intestazione come questa:

```
---
author: "Carmine Zaccagnino"
title: "Benvenuti!"
description: "Post che presenta e fornisce le prime indicazioni su come contribuire al blog."
tags: ["community", "blog", "presentazione", "flutter modena"]
date: 2022-12-31
thumbnail: /fluttermodena.png
---
```

in cui andranno inseriti i dati per il post che volete scrivere. Per tag, thumbnail e descrizione potete anche lasciar fare agli editor. La data verrà cambiata per riflettere l'effettiva data di pubblicazione.

Dopo l'intestazione, dovrete aggiungere il contenuto, scritto in Markdown, del post.

### Come effettivamente contribuire alla repository

Tutti quelli di voi che non hanno mai contribuito ad un progetto open-source su GitHub potrebbero non conoscere come fare a contribuire ad una repository su cui non si hanno i permessi di push.

Il processo è facile, anche se un po' articolato. Potete sempre chiedere aiuto per questa (o qualsiasi altra) fase.

La prima cosa da fare è **effettuare il fork** della repository su GitHub, premendo l'apposito bottone in alto a destra nella pagina della repository su GitHub. Questo creerà una copia della repository nel vostro account, a cui voi potrete apportare tutte le modifiche che volete.

Una volta effettuate le modifiche desiderate a quella copia (nel caso specifico del blog, aggiungendo il file del post che volete contribuire), dovrete **aprire una Pull Request** nella repository principale (ovvero [sempre questa](https://github.com/FlutterModena/blog)), che permetterà a chi ha permessi di push sulla repository principali di integrare le modifiche da voi effettuate dopo averle controllate.

## Altri dettagli tecnici

Tutto quello che interessa specificamente chi vuole contribuire al blog dal punto di vista dei contenuti è finito. Per quelli a cui invece interessano i dettagli di deployment (e che magari invece vogliono contribuire a quegli aspetti), ecco il modo in cui funziona la generazione e il deployment del blog.

Quando le modifiche effettuate sono ritenute pronte per la pubblicazione, quel branch viene pullato sul VPS su cui è ospitato il blog che, per la parte che riguarda il blog, è semplicemente un web server NGINX che serve file statici  in esecuzione su CentOS Stream 8. Il sito viene generato direttamente sul server.

Una possibile modifica sarebbe prevedere l'uso di GitHub Actions ed automatizzare il deployment, ma questo potrebbe rendere più facile pubblicare accidentalmente il blog in uno stato non pronto per la pubblicazione.

# Conclusione

Speriamo che le decisioni non abbiano fatto passare la voglia di contribuire a nessuno, e siamo pronti ad ascoltare proposte di miglioramento o ad aiutare a capire come contribuire. Speriamo di poter costruire insieme un blog attivo, utile e di successo!
