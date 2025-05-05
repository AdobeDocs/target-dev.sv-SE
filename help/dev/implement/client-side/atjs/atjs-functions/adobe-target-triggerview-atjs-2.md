---
keywords: adobe.target.triggerView, triggerView, trigger view, trigger view, at.js, functions, function, viewName, view name, view name, adobe.target.triggerView1
description: Använd funktionen adobe.target.triggerView() för JavaScript-biblioteket  [!DNL Adobe Target]  at.js för användning i Single Page-program (SPA). (at.js 2.x)
title: Hur använder jag funktionen adobe.target.triggerView()?
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
source-git-commit: fe4e607173c760f782035a10f52936d96e9db300
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

Den här funktionen kan anropas när en ny sida läses in eller när en komponent på en sida återges på nytt. `adobe.target.triggerView()` ska implementeras för enkelsidiga program (SPA) för att använda [!UICONTROL Visual Experience Composer] (VEC) för att skapa [!UICONTROL A/B Test]- och [!UICONTROL Experience Targeting] (XT)-aktiviteter. Om `[!UICONTROL adobe.target.triggerView()]` inte har implementerats på platsen kan VEC inte användas för SPA. Mer information finns i [Implementering av Single Page-program](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

>[!NOTE]
>
>Den här funktionen introducerades med at.js 2.*x*. Den här funktionen är inte tillgänglig för at.js version 1.*x*.

| Parameter | Typ | Obligatoriskt? | Beskrivning |
| --- | --- | --- | --- |
| viewName | Sträng | Ja | Ange valfritt namn som en strängtyp som du vill representera vyn. Vynamnet visas på [!UICONTROL Modifications]-panelen i VEC så att marknadsförare kan skapa åtgärder och köra sina [!UICONTROL A/B Test]- och [!UICONTROL Experience Targeting] XT-aktiviteter. |
| alternativ | Objekt | Nej |  |
| alternativ > sida | Boolean | Nej | **TRUE:** Standardvärdet för sidan är true. När page=true skickas meddelanden till [!DNL Target]-serverdelen för ökat antal visningar.<P>Ett meddelande skickas alltid som standard när `[!UICONTROL triggerView]` anropas, förutom när alternativen > sida är inställda på false.<P>**FALSKT:** När page=false skickas inga meddelanden för ökat antal visningar. Detta tillvägagångssätt bör användas när du endast vill återge en komponent på en sida med ett erbjudande.<P>**Obs!** Anpassade koderbjudanden i VEC återges inte igen när `[!UICONTROL triggerView()]` anropas med `{page: false}` som alternativ. |

## Exempel: Sant

`[!UICONTROL triggerView()]`-anrop om du vill skicka ett meddelande till [!DNL Target]-serverdelen för att öka antalet aktivitetsavtryck och andra mått.

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView")
```

## Exempel: Falskt

`[!UICONTROL triggerView()]` anrop om att inte ha meddelanden skickade till [!DNL Target]-serverdelen för att göra en inläsningsräkning.

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView", {page: false})
```

## Exempel: Löpande kedja med `getoffers()` och `applyOffers()`

Om du vill köra `triggerView()` när `getOffers()`-löftet är löst är det viktigt att köra `triggerView()` på det sista blocket, vilket visas i exemplet nedan. Detta är nödvändigt för att VEC ska kunna identifiera `Views` i redigeringsläge.

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

## Exempel: Bästa kompatibilitet för `triggerView()` med [!UICONTROL Adobe Visual Editing Helper extension]

Tänk på följande när du använder tillägget [Hjälp för visuell redigering i Adobe](https://experienceleague.adobe.com/sv/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank}:

På grund av [!DNL Googl]es nya V3-manifestprinciper för [!DNL Chrome] tillägg måste [!UICONTROL Visual Editing Helper extension] vänta på `DOMContentLoaded`-händelsen innan [!DNL Target]-biblioteken läses in i VEC. Den här fördröjningen kan göra att webbsidor utlöser anropet `triggerView()` innan redigeringsbiblioteken är klara, vilket gör att vyn inte fylls i vid inläsningen.

Använd en avlyssnare för händelsen `load` om du vill åtgärda problemet.

Här är ett exempel på implementering:

```javascript
function triggerViewIfLoaded() {
    adobe.target.triggerView("homeView");
}

if (document.readyState === "complete") {
    // If the page is already loaded
    triggerViewIfLoaded();
} else {
    // If the page is not yet loaded, set up an event listener
    window.addEventListener("load", triggerViewIfLoaded);
}
```


