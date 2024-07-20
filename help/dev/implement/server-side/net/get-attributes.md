---
title: Använd getAttributes i [!DNL Adobe Target] med .NET SDK
description: Lär dig hur du använder getAttributes() för att hämta experiment och personaliserade upplevelser från  [!DNL Target]  och extrahera attributvärden.
feature: APIs/SDKs
exl-id: 808da83d-3077-468b-a2ad-e35c25905f7d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Hämta attribut (.NET)

## Beskrivning

`GetAttributes()` används för att hämta experimentella och personaliserade upplevelser från [!DNL Target] och extrahera attributvärden.

## Metod

### getAttributes

```dotnet {line-numbers="true"}
TargetAttributes TargetClient.GetAttributes(TargetDeliveryRequest targetRequest, params string[] mboxes)
```

## Parametrar

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | Nej | null | Samma [!DNL Target]-begäran som den som används för [Get Offers &#x200B;](get-offers.md) |
| mboxNames | parametersträng[] | Nej | null | En parameterarray med mbox-namn |

## Resultat

Ett `TargetAttributes`-objekt returneras från `TargetClient.GetAttributes()` som har följande egenskaper och metoder:

| Egenskap/metod | Returtyp | Beskrivning |
| --- | --- | --- |
| Svar | MålLeveranssvar | Returnerar det svarsobjekt som normalt returneras av [Get Offers](get-offers.md) |
| ToDictionary | IReadOnlyDictionary | Returnerar ett lexikon med nyckelvärdepar grupperade efter mbox-namn |
| ToMboxDictionary(mboxName) | IReadOnlyDictionary | Returnerar en ordlista med nyckelvärdepar för den angivna mbox-filen |
| GetBoolean(mboxName, key, defaultValue) | bool | Returnerar värdet för ett angivet mbox-namn och attributnyckel |
| GetString(mboxName, key, defaultValue) | string | Returnerar värdet för ett angivet mbox-namn och attributnyckel |
| GetInteger(mboxName, key, defaultValue) | int | Returnerar värdet för ett angivet mbox-namn och attributnyckel |
| GetDouble(mboxName, key, defaultValue) | double | Returnerar värdet för ett angivet mbox-namn och attributnyckel |
| GetValue(mboxName, key, defaultValue) | T | Returnerar värdet för ett angivet mbox-namn och attributnyckel |

## Exempel

### \.NET

```dotnet {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

var targetClient = TargetClient.Create(targetClientConfig);

var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .Build();

var offerAttributes = targetClient.GetAttributes(targetDeliveryRequest, "demo-engineering-flags");

//returns just the value of searchProviderId from the mbox offer
var searchProviderId = offerAttributes.GetString("demo-engineering-flags", "searchProviderId");

//returns a simple Dictionary representing the mbox offer
var engineeringFlags = offerAttributes.ToMboxDictionary("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

var assetUrl = $"http://{engineeringFlags["cdnHostname"]}/path/to/asset";
```
