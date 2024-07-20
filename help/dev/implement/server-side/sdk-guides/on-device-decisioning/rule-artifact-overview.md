---
title: Förstå artefakten för beslutsregler på enheter
description: Lär dig hur du använder regelartefakten, som är en JSON-representation av dina  [!DNL Adobe Target] [!UICONTROL on-device decisioning]-aktiviteter.
feature: APIs/SDKs
exl-id: 3dfb08df-eaa9-43d4-b009-e5f64c3a96d7
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Översikt över regelartefakt

Regelartefakten är en JSON-representation av dina [!DNL Adobe Target] [!UICONTROL on-device decisioning]-aktiviteter. Den genereras av [!DNL Adobe Target] och sprids till Akamai CDN för att säkerställa att det finns en regelartefakt som är tillgänglig så nära dina slutanvändare som möjligt. Det innehåller metadata som gör att du kan utföra och leverera dina aktiviteter på ett exakt sätt, samtidigt som du kan göra realtidsanalyser via händelsespårning. SDK:erna för [!DNL Adobe Target] kan konfigureras på ett sätt som möjliggör automatisk hantering av regelartefakten, som kan hämtas eller uppdateras enligt ett användarspecificerat tidsintervall. Dessutom kan du underhålla din egen lokala kopia av regelartefakten med ett distribuerat minnescachningssystem som [Memcached](https://memcached.org/) för att initiera [!DNL Adobe Target] SDK, så att dina tillståndslösa servrar kan hantera begäranden direkt. Mer information om dessa alternativ finns i följande handböcker:

* [Hämta, lagra och uppdatera regelfelaktigheten automatiskt via  [!DNL Adobe Target] SDK](rule-artifact-sdk.md)
* [Hämta, lagra och uppdatera regelfelaktigheten via JSON-nyttolast](rule-artifact-json.md)

## Exempel på regelartefakt

Klicka här om du vill se ett exempel på [regelartefakt](rule-artifact-example.md).

## Så här visar du regelartefakten för klienten

Om du aktiverar spårningar genereras ytterligare information från [!DNL Adobe Target] om regelartefakten, särskilt URL:en.

1. Navigera till målgränssnittet.

   &lt;!— Insert image-target-ui-1.png —>
   ![alt-bild](assets/asset-rule-artifact-1.png)

1. Navigera till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** och klicka på **[!UICONTROL Generate New Authorization Token]**.

   &lt;!— Insert image-target-ui-2.png —>
   ![alt-bild](assets/asset-rule-artifact-2.png)

1. Kopiera den nyligen genererade auktoriseringstoken till Urklipp och lägg till den i din Target-begäran.

   ```javascript {line-numbers="true"}
   const request = {
     trace: {
       authorizationToken: '88f1a924-6bc5-4836-8560-2f9c86aeb36b'
     },
     execute: {
       mboxes: [{
         address: getAddress(req),
         name: "node-sdk-mbox"
       }]
   }};
   ```

1. Skriv ut målspårningen via terminalen för att visa detaljer om artefakten. URL:en är tillgänglig via variabeln `artifactLocation`.

   ```
   "trace": {
     "clientCode": "your-client-code",
     "artifact": {
       "artifactLocation": "https://assets.adobetarget.com/your-client-code/production/v1/rules.bin",
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
