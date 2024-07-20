---
title: Felsöka enhetsbeslut
description: Lär dig felsöka [!UICONTROL on-device decisioning]
exl-id: e76f95ce-afae-48e0-9dbb-2097133574dc
feature: APIs/SDKs
source-git-commit: 1d892d4d4d6f370f7772d0308ee0dd0d5c12e700
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 0%

---

# Felsökning av [!UICONTROL on-device decisioning]

## Verifierar konfiguration

### Sammanfattning av steg

1. Kontrollera att `logger` är konfigurerad
1. Kontrollera att [!DNL Target] spår är aktiverade
1. Kontrollera att [!UICONTROL on-device decisioning] *rule-felaktigheten* har hämtats och cachelagrats enligt det definierade avsökningsintervallet.
1. Validera innehållsleverans via den cachelagrade regelartefakten genom att skapa en [!UICONTROL on-device decisioning]-testaktivitet via den formulärbaserade upplevelsedispositionen.
1. Meddelandefel för Inspect

## 1. Kontrollera att loggaren är konfigurerad

När du initierar SDK måste du aktivera loggning.

**Node.js**

Ett `logger`-objekt måste anges för Node.js SDK.

```js {line-numbers="true"}
const CONFIG = {
  client: "<your client code>",
  organizationId: "<your organization ID>",
  logger: console
};
```

**Java SDK**

Java SDK `logRequests` på `ClientConfig` ska aktiveras.

```js {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("<your client code>")
  .organizationId("<your organization ID>")
  .logRequests(true)
  .build();
```

Dessutom bör JVM startas med följande kommandoradsparameter:

```bash {line-numbers="true"}
java -Dorg.slf4j.simpleLogger.defaultLogLevel=DEBUG ...
```

## 2. Kontrollera att [!DNL Target]Spår är aktiverat

Om du aktiverar spårningar genereras ytterligare information från [!DNL Adobe Target] med avseende på regelartefakten.

1. Navigera till [!DNL Target]användargränssnittet i [!DNL Experience Cloud].

   ![alt-bild](assets/asset-target-ui-1.png)

1. Navigera till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** och klicka på **[!UICONTROL Generate New Authorization Token]**.

   ![alt-bild](assets/asset-target-ui-2.png)

1. Kopiera den nyligen genererade auktoriseringstoken till Urklipp och lägg till den i din [!DNL Target]begäran:

   **Node.js**

   ```js {line-numbers="true"}
   const request = {
     trace: {
       authorizationToken: "88f1a924-6bc5-4836-8560-2f9c86aeb36b"
     },
     execute: {
       mboxes: [{
         name: "sdk-mbox"
       }]
   }};
   ```

   **Java**

   ```js {line-numbers="true"}
   Trace trace = new Trace()
     .authorizationToken("88f1a924-6bc5-4836-8560-2f9c86aeb36b");
   Context context = new Context()
     .channel(ChannelType.WEB);
   MboxRequest mbox = new MboxRequest()
     .name("sdk-mbox")
     .index(0);
   ExecuteRequest executeRequest = new ExecuteRequest()
     .mboxes(Arrays.asList(mbox));
   
   TargetDeliveryRequest request = TargetDeliveryRequest.builder()
     .trace(trace)
     .context(context)
     .execute(executeRequest)
     .build();
   ```

1. Med loggen och spårningen på plats startar du programmet och övervakar serverterminalen. Följande utdata från loggaren bekräftar att regelartefakten har hämtats:

   **Node.js SDK**

   ```text {line-numbers="true"}
     AT: LD.ArtifactProvider fetching artifact - https://assets.adobetarget.com/your-client-code/production/v1/rules.json
     AT: LD.ArtifactProvider artifact received - status=200
   ```

## 3. Verifiera att [!UICONTROL on-device decisioning] *regelartefakt* har hämtats och cachelagrats enligt det definierade avsökningsintervallet.

1. Vänta på avsökningsintervallets varaktighet (standardvärdet är 20 minuter) och kontrollera att artefakten hämtas av SDK:n. Samma terminalloggar genereras.

   Dessutom bör information från [!DNL Target]spårningen matas ut till terminalen med information om regelartefakten.

   ```text {line-numbers="true"}
   "trace": {
     "clientCode": "your-client-code",
     "artifact": {
       "artifactLocation": "https://assets.adobetarget.com/your-client-code/production/v1/rules.json",
       "pollingInterval": 300000,
       "pollingHalted": false,
       "artifactVersion": "1.0.0",
       "artifactRetrievalCount": 10,
       "artifactLastRetrieved": "2020-09-20T00:09:42.707Z",
       "clientCode": "your-client-code",
       "environment": "production",
       "generatedAt": "2020-09-22T17:17:59.783Z"
     },
   ```

