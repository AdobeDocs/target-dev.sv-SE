---
title: Adobe Target Delivery API Getting Started
description: Hur använder jag [!UICONTROL Adobe Target Delivery API]?
keywords: leverans-API
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# Komma igång med [!UICONTROL Adobe Target Delivery API]

A [!UICONTROL Target Delivery API] ring ser ut så här:

```
curl -X POST \
  'https://`clientCode`.tt.omtrdc.net/rest/v1/delivery?client=`clientCode`&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
      "context": {
        "channel": "web",
        "browser" : {
          "host" : "demo"
        },
        "address" : {
          "url" : "http://demo.dev.tt-demo.com/demo/store/index.html"
        },
        "screen" : {
          "width" : 1200,
          "height": 1400
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
            "index" : 1
          }
        ]
      }
    }'
```

The `clientCode` kan hämtas från [!DNL Target] Gränssnitt genom att navigera till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.

Innan du skapar en [!UICONTROL Target Delivery API] ring, följ dessa steg för att säkerställa att ett svar innehåller relevant upplevelse som visar slutanvändarna:

1. Skapa en [!DNL Target] (A/B, XT, AP eller Recommendations) med [Formulärbaserad disposition](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en) eller [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html).
1. Använd leverans-API:t för att få ett svar på de rutor som används i [!DNL Target] aktivitet som skapats i steg 2.
1. Presentera upplevelsen för besökaren.

## Postman Collection {#postman}

Postman är ett program som gör det enkelt att utlösa API-anrop. [Den här Postman-samlingen](https://run.pstmn.io/button.svg) innehåller exempel på API-anrop för leverans.
