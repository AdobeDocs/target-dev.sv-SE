---
keywords: implementering av single page-applikation, implementera single page application, spa, at.js 2.x, at.js, single page application, single page app, spa, SPA, single page application implementation
description: Lär dig hur du använder  [!DNL Adobe Target]  at.js 2.x för att implementera [!DNL Target] för Single Page-program (SPA).
title: Kan jag implementera  [!DNL Target] för Single Page-program (SPA)?
feature: Implement Server-side
exl-id: d59d7683-0a63-47a9-bbb5-0fe4a5bb7766
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '2728'
ht-degree: 0%

---

# Implementering av Single Page-program

Traditionella webbplatser arbetade på&quot;sida-till-sida&quot;-navigeringsmodeller, som annars kallas för flersidiga program, där webbplatsdesign var nära kopplad till webbadresser och övergångar mellan olika webbsidor kräver en sidladdning. Moderna webbprogram, som SPA (Single Page Applications), använder i stället en modell som möjliggör snabb användning av webbläsargränssnittsåtergivning, som ofta är oberoende av sidomladdning. De här upplevelserna triggas ofta av kundinteraktioner som rullningar, klick och markörrörelser. I takt med att de moderna webbens paradigmer har utvecklats fungerar inte längre de traditionella generiska eventernas relevans, som sidladdning, för personalisering och experimenterande.

