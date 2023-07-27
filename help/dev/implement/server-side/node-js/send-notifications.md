---
title: Skicka visnings- eller klickmeddelanden till [!DNL Adobe Target] med Node.js SDK
description: Lär dig hur du använder sendNotifications() för att skicka visnings- eller klickmeddelanden till [!DNL Adobe Target] för mätning och rapportering.
feature: APIs/SDKs
exl-id: 84bb6a28-423c-457f-8772-8e3f70e06a6c
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---

# Skicka meddelanden (Node.js)

## Beskrivning

`[!UICONTROL sendNotifications()]` används för att skicka visnings- eller klickmeddelanden till [!DNL Adobe Target] för mätning och rapportering.

>[!NOTE]
>
>När en `execute` om ett objekt med obligatoriska parametrar finns i själva begäran, ökas intrycket automatiskt för kvalificerande aktiviteter.

SDK-metoder som automatiskt ökar ett intryck är:

* `getOffers()`
* `getAttributes()`

När en `prefetch` -objektet skickas i begäran, intrycket ökas inte automatiskt för aktiviteter med rutor i `prefetch` -objekt. `sendNotifications()` måste användas för förhämtade upplevelser för att öka antalet visningar och konverteringar.

## Metod

### skapa

```js {line-numbers="true"}
TargetClient.sendNotifications(options: Object): Promise
```

### Parametrar

`options` har följande struktur:

| Namn | Typ | Obligatoriskt | Standard |
| --- | --- | --- | --- |
| förfrågan | Objekt | Ja | Ingen |

## Exempel

Först bygger vi mål-Dleverera API-begäran för förhämtning av innehåll för `home` och `product1` mboxes.

### Node.js

```js {line-numbers="true"}
const prefetchMboxesRequest = {
  prefetch: {
    mboxes: [
      { name: "home" },
      { name: "product1" }
    ]
  }
};
// Next, we fetch the offers via Target Node.js SDK getOffers() API
const targetResponse = await targetClient.getOffers({ request: prefetchMboxesRequest });
```

Ett godkänt svar innehåller en [!UICONTROL Target Delivery API] svarsobjekt, som innehåller förhämtat innehåll för de begärda rutorna. Ett exempel `targetResponse.response` -objektet kan se ut så här:

### Node.js

```js {line-numbers="true"}
{
  "status": 200,
  "requestId": "e8ac2dbf5f7d4a9f9280f6071f24a01e",
  "id": {
    "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "client": "adobetargetmobile",
  "edgeHost": "mboxedge21.tt.omtrdc.net",
  "prefetch": {
    "mboxes": [
      {
        "index": 0,
        "name": "home",
        "options": [
          {
            "type": "html",
            "content": "HOME OFFER",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      },
      {
        "index": 1,
        "name": "product1",
        "options": [
          {
            "type": "html",
            "content": "TEST OFFER 1",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      }
    ]
  }
}
```

Lägg märke till mbox `name` och `state` fält, samt `eventToken` -fält, i var och en av [!DNL Target] innehållsalternativ. Dessa bör anges i `sendNotifications()` begär, så snart varje innehållsalternativ visas. Låt oss anta att `product1` mbox har visats på en icke-webbläsarenhet. Begäran om meddelanden visas enligt följande:

### Node.js

```js {line-numbers="true"}
const mboxNotificationRequest = {
  notifications: [{
    id: "1",
    timestamp: Date.now(),
    type: "display",
    mbox: {
      name: "product1",
      state: "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
    },
    tokens: [ "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==" ]
  }]
};
```

Observera att vi har inkluderat både mbox-läget och händelsetoken som motsvarar [!DNL Target] Erbjudandet levereras som ett prefetch-svar. När vi har skapat meddelandebegäran kan vi skicka den till [!DNL Target] via `sendNotifications()` API-metod:

### Node.js

```js {line-numbers="true"}
const notificationResponse = await targetClient.sendNotifications({ request: mboxNotificationRequest });
```
