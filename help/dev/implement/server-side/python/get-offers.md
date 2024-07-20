---
title: Använd getOffers() i [!DNL Adobe Target] när du använder Python SDK
description: Lär dig hur du använder getOffers() för att köra ett beslut och hämta en upplevelse från  [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 9539b806-e070-430e-80cf-cf632ce3f207
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# Erbjudanden (Python)

## Beskrivning

`get_offers()` används för att köra ett beslut och hämta en upplevelse från [!DNL Adobe Target].


## Metod

### getOffers

```python {line-numbers="true"}
target_client_instance.get_offers(options)
```

## Parametrar

`options`-ordlistan har följande struktur:

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- | --- | --- | --- | --- |
| förfrågan | DeliveryRequest | Ja | Ingen | Följer begäran [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) |
| target_cookie | str | no | Ingen | [!DNL Target]-cookie |
| target_location_hint | str | no | Ingen | Platstips för [!DNL Target] |
| Consumer_id | str | no | Ingen | Olika konsument-ID bör anges när flera samtal sammanfogas |
| customer_ids | list[CustomerId] | no | Ingen | En lista med kund-ID:n i VisitorId-kompatibelt format |
| session_id | str | no | Ingen | Används för att länka flera begäranden |
| callback | anropbar | no | Ingen | Om begäran hanteras asynkront anropas återanropet när svaret är klart |
| err_callback | anropbar | no | Ingen | Om begäran hanteras asynkront, anropas felåteranrop när undantaget utlöses |

## Returnerar

Returnerar en `TargetDeliveryResponse` om den anropas synkront (standard), eller en `AsyncResult` om den anropas med ett återanrop. `TargetDeliveryResponse` har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| svar | Leveranssvar | Följer svaret på [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) |
| target_cookie | dict | [!DNL Target]-cookie |
| target_location_hint_cookie | dict | Webbplatstipscookie för [!DNL Target] |
| analytics_details | list[AnalyticsResponse] | Analysens nyttolast, om Analytics används på klientsidan |
| trace | list[dict] | Sammanlagda spårningsdata för alla begärandemrutor/vyer |
| response_tokens | list[dict] | En lista med &#x200B;[svarstoken](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) |
| meta | dict | Ytterligare metadata för beslutsfattande som kan användas med enhetsspecifik beslutsfattande |

`target_cookie`- och `target_location_hint_cookie`-objekt som används för att skicka data tillbaka till webbläsaren har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| name | str | Kaknamn |
| value | alla | Cookie-värde, värdet konverteras till sträng |
| max_age | int | `max_age option` är ett bekvämt sätt att ange förfaller i förhållande till den aktuella tiden i sekunder |

Objektet `meta` som används för att ange status för målsvaret har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| decisioning_method | str | Vilken beslutsmetod som användes: på enheten eller på serversidan |
| remote_mboxes | lista `[str]` | När metoden för beslutsfattande är `on-device` anges en matris med mbox-namn som inte helt kunde bestämmas på enheten. Med andra ord krävs en [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)-begäran. |
| remote_views | lista `[str]` | När metoden för beslutsfattande är på enheten anges en array med visningsnamn som inte helt kunde bestämmas på enheten. Med andra ord krävs en [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)-begäran. |

## Exempel

### Python

```python {line-numbers="true"}
def client_ready_callback():
    context = Context(channel=ChannelType.WEB)
    mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_offers_options = {
      "request": delivery_request
    }

    target_delivery_response = target_client.get_offers(get_offers_options)


client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