## 4. Validera innehållsleverans via den cachelagrade regelartefakten genom att skapa en [!UICONTROL on-device decisioning]-testaktivitet via den formulärbaserade upplevelsedispositionen

1. Navigera till användargränssnittet [!DNL Target]i Experience Cloud

   ![alt-bild](assets/asset-target-ui-1.png)

1. Skapa en ny XT-aktivitet med den formulärbaserade Experience Composer.

   ![alt-bild](assets/asset-form-base-composer-ui.png)

1. Ange mbox-namnet som används i din[!DNL Target]begäran som plats för XT-aktiviteten (observera att det ska vara ett unikt mbox-namn specifikt för utvecklingsändamål).

   ![alt-bild](assets/asset-mbox-location-ui.png)

1. Ändra innehållet till ett HTML-erbjudande eller JSON-erbjudande. Detta returneras i [!DNL Target]-begäran till ditt program. Lämna aktiviteten som&quot;Alla besökare&quot; som mål och välj önskat mätvärde. Ge aktiviteten ett namn, spara den och aktivera den sedan för att säkerställa att mbox/location som används bara är till för utveckling.

   ![alt-bild](assets/asset-target-content-ui.png)

1. Lägg till loggsatser för innehållet som tagits emot i ditt [!DNL Target]svar i ditt program

   **Node.js SDK**

   ```js {line-numbers="true"}
   try {
     const response = await targetClient.getOffers({ request });
     console.log('Response: ', response.response.execute.mboxes[0].options[0].content);
   } catch (error) {
     console.error('Something went wrong', error);
   }
   ```

   **Java SDK**

   ```js {line-numbers="true"}
   try {
     Context context = new Context()
       .channel(ChannelType.WEB);
     MboxRequest mbox = new MboxRequest()
       .name("sdk-mbox")
       .index(0);
     ExecuteRequest executeRequest = new ExecuteRequest()
       .mboxes(Arrays.asList(mbox));
   
     TargetDeliveryRequest request = TargetDeliveryRequest.builder()
       .context(context)
       .decisioningMethod(DecisioningMethod.ON_DEVICE)
       .execute(executeRequest)
       .build();
   
       TargetDeliveryResponse response = targetClient.getOffers(request);
     logger.debug("Response: ", response.getResponse().getExecute().getMboxes().get(0).getOptions().get(0).getContent());
   } catch (Exception exception) {
     logger.error("Something went wrong", exception);
   }
   ```

1. Granska loggarna i terminalen för att bekräfta att ditt innehåll levereras och att det levererades via regelartefakten på servern. Objektet `LD.DeciscionProvider` returneras när aktivitetskvalifikationen och aktivitetsbeslutet fastställdes på enheten baserat på regelartefakten. På grund av loggningen av `content` bör du se `<div>test</div>` eller så har du bestämt att svaret ska vara när du skapar testaktiviteten.

   **Loggningsutdata**

   ```text {line-numbers="true"}
   AT: LD.DecisionProvider {...}
   AT: Response received {...}
   Response:  <div>test</div>
   ```

## Meddelandefel för Inspect

När du använder On-device-beslut skickas meddelanden automatiskt för getOffers-körningsbegäranden. Dessa förfrågningar skickas tyst i bakgrunden. Eventuella fel kan kontrolleras genom att prenumerera på en händelse med namnet `sendNotificationError`. Här följer ett kodexempel som visar hur du prenumererar på meddelandefel med hjälp av Node.js SDK.

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
let client;

function onSendNotificationError({ notification, error }) {
  console.log(
    `There was an error when sending a notification: ${error.message}`
  );
  console.log(`Notification Payload: ${JSON.stringify(notification, null, 2)}`);
}

async function targetClientReady() {
  const request = {
    context: { channel: "web" },
    execute: {
      mboxes: [{
        name: "a1-serverside-ab",
        index: 1
      }]
    }
  };
  const targetResponse = await client.getOffers({ request });
}

