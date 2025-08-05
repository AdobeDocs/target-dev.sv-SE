---
title: Jämföra at.js med Experience Platform Web SDK
description: Lär dig hur funktionen at.js fungerar jämfört med  [!DNL Experience Platform Web SDK].
keywords: mål;adobe target;activity.id;experience.id;renderDecision;DecisionScopes;prehide snippet;vec;Form Based Experience Composer;xdm;audiences;Decision;scope;schema;system chart;chart
feature: AEP Web SDK
source-git-commit: d6b93537692a1efbc2650a015f5a44d4fd1fd422
workflow-type: tm+mt
source-wordcount: '2006'
ht-degree: 0%

---

# Jämför at.js-biblioteket med [!DNL Adobe Experience Platform Web SDK]

## Ökning

I den här artikeln finns en översikt över skillnaderna mellan biblioteket `at.js` och Experience Platform Web SDK.

## Installera biblioteken

### Installera at.js

Med [!DNL Adobe] kan kunder hämta biblioteket direkt från fliken [!DNL Adobe Experience Cloud], [!UICONTROL Implementation]. At.js-biblioteket anpassas med inställningar som kunden har: clientCode, imsOrgId osv.

### Installera SDK

Den färdiga versionen finns tillgänglig på ett CDN. Du kan referera till biblioteket på CDN direkt på din sida eller hämta och lagra det på din egen infrastruktur. Den finns i minifierade och ominifierade format. Den ominiatyrversionen är användbar i felsökningssyfte.

Mer information finns i [Installera Web SDK med JavaScript-biblioteket](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/install/library).

## Konfigurera biblioteken

### Konfigurerar at.js

I slutet av varje at.js-fil visas ett avsnitt där [!DNL Adobe] instansierar och skickar ett inställningsobjekt. Det går att anpassa, och när Adobe laddar ned den fylls den delen i med de aktuella kundinställningarna.

```javascript
window.adobe.target.init(window, document, {
  "clientCode": "demo",
  "imsOrgId": "",
  "serverDomain": "localhost:5000",
  "timeout": 2000,
  "globalMboxName": "target-global-mbox",
  "version": "2.0.0",
  "defaultContentHiddenStyle": "visibility: hidden;",
  "defaultContentVisibleStyle": "visibility: visible;",
  "bodyHiddenStyle": "body {opacity: 0 !important}",
  "bodyHidingEnabled": true,
  "deviceIdLifetime": 63244800000,
  "sessionIdLifetime": 1860000,
  "selectorsPollingTimeout": 5000,
  "visitorApiTimeout": 2000,
  "overrideMboxEdgeServer": false,
  "overrideMboxEdgeServerTimeout": 1860000,
  "optoutEnabled": false,
  "optinEnabled": false,
  "secureOnly": false,
  "supplementalDataIdParamTimeout": 30,
  "authoringScriptUrl": "//cdn.tt.omtrdc.net/cdn/target-vec.js",
  "urlSizeLimit": 2048,
  "endpoint": "/rest/v1/delivery",
  "pageLoadEnabled": true,
  "viewsEnabled": true,
  "analyticsLogging": "server_side",
  "serverState": {},
  "decisioningMethod": "server-side",
  "legacyBrowserSupport":  false
});
```

[Läs mer](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)

### Konfigurera Platform Web SDK

Konfigurationen för SDK görs med kommandot [`configure`](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/commands/configure/overview). Kommandot `configure` anropas *alltid* först.

## Så här begär och återger du sidinläsning [!DNL Target] automatiskt

### Använda at.js

Om du använder at.js 2.x och aktiverar inställningen `pageLoadEnabled,` utlöser biblioteket ett anrop till [!DNL Target] Edge med `execute -> pageLoad`. Om alla inställningar är inställda på standardvärden behövs ingen anpassad kodning. När at.js har lagts till på sidan och lästs in av webbläsaren körs ett [!DNL Target] Edge-anrop.

### Använder [!DNL PLatform Web SDK]

