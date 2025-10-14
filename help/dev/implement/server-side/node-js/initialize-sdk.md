---
title: Initiera Node.js SDK med metoden create
description: Lär dig hur du använder metoden create för att initiera Node.js SDK och instansiera  [!DNL Target] klienten för att anropa  [!DNL Adobe Target] för experiment och personaliserade upplevelser.
feature: APIs/SDKs
exl-id: 71516e44-508a-4d8d-9f2b-7c54243e9c60
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# Initiera Node.js SDK

## Beskrivning

Använd metoden `create` för att initiera Node.js SDK och instansiera klienten [!UICONTROL Target] för att ringa anrop till [!DNL Adobe Target] för experiment och personaliserade upplevelser.

## Metod

**skapa**

```js {line-numbers="true"}
TargetClient.create(options: Object): TargetClient
```

## Parametrar

`options` har följande struktur:

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- | --- | --- | --- | --- |
| klient | Sträng | Ja | Ingen | [!UICONTROL Adobe Target Client ID] |
| organizationId | Sträng | Ja | Ingen | [!UICONTROL Experience Cloud Organization ID] |
| miljö | Sträng | Nej | produktion | Målmiljöns namn. I [!DNL Target]-gränssnittet, [!UICONTROL Administration] > [!UICONTROL Environments]. |
| timeout | Nummer | Nej | 3000 | Timeout i millisekunder |
| serverDomain | Sträng | Nej | `*client*.tt.omtrdc.net` | Åsidosätter standardvärdnamn |
| säker | Boolean | Nej | true | Avmarkerad för att tillämpa HTTP-schema |
| loggare | Objekt | Nej | NOOP-loggare | Ersätter standardloggen för NOOP |
| targetLocationHint | Sträng | Nej | Ingen | Tips för målplats |
| fetchApi | Funktion | Nej | global.fetch eller window.fetch | [fetch](https://fetch.spec.whatwg.org/) används av SDK för http-begäranden. Som standard används nodhämtning eller webbläsarimplementeringen av hämtning. Men en alternativ implementering kan anges med `fetchApi` |
| propertyToken | Sträng | Nej | Ingen | **Målegenskapstoken**. Om det anges här kommer alla `getOffers` anrop att använda det här värdet. **För enhetsbeslut** hämtar SDK bara artefakten som innehåller de kvalificerade aktiviteterna för egenskapstoken som angetts i `propertyToken` |
| decisioningMethod | Sträng | Nej | server-side | Avgör vilken beslutsmetod som ska användas ([på enheten](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md), på serversidan, hybrid) |
| pollingInterval | Nummer | Nej | 300000 (5 minuter) | Avsökningsintervall för artefakten [för enhetsspecifik avlyssningsregel &#x200B;](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) (i millisekunder) |
| artifactLocation | Sträng | Nej | Ingen | En fullständigt kvalificerad URL till [regeln för enhetsbeslut](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Åsidosätter internt bestämd plats. |
| artifactPayload | Objekt | Nej | Ingen | JSON-nyttolasten för [enhetsregelartefakten &#x200B;](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Om den anges används den i stället för att begära en från en URL. |
| [händelser](sdk-events.md) | Object&lt;String,Function> | Nej | Ingen | Ett valfritt objekt med händelsenamnstangenter och callback-funktionsvärden |
| telemetryEnabled | Boolean | Nej | true | När det här alternativet är aktiverat samlar Adobe in SDK-funktionens användnings- och prestandatelemetridata. Personuppgifter samlas inte in. |

## Exempel

### Node.js

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    events: {clientReady: targetClientReady }
};

const targetClient = TargetClient.create(CONFIG);

function targetClientReady() {
    // make calls to Adobe Target
}
```
