---
author: "Elia Tolin"
title: "Status Vaccini"
description: "La prima applicazione italiana di monitoraggio della campagna vaccinale COVID19."
tags: ["vaccini", "app", "esperienze"]
date: 2023-05-02
thumbnail: https://www.auroradigital.it/wp-content/uploads/2022/10/111628344-687f3500-87f0-11eb-9c74-88e804c07da9-1024x513.png
latitude: 40.90025460
longitude: 14.27727490
altitude: 0.0000
---

In piena pandemia, Marzo 2021, si è voluto dare un servizio totalmente gratuito e senza scopo di lucro. 🦠\
In quel momento difficile si è deciso di fornire ai cittadini italiani un’applicazione per il monitoraggio della campagna vaccinale nel nostro paese, chiamata Status Vaccini. Uno degli obbiettivi sicuramente è stato la trasparenza dei dati mostrati e quindi anche questo progetto è OpenSource.

Forse ne avrai già sentito parlare dato il successo mediatico ricevuto tramite giornali e telegiornali, e anche di questo ne parleremo.

L’applicazione è stata scritta ovviamente tramite il nostro amato Flutter. ❤️

## UI/UX

L’obbiettivo è stato di fornire un interfaccia semplice e banale in quanto l’applicazione è destinata a qualsiasi target d’età e di capacità informatiche. E’ quindi stato scelto lo stile Material.

All’interno dell’applicazione è possibile vedere ogni singola informazione della campagna vaccinale, in forma numerica e sotto forma di grafico.

Per rendere Status Vaccini più scorrevole e semplice le informazioni sono state raggruppate e inserite all’interno di card o in modo da favorire la libertà di posizionamento all’interno dello screen.\
Sempre per dare un’informazione chiara e coincisa ai nostri utenti.

![](https://www.auroradigital.it/wp-content/uploads/2022/10/screenshot4-512x1024.jpeg) ![](https://www.auroradigital.it/wp-content/uploads/2022/10/screenshot3.jpeg) ![](https://www.auroradigital.it/wp-content/uploads/2022/10/screenshot2.jpeg)

All’interno dell’applicazione inoltre è stata sviluppata una sezione dove l’utente può selezionare la propria regione e verificare l’andamento rispetto ad essa. Questa feature, è stata una delle richieste degli utenti.

## OPEN DATA NAZIONALI

Ovviamente i dati da qualche parte gli siamo andati a pescare: sicuramente non potevamo inventarceli. 

Un progetto bellissimo a nostro parere, che ha contribuito all’esistenza di StatusVaccini sono stati gli OpenData Nazionali riguardo all’andamento della campagna vaccinale, che ogni giorno vengono costantemente aggiornati. 

L’applicazione ha una parte di backend che all’avvio del processo controlla se ci sono stati aggiornamenti riguardo ai dati rispetto a quelli salvati in cache sul dispositivo; Se sì la risposta ovviamente si procede all’aggiornamento automatico delle informazioni da mostrare.

Ogni informazione mostrata quindi non subisce elaborazione, se non ovviamente calcolo banale per dare statistiche matematiche rispetto ai giorni precedenti.

![](https://www.auroradigital.it/wp-content/uploads/2022/10/Simulator-Screen-Shot-iPhone-12-Pro-Max-2021-04-28-at-22.09.46-504x1024.png) ![](https://www.auroradigital.it/wp-content/uploads/2022/10/Simulator-Screen-Shot-iPhone-12-Pro-Max-2021-04-28-at-22.10.11-503x1024.png) ![](https://www.auroradigital.it/wp-content/uploads/2022/10/Simulator-Screen-Shot-iPhone-12-Pro-Max-2021-04-28-at-22.11.37-503x1024.png)
![](https://www.auroradigital.it/wp-content/uploads/2022/10/Simulator-Screen-Shot-iPhone-12-Pro-Max-2021-04-28-at-22.11.53-1-503x1024.png) ![](https://www.auroradigital.it/wp-content/uploads/2022/10/Simulator-Screen-Shot-iPhone-12-Pro-Max-2021-04-28-at-22.12.07-505x1024.png) ![](https://www.auroradigital.it/wp-content/uploads/2022/10/Simulator-Screen-Shot-iPhone-12-Pro-Max-2021-04-28-at-22.12.15-498x1024.png)


## Giornali e telegiornali

La creazione di Status Vaccini ha portato l'attenzione mediatica di diverse testate giornalistiche, dove con vivo interesse hanno intervistato il nostro founder Elia Tolin.

Vi lasciamo qualche articolo e intervista dei più importanti titoli giornalistici.


* [TRC Modena 📺](https://www.youtube.com/watch?v=-vB_dPz5yaM)
* [Il Resto del Carlino 📰](https://www.ilrestodelcarlino.it/cronaca/vaccino-covid-app-1.6948402)
* [Gazzetta di Modena 📰](https://www.gazzettadimodena.it/modena/cronaca/2021/05/07/news/l-idea-di-elia-l-applicazione-che-dice-tutto-sui-vaccini-1.40246570)
* [ModenaToday 📰](https://www.modenatoday.it/attualita/app-monitoraggio-vaccini-italia-modena-3-maggio-2021.html)
* [LaPressa 📰](https://www.lapressa.it/articoli/societa/status-vaccini)
* [TgRoseto 📰](http://tgroseto.it/2021/10/elia-tolin-informatica-a-disposizione-del-covid-19/)
* [Sanremo News 📰](https://www.sanremonews.it/2021/05/23/leggi-notizia/argomenti/altre-notizie/articolo/status-vaccini-lapp-che-informa-sulla-campagna-vaccinale-anti-covid-sviluppata-dal-bordig.html)
* [Redacon 📰](https://www.redacon.it/2021/06/04/lideatore-dellapp-che-rivela-tutto-sui-vaccini-e-di-baiso-e-si-chiama-elia-tolin/)
* [Sassuolo 2000 📺](https://www.youtube.com/watch?v=8hlf3ByHEu0)
