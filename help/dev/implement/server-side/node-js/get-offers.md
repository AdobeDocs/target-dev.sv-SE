---
title: Använd [!UICONTROL getOffers()] in [!DNL Adobe Target] när du använder Node.js SDK
description: Lär dig använda [!UICONTROL getOffers()] att fatta ett beslut och hämta en upplevelse från [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 3c4125ea-68d4-405e-9b9a-5fa832743153
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 0%

---

# [!UICONTROL Get Offers] (Node.js)

## Beskrivning

`[!UICONTROL getOffers()]` används för att verkställa ett beslut och hämta en upplevelse från [!DNL Adobe Target].


## Metod

### getOffers

```js {line-numbers="true"}
TargetClient.getOffers(options: Object): Promise
```

## Parametrar

The `options` objektet har följande struktur:

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- |--- | --- | --- | --- |
| Begäran | Objekt | Ja | Ingen | Följer [[!DNL Target] Leverans-API](/help/dev/implement/delivery-api/overview.md) förfrågan |
| visitorCookie | Sträng | Nej | Ingen | ECID-cookie (VisitorId) |
| targetCookie | Sträng | Nej | Ingen | [!DNL Target] cookie |
| targetLocationHint | Sträng | Nej | Ingen | [!DNL Target] platstips |
| ConsumerId | Sting | Nej | Ingen | ConsumerIds for [!UICONTROL Analytics for Target] (A4T) stygn |
| CustomerIds | Array | Nej | Ingen | Kund-ID i VisitorId-kompatibelt format |
| sessionId | Sträng | Nej | Ingen | Används för att länka flera [!DNL Target] förfrågningar |
| besökare | Objekt | Nej | new VisitorId | Ange en extern VisitorId-instans |

## Lova

`Promise` returnerade har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| förfrågan | Objekt | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) förfrågan |
| svar | Objekt | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) svar |
| visitorState | Objekt | Objekt som ska skickas till Visitor API `getInstance()` |
| targetCookie | Objekt | [!DNL Target] cookie |
| targetLocationHintCookie | Objekt | [!DNL Target] cookie för platstips |
| analyticsDetails | Array | Analysens nyttolast, om Analytics används på klientsidan |
| responseTokens | Array | En lista med [Svarstoken](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?). |
| trace | Array | Sammanlagda spårningsdata för alla begärandemrutor/vyer |
| status | Objekt | Ett objekt som innehåller svarsstatus. |
| decisioningMethod | Sträng | Bestämmer vilken beslutsmetod som ska användas ([på enheten](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md), serversida, hybrid) |

`targetCookie` och `targetLocationHintCookie` objekt som används för att skicka data tillbaka till webbläsaren har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| name | Sträng | Kaknamn |
| value | Alla | Cookie-värde, konverteras de till sträng |
| maxAge | Nummer | The `maxAge` är ett bekvämt alternativ för att ange förfallodatum i förhållande till den aktuella tiden i sekunder |

The `status` det objekt som används för att ange status för målsvaret har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| status | Nummer | HTTP-statuskod |
| message | Sträng | Ett meddelande om svaret. Den kan till exempel visa om svaret beslutades [på enheten](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md) eller på serversidan |
| remoteMboxes | Array | När beslutsmetoden är `on-device`, ges en matris med mbox-namn som inte helt kunde bestämmas på enheten. Med andra ord: [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) begäran krävs. |

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
