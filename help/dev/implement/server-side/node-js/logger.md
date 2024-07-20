---
title: Initiera  [!DNL Adobe Target] Node.js SDK för att logga begäranden
description: Lär dig hur du loggar begäranden i  [!DNL Adobe Target] Node.js SDK.
feature: APIs/SDKs
exl-id: 5db3e301-47b3-4330-b185-c0c03f72e790
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 0%

---

# Logger (Node.js)

## Beskrivning

När [SDK](initialize-sdk.md) initieras är objektet `options.logger` ett valfritt objekt. För att felsöka effektivt när ett problem uppstår bör dock ett `logger`-objekt anges när SDK initieras.

Objektet `logger` förväntas ha en `debug()`- och en `error()`-metod. När en lämplig loggare har angetts, till exempel `console`, loggas [!DNL Target] begäranden och svar.

## Exempel

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  logger: console
};

const targetClient = TargetClient.create(CONFIG);

const request = {
    execute: {
        mboxes: [{
            name: "a1-serverside-ab",
            index: 1
        }]
    }
};

const response = await targetClient.getOffers({ request, targetCookie });
```

Du bör se förfrågningar och svar som skrivs ut i konsolen.
