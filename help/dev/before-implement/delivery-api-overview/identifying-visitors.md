---
title: Adobe Target Delivery API Identifying Visitors
description: Hur identifierar jag användare inom  [!DNL Adobe Target]?
keywords: leverans-API
exl-id: 5b8c28aa-caad-44a9-880a-3c5f844e47b2
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 0%

---

# Identifiera besökare

Det finns flera sätt att identifiera en besökare inom [!DNL Adobe Target].

Målet använder tre identifierare:

| Fältnamn | Beskrivning |
| --- | --- |
| `tntId` | `tntId` är den primära identifieraren i [!DNL Target] för en användare. Du kan ange detta ID, annars genereras det automatiskt av [!DNL Target] om begäran inte innehåller någon. |
| `thirdPartyId` | `thirdPartyId` är företagets identifierare för användaren som du kan skicka med varje samtal. När en användare loggar in på ett företags webbplats skapar företaget vanligtvis ett ID som är knutet till besökarens konto, förmånskort, medlemsnummer eller andra tillämpliga identifierare för det företaget. |
| `marketingCloudVisitorId` | `marketingCloudVisitorId` används för att sammanfoga och dela data mellan olika Adobe-lösningar. `marketingCloudVisitorId` krävs för integrering med Adobe Analytics och Adobe Audience Manager. |
| `customerIds` | Tillsammans med besökar-ID:t för Experience Cloud kan ytterligare [kund-ID:n](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=sv-SE) och en autentiserad status för varje besökare användas. |

## [!DNL Target]-ID

[!DNL Target]-ID:t eller `tntId` kan ses som ett enhets-ID. `tntId` genereras automatiskt av [!DNL Target] om det inte anges i begäran. Därefter måste efterföljande begäranden inkludera `tntId` för att rätt innehåll ska kunna levereras till en enhet som används av användaren.

```http {line-numbers="true"}
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
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
        "name" : "SummerOffer",
        "index" : 1
      }
    ]
  }
}'
```

Exemplanropet ovan visar att `tntId` inte behöver skickas in. I det här scenariot genererar [!DNL Target] en `tntId` och anger den i svaret, vilket visas här:

```URI {line-numbers="true"}
{
  "status": 200,
  "requestId": "5b586f83-890c-46ae-93a2-610b1caa43ef",
  "client": "demo",
  "id": {
      "tntId": "10abf6304b2714215b1fd39a870f01afc.28_20"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  ...
}
```

Den genererade `tntId` är `10abf6304b2714215b1fd39a870f01afc.28_20`. Observera att `tntId` måste användas när du anropar [!UICONTROL Adobe Target Delivery API] för samma användare i flera sessioner.

## Marketing Cloud Visitor-ID

`marketingCloudVisitorId` är ett universellt och beständigt ID som identifierar dina besökare över alla lösningar i Experience Cloud. När din organisation implementerar ID-tjänsten kan du med detta ID identifiera samma besökare och deras data i olika Experience Cloud-lösningar som Adobe Target, Adobe Analytics eller Adobe Audience Manager. Observera att `marketingCloudVisitorId` krävs när du använder och integrerar med Analytics och Audience Manager.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "marketingCloudVisitorId": "10527837386392355901041112038610706884"
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
    "execute": {
    "mboxes" : [
      {
        "name" : "SummerOffer",
        "index" : 1
      }
    ]
  }
}'
```

Exemplanropet ovan visar hur en `marketingCloudVisitorId` som hämtades från Experience Cloud ID-tjänsten skickas till Adobe Target. I det här scenariot genererar [!DNL Target] en `tntId` eftersom den inte skickades till det ursprungliga anropet som mappas till den angivna `marketingCloudVisitorId` enligt svaret nedan.

## Tredjeparts-ID

Om din organisation använder ett ID för att identifiera besökaren kan du använda `thirdPartyID` för att leverera innehåll. Du måste dock ange `thirdPartyID` för varje [!UICONTROL Adobe Target Delivery API] samtal du gör.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "thirdPartyId": "B234A029348"
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
    "execute": {
    "mboxes" : [
      {
        "name" : "SummerOffer",
        "index" : 1
      }
    ]
  }
}'
```

Exemplanropet ovan visar en `thirdPartyId`, som är ett beständigt ID som används av ditt företag för att identifiera en slutanvändare oavsett om de interagerar med ditt företag via webb-, mobil- eller IoT-kanaler. `thirdPartyId` kommer med andra ord att referera till användarprofildata som kan användas i alla kanaler. I det här scenariot genererar [!DNL Target] en `tntId` eftersom den inte skickades till det ursprungliga anropet, som mappas till den angivna `thirdPartyId` enligt svaret nedan.

```
{
    "status": 200,
    "requestId": "55de9886-bd14-4dee-819c-7d1633b79b90",
    "client": "demo",
    "id": {
        "tntId": "10abf6304b2714215b1fd39a870f01afc.28_20",
        "thirdPartyId": "B234A029348"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    ...
}
```

## Kund-ID

[Kund-ID:n](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=sv-SE) kan läggas till och kopplas till ett Experience Cloud Visitor-ID. När `customerIds` skickas måste även `marketingCloudVisitorId` anges. Dessutom kan en autentiseringsstatus anges tillsammans med varje `customerId` för varje besökare. Följande autentiseringsstatus kan beaktas:

| Autentiseringsstatus | Användarstatus |
| --- | --- |
| `unknown` | Okänd eller aldrig autentiserad. Det här läget kan användas för scenarier som en besökare som har landat på din plats genom att klicka på ett visningsmeddelande. |
| `authenticated` | Användaren autentiseras för närvarande med en aktiv session på din webbplats eller i din app. |
| `logged_out` | Användaren autentiserades men loggades aktivt ut. Användaren avsåg att koppla från det autentiserade tillståndet. Användaren vill inte längre behandlas som autentiserad. |

Observera att endast när kund-ID:t är i läget `authenticated` kommer Target att referera till användarprofildata som lagras och länkas till kund-ID:t. Om kund-ID:t är i läget `unknown` eller `logged_out` ignoreras kund-ID:t och alla användarprofildata som är associerade med det används inte för målgruppsanpassning.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e044f14e1faeeba02d6ab23439914e' \
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
        "marketingCloudVisitorId" : "2304820394812039",
        "customerIds": [{
          "id": "134325423",
          "integrationCode" : "crm_data",
          "authenticatedState" : "authenticated"
        }]
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

Exemplanropet ovan visar hur du skickar en `customerId` med en `authenticatedState`. När du skickar en `customerId` krävs både `integrationCode`, `id` och `authenticatedState` samt `marketingCloudVisitorId`. `integrationCode` är aliaset för den [kundattributfil](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html?lang=sv-SE) som du tillhandahöll via CRS.

## Sammanfogad profil

Du kan kombinera `tntId`, `thirdPartyID` och `marketingCloudVisitorId` i samma begäran. I det här scenariot behåller Adobe Target mappningen av alla dessa ID:n och fäster den vid en besökare. Lär dig hur profiler [sammanfogas och synkroniseras i realtid](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=sv-SE) med olika identifierare.

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e044f14e1faeeba02d6ab23439914e' \
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
        "marketingCloudVisitorId" : "2304820394812039",
        "tntId": "d359234570e044f14e1faeeba02d6ab23439914e.28_78",
        "thirdPartyId":"23423432"
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

Exemplanropet ovan visar hur du kan kombinera `tntId`, `thirdPartyID` och `marketingCloudVisitorId` i samma begäran. Alla tre ID:n returneras också i svaret.
