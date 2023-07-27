---
title: Användarbehörigheter för Adobe Target Delivery API
description: Användarbehörigheter för Adobe Target Delivery API
badgePremium: label="Premium" type="Positive" url="https://experienceleague.adobe.com/docs/target/using/introduction/intro.html?lang=en#premium newtab=true" tooltip="Se vad som ingår i Target Premium."
keywords: leverans-API
exl-id: 332f90bd-4079-4653-aa38-b35837631c94
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# Användarbehörigheter (Premium)

[!DNL Adobe] ger kunderna möjlighet att hantera behörigheter för sina användare när de använder Adobe Target. För att lyckas [!UICONTROL Adobe Target Delivery API] anrop måste en token med rätt behörigheter skickas inom API-anropet. Om du vill veta mer om användarbehörighet och hur du hämtar token går du till [den här dokumentationen](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html).

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
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
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
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

När du har en motsvarande token skickar du den till `property` -> `token` för varje API-anrop som görs. Om `property` -> `token` skickas inte inom varje API-anrop, du får inga `content` tillbaka från Adobe Target.

```
{
    "status": 200,
    "requestId": "07ce783d-58b9-461c-9f4c-6873aeb00c01",
    "client": "demo",
    "id": {
        "tntId": "d359234570e04f14e1faeeba02d6ab9914e.28_7"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "execute": {
        "mboxes": [
            {
                "index": 1,
                "name": "homepage"
            }
        ]
    }
}
```

Som du kan se ovan, utan att skicka `property` -> `token`får du inget innehåll tillbaka. Om du förväntar dig innehåll från ditt API-anrop men inte hämtar något från svaret, är det mest troligt eftersom antingen  `property` -> `token` har inte angetts eller skickas utan rätt behörighet.
