---
keywords: at.js-versioner, at.js-versioner, single page app, spa, cross domain, cross domain
description: Lär dig hur du uppgraderar från  [!DNL Adobe Target] at.js 1.x till at.js 2.x. Granska systemflödesdiagram, läs om nya och inaktuella funktioner och mycket mer.
title: Hur uppgraderar jag från at.js version 1.x till version 2.x?
feature: at.js
exl-id: fbfa5743-0fa5-44c6-89b3-fdee9b50e126
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '2939'
ht-degree: 0%

---

# Uppgraderar från at.js 1.*x* till at.js 2.*x*

Den senaste versionen av at.js i [!DNL Adobe Target] innehåller många funktioner som gör det möjligt för ditt företag att utföra personalisering på nästa generations klienttekniker. Den nya versionen fokuserar på att uppgradera at.js för att få harmonisk interaktion med applikationer för en sida (SPA).

Här är några fördelar med att använda at.js 2.*x* som inte är tillgängliga i tidigare versioner:

* Möjligheten att cachelagra alla erbjudanden vid sidinläsning för att minska antalet serveranrop till ett enda serveranrop.
* Förbättra slutanvändarnas upplevelser enormt på er webbplats, eftersom erbjudandena visas direkt via cachen utan den fördröjning som traditionella serversamtal ger.
* Enkel kodrad och engångsinstallation av utvecklare så att era marknadsförare kan skapa och köra [!UICONTROL A/B Test]- och [!UICONTROL Experience Targeting]-aktiviteter via VEC på era SPA.

## at.js 2.*x* systemdiagram

