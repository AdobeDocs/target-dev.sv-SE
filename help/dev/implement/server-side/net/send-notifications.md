---
title: Skicka visnings- eller klickmeddelanden till  [!DNL Adobe Target] med .NET SDK
description: Lär dig hur du använder sendNotifications() för att skicka visnings- eller klickmeddelanden till  [!DNL Adobe Target]  för mätning och rapportering.
feature: APIs/SDKs
exl-id: 724e787c-af53-4152-8b20-136f7b5452e1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---

# Skicka meddelanden (.NET)

## Beskrivning

`SendNotifications()` används för att skicka visnings- eller klickmeddelanden till [!DNL Adobe Target] för mätning och rapportering.

>[!NOTE]
>
>När ett `Execute`-objekt med obligatoriska parametrar finns i själva begäran, ökas intrycket automatiskt för kvalificerande aktiviteter.

SDK-metoder som automatiskt ökar ett intryck är:

* `GetOffers()`
* `GetAttributes()`

När ett `Prefetch`-objekt skickas i begäran ökas inte intrycket automatiskt för aktiviteter med rutor i `Prefetch`-objektet. `SendNotifications()` måste användas för förhämtade upplevelser för att öka antalet visningar och konverteringar.

## Metod

### Skapa

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.SendNotifications(TargetDeliveryRequest request)
```

## Exempel

Först skapar vi [!UICONTROL Target Delivery API]-begäran om förhämtning av innehåll för mrutorna `home` och `product1`.

### \.NET

```dotnet {line-numbers="true"}
var mboxRequests = new List<MboxRequest>
    {
        new (index: 1, name: "home"),
        new (index: 2, name: "product1"),
    };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .SetPrefetch(new PrefetchRequest(mboxes: mboxRequests))
    .Build();

// Next, we fetch the offers via Target .NET SDK GetOffers() API
var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
```

Ett godkänt svar kommer att innehålla ett [!DNL Target Delivery API]-svarsobjekt, som innehåller förhämtat innehåll för de begärda rutorna. Ett exempel på `targetResponse.Response`-objekt kan se ut så här:

### \.NET

```dotnet {line-numbers="true"}
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

Observera fälten `mbox` name och `state` samt fältet `eventToken` i alla [!DNL Target]-innehållsalternativ. Dessa bör anges i `SendNotifications()`-begäran så snart varje innehållsalternativ visas. Låt oss anta att mbox `product1` har visats på en annan enhet än webbläsaren. Begäran om meddelanden visas enligt följande:

### \.NET

```dotnet {line-numbers="true"}
var mboxNotifications = new List<Notification>
{
    new (id: "1", type: MetricType.Display, timestamp: DateTimeOffset.UtcNow.ToUnixTimeMilliseconds(),
        mbox: new NotificationMbox("product1", "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"),
        tokens: new List<string> { "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==" })
}; 

var mboxNotificationRequest = new TargetDeliveryRequest.Builder()
    .SetNotifications(mboxNotifications)
    .Build();
```

Observera att vi har inkluderat både mbox-tillståndet och den händelsetoken som motsvarar erbjudandet [!DNL Target] som levererades i förhämtningssvaret. När vi har skapat meddelandebegäran kan vi skicka den till [!DNL Target] via API-metoden `SendNotifications()`:

### \.NET

```dotnet {line-numbers="true"}
var notificationResponse = targetClient.SendNotifications(mboxNotificationRequest);
```
