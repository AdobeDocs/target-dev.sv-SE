---
title: Skicka visnings- eller klickmeddelanden till  [!DNL Adobe Target] med Python SDK
description: Lär dig hur du använder sendNotifications() för att skicka visnings- eller klickmeddelanden till  [!DNL Adobe Target]  för mätning och rapportering.
feature: APIs/SDKs
exl-id: 03827b18-a546-4ec8-8762-391fcb3ac435
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# Skicka meddelanden (Python)

## Beskrivning

`send_notifications()` används för att skicka visnings- eller klickmeddelanden till [!DNL Adobe Target] för mätning och rapportering.

>[!NOTE]
>
>När ett `execute`-objekt med obligatoriska parametrar finns i själva begäran, ökas intrycket automatiskt för kvalificerande aktiviteter.

SDK-metoder som automatiskt ökar intrycket är:

* `get_offers()`
* `get_attributes()`

När ett `prefetch`-objekt skickas i begäran ökas inte intrycket automatiskt för aktiviteter med rutor i `prefetch`-objektet. `Send_notifications()` måste användas för förhämtade upplevelser för att öka antalet visningar och konverteringar.

## Metod

### send_notifications

```python {line-numbers="true"}
target_client.send_notifications(options)
```

## Parametrar

`options` har följande struktur:

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- | --- | --- | --- | --- |
| förfrågan | DeliveryRequest | Ja | Ingen | Följer begäran [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) |
| target_cookie | str | no | Ingen | [!DNL Target]-cookie |
| target_location_hint | str | no | Ingen | Platstips för [!DNL Target] |
| Consumer_id | str | no | Ingen | Olika konsument-ID bör anges när flera samtal sammanfogas |
| customer_ids | list[CustomerId] | no | Ingen | En lista med kund-ID:n i VisitorId-kompatibelt format |
| session_id | str | no | Ingen | Används för att länka flera begäranden |
| callback | anropbar | no | Ingen | Om begäran hanteras asynkront anropas återanropet när svaret är klart |
| err_callback | anropbar | no | Ingen | Om begäran hanteras asynkront, anropas felåteranrop när undantaget utlöses |

## Returnerar

`Returns` a `TargetDeliveryResponse` om det anropas synkront (standard), eller en `AsyncResult` om den anropas med ett återanrop. `TargetDeliveryResponse` har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| svar | Leveranssvar | Följer svaret på [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) |
| target_cookie | dict | [!DNL Target]-cookie |
| target_location_hint_cookie | dict | Webbplatstipscookie för [!DNL Target] |
| analytics_details | list[AnalyticsResponse] | [!DNL Analytics] nyttolast, om [!DNL Analytics] används på klientsidan |
| trace | list[dict] | Sammanlagda spårningsdata för alla begärandemrutor/vyer |
| response_tokens | list[dict] | En lista med [ &#x200B; svarstoken ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) |
| meta | dict | Ytterligare metadata för beslutsfattande som kan användas med enhetsspecifik beslutsfattande |

## Exempel

Först skapar vi [!UICONTROL Target Delivery API]-begäran om förhämtning av innehåll för mrutorna `home` och `product1`.

### Python

```python {line-numbers="true"}
mboxes = [MboxRequest(name="home"),
          MboxRequest(name="product1")]
prefetch = PrefetchRequest(mboxes=mboxes)
delivery_request = DeliveryRequest(prefetch=prefetch)

# Next, we fetch the offers via Target Python SDK getOffers() API
response = target_client.get_offers({ "request": delivery_request })
```

Ett godkänt svar kommer att innehålla ett [!UICONTROL Target Delivery API]-svarsobjekt, som innehåller förhämtat innehåll för de begärda rutorna. Ett exempel på ett `target_response["response"]`-objekt (formaterat som en ordlista) kan se ut så här:

### Python

```python {line-numbers="true"}
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

Observera fälten mbox `name`, `state` och `eventToken` i alla alternativ för målinnehåll. Dessa bör anges i `send_notifications()`-begäran så snart varje innehållsalternativ visas. Låt oss anta att mbox `product1` har visats på en annan enhet än webbläsaren. Begäran om meddelanden visas enligt följande:

### Python

```python {line-numbers="true"}
notification_mbox = NotificationMbox(name="product1",
                                     state="J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U")
notification = Notification(
    id="1",
    type=MetricType.DISPLAY,
    timestamp=1621530726000,  # Epoch time in milliseconds
    mbox=notification_mbox,
    tokens=["t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="]
)
notification_request = DeliveryRequest(notifications=[notification])
```

Observera att vi har inkluderat både mbox-tillståndet och den händelsetoken som motsvarar erbjudandet [!DNL Target] som levererades i förhämtningssvaret. När vi har skapat meddelandebegäran kan vi skicka den till [!DNL Target] via API-metoden `send_notifications()`:

### Python

```python {line-numbers="true"}
response = target_client.send_notifications({ "request": notification_request })
```