Följande diagram hjälper dig att förstå arbetsflödet i at.js 2.*x* med vyer och hur detta förbättrar SPA. För att få en bättre introduktion till de koncept som används i at.js 2.*x*, se [Implementering av Single Page-program](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

(Klicka på bilden för att expandera till full bredd.)

![Målflöde med at.js 2.x](/help/dev/implement/client-side/assets/system-diagram-atjs-20.png "Målflöde med at.js 2.x"){zoomable="yes"}

| Utlysning | Information |
| --- | --- |
| 1 | Samtalet returnerar [!UICONTROL Experience Cloud ID] om användaren är autentiserad. Ett annat anrop synkroniserar kund-ID:t. |
| 2 | At.js-biblioteket läses in synkront och döljer dokumentets brödtext.<P>at.js kan också läsas in asynkront med ett alternativ som gör att fragment för att dölja kan implementeras på sidan. |
| 3 | En sidinläsningsbegäran görs med alla konfigurerade parametrar (MCID, SDID och kund-ID). |
| 4 | Profilskript körs och matas sedan in i profilarkivet. Store begär kvalificerade målgrupper från målgruppsbiblioteket (till exempel målgrupper som delas från [!DNL Adobe Analytics], [!DNL Audience Manager] osv.).<P>Kundattribut skickas till profilarkivet i en gruppbearbetning. |
| 5 | Baserat på parametrar för URL-begäran och profildata avgör [!DNL Target] vilka aktiviteter och upplevelser som ska returneras till besökaren för den aktuella sidan och framtida vyer. |
| 6 | Målinriktat innehåll skickas tillbaka till sidan, eventuellt med profilvärden för ytterligare personalisering.<P>Målinriktat innehåll på den aktuella sidan visas så snabbt som möjligt utan att du behöver flimra standardinnehållet.<P>Målanpassat innehåll för vyer som visas som ett resultat av användaråtgärder i en SPA som cachelagras i webbläsaren så att det kan tillämpas direkt utan ett extra serveranrop när vyerna aktiveras via `triggerView()`. |
| 7 | [!UICONTROL Analytics] data skickas till datainsamlingsservrar. |
| 8 | Måldata matchas mot [!UICONTROL Analytics] data via SDID och bearbetas till rapportlagringen i [!UICONTROL Analytics].<P>[!UICONTROL Analytics] data kan sedan visas i både [!UICONTROL Analytics]- och [!DNL Target]-rapporter via [!UICONTROL Analytics for Target] (A4T). |

Nu hämtas vyer och åtgärder från cachen och visas för användaren utan ett serveranrop, oavsett var `triggerView()` implementeras på SPA. `triggerView()` skickar också en aviseringsbegäran till [!DNL Target]-serverdelen för att öka antalet och registrera antalet visningar.

(Klicka på bilden för att expandera till full bredd.)

![Målflöde vid .js 2.*x* triggerView](/help/dev/implement/client-side/assets/atjs-20-triggerview.png "Målflöde vid.js 2.*x* triggerView"){zoomable="yes"}

| Utlysning | Information |
| --- | --- |
| 1 | `triggerView()` anropas i SPA för att återge vyn och tillämpa åtgärder för att ändra visuella element. |
| 2 | Målinnehåll för vyn läses från cachen. |
| 3 | Målinriktat innehåll visas så snabbt som möjligt utan att man behöver flimra standardinnehållet. |
| 4 | Meddelandebegäran skickas till [!DNL Target]-profilarkivet för att räkna besökaren i aktiviteten och öka måtten. |
| 5 | [!UICONTROL Analytics] data har skickats till datainsamlingsservrar. |
| 6 | [!DNL Target]-data matchas mot [!UICONTROL Analytics]-data via SDID och bearbetas till rapportlagringen [!UICONTROL Analytics]. [!UICONTROL Analytics] data kan sedan visas i både [!UICONTROL Analytics] och [!DNL Target] via A4T-rapporter. |

## Driftsätt på js 2.*x*

Driftsätt på js 2.*x* via taggar i tillägget [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md).

>[!NOTE]
>
>Att distribuera at.js med taggar i [!DNL Adobe Experience Platform] är den bästa metoden.
>
>eller
>
>Ladda ned på.js 2 manuellt.*x* med [!DNL Target]-gränssnittet och distribuera det med den [metod du väljer](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md).

## Borttagna at.js-funktioner

Det finns flera funktioner som har tagits bort i at.js 2.*x*.

>[!WARNING]
>
>Om dessa inaktuella funktioner fortfarande används på din webbplats i .js 2.*x* är distribuerad, du kommer att se konsolvarningar. Det rekommenderade tillvägagångssättet vid uppgradering är att testa distributionen av at.js 2.*x* i en staging-miljö och se till att du går igenom alla varningar som har loggats i konsolen och översätter de borttagna funktionerna till nya funktioner som introducerats i at.js 2.*x*.

Du hittar de borttagna funktionerna och deras motsvarighet nedan. En fullständig lista över funktioner finns i [at.js-funktioner](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md).

>[!NOTE]
>
>at.js 2.*x* döljer inte längre automatiskt `mboxDefault` markerade element. Kunderna måste därför anpassa sig till fördöljningslogiken manuellt på webbplatsen eller via en tagghanterare.

### mboxCreate(mbox,params)

**Beskrivning**:

Kör en begäran och tillämpar erbjudandet på närmaste DIV med klassnamnet `mboxDefault`.

**Exempel**:

```html {line-numbers="true"}
<div class="mboxDefault">
  default content to replace by offer
</div>
<script>
  mboxCreate('mboxName','param1=value1','param2=value2');
</script>
```

**at.js 2.*x* motsvarande**

Ett alternativ till `mboxCreate(mbox, params)` är `getOffer()` och `applyOffer()`.

**Exempel**:

```html {line-numbers="true"}
<div class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  var el = document.currentScript.previousElementSibling;
  adobe.target.getOffer({
    mbox: "mboxName",
    params: {
      param1: "value1",
      param2: "value2"
    },
    success: function(offer) {
      adobe.target.applyOffer({
        mbox: "mboxName",
        selector: el,
        offer: offer
      });
    },
    error: function(error) {
      console.error(error);
      el.style.visibility = "visible";
    }
  });
</script> 
```

### mboxDefine() och mboxUpdate()

**Beskrivning**:

Skapar en intern mappning mellan ett element och ett mbox-namn, men kör inte begäran. Används tillsammans med `mboxUpdate()`, som kör begäran och tillämpar erbjudandet på elementet som identifieras av nodeId i `mboxDefine()`. Kan också användas för att uppdatera en mbox som initierats av `mboxCreate`.

**Exempel**:

```html {line-numbers="true"}
<div id="someId" class="mboxDefault"></div>
<script>
 mboxDefine('someId','mboxName','param1=value1','param2=value2');
 mboxUpdate('mboxName','param3=value3','param4=value4');
</script>
```

**at.js 2.*x* motsvarande**:

Ett alternativ till `mboxDefine()` och `mboxUpdate` är `getOffer()` och `applyOffer()`, med väljaralternativet i `applyOffer()`. På så sätt kan du mappa erbjudandet till ett element med hjälp av en CSS-väljare, inte bara en med ett ID.

**Exempel**:

```html {line-numbers="true"}
<div id="someId" class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  adobe.target.getOffer({
    mbox: "mboxName",
    params: {
      param1: "value1",
      param2: "value2",
      param3: "value3",
      param4: "value4" 
    },
    success: function(offer) {
      adobe.target.applyOffer({
        mbox: "mboxName",
        selector: "#someId",
        offer: offer
      });
    },
    error: function(error) {
      console.error(error);
      var el = document.getElementById("someId");
      el.style.visibility = "visible";
    }
  });
</script>
```

### adobe.target.registerExtension()

**Beskrivning**:

Tillhandahåller ett standardsätt att registrera ett specifikt tillägg.

Detta stöds inte längre och bör inte användas.

## Sammanfattning av borttagna, nya och stöds i 2. js-funktioner.*x*

| Metod | Stöds? | Nytt? | Föråldrad?<P>(Standardinnehåll visas) |
| --- | --- | --- | --- |
| `getOffer()` | Ja |  |  |
| `getOffers()` |  | Ja |  |
| `applyOffer()` | Ja |  |  |
| `applyOffers()` |  | Ja |  |
| `triggerView()` |  | Ja |  |
| `trackEvent()` | Ja |  |  |
| `mboxCreate()` |  |  | Ja |
| `mboxDefine()`<P>`mboxUpdate()` |  |  | Ja |
| `targetGlobalSettings()` | Ja |  |  |
| `Data Providers` | Ja |  |  |
| `targetPageParams()` | Ja |  |  |
| `targetPageParamsAll()` | Ja |  |  |
| `registerExtension()` |  |  | Ja |
| `At.js Custom Events` | Ja |  |  |

## Begränsningar och bildtexter

Tänk på följande begränsningar och hänvisningar:

### Konverteringsspårning

Kunder som använder `mboxCreate()` för konvertering måste använda `trackEvent()` eller `getOffer()`.

### Erbjudandeleverans

Kunder som inte ersätter `mboxCreate()` med `getOffer()` eller `applyOffer()` riskerar att inte få erbjudanden levererade.

### Can at.js 2 *x* kan användas på vissa sidor medan at.js 1.*x* finns på andra sidor?

Ja, besökarprofilen bevaras på olika sidor med olika versioner och bibliotek. Cookie-formatet är detsamma.

### Ny API-användning i at.js 2.*x*

at.js 2.*x* använder ett nytt API, som vi kallar leverans-API. För att kunna felsöka om at.js anropar edge-servern [!DNL Target] korrekt kan du filtrera fliken Nätverk i webbläsarens utvecklingsverktyg till &quot;delivery&quot;, &quot;`tt.omtrdc.net`&quot; eller din klientkod. Du kommer också att märka att [!DNL Target] skickar en JSON-nyttolast i stället för nyckelvärdepar.

### [!DNL Target] Global Mbox används inte längre

I at.js 2.*x*, du ser inte längre `target-global-mbox` synligt i nätverksanropen. I stället har vi ersatt syntaxen `target-global-mbox` med `execute > pageLoad` i JSON-nyttolasten som skickades till [!DNL Target]-servrarna, vilket visas nedan:

```json {line-numbers="true"}
{
  "id": {
    // ...
  },
  "context": {
    "channel": "web",
    // ...
  },
  "execute": {
    "pageLoad": {}
  }
}
```

Det globala mbox-konceptet introducerades för att tala om för [!DNL Target] om erbjudanden och innehåll ska hämtas vid sidinläsning. Därför har vi gjort detta tydligare i vår senaste version.

### Spelar det globala mbox-namnet i at.js längre någon roll?

Kunderna kan ange ett globalt mbox-namn via **[!UICONTROL Target]** > **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit at.js Settings]**. Den här inställningen används av edge-servrarna [!DNL Target] för att översätta execute > pageLoad till det globala mbox-namnet som visas i [!DNL Target] UI. Detta gör att kunderna kan fortsätta att använda serversidans API:er, den formulärbaserade dispositionen, profilskript och skapa målgrupper med hjälp av den globala mbox-namnet. Vi rekommenderar att du även kontrollerar att samma globala mbox-namn är konfigurerat på sidan **[!UICONTROL Administration]** > **[!UICONTROL Visual Experience Composer]** om du fortfarande har sidor som använder at.js 1.*x*, vilket visas på följande bilder.

