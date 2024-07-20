---
title: Adobe Target Delivery API Notifications
description: Hur skickar jag meddelanden med [!UICONTROL Adobe Target Delivery API]?
keywords: leverans-API
exl-id: 711388fd-2c1f-4ca4-939f-c56dc4bdc04a
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%

---

# Meddelanden

Meddelanden ska utlösas när en förhämtad ruta eller vy har besökts eller renderats för slutanvändaren.

Om du vill att meddelanden ska stängas av för rätt mbox eller vy måste du hålla reda på motsvarande `eventToken` för varje mbox eller vy. Meddelanden med rätt `eventToken` för motsvarande kryssrutor eller vyer måste utlösas för att rapporteringen ska kunna visas korrekt.

## Meddelanden för förhämtade kartor

Ett eller flera meddelanden kan skickas via ett enda leveranssamtal. Avgör om måttet som behöver spåras är antingen `click` eller `display` för varje mbox så att `type` i meddelandet kan återspeglas korrekt. Skicka dessutom en `id` för varje meddelande så att du kan avgöra om ett meddelande skickades korrekt via [!UICONTROL  Adobe Target Delivery API]. `timestamp` är också viktigt att vidarebefordras till [!DNL Target] för att ange när `click` eller `display` inträffade för en given mbox för rapportering.

```
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
-H 'Content-Type: application/json' \
-H 'cache-control: no-cache' \
-d '{
    "id": {
      "tntId": "abcdefghijkl00023.1_1"
    },
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
      "notifications": [
      {
      "id" : "SummerOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
      },
    {
      "id" : "SummerShoesOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerShoesOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
      },
    {
      "id" : "SummerDressOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerDressOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
    } 
    ]
  }'
```

Exempelanropet ovan resulterar i ett svar som anger att `notifications`-begäran har bearbetats.

```
{
  "status": 200,
  "requestId": "36014eed-4772-4c48-a9e2-e532762b6a85",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.28_20"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "notifications": [
      {
          "id": "SummerOfferNotification"
      },
      {
          "id": "SummerDressOfferNotification"
      },
      {
          "id": "SummerShoesOfferNotification"
      }
  ]
}
```

Om alla `notifications` som skickas till [!DNL Target] bearbetas korrekt visas de i arrayen `notifications` i svaret. Om `notifications` `id` saknas gick den speciella `notification` inte igenom. I det här scenariot kan en återförsökslogik användas tills ett `notification`-svar har hämtats. Kontrollera att tidsgränsen för återförsökslogiken har angetts så att API-anropet inte blockerar och orsakar prestandafördröjningar.

## Meddelanden för förhämtade vyer

Ett eller flera meddelanden kan skickas via ett enda leveranssamtal. Avgör om måttet som behöver spåras är antingen `click` eller `display` för varje mbox så att meddelandetypen kan återspeglas korrekt. Skicka dessutom en `id` för varje meddelande så att du kan avgöra om ett meddelande skickades korrekt via [!UICONTROL Adobe Target Delivery API]. Tidsstämpeln är också viktig att vidarebefordra till [!DNL Target] för att ange när `click` eller `display` inträffade för en viss vy i rapporteringssyfte.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
  },
  "context": {
    "channel": "web",
    "browser": {
      "host": "target.enablementadobe.com"
    },
    "address": {
      "url": "https://target.enablementadobe.com/react/demo/#/"
    }
  },
  "notifications": [{
      "id": "228",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "checkout-express",
      }
    },
    {
      "id": "5",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "home",
      }
    },
    {
      "id": "6",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "products",
      }
    }
  ]
}'
```

Exempelanropet ovan resulterar i ett svar som anger att `notifications`-begäran har bearbetats.

```
{
    "status": 200,
    "requestId": "85cc7394-c19a-4398-9b8b-bbee1e4c4579",
    "client": "demo",
    "id": {
        "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "notifications": [
        {
            "id": "5"
        },
        {
            "id": "6"
        },
        {
            "id": "228"
        }
    ]
}
```

Om alla `notifications` som skickas till [!DNL Target] bearbetas korrekt visas de i arrayen `notifications` i svaret. Om `notifications` `id` saknas gick dock det inte att slutföra det aktuella meddelandet. I det här scenariot kan en återförsökslogik implementeras tills ett godkänt meddelandesvar hämtas. Kontrollera att tidsgränsen för återförsökslogiken har angetts så att API-anropet inte blockerar och orsakar prestandafördröjningar.
