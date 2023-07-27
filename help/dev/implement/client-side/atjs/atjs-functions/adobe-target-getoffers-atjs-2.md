---
keywords: adobe.target.getOffers, getOffers, getoffers, get offers, at.js, functions, function, $8
description: Använd [!UICONTROL adobe.target.getOffers()] och dess alternativ för [!DNL Adobe Target] at.js-bibliotek som ska utlösa förfrågningar om att få flera [!DNL Target] erbjudanden. (at.js 2.x)
title: Hur jag använder [!UICONTROL adobe.target.getOffers()] Funktion?
feature: at.js
exl-id: b96a3018-93eb-49e7-9aed-b27bd9ae073a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1289'
ht-degree: 0%

---

# [!UICONTROL adobe.target.getOffers()] - at.js 2.x

Med den här funktionen kan du hämta flera erbjudanden genom att skicka in flera rutor. Dessutom kan flera erbjudanden hämtas för alla vyer i aktiva aktiviteter.

>[!NOTE]
>
>Den här funktionen introducerades med at.js 2.x. Den här funktionen är inte tillgänglig för at.js version 1.*x*.

| Nyckel | Typ | Obligatoriskt? | Beskrivning |
| --- | --- | --- | --- |
| `consumerId` | Sträng | Nej | Standardvärdet är klientens globala mbox om den inte anges. Den här nyckeln används för att generera det extra data-ID (SDID) som används för A4T-integrering.<P>När du använder `getOffers()`genererar varje anrop ett nytt SDID. Om du har flera mbox-begäranden på samma sida och vill bevara SDID (så att det matchar SDID:t från målglobal-mbox och [!DNL Adobe Analytics] SDID) använder du `consumerId` parameter.<P>If `getOffers()` innehåller tre rutor (mbox1, mbox2 och mbox3): `consumerId: "mbox1, mbox2, mbox3"` i `getOffers()` ring. |
| `decisioningMethod` | Sträng | Nej | &quot;server-side&quot;, &quot;on device&quot;, &quot;hybrid&quot; |
| `request` | Objekt | Ja | Se tabellen över förfrågningar nedan. |
| `timeout` | Nummer | Nej | Timeout för begäran. Om inget anges används standardtimeout för at.js. |

## Begäran

>[!NOTE]
>
>Läs [Dokumentation för leverans-API](/help/dev/implement/delivery-api/overview.md) för information om godkända typer för alla fält som listas nedan.

