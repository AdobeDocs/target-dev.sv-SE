---
title: Användaridentifiering och -spärring
description: Användaridentifiering och -spärring
exl-id: 4fcf235b-6a58-442c-ae13-9d05ec1033fc
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 0%

---

# Användaridentifiering och -spärring

## Användaridentifiering

Det finns flera sätt att identifiera en användare inom [!DNL Adobe Target]. [!UICONTROL Target] använder följande identifierare:

| Fältnamn | Beskrivning |
| --- | --- |
| `tntID` | `tntId` är den primära identifieraren i [!DNL Target] för en användare. Du kan ange det här ID:t, annars genereras det automatiskt av [!DNL Target] om begäran inte innehåller någon. |
| `thirdPartyId` | `thirdPartyId` är företagets identifierare för användaren, som du kan skicka med varje samtal. När en användare loggar in på ett företags webbplats skapar företaget vanligtvis ett ID som är knutet till besökarens konto, förmånskort, medlemsnummer eller andra tillämpliga identifierare för det företaget. |
| `marketingCloudVisitorId` | `marketingCloudVisitorId` används för att sammanfoga och dela data mellan olika Adobe-lösningar. marketingCloudVisitorId krävs för integrering med Adobe Analytics och Adobe Audience Manager. |
| `customerIds` | Förutom besökar-ID:t för Experience Cloud kan ytterligare [kund-ID:n](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html) och en autentiserad status för varje besökare också användas. |

## [!DNL Target]-ID (tntID)

[!DNL Target]-ID:t, eller `tntId`, kan betraktas som ett enhets-ID. `tntId` genereras automatiskt av [!DNL Adobe Target] om det inte anges i begäran. Efterföljande begäranden måste inkludera `tntId` för att rätt innehåll ska kunna levereras till en enhet som används av samma användare.

I följande exempelanrop visas en situation där `tntId` inte skickas till [!DNL Target].

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
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

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

Om `tntId` inte finns genererar [!DNL Adobe Target] en `tntId` och tillhandahåller den i svaret enligt följande.

```json {line-numbers="true"}
{
  "status": 200,
  "requestId": "5b586f83-890c-46ae-93a2-610b1caa43ef",
  "client": "acmeclient",
  "id": {
      "tntId": "10abf6304b2714215b1fd39a870f01afc.35_0"
  },
  "edgeHost": "mboxedge35.tt.omtrdc.net",
  ...
}
```

I det här exemplet är den genererade `tntId` `10abf6304b2714215b1fd39a870f01afc.35_0`. Observera att `tntId` måste användas för samma användare i alla sessioner.

## Tredje parts-ID (thirdPartyId)

Om din organisation använder ett ID för att identifiera besökaren kan du använda `thirdPartyID` för att leverera innehåll. En `thirdPartyID` är ett beständigt ID som används av ditt företag för att identifiera en slutanvändare, oavsett om de interagerar med ditt företag via webb-, mobil- eller IoT-kanaler. `thirdPartyId` refererar med andra ord till användarprofildata som kan användas i alla kanaler. Du måste dock ange `thirdPartyID` för varje [!DNL Adobe Target] leverans-API-anrop du gör.

I följande exempelanrop visas hur du använder en `thirdPartyId`.

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      thirdPartyId: "B234A029348"
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

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .thirdPartyId("B234A029348");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

I det här scenariot genererar [!DNL Adobe Target] en `tntId` eftersom den inte skickades till det ursprungliga anropet, som mappas till angiven `thirdPartyId`.

## Marketing Cloud Visitor-ID (marketingCloudVisitorId)

`marketingCloudVisitorId` är ett universellt och beständigt ID som identifierar dina besökare för alla lösningar i Adobe Experience Cloud. När din organisation implementerar ID-tjänsten kan du med det här ID:t identifiera samma besökare och deras data i olika Experience Cloud-lösningar, inklusive [!DNL Adobe Target], Adobe Analytics och Adobe Audience Manager. Observera att `marketingCloudVisitorId` krävs vid integrering av [!DNL Target] med [!DNL Adobe Analytics] och [!DNL Adobe Audience Manager].

Följande exempelanrop visar hur en `marketingCloudVisitorId` som hämtades från Experience Cloud ID-tjänsten skickas till [!DNL Target].

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      marketingCloudVisitorId: "10527837386392355901041112038610706884"
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

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .marketingCloudVisitorId("10527837386392355901041112038610706884");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

I det här scenariot genererar [!DNL Target] en `tntId` eftersom den inte skickades till det ursprungliga anropet, som mappas till angiven `marketingCloudVisitorId`.

## Kund-ID (customerIds)

[Kund-ID:n](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html) kan läggas till i, eller kopplas till, ett Experience Cloud Visitor-ID. När `customerIds` skickas måste även `marketingCloudVisitorId` anges. Dessutom kan en autentiseringsstatus anges tillsammans med varje `customerId` för varje besökare. Följande autentiseringsstatusar kan användas:

| Autentiseringsstatus | Användarstatus |
| --- | --- |
| `unknown` | Okänd eller aldrig autentiserad. Det här läget kan användas för scenarier där en besökare låser sig på platsen genom att klicka på en visningsannons. |
| `authenticated` | Användaren autentiseras för närvarande med en aktiv session på din webbplats eller i din app. |
| `logged_out` | Användaren autentiserades men loggades aktivt ut. Användaren ville koppla från det autentiserade läget. Användaren vill inte längre behandlas som autentiserad. |

