---
keywords: serverstate, targetGlobalSettings, targetingGlobalSettings, globalSettings, globalSettings, global settings, at.js, funktioner, clientCode, clientCode, serverDomain, serverdomain, cookieDomain, serverstate5, serverstate6, serverstate7, serverstate8, serverstate9, targetGlobalSettings0, targetGlobalSettings1, targetGlobalSettings2, targetGlobalSettings3, targetGlobalSettings4, targetGlobalSettings5, cookiedomain, crossDomain, crossdomain, timeout, globalMboxAutoCreate, visitorApiTimeout, defaultContentHiddenStyle, defaultContentVisibleStyle, bodyHiddenStyle, bodyHidingEnabled, imsOrgId, secureOnly, overrideMbox EdgeServer, overrideMboxEdgeServerTimeout, cookiedomain5, cookiedomain6, cookiedomain7, cookiedomain8, cookiedomain9, crossDomain0, crossDomain1, crossDomain2, crossDomain3, crossDomain4, crossDomain5, optoutEnabled, optout, opt out, selectorsPollingTimeout, dataProviders, Hybrid Personalization, deviceIdLifetime
description: Använd funktionen [!UICONTROL targetGlobalSettings()] för JavaScript-biblioteket  [!DNL Adobe Target]  at.js om du vill åsidosätta inställningarna i stället för att använda API:erna för  [!DNL Target] UI eller REST.
title: Hur använder jag funktionen [!UICONTROL targetGlobalSettings()]?
feature: at.js
exl-id: f6218313-6a70-448e-8555-b7b039e64b2c
source-git-commit: 12cf430b65695d38d1651f2a97df418d82d231f3
workflow-type: tm+mt
source-wordcount: '2565'
ht-degree: 0%

---

# [!UICONTROL targetGlobalSettings()]

Du kan åsidosätta inställningarna i at.js-biblioteket med `[!UICONTROL targetGlobalSettings()]` i stället för att konfigurera dem i [!DNL Target]-gränssnittet eller med REST API:er.

## Inställningar

Du kan åsidosätta följande inställningar:

### aepSandboxId

* **Typ**: Sträng
* **Standardvärde**: null
* **Beskrivning**: Valfri parameter som används för att skicka [!DNL Adobe Experience Platform] sandbox-ID för att dela [!DNL Adobe Experience Platform] mål som skapats i icke-standardsandlådan med [!DNL Target]. Om `aepSandboxId` inte är null måste `aepSandboxName` också anges.

### aepSandboxName

* **Typ**: Sträng
* **Standardvärde**: null
* **Beskrivning**: Valfri parameter som används för att skicka [!DNL Adobe Experience Platform] sandlådenamn för att dela [!DNL Adobe Experience Platform] mål som skapats i icke-standardsandlådan med [!DNL Target]. Om `aepSandboxName` inte är null måste `aepSandboxId` också anges.

### artifactLocation

* **Typ**: Sträng
* **Standardvärde**: Inget
* **Beskrivning**: En fullständigt kvalificerad URL till [avståndsregelartefakten på enheten](../../../server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)

### bodyHiddenStyle

* **Typ**: Sträng
* **Standardvärde**: body { opacity: 0}
* **Beskrivning**: Används bara när `globalMboxAutocreate === true` för att minimera risken för flimmer.

  Mer information finns i [How at.js Managages Flicker](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md).

### bodyHidingEnabled

* **Typ**: Boolean
* **Standardvärde**: true
* **Beskrivning**: Används för att styra flimmer när `target-global-mbox` används för att leverera erbjudanden som skapats i Visual Experience Composer, även kallat visuella erbjudanden.

### clientCode

* **Typ**: Sträng
* **Standardvärde**: Värdet har angetts via användargränssnittet.
* **Beskrivning**: Representerar klientkoden.

### cookieDomain

* **Typ**: Sträng
* **Standardvärde**: Om det är möjligt anges värdet till toppnivådomänen.
* **Beskrivning**: Representerar domänen som används när cookies sparas.

### crossDomain