Innehåll som skapats i [!DNL Target] [Visual Experience Composer](https://experienceleague.adobe.com/sv/docs/target/using/experiences/vec/visual-experience-composer) kan hämtas och återges automatiskt av SDK.

Om du vill begära och automatiskt återge [!DNL Target] erbjudanden använder du kommandot `sendEvent` och ställer in alternativet `renderDecisions` på `true.`. På så sätt tvingas SDK automatiskt att återge anpassat innehåll som är kvalificerat för automatisk återgivning.

Exempel:

```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

[!DNL Experience Platform Web SDK] skickar automatiskt ett meddelande med erbjudanden som kördes av [!DNL Platform WEB SDK]. Detta är ett exempel på hur en nyttolast för en meddelandebegäran ser ut:

```json
{
  "events": [{
      "xdm": {
        "_experience": {
          "decisioning": {
            "propositions": [
              {
                "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
                "scope": "cart",
                "scopeDetails": {
                  "decisionProvider": "TGT",
                  "activity": {
                    "id": "127019"
                  },
                  "experience": {
                    "id": "0"
                  },
                  "strategies": [
                    {
                      "step": "entry",
                      "algorithmID": "0",
                      "trafficType": "0"
                    },
                    {
                      "step": "display",
                      "algorithmID": "0",
                      "trafficType": "0"
                    }
                  ],
                  "characteristics": {
                    "eventToken": "bKMxJ8dCR1XlPfDCx+2vSGqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                  }
                }
              }
            ]
          }
        },
        "eventType": "display",
        "web": {
          "webPageDetails": {
            "viewName": "cart",
            "URL": "https://alloyio.com/personalizationSpa/cart"
          },
          "webReferrer": {
            "URL": ""
          }
        },
        "device": {
          "screenHeight": 800,
          "screenWidth": 1280,
          "screenOrientation": "landscape"
        },
        "environment": {
          "type": "browser",
          "browserDetails": {
            "viewportWidth": 1280,
            "viewportHeight": 284
          }
        },
        "placeContext": {
          "localTime": "2021-12-10T15:50:34.467+02:00",
          "localTimezoneOffset": -120
        },
        "timestamp": "2021-12-10T13:50:34.467Z",
        "implementationDetails": {
          "name": "https://ns.adobe.com/experience/alloy",
          "version": "2.6.2",
          "environment": "browser"
        }
      }
    }
  ]
}
```

[Läs mer](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## Så här begär och *INTE* återger automatiskt sidinläsningsmål

### Använda at.js

Det finns två sätt att utlösa ett anrop till [!DNL Target] Edge som hämtar erbjudanden för sidinläsning.

Exempel 1:

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox", 
   success: console.log,
   error: console.error
});
```

Exempel 2:

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {}
    }
  }
})
.then(console.log)
.catch(console.error);
```

[Läs mer](https://experienceleague.adobe.com/sv/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/atjs-functions)

### Använder [!DNL Platform Web SDK]

Kör ett `sendEvent`-kommando med ett särskilt omfång under `decisionScopes`: `__view__`. [!DNL Adobe] använder det här omfånget som en signal för att hämta alla sidinläsningsaktiviteter från [!DNL Target] och hämta alla vyer i förväg. [!DNL Platform Web SDK] försöker också utvärdera alla VEC-vybaserade aktiviteter. Inaktivering av vyförhämtning stöds för närvarande inte i [!DNL Platform Web SDK].

Om du vill få åtkomst till anpassat innehåll kan du tillhandahålla en callback-funktion som anropas när SDK har fått ett lyckat svar från servern. Ditt återanrop är ett resultatobjekt som kan innehålla egenskapen proposition som innehåller allt returnerat personaliseringsinnehåll.

Exempel:

```javascript
alloy("sendEvent", {
    xdm: {...},
    decisionScopes: ["__view__"]
  }).then(function(result) {
    if (result.propositions) {
      result.propositions.forEach(proposition => {
        proposition.items.forEach(item => {
          if (item.schema === HTML_SCHEMA) {
            // manually apply offer
            document.getElementById("form-based-offer-container").innerHTML =
              item.data.content;
            const executedPropositions = [
              {
                id: proposition.id,
                scope: proposition.scope,
                scopeDetails: proposition.scopeDetails
              }
            ];
          // manually send the display notification event, so that Target/Analytics impressions aare increased
            alloy("sendEvent",{
              "xdm": {
                "eventType": "decisioning.propositionDisplay",
                "_experience": {
                  "decisioning": {
                    "propositions": executedPropositions
                  }
                }
              }
            });
          }
        });
      });
    }
  });
