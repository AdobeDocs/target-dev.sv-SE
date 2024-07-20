---
title: Skicka visnings- eller klickmeddelanden till  [!DNL Adobe Target] med Java SDK
description: Lär dig hur du använder sendNotifications() för att skicka visnings- eller klickmeddelanden till  [!DNL Adobe Target]  för mätning och rapportering.
feature: APIs/SDKs
exl-id: 9231b480-f50f-40d1-ab06-0b9f2a2d79e3
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Skicka meddelanden (Java)

## Beskrivning

`sendNotifications()` används för att skicka visnings- eller klickmeddelanden till [!DNL Adobe Target] för mätning och rapportering.

>[!NOTE]
>
>När ett `execute`-objekt med obligatoriska parametrar finns i själva begäran, ökas intrycket automatiskt för kvalificerande aktiviteter.

SDK-metoder som automatiskt ökar ett intryck är:

* `getOffers()`
* `getAttributes()`

När ett `prefetch`-objekt skickas i begäran ökas inte intrycket automatiskt för aktiviteter med rutor i `prefetch`-objektet. `sendNotifications()` måste användas för förhämtade upplevelser för att öka antalet visningar och konverteringar.

## Metod

### skapa

```javascript {line-numbers="true"}
ResponseStatus TargetClient.sendNotifications(TargetDeliveryRequest request)
```

## Exempel

Först skapar vi [!DNL Target Delivery API]-begäran om förhämtning av innehåll för mrutorna `home` och `product1`.

### Förhämtning

```javascript {line-numbers="true"}
List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("home").index(1));
mboxRequests.add((MboxRequest) new MboxRequest().name("product1").index(2));
PrefetchRequest prefetchMboxesRequest = new PrefetchRequest().setMboxes(mboxRequests)

// Next, we fetch the offers via Target Java SDK getOffers() API
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```

Ett godkänt svar kommer att innehålla ett [!UICONTROL Target Delivery API]-svarsobjekt, som innehåller förhämtat innehåll för de begärda rutorna. Ett `targetResponse.response`-exempelobjekt kan se ut så här:

### Svar

```javascript {line-numbers="true"}
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

Observera fälten mbox `name`, `state` och `eventToken` i alla [!DNL Target] -innehållsalternativ. Dessa bör anges i `sendNotifications()`-begäran så snart varje innehållsalternativ visas. Låt oss anta att mbox `product1` har visats på en annan enhet än webbläsaren. Begäran om meddelanden ser ut så här:

### Begäran

```javascript {line-numbers="true"}
TargetDeliveryRequest mboxNotificationRequest = TargetDeliveryRequest.builder().notifications(new ArrayList() {{
    add(new Notification()
            .id("1")
            .timestamp(System.currentTimeMillis())
            .type(MetricType.DISPLAY)
            .mbox(new NotificationMbox()
                    .name("product1")
                    .state("J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U")
            )
            .tokens(Arrays.asList(new String[]{"t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="}))
    );
}}).build();
```

Observera att vi har inkluderat både mbox-tillståndet och den händelsetoken som motsvarar erbjudandet [!DNL Target] som levererades i förhämtningssvaret. När vi har skapat meddelandebegäran kan vi skicka den till [!DNL Target] via API-metoden `sendNotifications()`:

### Svar

```javascript {line-numbers="true"}
ResponseStatus notificationResponse = targetJavaClient.sendNotifications(mboxNotificationRequest);
```