![Ändra at.js-dialogrutan](../assets/modify-atjs.png)

och

![Anpassad global mbox](../assets/custom-global-mbox.png)

### Måste inställningen för att automatiskt skapa en global mbox aktiveras för at.js 2.*x*?

I de flesta fall, ja. Den här inställningen anger at.js 2.*x* om du vill utlösa en begäran till edge-servrarna för [!DNL Target] vid sidinläsning. Eftersom global mbox översätts till execute > pageLoad, och om du vill utlösa en begäran vid sidinläsning, bör den här inställningen vara aktiverad.

### Kommer befintliga VEC-aktiviteter att fortsätta fungera, även om målets globala mbox-namn inte anges från at.js 2.*x*?

Ja, eftersom execute > pageLoad behandlas på [!DNL Target]-serverdelen som `target-global-mbox`.

### Om mina formulärbaserade aktiviteter är riktade till `target-global-mbox`, kommer dessa aktiviteter att fortsätta att fungera?

Ja, eftersom execute > pageLoad behandlas på edge-servrarna [!DNL Target] som `target-global-mbox`.

### Stöds och stöds inte i .js 2.Inställningar för *x*

| Inställning | Stöds? |
| --- | --- |
| X-domän | Nej |
| Skapa global Mbox automatiskt | Ja |
| Namn på global mbox | Ja |

