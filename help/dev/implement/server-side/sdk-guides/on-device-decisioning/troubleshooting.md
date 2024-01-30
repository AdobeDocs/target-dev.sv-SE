---
title: Felsöka enhetsbeslut
description: Lär dig hur du felsöker [!UICONTROL on-device decisioning]
exl-id: e76f95ce-afae-48e0-9dbb-2097133574dc
feature: APIs/SDKs
source-git-commit: 1d892d4d4d6f370f7772d0308ee0dd0d5c12e700
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 0%

---

# Felsökning [!UICONTROL on-device decisioning]

## Verifierar konfiguration

### Sammanfattning av steg

1. Kontrollera `logger` är konfigurerad
1. Säkerställ [!DNL Target] spår är aktiverade
1. Verifiera [!UICONTROL on-device decisioning] *regelartefakt* har hämtats och cachelagrats enligt det definierade avsökningsintervallet.
1. Validera innehållsleverans via den cachelagrade regelartefakten genom att skapa ett test [!UICONTROL on-device decisioning] via den formulärbaserade upplevelsedispositionen.
1. Meddelandefel för Inspect

## 1. Kontrollera att loggaren är konfigurerad

När du initierar SDK måste du aktivera loggning.

**Node.js**

För Node.js SDK a `logger` ska anges.

```js {line-numbers="true"}
const CONFIG = {
  client: "<your client code>",
  organizationId: "<your organization ID>",
  logger: console
};
```

**Java SDK**

För Java SDK `logRequests` på `ClientConfig` ska vara aktiverat.

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

## 2. Säkerställ[!DNL Target]Spår är aktiverade

Om spårning aktiveras kommer ytterligare information att hämtas från [!DNL Adobe Target] när det gäller regelfelaktigheten.

1. Navigera till[!DNL Target]Användargränssnitt i [!DNL Experience Cloud].

   ![alt-bild](assets/asset-target-ui-1.png)

1. Navigera till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** och klicka **[!UICONTROL Generate New Authorization Token]**.

   ![alt-bild](assets/asset-target-ui-2.png)

1. Kopiera den nyligen genererade auktoriseringstoken till Urklipp och lägg till den i[!DNL Target]begäran:

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

## 3. Verifiera [!UICONTROL on-device decisioning] *regelartefakt* har hämtats och cachelagrats enligt det definierade avsökningsintervallet.

1. Vänta på avsökningsintervallets varaktighet (standardvärdet är 20 minuter) och kontrollera att artefakten hämtas av SDK:n. Samma terminalloggar genereras.

   Dessutom finns information från[!DNL Target]Spårningen ska skickas till terminalen med information om regelartefakten.

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

## 4. Validera innehållsleverans via den cachelagrade regelartefakten genom att skapa ett test [!UICONTROL on-device decisioning] via den formulärbaserade upplevelsedispositionen

1. Navigera till[!DNL Target]Gränssnitt i Experience Cloud

   ![alt-bild](assets/asset-target-ui-1.png)

1. Skapa en ny XT-aktivitet med den formulärbaserade Experience Composer.

   ![alt-bild](assets/asset-form-base-composer-ui.png)

1. Ange namnet på mbox som används i[!DNL Target]request as the location for the XT activity (note this should be a unique mbox name specific for development purposes).

   ![alt-bild](assets/asset-mbox-location-ui.png)

1. Ändra innehållet till ett HTML-erbjudande eller JSON-erbjudande. Detta returneras i dialogrutan[!DNL Target]förfrågan till ditt program. Lämna aktiviteten som&quot;Alla besökare&quot; som mål och välj önskat mätvärde. Ge aktiviteten ett namn, spara den och aktivera den sedan för att säkerställa att mbox/location som används bara är till för utveckling.

   ![alt-bild](assets/asset-target-content-ui.png)

1. Lägg till en loggsats för det innehåll som tas emot i svaret från ditt program[!DNL Target]förfrågan

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

1. Granska loggarna i terminalen för att bekräfta att ditt innehåll levereras och att det levererades via regelartefakten på servern. The `LD.DeciscionProvider` objektet returneras när aktivitetskvalifikationen och aktivitetsbestämningen bestämdes på enheten baserat på regelartefakten. På grund av loggningen av `content`bör du se `<div>test</div>` Du har dock bestämt att svaret ska vara när du skapar testaktiviteten.

   **Loggutdata**

   ```text {line-numbers="true"}
   AT: LD.DecisionProvider {...}
   AT: Response received {...}
   Response:  <div>test</div>
   ```