| Fältnamn | Obligatoriskt? | Begränsningar | Beskrivning |
| --- | --- | --- | --- |
| request > id | Nej |  | En av `tntId`, `thirdPartyId`, eller `marketingCloudVisitorId` är obligatoriskt. |
| Begäran > id > thirdPartyId | Nej | Maximal storlek = 128. |  |  |
| Request > experienceCloud | Nej |  |  |
| Request > experienceCloud > analytics | Nej |  | Integrering med Adobe Analytics |
| Request > experienceCloud > analytics > log | Nej | Följande måste implementeras på sidan:<ul><li>Tjänst för besökar-ID</li><li>Appmeasurement.js</li></ul> | Följande värden stöds:<P>**client_side**: När det anges returneras en analyslast till anroparen som ska användas för att skicka till [!UICONTROL Adobe Analytics] via [!UICONTROL Data Insertion API].<P>**server_side**: Detta är standardvärdet där [!DNL Target] och [!DNL Analytics] backend använder SDID för att sammanfoga samanrop i rapporteringssyfte. |
| Begäran > förhämtning | Nej |  |  |
| Request > prefetch > views | Nej | Högsta antal 50.<P>Namnet är inte tomt.<P>Namnlängd `<=` 128.<P>Värdelängd `<=` 5000.<P>Namnet får inte börja med &quot;profile&quot;.<P>Otillåtna namn: &quot;orderId&quot;, &quot;orderTotal&quot;, &quot;productPurchasedId&quot;. | Ange parametrar som ska användas för att hämta relevanta vyer i aktiva aktiviteter. |
| Request > prefetch > views > profileParameters | Nej | Maximantal 50.<P>Namnet är inte tomt.<P>Namnlängd `<=` 128.<P>Värdelängd `<=` 5000.<P>Accepterar endast strängvärden.<P>Namnet får inte börja med &quot;profile&quot;. | Ange profilparametrar som ska användas för att hämta relevanta vyer i aktiva aktiviteter. |
| Request > prefetch > views > product | Nej |  |  |
| Request > prefetch > views > product -> id | Nej | Inte tom.<P>maxstorlek = 128. | Ange produkt-ID:n som ska användas för att hämta relevanta vyer i aktiva aktiviteter. |
| Request > prefetch > views > product > categoryId | Nej | Inte tom.<P>maxstorlek = 128. | Ange produktkategori-ID:n som ska användas för att hämta relevanta vyer i aktiviteter. |
| Request > prefetch > views > order | Nej |  |  |
| Begäran > förhämtning > vyer > ordning > id | Nej | Maximal längd = 250. | Ange beställnings-ID:n som ska användas för att hämta relevanta vyer i aktiva aktiviteter. |
| Request > prefetch > views > order > total | Nej | Totalt `>=` 0. | Ange ordersummor som ska användas för att hämta relevanta vyer i aktiva aktiviteter. |
| Request > prefetch > views > order > purchaseProductIds | Nej | Inga tomma värden.<P>Varje värde har maxlängden 50.<P>Sammanfogad och avgränsad med komma.<P>Total längd för produkt-ID `<=` 250. | Skicka in köpta produkt-ID:n som ska användas för att hämta relevanta vyer i aktiva aktiviteter. |
| Begäran > Kör | Nej |  |  |
| Request > execute > pageLoad | Nej |  |  |
| Request > execute > pageLoad > parameters | Nej | Högsta antal 50.<P>Namnet är inte tomt.<P>Namnlängd `<=` 128.<P>Värdelängd `<=` 5000.<P>Accepterar endast strängvärden.<P>Namnet får inte börja med &quot;profile&quot;.<P>Otillåtna namn: &quot;orderId&quot;, &quot;orderTotal&quot;, &quot;productPurchasedId&quot;. | Hämta erbjudanden med angivna parametrar när sidan läses in. |
| Request > execute > pageLoad > profileParameters | Nej | Högsta antal 50.<P>Namnet är inte tomt.<P>Namnlängd `<=` 128.<P>Värdelängd `<=`256.<P>Namnet får inte börja med &quot;profile&quot;.<P>Accepterar endast strängvärden. | Hämta erbjudanden med angivna profilparametrar när sidan läses in. |
| Request > execute > pageLoad > product | Nej |  |  |
| Request > execute > pageLoad > product -> id | Nej | Inte tom.<P>Maximal storlek = 128. | Hämta erbjudanden med angivna produkt-ID när sidan läses in. |
| Request > execute > pageLoad > product > categoryId | Nej | Inte tom.<P>Maximal storlek = 128. | Hämta erbjudanden med angivna produktkategori-ID när sidan läses in. |
| Request > execute > pageLoad > order | Nej |  |  |
| Request > execute > pageLoad > order > id | Nej | Maximal längd = 250. | Hämta erbjudanden med angivna order-ID när sidan läses in. |
| Request > execute > pageLoad > order > total | Nej | `>=` 0. | Hämta erbjudanden med angivna ordersummor när sidan läses in. |
| Request > execute > pageLoad > order > purchaseProductIds | Nej | Inga tomma värden.<P>Varje värde har maxlängden 50.<P>Sammanfogad och avgränsad med komma.<P>Total längd för produkt-ID `<=` 250. | Hämta erbjudanden med angivna produkt-ID:n när sidan läses in. |
| Request > execute > mboxes | Nej | Maximal storlek = 50.<P>Inga null-element. |  |
| Request > execute > mboxes>mbox | Ja | Inte tom.<P>Inget &#39;-klickat&#39;-suffix.<P>Maximal storlek = 250.<P>Tecken: `'-, ._\/=:;&!@#$%^&*()_+|?~[]{}'` | Namn på mbox. |
| Begäran > Kör > mbox>mbox>index | Ja | Inte null.<P>Unik.<P>`>=` 0. | Observera att indexet inte representerar den ordning i vilken rutorna bearbetas. På samma sätt som på en webbsida med flera regionala kryssrutor kan ordningen som de ska bearbetas i inte anges. |
| Request > execute > mboxes > mbox > parameters | Nej | Högsta antal = 50.<P>Namnet är inte tomt.<P>Namnlängd `<=` 128.<P>Accepterar endast strängvärden.<P>Värdelängd `<=` 5000.<P>Namnet får inte börja med &quot;profile&quot;.<P>Otillåtna namn: &quot;orderId&quot;, &quot;orderTotal&quot;, &quot;productPurchasedId&quot;. | Hämta erbjudanden för en given mbox med de angivna parametrarna. |
| Request > execute > mboxes>mbox>profileParameters | Nej | Högsta antal = 50.<P>Namnet är inte tomt.<P>Namnlängd `<=` 128.<P>Accepterar endast strängvärden.<P>Värdelängd `<=`256.<P>Namnet får inte börja med &quot;profile&quot;. | Hämta erbjudanden för en given mbox med de angivna profilparametrarna. |
| Request > execute > mboxes>mbox > product | Nej |  |  |
| Request > execute > mboxes > mbox > product > id | Nej | Inte tom.<P>Maximal storlek = 128. | Hämta erbjudanden för en angiven mbox med angivet produkt-ID. |
| Request > execute > mboxes > mbox > product > categoryId | Nej | Inte tom.<P>Maximal storlek = 128. | Hämta erbjudanden för en angiven mbox med angivna produktkategori-ID:n. |
| Request > execute > mboxes > mbox > order | Nej |  |  |
| Request > execute > mbox>mbox > order > id | Nej | Maximal längd = 250. | Hämta erbjudanden för en angiven mbox med de angivna order-ID:n. |
| Request > execute > mboxes > mbox > order > total | Nej | `>=` 0. | Hämta erbjudanden för en angiven mbox med de angivna ordersummorna. |
| Request > execute > mboxes > mbox > order > purchaseProductIds | Nej | Inga tomma värden.<P>Varje värdes maximala längd = 50.<P>Sammanfogad och avgränsad med komma.<P>Total längd för produkt-id `<=` 250. | Hämta erbjudanden för en angiven mbox med angivet produkt-ID för beställning. |