```

[Läs mer](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## Hur man begär specifika formulärbaserade målmodeller

### Använda at.js

Du kan hämta aktiviteter med funktionen `getOffer`:

Exempel 1:

```javascript
adobe.target.getOffer({
   mbox: "hero-banner", 
   success: console.log,
   error: console.error
});
```

Exempel 2:

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        mboxes: [
        {
          index: 0,
          name: "hero-banner"
        }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

[Läs mer](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/atjs-functions.html?lang=sv-SE)

### Använder [!DNL Platform Web SDK]

Du kan hämta [!UICONTROL Form-Based Composer]-baserade aktiviteter genom att använda kommandot `sendEvent` och skicka mbox-namnen under alternativet `decisionScopes`. Kommandot `sendEvent` returnerar ett löfte som löses med ett objekt som innehåller de begärda aktiviteterna/förslagen:

Detta kodfragment är hur arrayen `propositions` ser ut:

```javascript
[
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "hero-banner",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "experience": {
        "id": "0"
      },
      "strategies": [
        {
          "algorithmID": "0",
          "trafficType": "0"
        }
      ],
      "characteristics": {
        "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw=="
      }
    },
    "items": [
      {
        "id": "1184844",
        "schema": "https://ns.adobe.com/personalization/html-content-item",
        "meta": {
          "geo.state": "bucuresti",
          "activity.id": "434689",
          "experience.id": "0",
          "activity.name": "a4t test form based activity",
          "offer.id": "1184844",
          "profile.tntId": "04608610399599289452943468926942466370-pybgfJ"
        },
        "data": {
          "id": "1184844",
          "format": "text/html",
          "content": "<div> analytics impressions </div>"
        }
      }
    ]
  },
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "hero-banner",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "characteristics": {
        "eventToken": "E0gb6q1+WyFW3FMbbQJmrg=="
      }
    },
    "items": [
      {
        "id": "434689",
        "schema": "https://ns.adobe.com/personalization/measurement",
        "data": {
          "type": "click",
          "format": "application/vnd.adobe.target.metric"
        }
      }
    ]
  }
]
```

Exempel:

```javascript
alloy("sendEvent", {
  xdm: { ...},
  decisionScopes: ["hero-banner"]
}).then(function (result) {
  var propositions = result.propositions;

  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      for (var j = 0; j < proposition.items; j++) {
        var item = proposition.items[j];
        if (item.schema === HTML_SCHEMA) {
          // apply offer
          document.getElementById("form-based-offer-container").innerHTML =
            item.data.content;
          const executedPropositions = [
            {
              id: proposition.id,
              scope: proposition.scope,
              scopeDetails: proposition.scopeDetails
            }
          ];

          alloy("sendEvent", {
            "xdm": {
              "eventType": "decisioning.propositionDisplay",
              "_experience": {
                "decisioning": {
                  "propositions": executedPropositions
                }
              }
            }
          });
        }
      }
    }
  }
});
```

[Läs mer](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## Så här tillämpar du [!DNL Target]-aktiviteterna

### Använda at.js

Du kan tillämpa [!DNL Target]-aktiviteterna med funktionen `applyOffers`: `adobe.target.applyOffer(options).`

Exempel:

```javascript
adobe.target.getOffers({...})
  .then(response => adobe.target.applyOffers({ response: response }))
  .then(() => console.log("Success"))
  .catch(error => console.log("Error", error));