* **Typ**: Sträng
* **Standardvärde**: Värdet har angetts via användargränssnittet.
* **Beskrivning**: Anger om spårning mellan domäner är aktiverat eller inte. Vilka värden som tillåts beror på din at.js-version. För at.js v1.*x*, ange om korsdomänfunktionerna är `disabled` (webbläsare anger cookies i din domän (endast cookies från första part)), `x only` (webbläsare anger cookies endast i domänen för [!DNL Target]) eller både och, genom att välja `enabled` (webbläsare anger cookies från både första och tredje part). För at.js v2.10 och senare anger du om korsdomänfunktionerna är `enabled` (webbläsare anger både cookies från första och tredje part) eller `disabled` (webbläsare anger inte cookies från tredje part).

### cspScriptNonce

* **Typ**: Se [Säkerhetspolicy för innehåll](#content-security-policy) nedan.
* **Standardvärde**: Se [Säkerhetspolicy för innehåll](#content-security-policy) nedan.
* **Beskrivning**: Se [Säkerhetspolicy för innehåll](#content-security-policy) nedan.

### cspStyleNonce

* **Typ**: Se [Säkerhetspolicy för innehåll](#content-security-policy) nedan.
* **Standardvärde**: Se [Säkerhetspolicy för innehåll](#content-security-policy) nedan.
* **Beskrivning**: Se [Säkerhetspolicy för innehåll](#content-security-policy) nedan.

### dataProviders

* **Typ**: Se [Dataproviders](#data-providers) nedan.
* **Standardvärde**: Se [Dataproviders](#data-providers) nedan.
* **Beskrivning**: Se [Dataproviders](#data-providers) nedan.

### decisioningMethod

* **Typ**: Sträng
* **Standardvärde**: på serversidan
* **Andra värden**: på enheten, hybrid
* **Beskrivning**: Se Beslutsmetoder nedan.

  **Beslutsmetoder**

  Med enhetsspecifik beslutsfattande introducerar [!DNL Target] en ny inställning som kallas beslutsmetod som bestämmer hur at.js levererar dina upplevelser. `decisioningMethod` har tre värden: endast på serversidan, endast på enheten och hybridversionen. När `decisioningMethod` anges i `targetGlobalSettings()` fungerar det som standardbeslutsmetod för alla [!DNL Target] -beslut.

  **Endast på serversidan**:

  Endast på serversidan är den standardmetod för beslut som anges i rutan när at.js 2.5+ implementeras och distribueras på dina webbegenskaper.

  Om endast serversidan används som standardkonfiguration innebär det att alla beslut fattas i gränsnätverket [!DNL Target], vilket innebär ett blockerande serveranrop. Den här metoden kan innebära inkrementell fördröjning, men den ger också avsevärda fördelar, som att du kan använda [!DNL Target]s maskininlärningsfunktioner som omfattar [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html)-, [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)- (AP) och [Automatiskt mål](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) -aktiviteter.

  Dessutom kan förbättringar av dina personaliserade upplevelser genom att använda användarprofilen för [!DNL Target], som är beständig i olika sessioner och kanaler, ge ett kraftfullt resultat för ditt företag.

  Slutligen kan du bara använda Adobe Experience Cloud på serversidan och finjustera målgrupper som kan riktas mot Audience Manager och Adobe Analytics.

  **Endast på enheten**:

  Endast på enheten är den beslutsmetod som måste anges i at.js 2.5+ när enhetsbeslut endast ska användas på alla webbsidor.

  Enhetsbeslut kan leverera upplevelser och personaliseringsaktiviteter blixtsnabbt eftersom besluten fattas utifrån en cache-lagrad regelartefakt som innehåller alla aktiviteter som är kvalificerade för enhetsbeslut.

  Mer information om vilka aktiviteter som är kvalificerade för enhetsbeslut finns i avsnittet med funktioner som stöds.

  Den här beslutsmetoden bör bara användas om prestanda är mycket kritiskt för alla sidor som kräver beslut från [!DNL Target]. Tänk dessutom på att när du väljer den här beslutsmetoden kommer [!DNL Target]-aktiviteter som inte är kvalificerade för enhetsbeslut inte att levereras eller köras. At.js-biblioteket 2.5+ är konfigurerat att endast söka efter den cachelagrade regelartefakten för att fatta beslut.

  **Hybrid**:

  Hybrid är den beslutsmetod som måste anges i at.js 2.5+ när både enhetsbeslut och aktiviteter som kräver ett nätverksanrop till Edge-nätverket [!DNL Adobe Target] måste köras.

  När du hanterar både beslutsaktiviteter på enheter och aktiviteter på serversidan kan det vara lite komplicerat och omständligt att fundera på hur du ska distribuera och distribuera [!DNL Target] på sidorna. Med hybridmetoden som beslutsmetod vet [!DNL Target] när det måste göra ett serveranrop till Edge-nätverket [!DNL Adobe Target] för aktiviteter som kräver körning på serversidan och även när endast enhetsbeslut ska verkställas.

  JSON-regelartefakten innehåller metadata som informerar at.js om en mbox har en aktivitet på serversidan som körs eller en beslutsaktivitet på enheten. Med den här beslutsmetoden kan du säkerställa att aktiviteter som du avser att leverera snabbt genomförs via enhetsspecifika beslut och för aktiviteter som kräver mer kraftfull ML-driven personalisering utförs dessa aktiviteter via Edge-nätverket [!DNL Adobe Target].

### defaultContentHiddenStyle

* **Typ**: Sträng
* **Standardvärde**: synlighet: dold
* **Beskrivning**: Används endast för kapslingsrutor som använder DIV med klassnamnet mboxDefault och som körs via `mboxCreate()`, `mboxUpdate()` eller `mboxDefine()` för att dölja standardinnehåll.

### defaultContentVisibleStyle

* **Typ**: Sträng
* **Standardvärde**: synlighet: synlig
* **Beskrivning**: Används endast för kapslingsrutor som använder DIV med klassnamnet &quot;mboxDefault&quot; och som körs via `mboxCreate()`, `mboxUpdate()` eller `mboxDefine()` för att visa det tillämpade erbjudandet om något eller standardinnehåll.

### deviceIdLifetime

* **Typ**: Number
* **Standardvärde**: 63244800000 ms = 2 år
* **Beskrivning**: `deviceId` sparas i cookies.

>[!NOTE]
>
>Inställningen deviceIdLifetime kan åsidosättas i version 2.3.1 eller senare av at.js.

### aktiverad

* **Typ**: Boolean
* **Standardvärde**: true
* **Beskrivning**: När det här alternativet är aktiverat utförs en [!DNL Target]-begäran om hämtning av upplevelser och DOM-manipulering för att återge upplevelserna automatiskt. Dessutom kan [!DNL Target] anrop utföras manuellt via `getOffer(s)` / `applyOffer(s)`.

  När det är inaktiverat körs inte [!DNL Target]-begäranden automatiskt eller manuellt.

### globalMboxAutoCreate

* **Typ**: Number
* **Standardvärde**: Värdet har angetts via användargränssnittet.
* **Beskrivning**: Anger om den globala mbox-begäran ska utlösas eller inte.

### imsOrgId

* **Typ**: Sträng
* **Standardvärde**: true
* **Beskrivning**: Representerar IMS-ORG-ID.

### optinEnabled

* **Typ**: Boolean
* **Standardvärde**: false
* **Beskrivning**: [!DNL Target] tillhandahåller stöd för tillvalsfunktioner via Adobe Experience Platform som kan hjälpa dig att stödja din strategi för samtyckeshantering. Med avanmälningsfunktionen kan kunderna styra hur och när taggen [!DNL Target] aktiveras. Det finns också ett alternativ via Adobe Experience Platform för att förgodkänna taggen [!DNL Target]. Om du vill aktivera möjligheten att använda Opt-In i biblioteket [!DNL Target] at.js lägger du till inställningen `optinEnabled=true`. I Adobe Experience Platform måste du välja &quot;enable&quot; i listrutan GDPR Opt-In i installationsvyn för tillägget. Mer information finns i [Adobe Experience Platform-dokumentationen](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md). Mer information om den här inställningen i förhållande till sekretess- och dataskyddsbestämmelser, inklusive EU:s allmänna dataskyddsförordning (GDPR) och Kaliforniens konsumentsekretesslag (CCPA), finns i [Sekretess- och dataskyddsbestämmelser](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md).

### optoutEnabled

* **Typ**: Boolean
* **Standardvärde**: false
* **Beskrivning**: Anger om [!DNL Target] ska anropa API:t `isOptedOut()` för besökare. Detta är en del av aktiveringen av Device Graph.

### overrideMboxEdgeServer

* **Typ**: Boolean
* **Standardvärde**: true (true från och med at.js version 1.6.2)
* **Beskrivning**: Anger om vi ska använda `<clientCode>.tt.omtrdc.net`-domän eller `mboxedge<clusterNumber>.tt.omtrdc.net`-domän.

  Om det här värdet är true sparas `mboxedge<clusterNumber>.tt.omtrdc.net`-domänen i en cookie. För närvarande fungerar inte med [CNAME](/help/dev/before-implement/implement-cname-support-in-target.md) när du använder at.js-versioner före at.js 1.8.2 och at.js 2.3.1. Om det här är ett problem för dig bör du överväga att [uppdatera at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md) till en nyare version som stöds.

### overrideMboxEdgeServerTimeout

* **Typ**: Number
* **Standardvärde**: 1860000 => 31 minuter
* **Beskrivning**: Anger den cookie-livstid som innehåller värdet `mboxedge<clusterNumber>.tt.omtrdc.net`.

### pageLoadEnabled

* **Typ**: Boolean
* **Standardvärde**: true
* **Beskrivning**: När det här alternativet är aktiverat hämtar du automatiskt upplevelser som måste returneras vid sidinläsning.

### pollingInterval

* **Typ**: Number
* **Standardvärde**: 300000 (fem minuter i millisekunder)
* **Beskrivning**: Intervall som at.js hämtar en ny version av en enhetsspecifik beslutsartefakt och uppdaterar cachen. 300000 är det minsta tillåtna värdet för `pollingInterval`.

### secureOnly

* **Typ**: Boolean
* **Standardvärde**: false
* **Beskrivning**: Anger om at.js endast ska använda HTTPS eller om det ska vara tillåtet att växla mellan HTTP och HTTPS baserat på sidprotokollet. När värdet är true anger secureOnly även attributen Secure och SameSite till mbox-cookien.

### selectorsPollingTimeout

* **Typ**: Number
* **Standardvärde**: 5 000 ms = 5 s
* **Beskrivning**: I at.js 0.9.6 introducerade [!DNL Target] den här nya inställningen som kan åsidosättas via `targetGlobalSettings`.

  Inställningen `selectorsPollingTimeout` representerar hur lång tid klienten kan vänta på att alla element som identifieras av väljarna ska visas på sidan.

  Aktiviteter som skapas via Visual Experience Composer (VEC) har erbjudanden som innehåller väljare.

### serverDomain

* **Typ**: Sträng
* **Standardvärde**: Värdet har angetts via användargränssnittet.
* **Beskrivning**: Representerar edge-servern [!DNL Target].

### serverState

* **Typ**: Se [Hybrid-anpassning](#hybrid-personalization) nedan.
* **Standardvärde**: Se [Hybridanpassning](#hybrid-personalization) nedan.
* **Beskrivning**: Se [Hybridanpassning](#hybrid-personalization) nedan.

### telemetryEnabled

* **Typ**: Boolean
* **Standardvärde**: true
* **Beskrivning**: När det här alternativet är aktiverat samlar Adobe in SDK-funktionens användnings- och prestandatelemetridata. Personuppgifter samlas inte in.

### timeout

* **Typ**: Number
* **Standardvärde**: Värdet har angetts via användargränssnittet.
* **Beskrivning**: Representerar tidsgränsen för kantbegäran [!DNL Target].

### viewsEnabled {#viewsenabled}

* **Typ**: Boolean
* **Standardvärde**: true
* **Beskrivning**: När den är aktiverad hämtas vyer automatiskt när sidan läses in. När `triggerView` anropas visas tillämpliga vyer i webbläsaren. Om det här alternativet är inaktiverat hämtas inte vyer vid sidinläsning och `triggerView` gör ingenting. Vyer stöds i at.js 2.Endast *x*.

### visitorApiTimeout

* **Typ**: Number
* **Standardvärde**: 2 000 ms = 2 s
* **Beskrivning**: Representerar timeout för Visitor API-begäran.

## Användning

Den här funktionen kan definieras innan at.js har lästs in eller i **Administration** > **Implementering** > **Redigera at.js-inställningar** > **Kodinställningar** > **Bibliotekshuvud** .

I fältet Bibliotekshuvud kan du ange kostnadsfria JavaScript. Anpassningskoden ska se ut ungefär som i följande exempel:

```javascript {line-numbers="true"}
window.targetGlobalSettings = {
   timeout: 200, // using custom timeout
   visitorApiTimeout: 500, // using custom API timeout
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com
};
```

## Dataleverantörer {#data-providers}

Med den här inställningen kan kunder samla in data från tredjepartsleverantörer av data, som Demandbase, BlueKai och anpassade tjänster, och skicka data till [!DNL Target] som mbox-parametrar i den globala mbox-begäran. Det stöder insamling av data från flera leverantörer via asynkrona och synkroniserade begäranden. Med den här metoden blir det enkelt att hantera flimmer av standardsidinnehåll, samtidigt som oberoende tidsgränser för varje leverantör tas med för att begränsa effekten på sidans prestanda

>[!NOTE]
>
>Data Providers kräver at.js 1.3 eller senare.

Följande videofilmer innehåller mer information:

| Video | Beskrivning |
|--- |--- |
| [Använda Data Providers i Adobe Target](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html) | Data Providers är en funktion som gör att du enkelt kan skicka data från tredje part till Target. En tredje part kan vara en vädertjänst, en datahanteringsplattform eller till och med en egen webbtjänst. Sedan kan ni använda dessa data för att skapa målgrupper, målinnehåll och berika besökarprofilen. |
| [Implementera Data Providers i Adobe Target](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html) | Implementeringsinformation och exempel på hur du använder funktionen dataProviders för Adobe [!DNL Target] för att hämta data från tredjepartsdataleverantörer och skicka den i [!DNL Target]-begäran. |

Inställningen `window.targetGlobalSettings.dataProviders` är en matris med dataleverantörer.

Varje dataleverantör har följande struktur:

| Nyckel | Typ | Beskrivning |
|--- |--- |--- |
| name | Sträng | Leverantörens namn. |
| version | Sträng | Providerversion. Den här nyckeln kommer att användas för leverantörens utveckling. |
| timeout | Nummer | Representerar providerns timeout om det här är en nätverksbegäran.  Den här nyckeln är valfri. |
| provider | Funktion | Funktionen som innehåller logiken för hämtning av providerdata.<p>Funktionen har en enda obligatorisk parameter: `callback`. Callback-parametern är en funktion som bara ska anropas när data har hämtats eller när ett fel uppstår.<p>Återanropet förväntar sig två parametrar:<ul><li>fel: Anger om ett fel uppstod. Om allt är OK ska den här parametern anges till null.</li><li>parametrar: Ett JSON-objekt som representerar de parametrar som ska skickas i en [!DNL Target]-begäran.</li></ul> |

I följande exempel visas var dataleverantören använder synkroniseringskörning:

```javascript {line-numbers="true"}
var syncDataProvider = {
  name: "simpleDataProvider",
  version: "1.0.0",
  provider: function(callback) {
    callback(null, {t1: 1});
  }
};

window.targetGlobalSettings = {
  dataProviders: [
    syncDataProvider
  ]
};
```

Efter att at.js bearbetat `window.targetGlobalSettings.dataProviders` kommer [!DNL Target]-begäran att innehålla en ny parameter: `t1=1`.

Följande är ett exempel om parametrarna som du vill lägga till i [!DNL Target]-begäran hämtas från en tredjepartstjänst, till exempel Bluekai, Demandbase o.s.v.:

```javascript {line-numbers="true"}
var blueKaiDataProvider = {
   name: "blueKai",
   version: "1.0.0",
   provider: function(callback) {
      // simulating network request
     setTimeout(function() {
       callback(null, {t1: 1, t2: 2, t3: 3});
     }, 1000);
   }
}

window.targetGlobalSettings = {
   dataProviders: [
      blueKaiDataProvider
   ]
};
```

Efter att at.js bearbetat `window.targetGlobalSettings.dataProviders` kommer [!DNL Target]-begäran att innehålla ytterligare parametrar: `t1=1`, `t2=2` och `t3=3`.

I följande exempel används dataleverantörer för att samla in väder-API-data och skicka dem som parametrar i en [!DNL Target]-begäran. [!DNL Target]-begäran kommer att ha ytterligare parametrar, till exempel `country` och `weatherCondition`.

```javascript {line-numbers="true"}
var weatherProvider = {
      name: "weather-api",
      version: "1.0.0",
      timeout: 2000,
      provider: function(callback) {
        var API_KEY = "caa84fc6f5dc77b6372d2570458b8699";
        var lat = 44.426767399999996;
        var lon = 26.1025384;
        var url = "//api.openweathermap.org/data/2.5/weather?";
        var data = {
          lat: lat,
          lon: lon,
          appId: API_KEY
        }

        $.ajax({
          type: "GET",
                url: url,
          dataType: "json",
          data: data,
          success: function(data) {
            console.log("Weather data", data);
            callback(null, {
              country: data.sys.country,
              weatherCondition: data.weather[0].main
            });
          },
          error: function(err) {
            console.log("Error", err);
            callback(err);
          }
        });
      }
    };

    window.targetGlobalSettings = {
      dataProviders: [weatherProvider]
    };
```

Tänk på följande när du arbetar med inställningen `dataProviders`:

* Om dataleverantörerna som läggs till i `window.targetGlobalSettings.dataProviders` är asynkrona kommer de att köras parallellt. Visitor-API-begäran kommer att köras parallellt med funktioner som lagts till i `window.targetGlobalSettings.dataProviders` för att ge en minimal väntetid.
* at.js försöker inte cachelagra data. Om dataleverantören bara hämtar data en gång, bör dataleverantören se till att data cachelagras och, när providerfunktionen anropas, hantera cachedata för det andra anropet.

## Skyddsprincip för innehåll

at.js 2.3.0+ stöder inställning av Content Security Policy-noces för SCRIPT- och STYLE-taggar som läggs till på sidan DOM när levererade [!DNL Target]-erbjudanden tillämpas.

Strängarna SCRIPT och STYLE ska anges i `targetGlobalSettings.cspScriptNonce` och `targetGlobalSettings.cspStyleNonce`, före inläsningen av at.js 2.3.0+. Se ett exempel nedan:

```javascript {line-numbers="true"}
...
<head>
 <script nonce="<script_nonce_value>">
window.targetGlobalSettings = {
  cspScriptNonce: "<csp_script_nonce_value>",
  cspStyleNonce: "<csp_style_nonce_value>"
};
 </script>
 <script nonce="<script_nonce_value>" src="at.js"></script>
...
</head>
...
```

När inställningarna för `cspScriptNonce` och `cspStyleNonce` har angetts anger at.js 2.3.0+ dessa som nonce-attribut för alla SCRIPT- och STYLE-taggar som läggs till i DOM när [!DNL Target]-erbjudanden tillämpas.

## Hybrid-personalisering

`serverState` är en inställning som är tillgänglig i at.js v2.2+ som kan användas för att optimera sidprestanda när en hybridintegrering av [!DNL Target] implementeras. Hybrid-integrering innebär att du använder både at.js v2.2+ på klientsidan och Delivery API eller en [!DNL Target] SDK på serversidan för att leverera upplevelser. `serverState` ger at.js v2.2+ möjlighet att tillämpa upplevelser direkt från innehåll som hämtas på serversidan och returneras till klienten som en del av sidan som skickas.

### Krav

Du måste ha en hybridintegrering av [!DNL Target].

* **Server-side**: Du måste använda [leverans-API](/help/dev/implement/delivery-api/overview.md) eller [mål-SDK](/help/dev/implement/server-side/sdk-guides/getting-started/getting-started.md).
* **Klientsidan**: Du måste använda [ at.js version 2.2 eller senare](/help/dev/implement/client-side/atjs/target-atjs-versions.md).

### Kodexempel

Mer information om hur detta fungerar finns i kodexemplen nedan som du skulle ha på servern. Koden förutsätter att du använder [Target Node.js SDK](https://github.com/adobe/target-nodejs-sdk).

```javascript {line-numbers="true"}
// First, we fetch the offers via Target Node.js SDK API, as usual
const targetResponse = await targetClient.getOffers(options);
// A successfull response will contain Target Delivery API request and response objects, which we need to set as serverState
const serverState = {
  request: targetResponse.request,
  response: targetResponse.response
};
// Finally, we should set window.targetGlobalSettings.serverState in the returned page, by replacing it in a page template, for example
const PAGE_TEMPLATE = `
<!doctype html>
<html>
<head>
  ...
  <script>
    window.targetGlobalSettings = {
      overrideMboxEdgeServer: true,
      serverState: ${JSON.stringify(serverState, null, " ")}
    };
  </script>
  <script src="at.js"></script>
</head>
...
</html>
`;
// Return PAGE_TEMPLATE to the client ...
```

Ett exempel på `serverState`-objekt-JSON för vyförhämtning ser ut så här:

```javascript {line-numbers="true"}
{
 "request": {
  "requestId": "076ace1cd3624048bae1ced1f9e0c536",
  "id": {
   "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "context": {
   "channel": "web",
   "timeOffsetInMinutes": 0
  },
  "experienceCloud": {
   "analytics": {
    "logging": "server_side",
    "supplementalDataId": "7D3AA246CC99FD7F-1B3DD2E75595498E"
   }
  },
  "prefetch": {
   "views": [
    {
     "address": {
      "url": "my.testsite.com/"
     }
    }
   ]
  }
 },
 "response": {
  "status": 200,
  "requestId": "076ace1cd3624048bae1ced1f9e0c536",
  "id": {
   "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "client": "testclient",
  "edgeHost": "mboxedge21.tt.omtrdc.net",
  "prefetch": {
   "views": [
    {
     "name": "home",
     "key": "home",
     "options": [
      {
       "type": "actions",
       "content": [
        {
         "type": "setHtml",
         "selector": "#app > DIV.app-container:eq(0) > DIV.page-container:eq(0) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(1) > DIV.heading:eq(0) > H1.title:eq(0)",
         "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(2) > DIV:nth-of-type(1) > H1:nth-of-type(1)",
         "content": "<span style=\"color:#FF0000;\">Latest</span> Products for 2020"
        }
       ],
       "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
       "responseTokens": {
        "profile.memberlevel": "0",
        "geo.city": "dublin",
        "activity.id": "302740",
        "experience.name": "Experience B",
        "geo.country": "ireland"
       }
      }
     ],
     "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
    }
   ]
  }
 }
}
```

När sidan har lästs in i webbläsaren tillämpar at.js alla [!DNL Target] erbjudanden från `serverState` omedelbart, utan att utlösa några nätverksanrop mot [!DNL Target]-kanten. Dessutom kan at.js bara dölja DOM-element för vilka [!DNL Target] erbjudanden är tillgängliga på den hämtade serversidan, vilket positivt påverkar sidinläsningsprestanda och slutanvändarupplevelsen.

### Viktiga anteckningar

Tänk på följande när du använder `serverState`:

* För närvarande stöder at.js v2.2 endast leverans av upplevelser via serverState för:

   * VEC-skapade aktiviteter som körs vid sidinläsning.
   * Förhämtade vyer.

     Om [!DNL Target]-vyer och `triggerView()` används i API:t at.js, cachelagras innehållet för alla vyer som är förhämtade på servern och dessa används så snart varje vy aktiveras via `triggerView()`, återigen utan att några ytterligare innehållshämtande anrop till [!DNL Target] aktiveras. 

   * **Obs!**: För närvarande stöds inte lådor som har hämtats på serversidan i `serverState`.

* När du tillämpar `serverState` erbjudanden tar at.js hänsyn till inställningarna för `pageLoadEnabled` och `viewsEnabled`, t.ex. kommer sidinläsning inte att användas om inställningen för `pageLoadEnabled` är false.

  Aktivera de här inställningarna genom att aktivera växlingen i **Administration > Implementering > Redigera > Sidinläsning aktiverad**.

  ![Inställningar för sidinläsning är aktiverade](../../assets/page-load-enabled-setting.png)

* Om du använder `serverState` och `<script>` -taggar i det returnerade innehållet kontrollerar du att ditt HTML-innehåll använder `<\/script>` i stället för `</script>`. Om du använder `</script>` tolkar webbläsaren `</script>` som slutet på ett textbundet SCRIPT och kan komma att bryta HTML-sidan.

### Ytterligare resurser

Mer information om hur `serverState` fungerar finns i följande resurser:

* [Exempelkod](https://github.com/Adobe-Marketing-Cloud/target-node-client-samples/tree/master/advanced-atjs-integration-serverstate).
* [Exempelappen för programmet för en sida (SPA) med `serverState`](https://github.com/Adobe-Marketing-Cloud/target-node-client-samples/tree/master/react-shopping-cart-demo).
