---
title: Adobe Target Delivery API Single eller Batch Delivery
description: Hur använder jag [!UICONTROL Adobe Target Delivery API] Enskilda samtal eller batchsamtal?
keywords: leverans-API
exl-id: 525cd1f2-616a-486c-8f49-8117615500bb
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# Enskild leverans eller gruppleverans

The [!UICONTROL Adobe Target Delivery API] har stöd för ett enstaka eller grupperat leveranssamtal. Man kan begära en server för innehåll för en eller flera mbox-filer.

Väg upp prestandakostnaderna när du bestämmer dig för att ringa ett samtal eller ett gruppsamtal. Om du känner till allt innehåll som behöver visas för en användare är det bästa sättet att hämta innehåll för alla mbox med ett enda batchleveranssamtal, för att undvika att ringa flera leveranssamtal.

## Enstaka leveranssamtal

Du kan hämta en upplevelse som ska visas för användaren för en mbox via [!UICONTROL Adobe Target Delivery API]. Observera att om du gör ett enda leveranssamtal måste du starta ett annat serversamtal för att hämta ytterligare innehåll för en mbox för en användare. Detta kan bli mycket kostsamt med tiden, så se till att utvärdera hur du använder ett enda API-anrop för leverans.

```
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=7abf6304b2714215b1fd39a870f01afc#1555632114' \
-H 'Content-Type: application/json' \
-H 'cache-control: no-cache' \
-d '
{
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

I exemplet med ett leveranssamtal ovan hämtas upplevelsen så att den visas för användaren med `tntId`: `abcdefghijkl00023.1_1` för `mbox`:`SummerOffer` på webbkanalen. Det här samtalet kommer att generera följande svar:

```
{
  "status": 200,
  "requestId": "25e0cc42-3d7b-456a-8b49-af60c1fb23d9",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.1_1"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "execute": {
      "mboxes": [
          {
              "index": 1,
              "name": "SummerOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                      "type": "html",
                  }
              ]
          }
      ]
    }
}
```

Observera följande i svaret: `content` -fältet innehåller HTML som beskriver den upplevelse som ska visas för användaren för webben som motsvarar Sommarerbjudandemlådan.

### Kör sidinläsning

Om det finns upplevelser som ska visas när en sidinläsning sker i webbkanalen, t.ex. AB-testning av teckensnitt i sidfoten eller sidhuvudet, kan du ange `pageLoad` i `execute` om du vill hämta alla ändringar som ska tillämpas.

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
    "window": {
      "width": 1819,
      "height": 842
    },
    "browser": {
      "host": "target.enablementadobe.com"
    },
    "address": {
      "url": "https://target.enablementadobe.com/react/demo/#/"
    }
  },
  "execute": {
    "pageLoad": {}
  }
}'
```

Exempelanropet ovan hämtar alla upplevelser som kan visa en användare när sidan `https://target.enablementadobe.com/react/demo/#/` laddas.

```
{
      "status": 200,
      "requestId": "355ebc47-edb6-481f-aeae-ae55d71afaca",
      "client": "demo",
      "id": {
          "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
      },
      "edgeHost": "mboxedge28.tt.omtrdc.net",
      "execute": {
          "pageLoad": {
              "options": [
                  {
                      "content": [
                          {
                              "type": "setHtml",
                              "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > NAV.nav:eq(0) > DIV.container:eq(0) > DIV.nav-right:eq(0) > A.nav-item:eq(0)",
                              "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > NAV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(2) > A:nth-of-type(1)",
                              "content": "Modified Home"
                          }
                      ],
                      "type": "actions"
                  }
              ],
              "metrics": [
                  {
                      "type": "click",
                      "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(0) > FORM.col-md-4:eq(0) > DIV.form-group:eq(0) > BUTTON.btn:eq(0)",
                      "eventToken": "QPaLjCeI9qKCBUylkRQKBg=="
                  }
              ]
          }
      }
  }
```

I `content` kan den ändring som ska tillämpas på en sidinläsning hämtas. Observera att en länk i rubriken måste namnges i exemplet ovan *Ändrad startsida*.

## Batchsamtal för leverans

Istället för att ringa flera leveranssamtal med en enda mbox i varje samtal, kan onödiga serversamtal undvikas om du gör ett leveranssamtal med en batch av mbox-meddelanden. Anrop av ett serversamtal bör minimeras så mycket som möjligt för att vara högpresterande.

```
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=7abf6304b2714215b1fd39a870f01afc#1555632114' \
-H 'Content-Type: application/json' \
-H 'cache-control: no-cache' \
-d '
{
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
    "execute": {
    "mboxes" : [
      {
        "name" : "SummerOffer",
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

I exemplet med gruppsamtal ovan hämtas upplevelser som ska visas för användaren med `tntId`: `abcdefghijkl00023.1_1` för flera `mbox`:`SummerOffer`, `SummerShoesOffer`och `SummerDressOffer`. Eftersom vi vet att vi behöver visa en upplevelse av flera kryssrutor för den här användaren kan vi batchera dessa förfrågningar och ringa ett serversamtal i stället för tre individuella leveranssamtal.

```
{
  "status": 200,
  "requestId": "fe15286f-effb-434f-85d8-c3db804075ce",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.28_120"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "execute": {
      "mboxes": [
          {
              "index": 1,
              "name": "SummerOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                      "type": "html",

                  }
              ]
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

I svaret ovan ser du att i `content` för varje mbox går det att hämta HTML-representationen av upplevelsen som ska visas för användaren för varje mbox.