### Stöd för spårning mellan domäner i at.js 2.x

Spårning mellan domäner gör det möjligt att sammanfoga besökare i olika domäner. Eftersom en ny cookie måste skapas för varje domän är det svårt att spåra besökare när de navigerar från domän till domän. [!DNL Target] använder en cookie från tredje part för att spåra besökare över domäner för att kunna utföra spårning mellan domäner. Detta gör att du kan skapa en [!DNL Target]-aktivitet som sträcker sig över `siteA.com` och `siteB.com` och besökarna behåller samma upplevelse när de navigerar mellan unika domäner. Den här funktionen är kopplad till cookie-beteendet hos [!DNL Target] och dess tredje part.

>[!NOTE]
>
>Spårning mellan domäner stöds från och med at.js 2.10, men inte direkt i at.js 2.*x* före 2.10. Spårning mellan domäner stöds i at.js 2.*x* via Experience Cloud ID-biblioteket v4.3.0+.

I [!DNL Target] lagras cookien från tredje part i `<CLIENTCODE>.tt.omtrdc.net`. Den första partens cookie lagras i `clientdomain.com`. Den första begäran returnerar HTTP-svarshuvuden som försöker ange cookies från tredje part med namnen `mboxSession` och `mboxPC`, medan en omdirigeringsbegäran skickas tillbaka med en extra parameter (`mboxXDomainCheck=true`). Om webbläsaren accepterar cookies från tredje part inkluderar omdirigeringsbegäran dessa cookies och upplevelsen returneras. Det här arbetsflödet är möjligt eftersom vi använder metoden HTTP GET.

I at.js 2.*x*, HTTP-GET används inte. I stället används HTTP-POST via at.js 2.*x* för att skicka JSON-nyttolaster till [!DNL Target] Edge-servrar. Med HTTP-POST avses omdirigeringsbegäran för att kontrollera om en webbläsare stöder cookies från tredje part. Detta beror på att HTTP GET-begäranden är viktiga transaktioner, medan HTTP-POST är icke-idempotent och inte får upprepas godtyckligt. Därför kan du spåra korsdomäner i at.js 2.*x* (före 2.10) stöds inte i paketet. Endast i js 1.*x* har körklart stöd för spårning mellan domäner.

Om du vill använda spårning mellan domäner för at.js v2.10 eller senare kan du göra något av följande:

