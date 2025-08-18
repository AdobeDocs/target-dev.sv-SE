---
title: Integrering med Experience Cloud A4T Reporting
description: Integrering med Experience Cloud, A4T-rapportering, Analytics for Target-integrering
keywords: delivery api, server-side, server-side, integration, a4t
exl-id: 0d09d7a1-528d-4e6a-bc6c-f7ccd61f5b75
feature: Implement Server-side
source-git-commit: cbae0f1758fb0dee4837e8c237f8617ecb46eb25
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 0%

---

# [!UICONTROL Analytics for Target] (A4T)-rapportering

[!DNL Adobe Target] har stöd för A4T-rapportering för både enhetsbeslut och [!DNL Target]-aktiviteter på serversidan. Det finns två konfigurationsalternativ för att aktivera A4T-rapportering:

* [!DNL Adobe Target] vidarebefordrar automatiskt analysnyttolasten till [!DNL Adobe Analytics], eller
* Användaren begär analysnyttolasten från [!DNL Adobe Target]. ([!DNL Adobe Target] returnerar [!DNL Adobe Analytics]-nyttolasten till anroparen.)

>[!NOTE]
>
>Enhetsbeslut stöder bara A4T-rapportering där [!DNL Adobe Target] automatiskt vidarebefordrar analysnyttolasten till [!DNL Adobe Analytics]. Hämtning av analysnyttolasten från [!DNL Adobe Target] stöds inte.

## Krav

1. Konfigurera aktiviteten i [!DNL Adobe Target]-gränssnittet med [!DNL Adobe Analytics] som rapportkälla och kontrollera att kontona är aktiverade för A4T.
1. API-användaren genererar Adobe [!UICONTROL Marketing Cloud Visitor ID] och ser till att det här ID:t är tillgängligt när [!DNL Target]-begäran körs.

## [!DNL Adobe Target] vidarebefordrar [!DNL Analytics]-nyttolasten automatiskt

[!DNL Adobe Target] kan automatiskt vidarebefordra analysnyttolasten till [!DNL Adobe Analytics] om följande identifierare anges:

1. `supplementalDataId`: Det ID som används för att sammanfoga mellan [!DNL Adobe Analytics] och [!DNL Adobe Target]. För att [!DNL Adobe Target] och [!DNL Adobe Analytics] ska kunna sammanfoga data på rätt sätt måste samma `supplementalDataId` skickas till både [!DNL Adobe Target] och [!DNL Adobe Analytics].
1. `trackingServer`: Servern [!DNL Adobe Analytics].

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {     
    id: {
      marketingCloudVisitorId : "2304820394812039",
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId:"23423432"
    },
    experienceCloud: {
      analytics: {
        logging: "server_side",
        supplementalDataId: "7D3AA246CC99FD7F-1B3DD2E75595498E",
        trackingServer: "jimsbrims.sc.omtrds.net"
      }
    }, 
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }       
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .tntId("d359234570e044f14e1faeeba02d6ab23439914e.35_0")
  .thirdPartyId("B234A029348")
  .marketingCloudVisitorId("10527837386392355901041112038610706884");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

AnalyticsRequest analyticsRequest =
    new AnalyticsRequest()
        .trackingServer("jimsbrims.sc.omtrds.net")
        .logging(LoggingType.SERVER_SIDE)
        .supplementalDataId("7D3AA246CC99FD7F-1B3DD2E75595498E");
ExperienceCloud expCloud =
    new ExperienceCloud()
        .setAnalytics(analyticsRequest);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .experienceCloud(expCloud)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

## Användaren hämtar analysnyttolast från [!DNL Adobe Target]

En användare kan hämta [!DNL Adobe Analytics]-nyttolasten för en given mbox och sedan skicka den till [!DNL Adobe Analytics] via [API:t för datainmatning](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md). När en [!DNL Adobe Target]-begäran har utlösts skickar du `client_side` till fältet `logging` i begäran. Denna begäran returnerar en nyttolast om den angivna mbox finns i en aktivitet som använder [!DNL Analytics] som rapportkälla.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};
const targetClient = TargetClient.create(CONFIG);
targetClient.getOffers({
  request: {     
    id: {
      marketingCloudVisitorId : "2304820394812039",
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId:"23423432"
    },
    experienceCloud: {
      analytics: {
        logging: "client_side"
      }
    },  
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }       
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .tntId("d359234570e044f14e1faeeba02d6ab23439914e.35_0")
  .thirdPartyId("B234A029348")
  .marketingCloudVisitorId("10527837386392355901041112038610706884");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

AnalyticsRequest analyticsRequest =
    new AnalyticsRequest()
        .logging(LoggingType.CLIENT_SIDE);
ExperienceCloud expCloud =
    new ExperienceCloud()
        .setAnalytics(analyticsRequest);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .experienceCloud(expCloud)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

När du har angett `logging = client_side` får du nyttolasten i mbox-fältet.

Om svaret från [!DNL Target] innehåller något i egenskapen `analytics -> payload` vidarebefordrar du det som det är till [!DNL Adobe Analytics]. [!DNL Adobe Analytics] vet hur den här nyttolasten behandlas. Detta kan göras i en GET-begäran i följande format:

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/{content_type_num}/{code_ver}/{session}?pe=tnt&tnta={payload}&c.&a.&target.&sessionId={sessionId}&.target&.a&.c&mid={mid}
```

### Fråga strängparametrar och variabler

| Fältnamn | Obligatoriskt | Beskrivning |
| --- | --- | --- |
| `rsid` | Ja | Rapportsvitens ID |
| `content_type_num` | Ja | Alltid inställd på &quot;0&quot; |
| `code_ver` | Ja | Ställ alltid in på &quot;MOBILE-1.0&quot; |
| `session` | Ja | Alltid inställd på &quot;0&quot; |
| `pe` | Ja | Sidhändelse. Alltid inställd på `tnt` |
| `tnta` | Ja | [!DNL Analytics] nyttolast returnerades av [!DNL Target]-servern i `analytics -> payload -> tnta` |
| `sessionId` | Ja | [!DNL Target] sessions-ID för den pågående sessionen |
| `mid` | Ja | Marketing Cloud Visitor-ID |

### Obligatoriska rubrikvärden

| Rubriknamn | Huvudvärde |
| --- | --- |
| Värd | Server för insamling av analysdata (t.ex.: `adobeags421.sc.omtrdc.net`) |

### Exempel på infogning av A4T-data HTTP Get call

Icke-URL-kodad version För läsbarhet (format som inte ska användas för API-anrop):

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0/0?tnta=253236:0:0|0,253236:0:0|2,253236:0:0|1,253613:0:0|0,253613:0:0|2,253613:0:0|1&c.&a.&target.&sessionId=45c08980-f4b9-4e11-96db-067d58e49f74&.target&.a&.c&pe=tnt&mid=69170113867710665996968872592584719577
```

URL-kodad version (format för API-anrop):

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0/0?tnta=253236%3A0%3A0%7C0%2C253236%3A0%3A0%7C2%2C253236%3A0%3A0%7C1%2C253613%3A0%3A0%7C0%2C253613%3A0%3A0%7C2%2C253613%3A0%3A0%7C1&c.%26a.%26target.%26sessionId=45c08980-f4b9-4e11-96db-067d58e49f74%26.target%26.a%26.c&pe=tnt&mid=69170113867710665996968872592584719577 
```