## Utlysning [!UICONTROL getOffers()] för alla vyer

```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
      prefetch: {
        views: [{}]
    }
  }
});
```

## Utlysning [!UICONTROL getOffers()] att fatta ett beslut på enheten

```javascript {line-numbers="true"}
adobe.target.getOffers({ 

  decisioningMethod:"on-device", 
  request: { 
    execute: { 
      mboxes: [ 
        { 
          index: 0, 
          name: "homepage" 
        } 
      ] 
    } 
 } 
}); 
```

## Utlysning [!UICONTROL getOffers()] för att hämta de senaste vyerna med skickade parametrar och profilparametrar

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    "prefetch": {
      "views": [
        {
          "parameters": {
            "ad": "1"
          },
          "profileParameters": {
            "age": 23
          }
        }
      ]
    }
  }
});
```

## Utlysning [!UICONTROL getOffers()] för att hämta rutor med parametrar och profilparametrar skickade.

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    execute: {
      mboxes: [
        {
          index: 0,
          name: "first-mbox"
        },
        {
          index: 1,
          name: "second-mbox",
          parameters: {
            a: 1
          },
          profileParameters: {
            b: 2
          }
        }
      ]
    }
  }
});
```

## Utlysning [!UICONTROL getOffers()] för att hämta analysens nyttolast från klientsidan