1. Installera [ECID-biblioteket v4.3.0+](https://experienceleague.adobe.com/docs/id-service/using/release-notes/release-notes.html) tillsammans med at.js 2.*x*. ECID-biblioteket finns för att hantera beständiga ID:n som används för att identifiera en besökare även mellan domäner. Efter installation av ECID-biblioteket v4.3.0+ och at.js 2.*x*, du kan skapa aktiviteter som spänner över unika domäner samt spåra användare. Det är viktigt att notera att den här funktionen fungerar först när sessionen har upphört.

1. Om du har at.js v2.10 eller senare kan du i stället för att installera ECID-biblioteket aktivera korsdomänsinställningen i [!DNL Target]-gränssnittet i **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**. (Du kan också ange alternativet _crossDomain_ till _enabled_ i koden at.js.)

Om du vill använda spårning mellan domäner för versioner av at.js v2.*x* före 2.10 kan du implementera alternativ 1 ovan (installera ECID-biblioteket).

### Automatisk generering av global Mbox stöds

Den här inställningen anger at.js 2.*x* om du vill utlösa en begäran till edge-servrarna [!DNL Target] vid sidinläsning. Eftersom den globala mbox översätts till execute > pageLoad, och detta tolkas av edge-servrarna [!DNL Target], bör kunderna aktivera detta om de vill starta en begäran vid sidinläsning.

### Namn på global Mbox stöds

Kunderna kan ange ett globalt mbox-namn via **[!UICONTROL Target]** > **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit]**. Den här inställningen används av edge-servrarna [!DNL Target] för att översätta execute > pageLoad till det inmatade globala mbox-namnet. Detta gör att kunderna kan fortsätta att använda serversidans API:er, den formulärbaserade dispositionen, profilskript och skapa målgrupper som har den globala mbox som mål.

### Gäller de anpassade at.js-händelserna nedan `triggerView()` eller endast `applyOffer()` eller `applyOffers()`?

* `adobe.target.event.CONTENT_RENDERING_FAILED`
* `adobe.target.event.CONTENT_RENDERING_SUCCEEDED`
* `adobe.target.event.CONTENT_RENDERING_NO_OFFERS`
* `adobe.target.event.CONTENT_RENDERING_REDIRECT`

Ja, de anpassade händelserna at.js gäller även för `triggerView()`.

### Det står att när jag anropar `triggerView()` med &amp;lbrace;`"page" : "true"`&amp;rbrace; skickas ett meddelande till [!DNL Target]-serverdelen och utseendet ökar. Gör det även att profilskripten körs?

När ett förhämtningsanrop görs till [!DNL Target]-serverdelen körs profilskripten. Därefter krypteras profildata som påverkas och skickas tillbaka till klientsidan. När `triggerView()` med `{"page": "true"}` har anropats skickas ett meddelande tillsammans med de krypterade profildata. Detta är när [!DNL Target]-serverdelen dekrypterar profildata och lagrar dem i databaserna.

### Behöver vi lägga till kod som döljs i förväg innan `triggerView()` anropas för att hantera flimmer?

Nej, du behöver inte lägga till kod som döljs innan du anropar `triggerView()`. at.js 2.*x* hanterar logiken för att dölja och filtrera innan vyn visas och används.

### Som i .js 1.Parametrarna *x* för att skapa målgrupper stöds inte i at.js 2.*x*?

Följande at.js 1.x-parametrar stöds för närvarande *NOT* för målgruppsgenerering när at.js 2 används.*x*:

* browserHeight
* browserWidth
* browserTimeOffset
* screenHeight
* screenWidth
* screenOrientation
* colorDepth
* devicePixelRatio
* vst.* parametrar (se nedan)

### at.js 2.*x* har inte stöd för att skapa målgrupper med hjälp av vst.* parametrar

Kunder på at.js 1.*x* kunde använda vst.* mbox-parametrar för att skapa målgrupper. Detta var en oavsiktlig biverkning av hur at.js 1.*x* skickade mbox-parametrar till [!DNL Target]-serverdelen. Efter migrering till at.js 2.*x*, du kan inte längre skapa målgrupper med de här parametrarna eftersom at.js 2.*x* skickar mbox-parametrar på ett annat sätt.

## kompatibilitet med at.js

I följande tabeller beskrivs at.js. 2.*x*-kompatibilitet med olika aktivitetstyper, integreringar, funktioner och at.js-funktioner.

### Typ av aktivitet

| Typ | Stöds? |
| --- | --- |
| [!UICONTROL A/B Test] | Ja |
| [!UICONTROL Auto-Allocate] | Ja |
| [!UICONTROL Auto-Target] | Ja |
| [!UICONTROL Experience Targeting] | Ja |
| [!UICONTROL Multivariate Test] | Ja |
| [!UICONTROL Automated Personalization] | Ja |
| [!DNL Recommendations] | Ja |

