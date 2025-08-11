---
title: Loggning på klientsidan för A4T-data i Experience Platform Web SDK
description: Lär dig hur du aktiverar klientloggning för Adobe Analytics for Target (A4T) med Experience Platform Web SDK.
seo-title: Client-side logging for A4T data in the Experience Platform Web SDK
seo-description: Learn how to enable client-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: mål;a4t;logga;web sdk;upplevelse;plattform;
feature: Implementation
source-git-commit: 4d4ca7dcffdbaee5770084bd85c3109df0d6a8d4
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# Loggning på klientsidan för A4T-data i [!DNL Experience Platform Web SDK]

Med [!DNL Adobe Experience Platform Web SDK] kan du samla in [ Adobe Analytics for Target-data (A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=sv-SE) på klientsidan av webbprogrammet.

Loggning på klientsidan innebär att relevanta [!DNL Target]-data returneras på klientsidan, vilket gör att du kan samla in data och dela dem med [!DNL Analytics]. Det här alternativet bör vara aktiverat om du tänker skicka data manuellt till Analytics med [API:t för datainfogning](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html?lang=sv-SE).

>[!NOTE]
>
>En metod för att utföra detta med [AppMeasurement.js](https://experienceleague.adobe.com/docs/analytics/implementation/js/overview.html?lang=sv-SE) håller på att utvecklas och kommer att vara tillgänglig inom den närmaste framtiden.

Det här dokumentet innehåller stegen för konfiguration av A4T-loggning på klientsidan för [!DNL Platform Web SDK] och exempel på implementering för vanliga användningsområden.

## Förutsättningar {#prerequisites}

I den här självstudiekursen antas att du är bekant med de grundläggande begreppen och processerna som är kopplade till användningen av [!DNL Platform Web SDK] i personaliseringssyfte. Läs följande dokumentation om du behöver en introduktion:

* [Konfigurera SDK för webben](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/commands/configure/overview)
* [Skickar händelser](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/commands/sendevent/overview)
* [Återger anpassat innehåll](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## Konfigurera [!DNL Analytics]-loggning på klientsidan {#set-up-client-side-logging}

I följande underavsnitt beskrivs hur du aktiverar [!DNL Analytics]-loggning på klientsidan för din [!DNL Platform Web SDK]-implementering.

### Aktivera loggning på klientsidan för [!DNL Analytics] {#enable-analytics-client-side-logging}

Om du vill ta hänsyn till att [!DNL Analytics]-loggning på klientsidan är aktiverad för din implementering måste du inaktivera [!DNL Adobe Analytics]-konfigurationen i [datastream](https://experienceleague.adobe.com/sv/docs/experience-platform/datastreams/overview).

![Analytics-datastream-konfiguration inaktiverad](/help/dev/implement/a4t/assets/disable-analytics-datastream.png)

### Hämta [!DNL A4T]-data från SDK och skicka dem till [!DNL Analytics] {#a4t-to-analytics}

För att den här rapporteringsmetoden ska fungera på rätt sätt måste du skicka [!DNL A4T]-relaterade data som hämtats från kommandot [`sendEvent`](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/commands/sendevent/overview) i [!DNL Analytics]-träffen.

När [!DNL Target] Edge beräknar ett svar på en offert kontrollerar det om [!DNL Analytics]-loggning på klientsidan är aktiverad (till exempel om [!DNL Analytics] är inaktiverad i din datastream). Om loggning på klientsidan är aktiverad lägger systemet till en [!DNL Analytics]-token i varje förslag i svaret.

Flödet ser ut ungefär så här:

![Loggningsflöde på klientsidan](/help/dev/implement/a4t/assets/analytics-client-side-logging.png)

Följande är ett exempel på ett `interact`-svar när [!DNL Analytics]-loggning på klientsidan är aktiverad. Om förslaget gäller en aktivitet som har [!DNL Analytics] rapporter har det en `scopeDetails.characteristics.analyticsToken`-egenskap.

```json
{
  "requestId": "1234",
  "handle": [
    {
      "payload": [
        {
          "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
          "scope": "a4t-test",
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
              "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
              "analyticsToken": "434689:0:0|2,434689:0:0|1"
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
          "scope": "a4t-test",
          "scopeDetails": {
            "decisionProvider": "TGT",
            "activity": {
              "id": "434689"
            },
            "characteristics": {
              "eventToken": "E0gb6q1+WyFW3FMbbQJmrg==",
              "analyticsToken": "434689:0:0|32767"
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
      ],
      "type": "personalization:decisions",
      "eventIndex": 0
    }
  ]
}
```

Förslag för [!UICONTROL Form-based Experience Composer]-aktiviteter kan innehålla både innehåll och klicka på måttobjekt under samma förslag. I stället för att ha en enda analystoken för visning av innehåll i egenskapen `scopeDetails.characteristics.analyticsToken` kan de därför ha både en visnings- och en klickanalystoken angiven i egenskaperna `scopeDetails.characteristics.analyticsDisplayToken` och `scopeDetails.characteristics.analyticsClickToken`.

```json
{
  "requestId": "1234",
  "handle": [
    {
      "payload": [
        {
          "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
          "scope": "a4t-test",
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
               "displayToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
               "clickToken": "E0gb6q1+WyFW3FMbbQJmrg==",
               "analyticsDisplayToken": "434689:0:0|2,434689:0:0|1", 
               "analyticsClickToken": "434689:0:0|32767"
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
            },
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
      ],
      "type": "personalization:decisions",
      "eventIndex": 0
    }
  ]
}
```

Alla värden från `scopeDetails.characteristics.analyticsToken` samt `scopeDetails.characteristics.analyticsDisplayToken` (för visat innehåll) och `scopeDetails.characteristics.analyticsClickToken` (för klickvärden) är de A4T-nyttolaster som måste samlas in och inkluderas som `tnta` -tagg i [API-anropet för datainmatning](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md).

>[!IMPORTANT]
>
>Egenskaperna `analyticsToken`, `analyticsDisplayToken`, `analyticsClickToken` kan innehålla flera tokens, sammanfogade som en kommaavgränsad sträng.
>
>I implementeringsexemplen i nästa avsnitt samlas flera [!DNL Analytics]-tokens in iterativt. Om du vill sammanfoga en matris med [!DNL Analytics]-tokens använder du en funktion som ser ut så här:
>
>```javascript
>var concatenateAnalyticsPayloads = function concatenateAnalyticsPayloads(analyticsPayloads) {
>   if (analyticsPayloads.size > 1) {
>       return [].concat(analyticsPayloads).join(',');
>   }
>   return [].concat(analyticsPayloads).join();
>};
>```

## Exempel på implementering {#implementation-examples}

I följande underavsnitt visas hur du implementerar [!DNL Analytics]-loggning på klientsidan för vanliga användningsfall.

### [!UICONTROL Form-Based Experience Composer] aktiviteter {#form-based-composer}

Du kan använda [!DNL Platform Web SDK] för att styra körningen av förslag från [ Adobe Target Form-Based Experience Composer ](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=sv-SE) -aktiviteter.

När du begär förslag för ett specifikt beslutsområde innehåller det returnerade förslaget en lämplig [!DNL Analytics]-token. Det bästa sättet är att kedja kommandot [!DNL Experience Platform Web SDK] `sendEvent` och iterera genom de returnerade förslagen för att köra dem när [!DNL Analytics]-tokenen samlas in samtidigt.

Du kan utlösa ett `sendEvent`-kommando för ett [!UICONTROL Form-Based Experience Composer]-aktivitetsomfång så här:

```javascript
alloy("sendEvent", {
    "decisionScopes": ["a4t-test"],
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function(results) {
  for (var i = 0; i < results.propositions.length; i++) {
    //Execute the propositions and collect the Analytics payload
  }
});
```

Härifrån måste du implementera kod för att kunna köra förslagen och skapa en nyttolast som slutligen skickas till [!DNL Analytics]. Detta är ett exempel på vad `results.propositions` kan innehålla:

```json
[
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "a4t-test",
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
        "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
        "analyticsToken": "434689:0:0|2,434689:0:0|1"
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
    "scope": "a4t-test",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "characteristics": {
        "eventToken": "E0gb6q1+WyFW3FMbbQJmrg==",
        "analyticsToken": "434689:0:0|32767"
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
  },
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "a4t-test",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434688"
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
          "displayToken": "91TS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqgEt==",
          "clickToken": "Tagb6q1+WyFW3FMbbQJrtg==",
          "analyticsDisplayTokens": "434688:0:0|2,434688:0:0|1",
          "analyticsClickTokens": "434688:0:0|32767"
        }
      }
    },
    "items": [
      {
        "id": "1184845",
        "schema": "https://ns.adobe.com/personalization/html-content-item",
        "meta": {
          "geo.state": "bucuresti",
          "activity.id": "434688",
          "experience.id": "0",
          "activity.name": "a4t test form based activity 1",
          "offer.id": "1184845"
        },
        "data": {
          "id": "1184845",
          "format": "text/html",
          "content": "<div> analytics impressions 1</div>"
        }
      },
      {
        "id": "434688",
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

Om du vill extrahera [!DNL Analytics]-token från ett förslag med innehållsobjekt kan du implementera en funktion som liknar följande:

```javascript
function getDisplayAnalyticsPayload(proposition) {
  if (!proposition || !proposition.scopeDetails || !proposition.scopeDetails.characteristics) {
    return;
  }
  var characteristics = proposition.scopeDetails.characteristics;
  if (characteristics.analyticsDisplayToken) {
    return characteristics.analyticsDisplayToken;
  }
  return characteristics.analyticsToken;
}
```

Ett förslag kan ha olika typer av objekt, vilket anges av egenskapen `schema` för det aktuella objektet. Det finns fyra offert-objektscheman som stöds för [!UICONTROL Form-Based Experience Composer]-aktiviteter:

```javascript
var HTML_SCHEMA = "https://ns.adobe.com/personalization/html-content-item";
var MEASUREMENT_SCHEMA = "https://ns.adobe.com/personalization/measurement";
var JSON_SCHEMA = "https://ns.adobe.com/personalization/json-content-item";
var REDIRECT_SCHEMA = "https://ns.adobe.com/personalization/redirect-item";
```

`HTML_SCHEMA` och `JSON_SCHEMA` är de scheman som återspeglar erbjudandets typ, medan `MEASUREMENT_SCHEMA` speglar de mått som ska bifogas till ett DOM-element.

[!DNL Analytics] nyttolaster för klickmått ska samlas in och skickas till [!DNL Analytics] separat från innehållsobjekten, när besökaren faktiskt klickar på det innehåll som visades tidigare.

Följande hjälpfunktion för att hämta A4T-nyttolasterna för klickmått är användbar i det här fallet:

```javascript
function getClickAnalyticsPayload(proposition) {
  if (!proposition || !proposition.scopeDetails || !proposition.scopeDetails.characteristics) {
    return;
  }
  var characteristics = proposition.scopeDetails.characteristics;
  if (characteristics.analyticsClickToken) {
    return characteristics.analyticsClickToken;
  }
  return characteristics.analyticsToken;
}
```

#### Sammanfattning av implementering {#implementation-summary}

Sammanfattningsvis måste följande steg utföras när [!UICONTROL Form-Based Experience Composer]-aktiviteter används med [!DNL Experience Platform Web SDK]:

1. Skicka en händelse som hämtar [!UICONTROL Form-Based Experience Composer] aktivitetserbjudanden;
1. Använda innehållsändringarna på sidan;
1. Skicka meddelandehändelsen `decisioning.propositionDisplay`;
1. Samla in [!DNL Analytics]-visningstoken från SDK-svaret och konstruera en nyttolast för [!DNL Analytics]-träffen;
1. Skicka nyttolasten till [!DNL Analytics] med [API för datainmatning](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md);
1. Om det finns klickvärden i levererade förslag bör klickavlyssnare ställas in så att när en klickning utförs skickas meddelandehändelsen `decisioning.propositionInteract`. Hanteraren `onBeforeEventSend` bör konfigureras så att följande åtgärder inträffar när `decisioning.propositionInteract`-händelser fångas upp:
   1. Samlar in klicktoken [!DNL Analytics] från `xdm._experience.decisioning.propositions`
   1. Skickar klickhändelsen [!DNL Analytics] med den insamlade [!DNL Analytics]-nyttolasten via [API för datainmatning](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md);

```javascript
alloy("sendEvent", {
    "decisionScopes": ["a4t-test"],
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function(results) {
  var analyticsPayload = new Set();
  results.propositions.forEach(function (proposition) {
    proposition.items.forEach(function (item) {
      if (item.schema === HTML_SCHEMA) {
        // 1. Apply offer
        // 2. Collect executed propositions and send the decisioning.propositionDisplay notification event
        // 3. Collect the display Analytics tokens
      }
      if (item.schema === MEASUREMENT_SCHEMA) {
        // Setup click listener, so that when clicked:
        // 1. Collect clicked propositions and send the decisioning.propositionInteract notification event
        // Note: onBeforeEventSend handler should be configured, so that when intercepting decisioning.propositionInteract events:
        //   1. Collect the click Analytics tokens from xdm._experience.decisioning.propositions
        //   2. Send the click Analytics hit with the collected Analytics payload via Data Insertion API
      }
    });
  });
  // Send the page view Analytics hit with the collected display Analytics payload via Data Insertion API
});
```

### [!UICONTROL Visual Experience Composer] (VEC) aktiviteter {#visual-experience-composer-acitivties}

Med [!DNL Platform Web SDK] kan du hantera erbjudanden som har skapats med [Visual Experience Composer (VEC)](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=sv-SE).

>[!NOTE]
>
>Stegen för implementering av det här användningsexemplet liknar mycket stegen för [formulärbaserade Experience Composer-aktiviteter](#form-based-composer). I föregående avsnitt finns mer information.

När automatisk återgivning är aktiverat kan du samla in [!DNL Analytics]-tokens från de förslag som kördes på sidan. Bästa sättet är att kedja kommandot [!DNL Experience Platform Web SDK] `sendEvent` och iterera genom de returnerade förslagen för att filtrera dem som Web SDK har försökt återge.

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
  
    var proposition = propositions[i];
    var renderAttempted = proposition.renderAttempted;

    if (renderAttempted === true) {
      var analyticsPayload = getDisplayAnalyticsPayload(proposition);
      
      if (analyticsPayload !== undefined) {
        analyticsPayloads.add(analyticsPayload);
      }
    }
  }
  var analyticsPayloadsToken = concatenateAnalyticsPayloads(analyticsPayloads);
  // Send the page view Analytics hit with collected Analytics payload via Data Insertion API
});
```

### Använder `onBeforeEventSend` för att hantera sidmått {#using-onbeforeeventsend}

Med hjälp av [!DNL Adobe Target]-aktiviteter kan du ställa in olika mått på sidan, antingen manuellt kopplade till DOM eller automatiskt kopplade till DOM (VEC-skapade aktiviteter). Båda typerna är en fördröjd användarinteraktion på webbsidan.

För att ta hänsyn till detta är det bästa sättet att samla in [!DNL Analytics]-nyttolaster med hjälp av kroken `onBeforeEventSend` [!DNL Adobe Experience Platform Web SDK]. Koppeln `onBeforeEventSend` bör konfigureras med kommandot `configure` och återspeglas i alla händelser som skickas via datastream.

Följande är ett exempel på hur `onBeforeEventSent` kan konfigureras för att utlösa [!DNL Analytics] träffar:

```javascript
alloy("configure", {
  datastreamId: "datastream configuration ID",
  orgId: "adobe ORG ID",
  onBeforeEventSend: function(options) {
    const xdm = options.xdm;
    const eventType = xdm.eventType;
    if (eventType === "decisioning.propositionInteract") {
      const analyticsPayloads = new Set();
      const propositions = xdm._experience.decisioning.propositions;

      for (var i = 0; i < propositions.length; i++) {
        var proposition = propositions[i];
        analyticsPayloads.add(getClickAnalyticsPayload(proposition));
      }
      // Trigger the Analytics hit
    }
  }
});
```

## Nästa steg {#next-steps}

Den här guiden täcker klientloggning för A4T-data i [!DNL Platform Web SDK]. Mer information om hur du hanterar A4T-data på Edge Network finns i handboken om [serverloggning](/help/dev/implement/a4t/server-side-a4t.md).