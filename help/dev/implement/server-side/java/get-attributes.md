---
title: Använd getAttributes i [!DNL Adobe Target] med Java SDK
description: Lär dig hur du använder getAttributes() för att hämta experiment och personaliserade upplevelser från [!DNL Target] och extrahera attributvärden.
feature: APIs/SDKs
exl-id: e493e1b9-7180-4a7c-b98d-be84cc3a57c3
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---

# Hämta attribut (Java)

## Beskrivning

`getAttributes()` används för att hämta in experiment och personaliserade upplevelser från [!DNL Target] och extrahera attributvärden.

## Metod

### getAttributes

```javascript {line-numbers="true"}
Attributes TargetClient.getAttributes(TargetDeliveryRequest targetRequest, String ...mboxes)
```

## Parametrar

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | Ja | Ingen | Samma målbegäran som används för [Få &#x200B;](get-offers.md) |
| mboxNames | var-args-array | Nej | Ingen | En var-args-array med mbox-namn |


## Resultat

An `Attributes` objektet returneras från `TargetClient.getAttributes()` som har följande metoder:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| getBoolean(mboxName, key) | Boolean | Returnerar värdet för ett angivet mbox-namn och attributnyckel |
| getString(mboxName, key) | Sträng | Returnerar värdet för ett angivet mbox-namn och attributnyckel |
| getInteger(mboxName, key) | Heltal | Returnerar värdet för ett angivet mbox-namn och attributnyckel |
| getDouble(mboxName, key) | Dubbel | Returnerar värdet för ett angivet mbox-namn och attributnyckel |
| toMboxMap(mboxName) | Karta | Returnerar en enkel karta med nyckelvärdepar |
| getResponse() | MålLeveranssvar | Returnerar det svarsobjekt som normalt returneras av getOffers |

## Exempel

### Java

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);

TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
        .context(new Context().channel(ChannelType.WEB))
        .build();

Attributes offerAttributes = targetJavaClient.getAttributes(targetDeliveryRequest, "demo-engineering-flags");

//returns just the value of searchProviderId from the mbox offer
String searchProviderId = offerAttributes.getString("demo-engineering-flags", "searchProviderId");

//returns a simple Map representing the mbox offer
Map<String, Object> engineeringFlags = offerAttributes.toMboxMap("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

String assetUrl = "http://" + engineeringFlags.cdnHostname + "/path/to/asset";
```