client = TargetClient.create({
  events: {
    clientReady: targetClientReady,
    sendNotificationError: onSendNotificationError
  }
});
```

## Vanliga felsökningsscenarier

Granska [funktioner som stöds](supported-features.md) för [!UICONTROL on-device decisioning] när du stöter på problem.

### Beslutsaktiviteter på enheten körs inte på grund av målgrupp eller aktivitet som inte stöds

Ett vanligt problem som kan uppstå är att [!UICONTROL on-device decisioning] aktiviteter inte körs på grund av att målgruppen används eller att aktivitetstypen inte stöds.

(1) Använd loggningsutdata för att granska posterna i egenskapen trace i ditt svarsobjekt. Identifiera kampanjegenskapen:

**Kalkera utdata**

```text {line-numbers="true"}
  "execute": {
  "mboxes": [
    {
      "name": "your-mbox-name",
      "index": 0,
      "trace": {
        "clientCode": "your-client-code",
        ...
        "campaigns": [],
        ...
      }
    }
```

Du kommer att märka att aktiviteten som du försöker kvalificera dig för inte finns i egenskapen `campaigns` eftersom målgrupps- eller aktivitetstypen inte stöds. Om aktiviteten listas under egenskapen `campaigns` beror ditt problem inte på en målgrupp eller aktivitetstyp som inte stöds.

(2) Leta reda på filen `rules.json` genom att titta på `trace` > `artifact` > `artifactLocation` i loggutdata och observera att din aktivitet saknas i egenskapen `rules` > `mboxes`:

**Loggningsutdata**

```text {line-numbers="true"}
 ...
 rules: {
   mboxes: { },
   views: { }
 }
```

Till sist går du till [!DNL Target]användargränssnittet och letar upp aktiviteten i fråga: [experience.adobe.com/target](https://experience.adobe.com/target)

Granska reglerna som används i publiken och se till att du bara använder de som stöds. Kontrollera dessutom att aktivitetstypen är A/B eller XT.

![alt-bild](assets/asset-target-audience-ui.png)

### Beslutsaktiviteter på enheten körs inte på grund av okvalificerad publik

Om en beslutsaktivitet på enheten inte körs, men du har verifierat att filen rules.json innehåller aktiviteten, ska du utföra följande steg:

(1) Se till att mbox du kör i programmet är samma som aktiviteten använder:

>[!BEGINTABS]

>[!TAB rule.json]

```text {line-numbers="true"}
 ...
 rules: {
   mboxes: {
    target-only-node-sdk-mbox: [{ // this mbox name must match the mbox in your request
      ...
    }]
   }
 ...
```

>[!TAB Node.js SDK]

```js {line-numbers="true"}
 const request = {
   trace: {
     authorizationToken: '2dfc1dce-1e58-4e05-bbd6-a6725893d4d6'
   },
   execute: {
     mboxes: [{
       address: getAddress(req),
       name: "target-only-node-sdk-mbox-two" // this mbox name must match the mbox the activity is using
     }]
   }};
```

>[!TAB Java SDK]

```js {line-numbers="true"}
Context context = new Context()
  .channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("target-only-node-sdk-mbox-two")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .decisioningMethod(DecisioningMethod.ON_DEVICE)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse response = targetClient.getOffers(request);
```

>[!ENDTABS]

(2) Se till att du är kvalificerad för målgruppen för din aktivitet genom att granska egenskapen `matchedRuleConditions` eller `unmatchedRuleConditions` för dina trace-utdata:

**Kalkera utdata**

```text {line-numbers="true"}
...
},
"campaignId": 368564,
"campaignType": "landing",
"matchedSegmentIds": [],
"unmatchedSegmentIds": [
  6188838
      ],
      "matchedRuleConditions": [],
          "unmatchedRuleConditions": [
            {
              "in": [
                "true",
                {
                  "var": "mbox.auth_lc"
                }
              ]
            }
          ]
    ...
```

Om du har oöverträffade regelvillkor är du inte kvalificerad för aktiviteten och aktiviteten kommer därför inte att köras. Granska reglerna för er målgrupp för att se varför ni inte är kvalificerade.

### Beslutsaktiviteten på enheten körs inte, men orsaken är inte uppenbar

Det kanske inte är uppenbart varför en beslutsaktivitet på enheten inte utförs. I det här fallet följer du dessa felsökningssteg för att identifiera problemet:

(1) Läs igenom loggspårningsutdata i konsolen och identifiera artefaktegenskapen som ser ut ungefär som följande:

**Kalkera utdata**

```text {line-numbers="true"}
...
      "artifact": {
          "artifactLocation": "https://assets.adobetarget.com/your-client-code/production/v1/rules.json",
          "pollingInterval": 300000,
          "pollingHalted": false,
          "artifactVersion": "1.0.0",
          "artifactRetrievalCount": 3,
          "artifactLastRetrieved": "2020-10-16T00:56:27.596Z",
          "clientCode": "adobeinterikleisch",
          "environment": "production"
        },
...
```

Titta på artefaktens `artifactLastRetrieved`-datum och se till att du har den senaste `rules.json`-filen hämtad till din app.

(2) Hitta egenskapen `evaluatedCampaignTargets` i loggutdata:

**Loggningsutdata**

```text {line-numbers="true"}
...
  "evaluatedCampaignTargets": [
      {
        "context": {
          "current_timestamp": 1602812599608,
          "current_time": "0143",
          "current_day": 5,
          "user": {
            "browserType": "unknown",
            "platform": "Unknown",
            "locale": "en",
            "browserVersion": -1
          },
          "page": {
            "url": "localhost:3000/",
            "path": "/",
            "query": "",
            "fragment": "",
            "subdomain": "",
            "domain": "3000",
            "topLevelDomain": "",
            "url_lc": "localhost:3000/",
            "path_lc": "/",
            "query_lc": "",
            "fragment_lc": "",
            "subdomain_lc": "",
            "domain_lc": "3000",
            "topLevelDomain_lc": ""
          },
          "referring": {
            "url": "localhost:3000/",
            "path": "/",
            "query": "",
            "fragment": "",
            "subdomain": "",
            "domain": "3000",
            "topLevelDomain": "",
            "url_lc": "localhost:3000/",
            "path_lc": "/",
            "query_lc": "",
            "fragment_lc": "",
            "subdomain_lc": "",
            "domain_lc": "3000",
            "topLevelDomain_lc": ""
          },
          "geo": {},
          "mbox": {},
          "allocation": 23.79
        },
        "campaignId": 368564,
        "campaignType": "landing",
        "matchedSegmentIds": [],
        "unmatchedSegmentIds": [
          6188838
        ],
        "matchedRuleConditions": [],
        "unmatchedRuleConditions": [
          {
            "in": [
              "true",
              {
                "var": "mbox.auth_lc"
              }
            ]
          }
        ]
...
```

(3) Granska `context`-, `page`- och `referring`-data för att säkerställa att de är så förväntade som detta kan påverka aktivitetens målinriktningskvalifikation.

(4) Granska `campaignId` för att kontrollera att aktiviteten eller aktiviteterna som du väntar dig att köra utvärderas. `campaignId` matchar aktivitets-ID på aktivitetsöversiktsfliken i [!DNL Target]gränssnittet:

![alt-bild](assets/asset-activity-id-target-ui.png)

(5) Granska `matchedRuleConditions` och `unmatchedRuleConditions` för att identifiera problem med kvalificering för målgruppsregler för en viss aktivitet.

(6) Granska den senaste `rules.json`-filen för att kontrollera att aktiviteten eller aktiviteterna som du vill köra lokalt ingår. Platsen anges ovan i steg 1.

(7) Se till att du använder samma mbox-namn i din begäran och dina aktiviteter.

(8) Se till att du använder målgruppsregler och aktivitetstyper som stöds.

### Ett serveranrop görs trots att aktivitetsinställningarna under en mbox säger &quot;Enhetsbeslut är valbart&quot; i användargränssnittet i [!DNL Target]

Det finns några skäl till att ett serveranrop görs trots att enheten är berättigad till enhetsbeslut:

* När mbox som används för en aktivitet av typen &quot;Enhetsbeslut är berättigade&quot; också används för andra aktiviteter som inte är &quot;Enhetsbeslut är berättigade&quot; visas mbox under avsnittet `remoteMboxes` i artefakten `rules.json`. När en mbox visas under `remoteMboxes` resulterar alla `getOffer(s)` anrop till den mbox i ett serveranrop.

* Om du ställer in en aktivitet under en arbetsyta/egenskap och inte inkluderar samma aktivitet när du konfigurerar SDK, kan det göra att `rules.josn` av standardarbetsytan hämtas, som kan använda mbox under avsnittet `remoteMboxes`.
