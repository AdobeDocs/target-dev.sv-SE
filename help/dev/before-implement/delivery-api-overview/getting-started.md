---
title: Adobe Target Delivery API Getting Started
description: Hur använder jag [!UICONTROL Adobe Target Delivery API]?
keywords: leverans-API
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
source-git-commit: e5a1c38d448cb7446b7b26cd0dc882976ba94dd3
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 0%

---

# Komma igång med [!UICONTROL Adobe Target Delivery API]

Ett [!UICONTROL Target Delivery API]-samtal ser ut så här:

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

`clientCode` kan hämtas från [!DNL Target]-gränssnittet genom att gå till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.

Innan du gör ett [!UICONTROL Target Delivery API]-anrop följer du de här stegen för att se till att ett svar innehåller den relevanta upplevelsen för att visa slutanvändarna:

1. Skapa en [!DNL Target]-aktivitet (A/B, XT, AP eller Recommendations) med [formulärbaserad disposition](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=sv-SE) eller [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=sv-SE).
1. Använd leverans-API:t för att få ett svar för rutorna som används i aktiviteten [!DNL Target] som skapades i steg 2.
1. Presentera upplevelsen för besökaren.
