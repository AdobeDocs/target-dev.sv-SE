---
title: Så här använder du asynkrona begäranden i  [!DNL Adobe Target] Node.js SDK
description: Lär dig hur  [!DNL Target] Node.js SDK stöder asynkrona begäranden, vilket kan minska den effektiva måltiden till noll.
feature: APIs/SDKs
exl-id: aa06f3ca-7d2a-4334-8092-730a8705dfb0
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# Hämta attribut (Node.js)

## Beskrivning

`[!UICONTROL getAttributes()]` används för att hämta experimentella och personaliserade upplevelser från [!DNL Target] och extrahera attributvärden.

## Metod

### getAttributes

```js {line-numbers="true"}
TargetClient.getAttributes(mboxNames: Array, options: Object): Promise
```

## Parametrar

| Namn | Typ | Obligatoriskt | Standard |
| --- | --- | --- |--- |
| mboxNames | Array | Ja | Ingen |
| alternativ | Objekt | Nej | Ingen |

## Lova

`Promise` som returneras av `TargetClient.getAttributes()` löser ett objekt med följande metoder:

| Metod | Returtyp | Beskrivning |
| --- | --- | --- |
| getValue(mboxName, key) | Alla | Returnerar värdet för ett angivet mbox-namn och attributnyckel |
| asObject(mboxName) | Objekt | Returnerar ett enkelt json-objekt med nyckelvärdepar |
| getResponse() | [getOffers-svar](https://github.com/jasonwaters/target-nodejs-sdk#targetclientgetoffers) | Returnerar det svarsobjekt som normalt returneras av `getOffers` |

## Exempel

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);
const offerAttributes = await targetClient.getAttributes(["demo-engineering-flags"]);


//returns just the value of searchProviderId from the mbox offer
const searchProviderId = offerAttributes.getValue("demo-engineering-flags", "searchProviderId");

//returns a simple JSON object representing the mbox offer
const engineeringFlags = offerAttributes.asObject("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

const assetUrl = `http://${engineeringFlags.cdnHostname}/path/to/asset`;
```
