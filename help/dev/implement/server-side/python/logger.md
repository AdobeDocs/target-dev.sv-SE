---
title: Initiera  [!DNL Adobe Target] Python SDK för att logga begäranden
description: Lär dig hur du loggar begäranden i  [!DNL Adobe Target] Python SDK.
feature: APIs/SDKs
exl-id: 0b3792a5-a9a7-4768-a429-598b49f1fd93
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---

# Logger (Python)

## Beskrivning

När [SDK](initialize-sdk.md) initieras är objektet `options["logger"]` ett valfritt objekt. Som standard skapas en INFO-nivålogg under namnutrymmet `adobe.target`. För att anpassa loggnivån eller felsöka effektivt när ett problem uppstår, kan ett `logger`-objekt tillhandahållas när SDK initieras.

Objektet `logger` förväntas ha en `debug()`- och en `error()`-metod. När en lämplig loggare har angetts loggas [!DNL Target] förfrågningar och svar.

## Exempel

### Python

```python {line-numbers="true"}
logger = logging.getLogger("org.logger")
logger.setLevel(logging.DEBUG)

client_options = {
  "client": "acmeclient",
  "organization_id": "1234567890@AdobeOrg",
  "logger": logger
}
target_client = TargetClient.create(client_options)
```

Du bör se förfrågningar och svar som skrivs ut i konsolen.