```

Läs mer om kommandot `applyOffers` i den [dedikerade dokumentationen](https://experienceleague.adobe.com/sv/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/adobe-target-applyoffers-atjs-2).

### Använder [!DNL Platform Web SDK]

Du kan använda [!DNL Target]-aktiviteter med kommandot `applyPropositions`.

Exempel:

```javascript
alloy("applyPropositions", {
    propositions: [...]
});
```

Läs mer om kommandot `applyPropositions` i den [dedikerade dokumentationen](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/personalization/rendering-personalization-content).

## Spåra händelser

### Använda at.js

Du kan spåra händelser med funktionen `trackEvent` eller med `sendNotifications.`

Den här funktionen utlöser en begäran om att rapportera användaråtgärder, till exempel klickningar och konverteringar. Den här funktionen levererar inte aktiviteter som svar.

**Exempel 1**

```javascript
adobe.target.trackEvent({ 
    "type": "click",
    "mbox": "some-mbox"
});
```

**Exempel 2**

```javascript
adobe.target.sendNotifications({ 
    request: {
       notifications: [{
          ...,
          mbox: {
            name: "some-mbox"
          },
          type: "click",
          ...
       }]
    }
});
```

[Läs mer](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-trackevent.html?lang=sv-SE)

### Använder [!DNL Platform Web SDK]

Du kan spåra händelser och användaråtgärder genom att anropa kommandot `sendEvent`, fylla i `_experience.decisioning.propositions` XDM `fieldgroup` och ställa in `eventType` på ett av två värden:

* `decisioning.propositionDisplay`: Signalerar återgivningen av aktiviteten [!DNL Target].
* `decisioning.propositionInteract`: Signalerar en användarinteraktion med aktiviteten, som ett musklick.

`_experience.decisioning.propositions` XDM `fieldgroup` är en objektmatris. Egenskaperna för varje objekt härleds från `result.propositions` som returneras i kommandot `sendEvent`: `{ id, scope, scopeDetails }.`

**Exempel 1 - Spåra en `decisioning.propositionDisplay` -händelse efter återgivning av en aktivitet**

```javascript
alloy("sendEvent", {
  xdm: {},
  decisionScopes: ['discount']
}).then(function(result) {
  var propositions = result.propositions;

  var discountProposition;
  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      if (proposition.scope === "discount") {
        discountProposition = proposition;
        break;
      }
    }
  }

  if (discountProposition) {
    // Find the item from proposition that should be rendered.
    // Rather than assuming there a single item that has HTML
    // content, find the first item whose schema indicates
    // it contains HTML content.
    for (var j = 0; j < discountProposition.items.length; j++) {
      var discountPropositionItem = discountProposition.items[i];
      if (discountPropositionItem.schema === "https://ns.adobe.com/personalization/html-content-item") {
        var discountHtml = discountPropositionItem.data.content;
        // Render the content
        var dailySpecialElement = document.getElementById("daily-special");
        dailySpecialElement.innerHTML = discountHtml;
        
        // For this example, we assume there is only a single place to update in the HTML.
        break;  
      }
    }
    // Send a "decisioning.propositionDisplay" event signaling that the proposition has been rendered.
    alloy("sendEvent", {
      "xdm": {
        "eventType": "decisioning.propositionDisplay",
        "_experience": {
          "decisioning": {
            "propositions": [{
              "id": id,
              "scope": scope,
              "scopeDetails": scopeDetails
            }],
            "propositionEventType": {
              "display": 1
            }
          }
        }
      }
    });
  }
});
```

**Exempel 2 - Spåra en `decisioning.propositionInteract` -händelse när ett klickmått inträffar**

```javascript
alloy("sendEvent", {
  xdm: { ...},
  decisionScopes: ["hero-banner"]
}).then(function (result) {
  var propositions = result.propositions;

  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      for (var j = 0; j < proposition.items.length; j++) {
        var item = proposition.items[j];

        if (item.schema === "https://ns.adobe.com/personalization/measurement") {
          // add metric to the DOM element
          const button = document.getElementById("form-based-click-metric");

          button.addEventListener("click", event => {
            const executedPropositions = [
              {
                id: proposition.id,
                scope: proposition.scope,
                scopeDetails: proposition.scopeDetails
              }
            ];
            // send the click track event
            alloy("sendEvent", {
              "xdm": {
                "eventType": "decisioning.propositionInteract",
                "_experience": {
                  "decisioning": {
                    "propositions": executedPropositions
                  }
                }
              }
            });
          });
        }
      }
    }
  }
});
```

[Läs mer](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/personalization/rendering-personalization-content#manual)

**Exempel 3 - Spåra en händelse som utlösts efter en åtgärd**

I det här exemplet spåras en händelse som utlöstes när en viss åtgärd utfördes, till exempel när en knapp klickades.
Du kan lägga till ytterligare anpassade parametrar via dataobjektet `__adobe.target`.

Du kan också lägga till XDM-objektet `commerce`.

```js
alloy("sendEvent", {
    "xdm": {
        "_experience": {
            "decisioning": {
                "propositions": [
                    {
                        "scope": "orderConfirm" //example scope name
                    }
                ],
                "propositionEventType": {
                    "display": 1
                }
            }
        },
        "eventType": "decisioning.propositionDisplay"
    },
    "commerce": {
        "order": {
            "purchaseID": "a8g784hjq1mnp3",
            "purchaseOrderNumber": "VAU3123",
            "currencyCode": "USD",
            "priceTotal": 999.98
        }
    },
    "data": {
        "__adobe": {
            "target": {
                "pageType": "Order Confirmation",
                "user.categoryId": "Insurance"
            }
        }
    }
})
```

## Så här utlöser du en vyändring i ett enkelsidigt program

### Använda at.js

Använd funktionen `adobe.target.triggerView`. Den här funktionen kan anropas när en ny sida läses in eller när en komponent på en sida återges på nytt. Funktionen `adobe.target.triggerView()` ska implementeras för SPA (Single page applications) för att använda [!UICONTROL Visual Experience Composer] (VEC) för att skapa [!UICONTROL A/B Test] - och [!UICONTROL Experience Targeting] (XT)-aktiviteter. Om `adobe.target.triggerView()` inte har implementerats på platsen kan VEC inte användas för SPA.

**Exempel**

```javascript
adobe.target.triggerView("homeView")
```

[Läs mer](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)

### Använder [!DNL Platform Web SDK]

Om du vill utlösa eller signalera ett enkelsidigt program [!UICONTROL View Change] anger du egenskapen `web.webPageDetails.viewName` under alternativet `xdm` för kommandot `sendEvent`. [!DNL Platform Web SDK] kontrollerar visningscachen. Om det finns erbjudanden för `viewName` som anges i `sendEvent` körs de och en visningsmeddelandehändelse skickas.

**Exempel**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  xdm:{
    web:{
      webPageDetails:{
        viewName: "homeView"
      }
    }
  }
});
```