![Traditionell sidlivscykel jämfört med SPA &#x200B;](assets/trad-vs-spa.png)

at.js 2.x innehåller många funktioner som gör det möjligt för ditt företag att utföra personalisering på nästa generations klienttekniker. Den här versionen fokuserar på att förbättra at.js för att få harmonisk interaktion med SPA.

Här är några fördelar med att använda at.js 2.x som inte finns i tidigare versioner:

* Möjlighet att cachelagra alla erbjudanden vid sidinläsning för att minska antalet serveranrop till ett enda serveranrop.
* Förbättra slutanvändarnas upplevelser enormt på er webbplats eftersom erbjudandena visas direkt via cachen utan fördröjning som introducerats av traditionella serversamtal.
* En enkel kodrad och en engångskonfiguration för utvecklare som gör det möjligt för era marknadsförare att skapa och köra A/B- och Experience Targeting-aktiviteter (XT) via VEC på era SPA.

## [!DNL Adobe Target] vyer och program med en sida

VEC-värdet [!DNL Adobe Target] för SPA utnyttjar det nya konceptet Vyer: en logisk grupp visuella element som tillsammans utgör en SPA. En SPA kan därför betraktas som en övergång via vyer i stället för URL-adresser som baseras på användarinteraktioner. En vy kan vanligtvis representera en hel plats eller grupperade visuella element inom en plats.

Om du vill förklara vad Vyer är kan du navigera på den hypotetiska e-handelsplatsen som implementeras i React och utforska några exempel på Vyer. Klicka på länkarna nedan för att öppna den här webbplatsen på en ny flik i webbläsaren.

**Länk: [Hemwebbplats](https://target.enablementadobe.com/react/demo/#/)**

![hemwebbplats](assets/home.png)

När vi navigerar till hemsidan ser vi omedelbart en hjältebild som marknadsför en påskförsäljning samt de senaste produkterna som säljer på webbplatsen. I det här fallet kan en vy definieras som hela hemsidan. Detta är praktiskt att notera eftersom vi kommer att utöka detta mer i avsnittet [!DNL Adobe Target]-vyer som implementeras nedan.

**Länk: [Produktwebbplats](https://target.enablementadobe.com/react/demo/#/products)**

![produktwebbplats](assets/product-site.png)

När vi blir mer intresserade av de produkter som företaget säljer väljer vi att klicka på länken Produkter. På samma sätt som hemsidan kan hela produktwebbplatsen definieras som en vy. Vi kan ge den här vyn namnet&quot;products&quot; precis som sökvägsnamnet i `https://target.enablementadobe.com/react/demo/#/products)`.

![produktwebbplats 2](assets/product-site-2.png)

I början av det här avsnittet definierade vi Vyer som hela webbplatsen eller till och med en grupp visuella element på webbplatsen. Som framgår ovan kan de fyra produkter som visas på webbplatsen också grupperas och betraktas som en vy. Om vi vill kalla den här vyn kan vi kalla den&quot;Produkter&quot;.

![produktwebbplats 3](assets/product-site-3.png)

Vi väljer att klicka på knappen Läs in mer för att utforska fler produkter på webbplatsen. Webbplatsens URL ändras inte i det här fallet. Men en vy här kan bara representera den andra produktraden som visas ovan. Vynamnet kan kallas&quot;PRODUCTS-PAGE-2&quot;.

**Länk: [Utcheckning](https://target.enablementadobe.com/react/demo/#/checkout)**

![utcheckningssida](assets/checkout.png)

Eftersom vi tyckte om vissa produkter som visas på webbplatsen bestämde vi oss för att köpa ett par. På utcheckningssajten har vi fått möjlighet att välja normal leverans eller expressleverans. Eftersom en vy kan vara vilken grupp som helst av visuella element på en webbplats kan vi kalla den här&quot;Visa leveransinställningar&quot;.

Dessutom kan vykonceptet utökas betydligt mer än så. Om marknadsförarna vill anpassa innehållet på webbplatsen beroende på vilken leveransinställning som har valts, kan en vy skapas för varje leveransinställning. När vi väljer Normal leverans får du i så fall namnet&quot;Normal leverans&quot;. Om du väljer Express Delivery får du namnet &quot;Express Delivery&quot;.

Nu kanske marknadsförarna vill köra ett A/B-test för att se om en ändring av färgen från blå till röd när expressleverans är valt kan öka konverteringsgraden, i stället för att knappfärgen ska vara blå för båda leveransalternativen.

## Implementera [!DNL Adobe Target]-vyer

Nu när vi har täckt det som [!DNL Adobe Target] vyer är kan vi utnyttja det här konceptet i [!DNL Target] för att göra det möjligt för marknadsförare att köra A/B- och XT-tester på SPA via VEC. Detta kräver en engångsinstallation av utvecklare. Låt oss gå igenom stegen för att konfigurera detta.

1. Installera på .js 2.*x*.

   Först måste vi installera på .js 2.*x*. Den här versionen av at.js utvecklades med SPA i åtanke. Tidigare versioner av at.js stöder inte [!DNL Adobe Target]-vyer och VEC för SPA.

   Ladda ned på .js 2.*x* via användargränssnittet [!DNL Adobe Target] som finns i **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**. at.js 2.*x* kan också distribueras via taggar i [!DNL Adobe Experience Platform].

1. Implementera at.js 2.Funktionen *x*, `[triggerView()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)` på dina webbplatser.

   När du har definierat vyerna för SPA där du vill köra ett A/B- eller XT-test implementerar du at.js 2.Funktionen *x* `triggerView()` med vyerna skickade som en parameter. Detta gör att marknadsförarna kan använda VEC för att utforma och köra A/B- och XT-tester för de vyer som definierats. Om funktionen `triggerView()` inte har definierats för dessa vyer kommer VEC inte att identifiera vyerna och marknadsförarna kan därför inte använda VEC för att utforma och köra A/B- och XT-tester.

   >[!NOTE]
   >
   >Om du vill visa stöd i at.js måste [viewsEnabled](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#viewsenbabled) anges till true, annars inaktiveras alla visningsfunktioner.

   **`adobe.target.triggerView(viewName, options)`**

   | Parameter | Typ | Obligatoriskt? | Validering | Beskrivning |
   | --- | --- | --- | --- | --- |
   | viewName | Sträng | Ja | 1. Inga efterföljande blanksteg.<br />2. Kan inte vara tom.<br />3. Visningsnamnet måste vara unikt för alla sidor.<br />4. **Varning**: Visningsnamnet får inte börja eller sluta med `/`. Detta beror på att kunden vanligtvis extraherar visningsnamnet från URL-sökvägen. För oss är&quot;home&quot; och &quot;`/home`&quot; olika.<br />5. **Varning**: Samma vy ska inte aktiveras flera gånger i följd med alternativet `{page: true}`. | Ange valfritt namn som en strängtyp som du vill representera din vy. Det här visningsnamnet visas på panelen **[!UICONTROL Modifications]** i VEC så att marknadsförare kan skapa åtgärder och köra A/B- och XT-aktiviteter för dem. |
   | alternativ | Objekt | Nej |  |  |
   | alternativ > sida | Boolean | Nej |  | **TRUE**: Standardvärdet för sidan är true. När `page=true` skickas meddelanden till Edge-servrarna för ökat antal visningar.<br />**FALSE**: När `page=false` skickas inga meddelanden för ökat antal inläsningar. Detta bör användas när du endast vill återge en komponent på en sida med ett erbjudande. |

   Nu ska vi gå igenom några exempel på hur funktionen `triggerView()` kan anropas i React för vår hypotetiska e-SPA:

   **Länk: [Hemwebbplats](https://target.enablementadobe.com/react/demo/#/)**

   ![home-response-1](assets/react1.png)

   Om vi som marknadsförare vill köra A/B-tester på hela hemsidan kanske vi vill kalla vyn&quot;home&quot;:

```
 function targetView() {
   var viewName = window.location.hash; // or use window.location.pathName if router works on path and not hash

   viewName = viewName || 'home'; // view name cannot be empty

   // Sanitize viewName to get rid of any trailing symbols derived from URL
   if (viewName.startsWith('#') || viewName.startsWith('/')) {
     viewName = viewName.substr(1);
   }

   // Validate if the Target Libraries are available on your website
   if (typeof adobe != 'undefined' && adobe.target && typeof adobe.target.triggerView === 'function') {
     adobe.target.triggerView(viewName);
   }
 }

 // react router v4
 const history = syncHistoryWithStore(createBrowserHistory(), store);
 history.listen(targetView);

 // react router v3
 <Router history={hashHistory} onUpdate={targetView} >
```

**Länk: [Produkter &#x200B;](https://target.enablementadobe.com/react/demo/#/products)**

Låt oss titta på ett exempel som är lite mer komplicerat. Som marknadsförare vill vi personalisera produktraden genom att ändra etikettfärgen&quot;Price&quot; till röd efter att en användare klickat på knappen Läs in mer.

![reagerar-produkter](assets/react4.png)

```
 function targetView(viewName) {
   // Validate if the Target Libraries are available on your website
   if (typeof adobe != 'undefined' && adobe.target && typeof adobe.target.triggerView === 'function') {
     adobe.target.triggerView(viewName);
   }
 }

 class Products extends Component {
   render() {
     return (
       <button type="button" onClick={this.handleLoadMoreClicked}>Load more</button>
     );
   }

   handleLoadMoreClicked() {
     var page = this.state.page + 1; // assuming page number is derived from component's state
     this.setState({page: page});
     targetView('PRODUCTS-PAGE-' + page);
   }
 }
```

**Länk: [Utcheckning](https://target.enablementadobe.com/react/demo/#/checkout)**

![reagerar utcheckning](assets/react6.png)

Om marknadsförarna vill anpassa innehållet på webbplatsen beroende på vilken leveransinställning som har valts, kan en vy skapas för varje leveransinställning. När vi väljer Normal leverans får du i så fall namnet&quot;Normal leverans&quot;. Om du väljer Express Delivery får du namnet &quot;Express Delivery&quot;.

Nu kanske marknadsförarna vill göra ett A/B-test för att se om en ändring av färgen från blå till röd när expressleverans är valt kan öka konverteringen i stället för att behålla knappfärgen blå för båda leveransalternativen.

```
 function targetView(viewName) {
   // Validate if the Target Libraries are available on your website
   if (typeof adobe != 'undefined' && adobe.target && typeof adobe.target.triggerView === 'function') {
     adobe.target.triggerView(viewName);
   }
 }

 class Checkout extends Component {
   render() {
     return (
       <div onChange={this.onDeliveryPreferenceChanged}>
         <label>
           <input type="radio" id="normal" name="deliveryPreference" value={"Normal Delivery"} defaultChecked={true}/>
           <span> Normal Delivery (7-10 business days)</span>
         </label>

         <label>
           <input type="radio" id="express" name="deliveryPreference" value={"Express Delivery"}/>
           <span> Express Delivery* (2-3 business days)</span>
         </label>
       </div>
     );
   }
   onDeliveryPreferenceChanged(evt) {
     var selectedPreferenceValue = evt.target.value;
     targetView(selectedPreferenceValue);
   }
 }
```

## at.js 2.x systemdiagram

Följande diagram hjälper dig att förstå arbetsflödet i at.js 2.x med Vyer och hur detta förbättrar SPA integrering. Mer information om begreppen som används i at.js 2.x finns i [Implementering av enkelsidiga program](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

![Målflöde med at.js 2.x](../../assets/system-diagram-atjs-20.png)

| Steg | Information |
| --- | --- |
| 1 | Samtalet returnerar Experience Cloud-ID om användaren är autentiserad. Ett annat anrop synkroniserar kund-ID:t. |
| 2 | At.js-biblioteket läses in synkront och döljer dokumentets brödtext.<br /> at.js kan också läsas in asynkront med ett inledande kodfragment som implementerats på sidan. |
| 3 | En sidinläsningsbegäran görs med alla konfigurerade parametrar (MCID, SDID och kund-ID). |
| 4 | Profilskript körs och matas sedan in i profilarkivet. Store begär kvalificerade målgrupper från Audience Library (till exempel målgrupper som delas från Adobe Analytics, Audience Management, osv.).<br />Kundattribut skickas till profilarkivet i en gruppbearbetning. |
| 5 | Baserat på parametrar för URL-begäran och profildata avgör [!DNL Target] vilka aktiviteter och upplevelser som ska returneras till besökaren för den aktuella sidan och framtida vyer. |
| 6 | Målinriktat innehåll skickas tillbaka till sidan, eventuellt med profilvärden för ytterligare personalisering.<br />Målinnehåll på den aktuella sidan visas så snabbt som möjligt utan att standardinnehållet flimrar.<br />Målanpassat innehåll för vyer som visas som ett resultat av användaråtgärder i en SPA som cachelagras i webbläsaren så att det kan tillämpas direkt utan ett extra serveranrop när vyer aktiveras via `triggerView()`. |
| 7 | Analysdata skickas till datainsamlingsservrar. |
| 8 | Måldata matchas mot [!DNL Analytics] data via SDID och bearbetas till rapportlagringen i [!DNL Analytics].<br />Analysdata kan sedan visas i både [!DNL Analytics]- och [!DNL Target]-rapporter via [!DNL Analytics] for [!DNL Target]-rapporter (A4T). |

Nu hämtas vyer och åtgärder från cachen och visas för användaren utan ett serveranrop, oavsett var `triggerView()` implementeras på SPA. `triggerView()` skickar också en aviseringsbegäran till [!DNL Target]-serverdelen för att öka antalet och registrera antalet visningar.

![Målflöde vid.js 2.x triggerView](../../assets/atjs-20-triggerview.png)

| Steg | Information |
| --- | --- |
| 1 | `triggerView()` anropas i SPA för att återge vyn och tillämpa åtgärder för att ändra visuella element. |
| 2 | Målinnehåll för vyn läses från cachen. |
| 3 | Målinriktat innehåll visas så snabbt som möjligt utan att man behöver flimra standardinnehållet. |
| 4 | Meddelandebegäran skickas till [!DNL Target]-profilarkivet för att räkna besökaren i aktiviteten och öka måtten. |
| 5 | Analysdata skickas till datainsamlingsservrar. |
| 6 | Måldata matchas mot [!DNL Analytics] data via SDID och bearbetas till rapportlagringen i [!DNL Analytics]. [!DNL Analytics] data kan sedan visas i både [!DNL Analytics] och [!DNL Target] via A4T-rapporter. |

## Visual Experience Composer för enkelsidig app

När du har slutfört installationen på .js 2.x och lagt till `triggerView()` på platsen använder du VEC för att köra A/B- och XT-aktiviteter. Mer information finns i [Single Page App (SPA) Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/spa-visual-experience-composer.html?lang=sv-SE).

>[!NOTE]
>
>VEC för SPA är i själva verket samma VEC som du använder på vanliga webbsidor, men vissa ytterligare funktioner är tillgängliga när du öppnar ett enkelsidigt program med `triggerView()` implementerat.

## Använd TriggerView för att säkerställa att A4T fungerar korrekt med at.js 2.x och SPA

För att [Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=sv-SE) (A4T) ska fungera korrekt med at.js 2.x måste du skicka samma SDID i [!DNL Target]-begäran och i [!DNL Analytics]-begäran.

Som bästa praxis SPA

* Använd anpassade händelser för att meddela att något intressant händer i programmet
* Starta en anpassad händelse innan vyn börjar rendera
* Starta en anpassad händelse när vyn har återgetts

at.js 2.x lade till en ny API-funktion, [triggerView()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md) . Du bör använda `triggerView()` för att meddela at.js att en vy kommer att starta återgivningen.

För att se hur man kombinerar anpassade händelser, at.js 2.x och Analytics, ska vi ta ett exempel. I det här exemplet antas att HTML-sidan innehåller Visitor-API:t, följt av at.js 2.x, följt av AppMeasurement.

Låt oss anta att följande anpassade händelser finns:

* `at-view-start` - När vyn börjar rendera
* `at-view-end` - När vyn har återgetts

För att vara säker på att A4T fungerar med at.js 2.x

Vystartshanteraren ska se ut ungefär så här:

```jsx {line-numbers="true"}
document.addEventListener("at-view-start", function(e) {
  var visitor = Visitor.getInstance("<your Adobe Org ID>");
  
  visitor.resetState();
  adobe.target.triggerView("<view name>");
});
```

Vysluthanteraren ska se ut ungefär så här:

```jsx {line-numbers="true"}
document.addEventListener("at-view-end", function(e) {
  // s - is the AppMeasurement tracker object
  s.t();
});
```

>[!NOTE]
>
>Du måste utlösa händelserna `at-view-start` och `at-view-end`. Dessa händelser ingår inte i anpassade at.js-händelser.

Även om de här exemplen använder JavaScript-kod kan allt detta förenklas om du använder en tagghanterare, till exempel taggar i [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md).

Om ovanstående steg följs bör du ha en robust A4T-lösning för SPA.

## Bästa praxis för implementering

Med API:erna för at.js 2.x kan du anpassa implementeringen av [!DNL Target] på många sätt, men det är viktigt att du följer rätt ordning på åtgärderna under den här processen.

Följande information beskriver den ordning i vilken du måste följa när du läser in ett Single Page-program för första gången i en webbläsare och för eventuella vyändringar som sker senare.

### Operationsordning för inledande sidinläsning {#order}

| Steg | Åtgärd | Information |
| --- | --- | --- |
| 1 | Läs in VisitorAPI JS | Det här biblioteket ansvarar för att tilldela besökaren ett ECID. Detta ID används senare av andra Adobe-lösningar på webbsidan. |
| 2 | Load at.js 2.x | at.js 2.x läser in alla nödvändiga API:er som du använder för att implementera [!DNL Target]-begäranden och vyer. |
| 3 | Kör [!DNL Target]-begäran | Om du har ett datalager rekommenderar vi att du läser in viktiga data som krävs för att skicka till [!DNL Target] innan [!DNL Target]-begäran körs. Detta gör att du kan använda `targetPageParams` för att inkludera alla data som du vill använda för målinriktning.<P>När `pageLoadEnabled` och `viewsEnabled` är inställda på true i [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) begär at.js automatiskt alla VEC [!DNL Target]-erbjudanden åt dig i steg 2.<P>Observera att `getOffers` också kan användas för att hämta VEC-erbjudanden efter att sidan har lästs in. Om du vill göra det kontrollerar du att begäran innehåller `execute>pageLoad` och `prefetch>views` i API-anropet. |
| 4 | Ring `triggerView()` | Eftersom [!DNL Target]-begäran som du initierade i steg 3 kan returnera upplevelser både för körning av sidinläsning och vyer, kontrollerar du att `triggerView()` anropas efter att [!DNL Target]-begäran har returnerats och att erbjudandena har tillämpats på cachen. Du får bara utföra det här steget en gång per vy. |
| 5 | Anropa beacon för sidvyn [!DNL Analytics] | Den här beacon skickar det SDID som är associerat med steg 3 och 4 till [!DNL Analytics] för sammanfogning av data. |
| 6 | Ring ytterligare `triggerView({"page": false})` | Detta är ett valfritt steg för SPA ramverk som skulle kunna återge vissa komponenter på sidan utan att en vyförändring inträffar. I sådana fall är det viktigt att du anropar det här API:t för att se till att [!DNL Target]-upplevelser tillämpas igen efter att SPA har återgett komponenterna. Du kan utföra det här steget så många gånger du vill för att se till att [!DNL Target]-upplevelser finns kvar i dina SPA. |

### Ordning för åtgärder för SPA (ingen helsidesinläsning)

| Steg | Åtgärd | Information |
| --- | --- | --- |
| 1 | Ring `visitor.resetState()` | Detta API säkerställer att SDID genereras om för den nya vyn när den läses in. |
| 2 | Uppdatera cache genom att anropa API:t `getOffers()` | Detta är ett valfritt steg att ta om den här vyändringen kan kvalificera den aktuella besökaren för fler [!DNL Target] aktiviteter eller diskvalificera dem från aktiviteter. Nu kan du även välja att skicka ytterligare data till [!DNL Target] för att aktivera ytterligare målinriktningsfunktioner. |
| 3 | Ring `triggerView()` | Om du har kört steg 2 måste du vänta på [!DNL Target]-begäran och tillämpa erbjudandena på cacheminnet innan du kör det här steget. Du får bara utföra det här steget en gång per vy. |
| 4 | Ring `triggerView()` | Om du inte har kört steg 2 kan du utföra det här steget så snart du slutför steg 1. Om du har kört steg 2 och steg 3 bör du hoppa över det här steget. Du får bara utföra det här steget en gång per vy. |
| 5 | Anropa beacon för sidvyn [!DNL Analytics] | Den här beacon skickar det SDID som är associerat med steg 2, 3 och 4 till [!DNL Analytics] för sammanfogning av data. |
| 6 | Ring ytterligare `triggerView({"page": false})` | Detta är ett valfritt steg för SPA ramverk som skulle kunna återge vissa komponenter på sidan utan att en vyförändring inträffar. I sådana fall är det viktigt att du anropar det här API:t för att se till att [!DNL Target]-upplevelser tillämpas igen efter att SPA har återgett komponenterna. Du kan utföra det här steget så många gånger du vill för att se till att [!DNL Target]-upplevelser finns kvar i dina SPA. |

## Utbildningsvideor

Följande videofilmer innehåller mer information:

### Så här fungerar at.js 2.x

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

Mer information finns i [Förstå hur at.js 2.x fungerar](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html?lang=sv-SE).

### Implementera at.js 2.x i en SPA

>[!VIDEO](https://video.tv.adobe.com/v/26248/?quality=12)

Mer information finns i [Implementera Adobe Target at.js 2.x i ett enkelsidigt program (SPA)](https://experienceleague.adobe.com/docs/target-learn/tutorials/experiences/use-the-visual-experience-composer-for-single-page-applications.html?lang=sv-SE).

### Använda VEC för SPA i [!DNL Adobe Target]

>[!VIDEO](https://video.tv.adobe.com/v/26249/?quality=12)

Mer information finns i [Använda Visual Experience Composer för enkelsidigt program (SPA VEC) i Adobe Target](https://experienceleague.adobe.com/docs/target-learn/tutorials/experiences/use-the-visual-experience-composer-for-single-page-applications.html?lang=sv-SE).
