---
title: Adobe Target Profiles API
description: Lär dig hur du använder Adobe Target Profile API:er för att skicka besöksdata till [!DNL Target].
contributors: https://github.com/icaraps
feature: APIs/SDKs
source-git-commit: 289299a52e5611c0da341f313aa4a447fcf3666a
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# [!DNL Adobe Target Profiles API] översikt

[!DNL Adobe Target] skapar och underhåller en profil för varje enskild användare. Den här profilen lagras på [!DNL Target] Edge Cluster och uppdateras i realtid efter varje besök.

## Profiler [!DNL Postman] samling

[!DNL Postman] är ett program som gör det enkelt att utlösa API-anrop. Detta [!DNL Postman] samlingen innehåller alla [!DNL Profile API] samtal. Klicka [Kör i Postman](https://www.getpostman.com/collections/ec7376f9028977ccaa99){target=_blank} för att importera API-samlingen för profiler.

## API-dokumentation för äldre profiler.

Den äldre API-dokumentationen för profiler finns här: [https://developers.adobetarget.com/api/#profiles](https://developers.adobetarget.com/api/#profiles){target=_blank}

## Struktur för en [!DNL Target] profil

En målprofil består av följande objekt:

| Objekt | Information |
| --- | --- |
| `clientcode` | The [!DNL Target] klientkod som profilen är associerad med. |
| `visitorId` | Identifieraren för profilen. Det här kan vara en `tntid`, `thirdpartyid`, eller `marketingcloudvisitorid`. |
| `modifiedAt` | Tidsstämpeln för när profilen senast uppdaterades. |
| `profileAttributes` | Lista över alla profilattribut som lagras som nyckelvärdepar i den enskilda profilen. |

### Exempel på profilstruktur

```
{
    "client": "<your-tenant-name>",
    "visitorId": "a1-mbox3rdPartyId",
    "modifiedAt": "2017-08-18T17:53:39.003-04:00",
    "profileAttributes": {
        "insurance": {
            "value": "false",
            "modifiedAt": "2017-07-31T20:34:55.625-04:00"
        },
        "country": {
            "value": "france",
            "modifiedAt": "2017-07-31T14:26:30.879-04:00"
        },
        "checking": {
            "value": "true",
            "modifiedAt": "2017-07-31T20:34:55.625-04:00"
        },
        "user.memberlevel": {
            "value": "0.0",
            "modifiedAt": "2017-08-09T18:18:04.661-04:00"
        },
        "param1": {
            "value": "value1",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "param2": {
            "value": "value2",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "firstSessionStart": {
            "value": "1500648715286",
            "modifiedAt": "2017-07-21T10:51:55.286-04:00"
        },
        "entity.name": {
            "value": "my-entityName",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "entity.id": {
            "value": "my-entityId",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        }
    }
}
```