>[!NOTE]
>
>[!UICONTROL Auto-Target] aktiviteter stöds via at.js 2.*x* och VEC när alla ändringar tillämpas på `Page Load Event`. När ändringar läggs till i vissa vyer stöds endast aktiviteterna [!UICONTROL A/B Test], [!UICONTROL Auto-Allocate] och [!UICONTROL Experience Targeting] (XT).

### Integreringar

| Typ | Stöds? |
| --- | --- |
| [!UICONTROL Analytics for Target] (A4T) | Ja |
| Målgrupper | Ja |
| Kundattribut | Ja |
| AEM Experience Fragments | Ja |
| [Adobe Experience Platform-tillägg](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) | Ja |
| Felsökning | Ja |
| Revisor | Regler har ännu inte uppdaterats för kl. 2.js.*x* |
| Stöd för deltagande för [GDPR](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) | Detta stöds i [at.js version 2.1.0](/help/dev/implement/client-side/atjs/target-atjs-versions.md#atjs-version-210-june-3-2019) eller senare. |
| AEM Förbättrad Personalization från [!DNL Adobe Target] | Nej |

### Funktioner

| Funktion | Stöds? |
| --- | --- |
| X-domän | Nej |
| Egenskaper/arbetsytor | Ja |
| QA-länkar | Ja |
| Formulärbaserad Experience Composer | Ja |
| Visual Experience Composer (VEC) | Ja |
| Egen kod | Ja |
| [Svarstoken](#response-tokens) | Ja |
| Klickspårning | Ja |
| Fleraktivitetsleverans | Ja |
| targetGlobalSettings | Ja (men inte x-domain) |
| at.js-metoder | Allt stöds förutom<P>`mboxCreate()`<P>`mboxUpdate()`<P>`mboxDefine()`<P>som visar standardinnehåll. |

### Frågesträngsparametrar

| Parameter | Stöds? |
| --- | --- |
| `?mboxDisable` | Ja |
| `?mboxDisable` | Ja |
| `?mboxTrace` | Ja |
| `?mboxSession` | Nej |
| `?mboxOverride.browserIp` | Ja |

## Svarstoken

at.js 2.*x*, precis som at.js 1.*x* använder den anpassade händelsen `at-request-succeeded` för att visa svarstoken. Exempel på kod som använder den anpassade händelsen `at-request-succeeded` finns i [Svarstoken](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html).

## at.js 1.*x* parametrar till at.js 2.*x* nyttolastmappning

I det här avsnittet beskrivs mappningarna mellan at.js 1.*x* och at.js 2.*x*.

Innan parametermappningen tas bort har slutpunkterna som används i dessa biblioteksversioner ändrats:

* at.js 1.*x* - `http://<client code>.tt.omtrdc.net/m2/<client code>/mbox/json`
* at.js 2.*x* - `http://<client code>.tt.omtrdc.net/rest/v1/delivery`

En annan viktig skillnad är att

* at.js 1.*x* - Klientkoden ingår i sökvägen
* at.js 2.*x* - Klientkoden skickas som en frågesträngsparameter, till exempel:
  `http://<client code>.tt.omtrdc.net/rest/v1/delivery?client=democlient`

I följande avsnitt listas var och en at.js 1.*x*-parametern, dess beskrivning och motsvarande 2.*x* JSON-nyttolast (om tillämpligt):

### at_property

(at.js 1.*x* parameter)

Används för [Enterprise-användarbehörigheter](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html).

```json {line-numbers="true"}
{
  ....
  "property": {
    "token": "1213213123122313121"
  }
  ....
}
```

### mboxHost

(at.js 1.*x* parameter)

Domänen för sidan där biblioteket [!DNL Target] körs.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "context": {
    "browser": {
       "host": "test.com"
    }
  }
}
```

### webGLRenderer

(at.js 1.*x* parameter)

Webbläsarens WEB GL-återgivningsfunktioner. Detta används av vår mekanism för enhetsidentifiering för att avgöra om besökarens enhet är en stationär dator, iPhone, Android-enhet osv.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "context": {
    "browser": {
       "webGLRenderer": "AMD Radeon Pro 560X OpenGL Engine"
    }
  }
}
```

### mboxURL

(at.js 1.*x* parameter)

Sidans URL.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "context": {
    "address": {
       "url": "http://test.com"
    }
  }
}
```

### mboxReferer

(at.js 1.*x* parameter)

Sidreferenten.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "context": {
    "address": {
       "referringUrl": "http://google.com"
    }
  }
}
```

### mbox (namnet) är lika med global mbox