Observera att endast när `customerId` är i ett autentiserat tillstånd [!DNL Target] refererar till användarprofildata som är lagrade och länkade till customerId. Om `customerId` är i ett okänt eller `logged_out`-läge ignoreras det och alla användarprofildata som kan associeras med `customerId` utnyttjas inte för målgruppsanpassning.

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      marketingCloudVisitorId : "10527837386392355901041112038610706884",
      customerIds: [{
        id: "134325423",
        integrationCode : "crm_data",
        authenticatedState : "authenticated"
      }]
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

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

CustomerId customerId = new CustomerId()
  .id("134325423")
  .integrationCode("crm_data")
  .authenticatedState(AuthenticatedState.AUTHENTICATED);
VisitorId id = new VisitorId()
  .marketingCloudVisitorId("10527837386392355901041112038610706884")
  .customerIds(Arrays.asList(customerId));
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

Exemplet ovan visar hur du skickar en `customerId` med en `authenticatedState`. När du skickar en `customerId` krävs både `integrationCode`, `id` och `authenticatedState` samt `marketingCloudVisitorId`. `integrationCode` är aliaset för den [kundattributfil](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html) som du tillhandahöll via CRS.

## Sammanfogad profil

Du kan kombinera `tntId`, `thirdPartyID` och `marketingCloudVisitorId` i samma begäran. I det här scenariot behåller [!DNL Adobe Target] mappningen av alla dessa ID:n och fäster den till en besökare. Lär dig hur profiler [sammanfogas och synkroniseras i realtid](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html) med olika identifierare.

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId: "B234A029348",
      marketingCloudVisitorId : "10527837386392355901041112038610706884"
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

>[!TAB Java SDK]

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

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

Exemplet ovan visar hur du kan kombinera `tntId`, `thirdPartyID` och `marketingCloudVisitorId` i samma begäran.

## Bucketing

Dina användare ser en upplevelse som är beroende av hur du konfigurerar dina [!DNL Adobe Target]-aktiviteter. I [!DNL Adobe Target] är bucketing:

* **Deterministisk**: MurmurHash3 används för att se till att användaren är spärrad och ser rätt variant varje gång så länge användar-ID:t är konsekvent.
* **Fäst**: [!DNL Adobe Target] lagrar variationen som din användare ser i användarprofilen för att säkerställa att variationen visas på ett konsekvent sätt för användaren i olika sessioner och kanaler. Variationer och krånglighet garanteras när beslut på serversidan används. När beslut på enheten används, kan man inte garantera att de hålls nere.

## Arbetsflöde för komplett paketering

Innan du börjar använda den faktiska låsningsalgoritmen är det viktigt att understryka att liknande steg används både för att välja aktiviteter baserat på deras procenttal för trafikallokering och för att välja en upplevelse inom en aktivitet.

### Val av aktivitet

1. Generera ett enhets-ID, vanligtvis ett UUID
1. Hämta klientkoden
1. Hämta aktivitets-ID
1. Hämta saltet, vilket vanligtvis är en sträng som &quot;activity&quot;
1. Beräkna hash med hjälp av MurmurHash3
1. Hämta det absoluta värdet för hashen
1. Dela det absoluta hash-värdet med 10000
1. Dela upp resten med 10000, vilket ger ett värde mellan 0 och 1
1. Multiplicera resultatet med 100 %
1. Jämför tilldelningsprocent för aktivitetstrafik med den erhållna procentandelen. Om trafikallokeringsprocenten är lägre väljs aktiviteten. Annars hoppas aktiviteten över.

### Upplev urvalssteg

1. Generera ett enhets-ID, vanligtvis ett UUID
1. Hämta klientkoden
1. Hämta aktivitets-ID
1. Hämta saltet, vilket vanligtvis är en sträng som &quot;upplevelse&quot;
1. Beräkna hash med hjälp av MurmurHash3
1. Hämta det absoluta värdet för hashen
1. Dela det absoluta hash-värdet med 10000
1. Dela upp resten med 10000, vilket ger ett värde mellan 0 och 1
1. Multiplicera resultatet med det totala antalet upplevelser inom aktiviteten
1. Avrunda resultatet. Detta bör skapa ett upplevelseindex.

### Exempel

Anta följande:

* Klient-C med klientkod `acmeclient`
* Aktivitet A som har ID `1111` och tre upplevelser `E1`, `E2`, `E3`
* Erfarenheter har följande fördelning: `E1` - 33 %, `E2` - 33 %, `E3` - 34 %

Markeringsflödet ser ut så här:

1. Enhets-ID `702ff4d0-83b1-4e2e-a0a6-22cbe460eb15`
1. Klientkod `acmeclient`
1. Aktivitets-ID `1111`
1. Salt `experience`
1. Värde för hash `acmeclient.1111.702ff4d0-83b1-4e2e-a0a6-22cbe460eb15.experience`, hash-värde `-919077116`
1. Absolut värde för hash `919077116`
1. Återstående efter division med 10000, `7116`
1. Värdet efter resten divideras med 10000, `0.7116`
1. Resultat efter att värdet har multiplicerats med det totala antalet upplevelser `3 * 0.7116 = 2.1348`
1. Erfarenhetsindexet är `2`, vilket innebär den tredje upplevelsen eftersom vi använder `0`-baserad indexering.
