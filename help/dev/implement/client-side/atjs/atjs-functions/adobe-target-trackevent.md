---
keywords: adobe.target.trackEvent, trackEvent, trackevent, track event, at.js, functions, function, preventDefault, preventDefault, preventDefault, prevent default, adobe.target.trackEvent
description: Använd funktionen [!UICONTROL adobe.target.trackEvent()] för JavaScript-biblioteket  [!DNL Adobe Target]  at.js om du vill utlösa en begäran om att rapportera användaråtgärder, till exempel klickningar och konverteringar på din webbplats.
title: Hur använder jag funktionen [!UICONTROL adobe.target.trackEvent()]?
feature: at.js
exl-id: 9a55e4f1-d7f9-47c1-867c-2ce06fb26f9f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# [!UICONTROL adobe.target.trackEvent(options)]

Den här funktionen utlöser en begäran om att rapportera användaråtgärder, till exempel klickningar och konverteringar. Den levererar inte någon verksamhet som svar.

Dessa händelsespårningsanrop kan sedan användas för att definiera mått i aktiviteter. Mer information finns i [Success Metrics](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html) och [Track Conversions](../how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions).

Här är API-informationen:

| Nyckel | Typ | Obligatoriskt | Beskrivning |
|--- |--- |--- |--- |
| mbox | Sträng | Ja | Namn på mbox<P>**Obs!** Om ett [!UICONTROL trackEvent()]-anrop utlöses med ett mbox-namn som redan har utlösts på sidan, återställs SDID för [!UICONTROL trackEvent()] och skiljer sig från [!DNL Target] -anropen på sidan. Om du däremot utlöser ett [!UICONTROL trackEvent()]-anrop med ett annat mbox-namn, blir [!UICONTROL trackEvent()] anropets SDID konsekvent med [!UICONTROL Page Load Request]/[!UICONTROL triggerView()] -anropen på sidan. |
| väljare | Sträng | Nej | CSS-väljare som används för att hitta elementen i HTML. Händelseavlyssnarna bifogas till de hittade elementen. |
| type | Sträng | Nej | Representerar en registrerad händelsetyp. Det kan vara både händelser som är kända för HTML, som: click, mouseDown o.s.v., samt anpassade HTML-händelser. |
| preventDefault | Boolean | Nej | Anger om `[!UICONTROL event.preventDefault()]` ska användas i händelseavlyssnaråteranropet. Standardvärdet är false.<P>**Obs!**: Endast `[!UICONTROL form[submit]]` och `a[click]` stöds. Andra scenarier stöds inte på grund av komplexitet och ett stort antal scenarier som ska stödjas. |
| parametrar | Objekt | Nej | Mbox-parametrar. Ett objekt med nyckelvärdepar som har följande struktur:<P>`{ "param1": "value1", "param2": "value2"}` |
| timeout | Nummer | Nej | Timeout i millisekunder.<P>Om inget anges används standardvärdet:<P>`...timeoutInSeconds: 0.15...}` |
| framgång | Funktion | Nej | En callback-funktion som används för att signalera att en händelse har rapporterats. |
| fel | Funktion | Nej | En återanropsfunktion som används för att signalera att händelsen inte kunde rapporteras. |

## Exempel

```javascript {line-numbers="true"}
<a href="https://asite.com">click me!</a> 
```

plus JavaScript-kod för att tilldela `trackEvent`:

```javascript {line-numbers="true"}
<script> 
$('a').click(function(event){ 
  adobe.target.trackEvent({'mbox':'homePageHero'}) 
}); 
</script> 
```

Eller:

```javascript {line-numbers="true"}
adobe.target.trackEvent({ 
    "mbox": "clicked-cta", 
    "params": { 
        "param1": "value1" 
    } 
});
```

>[!WARNING]
>
>Om de obligatoriska fälten inte anges utförs ingen begäran och ett fel genereras.
