---
keywords: adobe.target.triggerView, triggerView, trigger view, trigger view, at.js, functions, function, viewName, view name, view name, adobe.target.triggerView1
description: Använd funktionen adobe.target.triggerView() för [!DNL Adobe Target] at.js JavaScript-bibliotek för användning i Single Page Applications (SPA). (at.js 2.x)
title: Hur använder jag funktionen adobe.target.triggerView()?
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

Den här funktionen kan anropas när en ny sida läses in eller när en komponent på en sida återges på nytt. `adobe.target.triggerView()` ska implementeras för enkelsidiga program (SPA) för att använda [!UICONTROL Visual Experience Composer] (VEC) för att skapa [!UICONTROL A/B Test] och [!UICONTROL Experience Targeting] (XT) aktiviteter. If `[!UICONTROL adobe.target.triggerView()]` inte implementeras på platsen, VEC kan inte användas för SPA. Mer information finns i [Implementering av Single Page-program](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

>[!NOTE]
>
>Den här funktionen introducerades med at.js 2.*x*. Den här funktionen är inte tillgänglig för at.js version 1.*x*.

| Parameter | Typ | Obligatoriskt? | Beskrivning |
| --- | --- | --- | --- |
| viewName | Sträng | Ja | Ange valfritt namn som en strängtyp som du vill representera vyn. Vynamnet visas i dialogrutan [!UICONTROL Modifications] för marknadsförare att skapa funktionsmakron och köra [!UICONTROL A/B Test] och [!UICONTROL Experience Targeting] XT-aktiviteter |
| alternativ | Objekt | Nej |  |
| alternativ > sida | Boolean | Nej | **TRUE:** Standardvärdet för sidan är true. När page=true skickas meddelanden till [!DNL Target] för att öka antalet intryckta.<P>Ett meddelande skickas alltid som standard när en `[!UICONTROL triggerView]` anropas utom när alternativ > sida är inställd på false.<P>**FALSE:** När page=false skickas inga meddelanden för ökat antal visningar. Detta tillvägagångssätt bör användas när du endast vill återge en komponent på en sida med ett erbjudande.<P>**Anteckning**: Anpassade koderbjudanden i VEC återges inte om `[!UICONTROL triggerView()]` anropas med `{page: false}` som alternativ. |

## Exempel: Sant

`[!UICONTROL triggerView()]` ring för att skicka ett meddelande till [!DNL Target] för att öka aktivitetsintrycket och andra mätvärden.

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView")
```

## Exempel: Falskt

`[!UICONTROL triggerView()]` anrop om att inte få meddelanden skickade till [!DNL Target] backend för inläsningsräkning.

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView", {page: false})
```

## Exempel: Lova att kedja med `getoffers()` och `applyOffers()`

Att köra `triggerView()` när `getOffers()` löftet är löst, det är viktigt att genomföra `triggerView()` på det sista blocket, vilket visas i exemplet nedan. Detta är nödvändigt för att VEC ska kunna upptäcka `Views` i redigeringsläge.

```javascript {line-numbers="true"}
adobe.target.getOffers({
    'request': {
        'prefetch': {
            'views': [{
                'parameters': {}
            }]
        }
    }
}).then(function(response) {
    // Apply Offers
    adobe.target.applyOffers({
        response: response
    });
}).catch(function(error) {
    console.log("AT: getOffers failed - Error", error);
}).finally(() => {
    // Trigger View call, assuming pageView is defined elsewhere
    adobe.target.triggerView(pageView, {
        page: true
    });
    console.log('AT: View triggered on : ' + pageView);
});
```