[Läs mer](/help/dev/implement/client-side/aep-web-sdk/spa-implementation.md)

## Så här använder du [!UICONTROL Response Tokens]

Personalization-innehåll som returneras från [!DNL Target] innehåller [svarstoken](https://experienceleague.adobe.com/sv/docs/target/using/administer/response-tokens). Svarstoken är information om aktivitet, erbjudande, upplevelse, användarprofil, geo-information med mera. Dessa uppgifter kan delas med verktyg från tredje part eller användas för felsökning. Svarstoken kan konfigureras i användargränssnittet [!DNL Target].

### Använda at.js

Använd anpassade at.js-händelser för att lyssna efter [!DNL Target]-svaret och läsa svarstoken.

**Exempel**

```javascript
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(e) { 
  console.log("Request succeeded", e.detail); 
}); 
```

[Läs mer](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=sv-SE)

### Använder [!DNL Platform Web SDK]

>[!IMPORTANT]
>
>Kontrollera att du använder [!DNL Experience Platform Web SDK] version 2.6.0 eller senare.

Svarstoken returneras som en del av `propositions` som visas i resultatet av kommandot `sendEvent`. Varje förslag innehåller en matris på `items,` och varje objekt har ett `meta`-objekt ifyllt med svarstoken om de är aktiverade i [!DNL Target]-administratörens användargränssnitt. [Läs mer](https://experienceleague.adobe.com/sv/docs/target/using/administer/response-tokens)

**Exempel**

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Format of result.propositions:
      /*
        [
            {
                "id": "",
                "scope": "",
                "items": [
                    {
                        "id": "",
                        "schema": "",
                        "data": {},
                        "meta": { // RESPONSE TOKENS
                            "activity.name": ...,
                            "offer.id": ...,
                            "profile.activeActivities": ...
                        }
                    }
                ],
                "scopeDetails": {}
                "renderAttempted": false
            }
        ]
      */
    }
  });
