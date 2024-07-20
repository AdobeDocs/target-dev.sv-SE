---
title: Integration med Experience Cloud
description: Integration med Experience Cloud
keywords: leverans-API
source-git-commit: f16903556954d2b1854acd429f60fbf6fc2920de
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 0%

---

<!-- The content on this page was originally pulled from the legacy Delivery API doc site, and was subsequently found to be an outdated version of information now found on [Implement > Server-side > Integration > A4T Reporting](../../implement/server-side/sdk-guides/integration-with-experience-cloud/a4t-reporting.md) and [Implement > Server-side > Integration > AAM Segments](../../implement/server-side/sdk-guides/integration-with-experience-cloud/aam-segments.md). Therefore sunsetting this page, but not deleting it from the repo immediately, pending a more thorough examination to avoid inadvertently deleting relevant content. -->


# Integration med Experience Cloud

## Adobe Analytics for Target (A4T)

När ett Target Delivery API-anrop utlöses från servern returnerar Adobe Target användarupplevelsen och dessutom returnerar Adobe Target Adobe Analytics-nyttolasten till anroparen eller vidarebefordrar den automatiskt till Adobe Analytics. För att kunna skicka Target-aktivitetsinformation till Adobe Analytics på serversidan finns det några krav som måste uppfyllas:

1. Aktiviteten ställs in i Adobe Target-gränssnittet med Adobe Analytics som rapportkälla och konton aktiveras för A4T
1. Adobe Marketing Cloud Visitor-ID genereras av API-användaren och är tillgängligt när API-anropet för målleverans aktiveras

### Adobe Target Vidarebefordrar Analytics-nyttolasten automatiskt

Adobe Target kan automatiskt vidarebefordra analysnyttolasten till Adobe Analytics via serversidan om följande identifierare anges:

1. `supplementalDataId` - ID:t som används för att sammanfoga objekt mellan Adobe Analytics och Adobe Target
1. `trackingServer` - Analaytics-servern Adobe För att Adobe Target och Adobe Analytics ska kunna sammanfoga data på rätt sätt måste samma `supplementalDataId` skickas till både Adobe Target och Adobe Analytics.

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
      "id": {
        "marketingCloudVisitorId": "2304820394812039"
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
      "experienceCloud": {
        "analytics": {
          "supplementalDataId" : "23423498732598234",
          "trackingServer": "ags041.sc.omtrdc.net",
          "logging": "server_side"
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

### Hämta Analytics-nyttolast från Adobe Target

Konsumenterna av Adobe Target Delivery API kan hämta Adobe Analytics-nyttolasten för en motsvarande ruta så att konsumenten kan skicka nyttolasten till Adobe Analytics via [API:t för datainmatning](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md). När ett Adobe Target-anrop på serversidan utlöses skickar du `client_side` till fältet `logging` i begäran. Detta returnerar i sin tur en nyttolast om mbox finns i en aktivitet som använder Analytics som rapportkälla.

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
      "experienceCloud": {
        "analytics": {
          "logging": "client_side"
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
            "index" : 1
          },
          {
            "name" : "SummerShoesOffer",
            "index" : 2       
          },
          {
            "name" : "SummerDressOffer",
            "index" : 3       
          }      
        ]
      }
    }'
```

När du har angett `logging` = `client_side` får du nyttolasten i fältet `mbox` enligt nedan.

```
{
    "status": 200,
    "requestId": "4b8855a5-8354-4ac4-8ae7-c551f7c0bb8a",
    "client": "demo",
    "id": {
        "tntId": "d359234570e04f14e1faeeba02d6ab9914e.28_7"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "execute": {
        "mboxes": [
            {
                "index": 1,
                "name": "homepage",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                        "type": "html",

                    }
                ],
                "analytics": {
                    "payload": {
                        "pe": "tnt",
                        "tnta": "285408:0:0|2"
                    }
                }
            },
            {
                "index": 2,
                "name": "SummerShoesOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next shoe purchase</b></p>",
                        "type": "html",
                    }
                ]
            },
            {
                "index": 3,
                "name": "SummerDressOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next dress purchase</b></p>",
                        "type": "html",
                    }
                ]
            }
        ]
    }
}
```

Om svaret från Target innehåller något i egenskapen `analytics` -> `payload` vidarebefordrar du det som det är till Adobe Analytics. Analyserna kan hantera den här nyttolasten. Detta kan göras i en GET-begäran i följande format:

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/0/CODEVERSION?pe=tnt&tnta={payload}&mid={mid}&vid={vid}&aid={aid}
```

### Frågesträngsparametrar och variabler

| Fältnamn | Obligatoriskt | Beskrivning |
| --- | --- | --- |
| `rsid` | Ja | Rapportsvitens ID |
| `pe` | Ja | Sidhändelse. Alltid inställd på `tnt` |
| `tnta` | Ja | Analysnyttolasten returnerades av målservern i `analytics` -> `payload` -> `tnta` |
| `mid` | Marketing Cloud Visitor-ID |

### Obligatoriska rubrikvärden

| Rubriknamn | Huvudvärde |
| --- | --- |
| Värd | Server för insamling av analysdata (t.ex. adobeags421.sc.omtrdc.net) |

### Exempel på A4T-datainmatning HTTP - Hämta anrop

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0?pe=tnt&tnta=285408:0:0|2&mid=2304820394812039
```

## Adobe Audience Manager

Adobe Audience Manager (AAM)-segment kan också utnyttjas via Adobe Target Delivery API:er. För att utnyttja AAM ska följande fält anges:

| Fältnamn | Obligatoriskt | Beskrivning |
| --- | --- | --- |
| `locationHint` | Ja | DCS-platstips används för att avgöra vilken AAM DCS-slutpunkt som ska tryckas för att hämta profilen. Måste vara >= 1. |
| `marketingCloudVisitorId` | Ja | Marketing Cloud Visitor-ID |
| `blob` | Ja | AAM används för att skicka ytterligare data till AAM. Får inte vara tom och storleken &lt;= 1024. |

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
      "id": {
        "marketingCloudVisitorId": "2304820394812039"
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
      "experienceCloud": {
        "audienceManager": {
          "locationHint": 9,
          "blob": "32fdghkjh34kj5h43"
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
            "index" : 1
          },
          {
            "name" : "SummerShoesOffer",
            "index" : 2       
          },
          {
            "name" : "SummerDressOffer",
            "index" : 3       
          }      
        ]
      }
    }'
```