(at.js 1.*x* parameter)

Leverans-API har inte längre något globalt mbox-koncept. I JSON-nyttolasten måste du använda `execute > pageLoad`.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "execute": {
    "pageLoad": {
       "parameters": ....
       "profileParameters": ...
       .....
    }
  }
}
```

### mbox (namnet) är *inte* lika med global mbox

(at.js 1.*x* parameter)

Om du vill använda ett mbox-namn skickar du det till `execute > mboxes`. En mbox kräver ett index och namn.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "execute": {
    "mboxes": [{
       "index": 0,
       "name": "some-mbox",
       "parameters": ....
       "profileParameters": ...
       .....
    }]
  }
}
```

### mboxId

(at.js 1.*x* parameter)

Används inte längre.

### mboxCount

(at.js 1.*x* parameter)

Används inte längre.

### mboxRid

(at.js 1.*x* parameter)

Begär-ID som används av system längre fram i kedjan för att hjälpa till med felsökningen.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "requestId": "2412234442342"
  ....
}
```

### mboxTime

(at.js 1.*x* parameter)

Används inte längre.

### mboxSession

(at.js 1.*x* parameter)

Sessions-ID skickas som en frågesträngsparameter (`sessionId`) till slutpunkten för leverans-API.

### mboxPC

(at.js 1.*x* parameter)

TNT-ID skickas till `id > tntId`.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "id": {
    "tntId": "ca5ddd7e33504c58b70d45d0368bcc70.21_3"
  }
  ....
}
```

### mboxMCGVID

(at.js 1.*x* parameter)

Marketing Cloud Visitor-ID har skickats till `id > marketingCloudVisitorId`.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "id": {
    "marketingCloudVisitorId": "797110122341429343505"
  }
  ....
}
```

### `vst.aaaa.id` och `vst.aaaa.authState`

(at.js 1.*x* parametrar)

Kund-ID ska skickas till `id > customerIds`.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "id": {
    "customerIds": [{
       "id": "1232131",
       "integrationCode": "aaaa",
       "authenticatedState": "....."
     }]
  }
  ....
}
```

### mbox3rdPartyId

(at.js 1.*x* parameter)

Kundens tredje parts-ID används för att länka olika [!DNL Target]-ID.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "id": {
    "thirdPartyId": "1232312323123"
  }
  ....
}
```

### mboxMCSDID

(at.js 1.*x* parameter)

SDID, även kallat kompletterande data-ID. Ska skickas till `experienceCloud > analytics > supplementalDataId`.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "experienceCloud": {
    "analytics": {
      "supplementalDataId": "1212321132123131"
    }
  }
  ....
}
```

### vst.trk

(at.js 1.*x* parameter)

Spårningsserver för [!UICONTROL Analytics]. Ska skickas till `experienceCloud > analytics > trackingServer`.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "experienceCloud": {
    "analytics": {
      "trackingServer": "analytics.test.com"
    }
  }
  ....
}
```

### vst.trks

(at.js 1.*x* parameter)

Analysspårningsservern är säker. Ska skickas till `experienceCloud > analytics > trackingServerSecure`.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "experienceCloud": {
    "analytics": {
      "trackingServerSecure": "secure-analytics.test.com"
    }
  }
  ....
}
```

### mboxMCGLH

(at.js 1.*x* parameter)

Platstips för Audience Manager. Ska skickas till `experienceCloud > audienceManager > locationHint`.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "experienceCloud": {
    "audienceManager": {
      "locationHint": 9
    }
  }
  ....
}
```

### mboxAAMB

(at.js 1.*x* parameter)

Audience Manager blob. Ska skickas till `experienceCloud > audienceManager > blob`.

at.js 2.*x* JSON-nyttolast:

```json {line-numbers="true"}
{
  "experienceCloud": {
    "audienceManager": {
      "blob": "2142342343242342"
    }
  }
  ....
}
```

### mboxVersion

(at.js 1.*x* parameter)

Version skickas som en frågesträngsparameter via versionsparametern.

## Utbildningsvideo: at.js 2.*x* arkitekturdiagram ![Översikt ](../../assets/overview.png)

at.js 2.*x* förbättrar Adobe [!DNL Target] stöd för SPA och kan integreras med andra Experience Cloud-lösningar. Den här videon förklarar hur allt hänger ihop.

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

Se [Förstå hur at.js 2.*x* fungerar](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html) om du vill ha mer information.