```

[Läs mer](/help/dev/implement/client-side/aep-web-sdk/accessing-response-tokens.md)

## Hantera flimmer

### Använda at.js

Med at.js kan du hantera flimmer genom att ställa in `bodyHidingEnabled: true` så att at.js är den som tar hand om
fördölja de anpassade behållarna innan den hämtar och tillämpar DOM-ändringarna.

Sidavsnitten som innehåller anpassat innehåll kan döljas i förväg genom att åsidosätta at.js `bodyHiddenStyle.`

Som standard döljer `bodyHiddenStyle` hela HTML `body.`

Båda inställningarna kan åsidosättas med `window.targetGlobalSettings.` `window.targetGlobalSettings` innan du läser in at.js.

### Använder [!DNL Platform Web SDK]

Med hjälp av [!DNL Platform Web SDK] kan kunden ställa in sin tidigare dolda stil i kommandot configure, som i följande exempel:

```javascript
alloy("configure", {
  datastreamId: "configurationId",
  orgId: "orgId@AdobeOrg",
  debugEnabled: true,
  prehidingStyle: "body { opacity: 0 !important }"
});
```

Vid inläsning av asynkron [!DNL Platform Web SDK] rekommenderar [!DNL Adobe] att följande utdrag injiceras på sidan innan [!DNL Platform Web SDK] injiceras:

```html
<script>
  !function(e,a,n,t){
  if (a) return;
  var i=e.head;if(i){
  var o=e.createElement("style");
  o.id="alloy-prehiding",o.innerText=n,i.appendChild(o),
  setTimeout(function(){o.parentNode&&o.parentNode.removeChild(o)},t)}}
  (document, document.location.href.indexOf("adobe_authoring_enabled") !== -1, "body { opacity: 0 !important }", 3000);
</script>
```

## Hur hanteras A4T

### Använda at.js

Det finns två typer av A4T-loggning som stöds med at.js:

* Loggning på klientsidan för analys
* Analytics Server Side Logging

#### Loggning på klientsidan för analys

**Exempel 1: Använder [!DNL Target] global inställning**

Loggning på klientsidan för analys kan aktiveras genom att `analyticsLogging: client_side` anges i at.js-inställningarna eller genom att `window.targetglobalSettings`-objektet åsidosätts.

När det här alternativet är konfigurerat ser nyttolastens format ut så här:

```json
{
  "analytics": {
    "payload": {
      "pe": "tnt",
      "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
    }
  }
}
```

Nyttolasten kan sedan vidarebefordras till [!DNL Analytics] via [!DNL &#x200B; Data Insertion API].

Exempel 2: Konfigurera den i varje `getOffers`-funktion:

```javascript
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

Detta kodfragment är hur svarsnyttolasten ser ut:

```json
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

[!DNL Analytics]-nyttolasten (`tnta`-token) ska inkluderas i [!DNL Analytics]-träffen med [API för datainmatning](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md).

#### [!DNL Analytics] loggning på serversidan

[!DNL Analytics] Loggning på serversidan kan aktiveras genom att `analyticsLogging: server_side` anges i at.js-inställningarna eller genom att `window.targetglobalSettings`-objektet åsidosätts.

Därefter flödar data enligt följande:

![Diagram som visar arbetsflödet för loggning på serversidan i Analytics](/help/dev/implement/client-side/aep-web-sdk/assets/a4t-server-side-atjs.png)

[Läs mer](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html?lang=sv-SE)

### Använder [!DNL Platform Web SDK]

SDK på webben har också stöd för:

* Loggning på klientsidan för analyser
* Loggning på serversidan i Analytics

#### [!DNL Analytics] loggning på klientsidan

[!DNL Analytics] Loggning på klientsidan aktiveras när [!DNL Adobe Analytics] inaktiveras för den DataStream-konfigurationen.

![Diagram som visar arbetsflödet för loggning på klientsidan för Analytics](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-disabled-datastream-config.png)

Kunden har åtkomst till den [!DNL Analytics]-token (`tnta`) som måste delas med [!DNL Analytics] med hjälp av [API för datainmatning](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) genom att kedja kommandot `sendEvent` och iterera genom den resulterande propositionsarrayen.

**Exempel**

```javascript
alloy("sendEvent", {
    "renderDecisions": true,
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function (results) {
  var analyticsPayloads = new Set();
  for (var i = 0; i < results.propositions.length; i++) {
    var proposition = results.propositions[i];
    var renderAttempted = proposition.renderAttempted;

    if (renderAttempted === true) {
      var analyticsPayload = getAnalyticsPayload(proposition);
      if (analyticsPayload !== undefined) {
        analyticsPayloads.add(analyticsPayload);
      }
    }
  }
  var analyticsPayloadsToken = concatenateAnalyticsPayloads(analyticsPayloads);
  // send the page view Analytics hit with collected Analytics payload using Data Insertion API
});
```

Här följer ett diagram som visar hur data flödar när [!DNL Analytics]-klientsidan är aktiverad:

![Dataflödesdiagram i loggning på klientsidan för Analytics](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-client-side-logging.png)

#### [!DNL Analytics] loggning på serversidan

[!DNL Analytics] Loggning på serversidan aktiveras när [!DNL Analytics] aktiveras för den DataStream-konfigurationen.

![Användargränssnitt för datastreams som visar analysinställningarna.](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-enabled-datastream-config.png)

När loggning på serversidan [!DNL Analytics] är aktiverad, måste A4T-nyttolasten delas med [!DNL Analytics] så att [!DNL Analytics]-rapporten visar korrekta avbildningar och konverteringar på Edge Network-nivå, så att kunden inte behöver utföra någon ytterligare bearbetning.

Så här flödar data in i systemen när loggning av serveranalys är aktiverat:

![Diagram som visar dataflödet i loggning för analys på serversidan](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-server-side-logging.png)

## Ange globala inställningar för [!DNL Target]

### Använda at.js

Du kan åsidosätta inställningarna i at.js-biblioteket med `window.targetGlobalSettings,` i stället för att konfigurera inställningarna i [!DNL Target]-gränssnittet eller med REST API:er.

Åsidosättningen bör definieras innan at.js läses in eller i Administration > Implementering > Redigera at.js-inställningar > Kodinställningar > Bibliotekshuvud.

Exempel:

```javascript
window.targetGlobalSettings = {  
   timeout: 200, // using custom timeout  
   visitorApiTimeout: 500, // using custom API timeout  
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com  
};
```

[Läs mer](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetgobalsettings.html?lang=sv-SE)

### Använder [!DNL Platform Web SDK]

Den här funktionen stöds inte i Web SDK.

## Så här uppdaterar du profilattribut för [!DNL Target]

### Använda at.js

**Exempel 1**

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox",
   params: {
     "profile.name": "test",
     "profile.gender": "female"
   },
   success: console.log,
   error: console.error
});
```

**Exempel 2**

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {
          profileParameters: {
            name: "test",
            gender: "female"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

### Använder [!DNL Platform Web SDK]

Om du vill uppdatera en [!DNL Target]-profil använder du kommandot `sendEvent` och anger egenskapen `data.__adobe.target` med prefix för nyckelnamnen med `profile.`

**Exempel**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "profile.gender": "female",
        "profile.age": 30
      }
    }
  }
});
```

## Hur använder jag [!DNL Target Recommendations]?

### Använda at.js

**Exempel 1**

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox",
   params: {
     "entity.name": "T-shirt",
     "entity.id": "1234"
   },
   success: console.log,
   error: console.error
});
```