```javascript {line-numbers="true"}
adobe.target.getOffers({
      request: {
        experienceCloud: {
          analytics: {
            logging: "client_side"
          }
        },
        prefetch: {
          mboxes: [{
            index: 0,
            name: "a1-serverside-xt"
          }]
        }
      }
    })
    .then(console.log)
```

**Svar**:

```javascript {line-numbers="true"}
{
  "prefetch": {
    "mboxes": [{
      "index": 0,
      "name": "a1-serverside-xt",
      "options": [{
        "content": "<img src=\"http://s7d2.scene7.com/is/image/TargetAdobeTargetMobile/L4242-xt-usa?tm=1490025518668&fit=constrain&hei=491&wid=980&fmt=png-alpha\"/>",
        "type": "html",
        "eventToken": "n/K05qdH0MxsiyH4gX05/2qipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
        "responseTokens": {
          "profile.memberlevel": "0",
          "geo.city": "bucharest",
          "activity.id": "167169",
          "experience.name": "USA Experience",
          "geo.country": "romania"
        }
      }],
      "analytics": {
        "payload": {
          "pe": "tnt",
          "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
        }
      }
    }]
  }
}
```

Nyttolasten kan sedan vidarebefordras till [!DNL Adobe Analytics] via [API för datainfogning](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md).

## Hämta och återge data från flera rutor via [!UICONTROL getOffers()] och [!UICONTROL applyOffers()]

Med at.js 2.x kan du hämta flera mbox via `[!UICONTROL getOffers()]` API. Du kan även hämta data för flera mbox-rutor och sedan använda `[!UICONTROL applyOffers()]` för att återge data på olika platser som identifieras av en CSS-väljare.

I följande exempel visas en enkel HTML-sida med at.js 2.x implementerad:

```html {line-numbers="true"}
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>at.js 2.x, multiple selectors and multiple mboxes</title>
  <script src="at.js"></script>
</head>
<body>
  <div id="container1">Default content 1</div>
  
  <div id="container2">Default content 2</div>

  <div id="container3">Default content 3</div>
</body>
</html>
```

Anta att du har tre behållare som du vill ändra via innehåll som tagits emot från [!DNL Target]. Du kan skapa en enda begäran för tre mrutor där varje mbox har visst innehåll som ska återges i respektive behållare.

Koden för begäran och återgivning kan se ut som i följande exempel:

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    prefetch: {
      mboxes: [
        {
          index: 0,
          name: "mbox1"
        },
        {
          index: 1,
          name: "mbox2"
        },
        {
          index: 2,
          name: "mbox3"
        }
      ]
    }
  }
})
.then(response => {
  // get all mboxes from response
  const mboxes = response.prefetch.mboxes;
  let count = 1;

  mboxes.forEach(el => {
    adobe.target.applyOffers({
      selector: "#container" + count,
      response: {
        prefetch: {
          mboxes: [el]
        }
      }
    });

    count += 1;
  });
});
```

I `request > prefetch > mboxes` finns det tre olika kartonger. Om begäran slutfördes utan fel får du svaret för varje ruta från `response > prefetch > mboxes`. När du har fått svaren och de platser du vill använda för återgivningen kan du anropa `applyOffers()` för att återge innehåll som hämtats från [!DNL Target]. I det här exemplet har vi följande mappning:

* mbox1 > CSS-väljare #container1
* mbox2 > CSS-väljare #container2
* mbox3 > CSS-väljare #container3

I det här exemplet används variabeln count för att skapa CSS-väljarna. I ett verkligt scenario kan du använda en annan mappning mellan CSS-väljaren och mbox.

Observera att det här exemplet använder `prefetch > mboxes`, men du kan också använda `execute > mboxes`. Om du använder förhämtning i `getOffers()`ska du också använda förhämtning i `applyOffers()` anrop.

## Utlysning [!UICONTROL getOffers()] för att utföra en pageLoad

I följande exempel visas hur du utför en pageLoad med [!UICONTROL getOffers()] med at.js 2.*x*

```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
        execute: {
            pageLoad: {
                parameters: {}
            }
        }
    }
});
```