## Meddelandefel för Inspect

När du använder On-device-beslut skickas meddelanden automatiskt för getOffers-körningsbegäranden. Dessa förfrågningar skickas tyst i bakgrunden. Alla fel kan kontrolleras genom att prenumerera på en händelse som kallas `sendNotificationError`. Här följer ett kodexempel som visar hur du prenumererar på meddelandefel med hjälp av Node.js SDK.

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

Se till att granska [funktioner](supported-features.md) for [!UICONTROL on-device decisioning] när du stöter på problem.

### Beslutsaktiviteter på enheten körs inte på grund av målgrupp eller aktivitet som inte stöds

Ett vanligt problem som kan inträffa är [!UICONTROL on-device decisioning] aktiviteter körs inte på grund av att målgruppen används eller att aktivitetstypen inte stöds.

(1) Använd loggningsutdata för att granska posterna i egenskapen trace i ditt svarsobjekt. Identifiera kampanjegenskapen:

**Spåra utdata**

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

Du kommer att märka att den aktivitet du försöker kvalificera dig för inte finns i `campaigns` eftersom målgrupps- eller aktivitetstypen inte stöds. Om aktiviteten listas under `campaigns` -egenskapen beror ditt problem inte på en målgrupp eller aktivitetstyp som inte stöds.

(2) Leta reda på `rules.json` genom att titta på `trace` > `artifact` > `artifactLocation` i loggutdata och observera att din aktivitet saknas i `rules` > `mboxes` egenskap:

**Loggutdata**

```text {line-numbers="true"}
 ...
 rules: {
   mboxes: { },
   views: { }
 }
```

Till sist navigerar du till[!DNL Target]Användargränssnitt och lokalisering av aktiviteten i fråga: [experience.adobe.com/target](https://experience.adobe.com/target)

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

(2) Se till att du är kvalificerad för målgruppen genom att läsa `matchedRuleConditions` eller `unmatchedRuleConditions` egenskapen för dina trace-utdata:

**Spåra utdata**

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

**Spåra utdata**

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

Titta på `artifactLastRetrieved` datum för artefakten och se till att du har den senaste `rules.json` fil som laddats ned till din app.

(2) Hitta `evaluatedCampaignTargets` i loggutdata:

**Loggutdata**

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

(3) Granska `context`, `page`och `referring` data för att säkerställa att de är så förväntade som detta kan påverka aktivitetens målinriktningskvalifikation.

(4) Granska `campaignId` för att säkerställa att aktiviteten eller aktiviteterna som du väntar dig att utföra utvärderas. The `campaignId` matchar aktivitets-ID på aktivitetsöversiktsfliken i[!DNL Target]Gränssnitt:

![alt-bild](assets/asset-activity-id-target-ui.png)

(5) Granska `matchedRuleConditions` och `unmatchedRuleConditions` identifiera problem med kvalificering för målgruppsregler för en viss aktivitet.

(6) Granska den senaste `rules.json` för att säkerställa att aktiviteten eller aktiviteterna som du vill köra lokalt inkluderas. Platsen anges ovan i steg 1.

(7) Se till att du använder samma mbox-namn i din begäran och dina aktiviteter.

(8) Se till att du använder målgruppsregler och aktivitetstyper som stöds.

### Ett serveranrop görs trots att aktivitetsinställningarna under en mbox säger &quot;Enhetsbeslut är berättigade&quot; i[!DNL Target]användargränssnitt

Det finns några skäl till att ett serveranrop görs trots att enheten är berättigad till enhetsbeslut:

* När mbox som används för aktiviteten&quot;Enhetsbeslut är berättigade&quot; även används för andra aktiviteter som inte är&quot;Enhetsval är berättigade&quot; visas mbox under `remoteMboxes` i `rules.json` artefakt. När en mbox visas under `remoteMboxes`, alla `getOffer(s)` anrop till mbox resulterar i ett serveranrop.

* Om du ställer in en aktivitet under en arbetsyta/egenskap och inte inkluderar samma aktivitet när du konfigurerar SDK kan detta orsaka `rules.josn` av standardarbetsytan som ska hämtas, som kan använda mbox under `remoteMboxes` -avsnitt.