**Exempel 2**

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {
          parameters: {
            "entity.name": "T-shirt",
            "entity.id": "1234"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

[Läs mer](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-getoffers-atjs-2.html?lang=sv-SE)

### Använder [!DNL Platform Web SDK]

Om du vill skicka [!DNL Recommendations]-data använder du kommandot `sendEvent` och anger egenskapen `data.__adobe.target` med prefix för nyckelnamnen med `entity.`

**Exempel**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "entity.name": "T-shirt",
        "entity.id": "1234"
      }
    }
  }
});
```

## Hur använder jag ID:n från tredje part?

### Använda at.js

Med at.js finns det flera sätt att skicka `mbox3rdPartyId`, med `getOffer,` eller `getOffers`:

**Exempel 1**

```javascript
adobe.target.getOffer({
  mbox:"test",
  params:{
    "mbox3rdPartyId": "1234"
  },
  success: console.log,
  error: console.error
});
```

**Exempel 2**

```javascript
adobe.target.getOffers({
    request: {
      id:{
        thirdPartyId: "1234"
      },
      execute: {
        pageLoad: {}
    }
  }
})
.then(console.log)
.catch(console.error);
```

Eller så finns det ett sätt att konfigurera `mbox3rdPartyId` i `targetPageParams` eller `targetPageParamsAll.`

När du anger `targetPageParams` skickas förfrågningar för `target-global-mbox` som också kallas `pag-lLoad`.

Rekommendationen ska anges med `targetPageParamsAll` eftersom den skickas i varje [!DNL Target]-begäran. Fördelen med att använda `targetPageParamsAll` är att du kan definiera `mbox3rdPartyId` på sidan en gång för att se till att alla [!DNL Target] -begäranden har rätt `mbox3rdPartyId.`

```javascript
window.targetPageParamsAll = function() {
      return {
        "mbox3rdPartyId": "1234"
      };
    };
