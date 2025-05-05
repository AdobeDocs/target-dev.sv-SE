---
title: Använd [!UICONTROL getOffers()] i [!DNL Adobe Target] när du använder Node.js SDK
description: Lär dig hur du använder [!UICONTROL getOffers()] för att köra ett beslut och hämta en upplevelse från  [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 3c4125ea-68d4-405e-9b9a-5fa832743153
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# [!UICONTROL Get Offers] (Node.js)

## Beskrivning

`[!UICONTROL getOffers()]` används för att köra ett beslut och hämta en upplevelse från [!DNL Adobe Target].


## Metod

### getOffers

```js {line-numbers="true"}
TargetClient.getOffers(options: Object): Promise
```

## Parametrar

Objektet `options` har följande struktur:

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- |--- | --- | --- | --- |
| Begäran | Objekt | Ja | Ingen | Följer [[!DNL Target] leverans-API](/help/dev/implement/delivery-api/overview.md)-begäran |
| visitorCookie | Sträng | Nej | Ingen | ECID-cookie (VisitorId) |
| targetCookie | Sträng | Nej | Ingen | [!DNL Target]-cookie |
| targetLocationHint | Sträng | Nej | Ingen | Platstips för [!DNL Target] |
| ConsumerId | Sting | Nej | Ingen | ConsumerIds för [!UICONTROL Analytics for Target] (A4T) sammanfogning |
| CustomerIds | Array | Nej | Ingen | Kund-ID i VisitorId-kompatibelt format |
| sessionId | Sträng | Nej | Ingen | Används för att länka flera [!DNL Target]-begäranden |
| besökare | Objekt | Nej | new VisitorId | Ange en extern VisitorId-instans |

## Lova

`Promise` som returnerades har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| förfrågan | Objekt | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) begäran |
| svar | Objekt | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) svar |
| visitorState | Objekt | Objekt som ska skickas till Visitor API `getInstance()` |
| targetCookie | Objekt | [!DNL Target]-cookie |
| targetLocationHintCookie | Objekt | Webbplatstipscookie för [!DNL Target] |
| analyticsDetails | Array | Analysens nyttolast, om Analytics används på klientsidan |
| responseTokens | Array | En lista med [svarstoken](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=sv-SE&). |
| trace | Array | Sammanlagda spårningsdata för alla begärandemrutor/vyer |
| status | Objekt | Ett objekt som innehåller svarsstatus. |
| decisioningMethod | Sträng | Avgör vilken beslutsmetod som ska användas ([på enheten](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md), på serversidan, hybrid) |

`targetCookie`- och `targetLocationHintCookie`-objekt som används för att skicka data tillbaka till webbläsaren har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| name | Sträng | Kaknamn |
| value | Alla | Cookie-värde, konverteras de till sträng |
| maxAge | Nummer | Alternativet `maxAge` är ett bekvämt sätt att ange förfallodatum i förhållande till den aktuella tiden i sekunder |

Objektet `status` som används för att ange status för målsvaret har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| status | Nummer | HTTP-statuskod |
| message | Sträng | Ett meddelande om svaret. Den kan till exempel visa om svaret avgjordes [på enheten](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md) eller på serversidan |
| remoteMboxes | Array | När metoden för beslutsfattande är `on-device` anges en matris med mbox-namn som inte helt kunde bestämmas på enheten. Med andra ord krävs en [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)-begäran. |

## Exempel

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

const request = {
    context: {channel: "web"},
    execute: {
        mboxes: [{
            name: "a1-serverside-ab",
            index: 1
        }]
}};

const response = await targetClient.getOffers({ request });
```