```

```javascript
window.targetPageParams = function() {
  return {
    "mbox3rdPartyId": "1234"
  };
};
```

[Läs mer](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetpageparams.html?lang=sv-SE)

### Använder [!DNL Platform Web SDK]

[!DNL Platform Web SDK] stöder [!DNL Target] Tredje parts-ID. Det kräver dock några steg till.

Med identitetskartan kan kunderna skicka flera identiteter. Alla identiteter har ett namn. Varje namnutrymme kan ha en eller flera identiteter. En viss identitet kan markeras som primär. Med den här kunskapen i åtanke kan du se vilka steg som krävs för att konfigurera [!DNL Platform Web SDK] så att [!DNL Target] tredjeparts-ID används.

1. Konfigurera namnutrymmet som innehåller [!DNL Target] Tredje parts-ID på konfigurationssidan för datastream:

![Datastreams-gränssnitt som visar namnområdesfältet för mål-tredje parts-ID](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

1. Skicka identitetsnamnutrymmet i varje `sendEvent`-kommando så här:

```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "identityMap": {
      "TGT3PID": [
        {
          "id": "1234",
          "primary": true
        }
      ]
    }
  }
});
```

## Hur anger jag egenskapstoken?

### Använda at.js

Med at.js finns det två sätt att konfigurera egenskapstoken, antingen med `targetPageParams` eller med `targetPageParamsAll.` Using `targetPageParams` läggs egenskapstoken till i `target-global-mbox` -anropet, men med `targetPageParamsAll` läggs variabeln till i alla [!DNL Target] -anrop:

**Exempel 1**

```javascript
   window.targetPageParamsAll = function() {
      return {
        "at_property": "1234"
      };
    };
```

**Exempel 2**

```javascript
window.targetPageParams = function() {
      return {
        "at_property": "1234"
      };
    };
```

### Använder [!DNL Platform Web SDK]

Om du använder [!DNL Platform Web SDK]-kunder kan de konfigurera egenskapen på en högre nivå, när de konfigurerar datastream, under namnutrymmet [!DNL Adobe Target]:

![Användargränssnitt för datastreams som visar Adobe Target-inställningarna.](/help/dev/implement/client-side/aep-web-sdk/assets/at-property-setup.png)

Det innebär att varje [!DNL Target]-anrop för den specifika dataströmskonfigurationen innehåller den egenskapstoken.

## Hur förhämtar jag mbox-filer

### Använda at.js

Den här funktionen är bara tillgänglig i at.js 2.x. at.js 2.x har en ny funktion som heter `getOffers`. Funktionen `getOffers` gör att kunder kan hämta innehåll i förväg för en eller flera mbox-filer. Här är ett exempel:

```javascript
adobe.target.getOffers({
    request: {
      prefetch: {
        mboxes: [{
          index: 0,
          name: "test-mbox",
          parameters: {
            ...
          },
          profileParameters: {
            ...
          }
        }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!NOTE]
>
>Adobe rekommenderar att du ser till att varje `mbox` i arrayen `mboxes` har ett eget index. Vanligtvis har den första mbox `index=0` nästa `index=1,` och så vidare.

### Använder [!DNL Platform Web SDK]

Den här funktionen stöds inte i [!DNL Platform Web SDK].

## Hur felsöker jag min [!DNL Target]-implementering?

### Använda at.js

I biblioteket at.js visas dessa felsökningsfunktioner:

* Inaktivera Mbox - inaktivera [!DNL Target] från att hämta och återge för att kontrollera om sidan har brutits utan [!DNL Target]-interaktioner
* Mbox Debug - at.js loggar varje åtgärd
* Målspårning - med en mbox-spårningstoken som genereras i ett trace-objekt med information som deltar i beslutsprocessen är tillgänglig under objektet `window.___target_trace`.

>[!NOTE]
>
>Alla dessa felsökningsfunktioner är tillgängliga med förbättrade funktioner i [Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob).

### Använder [!DNL Platform Web SDK]

Du har flera felsökningsfunktioner när du använder [!DNL Platform Web SDK]:

* Använda [Assurance](https://experienceleague.adobe.com/sv/docs/experience-platform/assurance/home)
* [Felsökning för SDK på webben har aktiverats](https://experienceleague.adobe.com/sv/docs/experience-platform/assurance/home)
* Använd [SDK-webbövervakningskopplingar](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)
* Använd [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/sv/docs/experience-platform/debugger/home)
* Målspårning