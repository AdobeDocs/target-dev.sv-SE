---
keywords: flimmer, at.js, implementering, asynkront, asynkront, synkront, synkront, $8
description: Lär dig hur at.js och [!DNL Target] förhindrar flimmer (standardinnehåll visas tillfälligt innan det ersätts av aktivitetsinnehåll) under sidinläsning eller appinläsning.
title: Hur hanterar At.js Flicker?
feature: at.js
exl-id: 8aacf254-ec3d-4831-89bb-db7f163b3869
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---

# Hur at.js hanterar flimmer

Information om hur JavaScript-biblioteket [!DNL Adobe Target] at.js förhindrar flimmer under sidinläsning eller appinläsning.

Flimmer inträffar när standardinnehåll visas för besökare innan det ersätts av aktivitetsinnehållet. Flimmer är inte önskvärt eftersom det kan vara förvirrande för besökare.

## Använda en automatiskt skapad global mbox

Om du aktiverar inställningen [Skapa global Mbox automatiskt](/help/dev/implement/client-side/atjs/global-mbox/customize-global-mbox.md) när du konfigurerar at.js hanterar at.js flimmer genom att ändra opacitetsinställningen när sidan läses in. När at.js läses in ändras opacitetsinställningen för elementet `<body>` till &quot;0&quot;, vilket gör sidan osynlig för besökarna. När ett svar från [!DNL Target] har tagits emot, eller om ett fel med begäran [!DNL Target] upptäcks, återställer at opacitet till &quot;1&quot;. Detta garanterar att besökaren bara ser sidan efter att innehållet i dina aktiviteter har tillämpats.

Om du aktiverar inställningen när du konfigurerar at.js anger at.js opaciteten för HTML BODY-formatet till 0. När ett svar från [!DNL Target] har tagits emot, återställer at.js opaciteten för HTML BODY till 1.

Med Opacitet 0 döljs sidinnehållet för att förhindra flimmer, men webbläsaren återger sidan och läser in alla nödvändiga resurser som CSS, bilder osv.

Om `opacity: 0` inte fungerar i din implementering kan du även hantera flimmer genom att anpassa `bodyHiddenStyle` och ange det som `body {visibility:hidden !important}`. Du kan använda antingen `body {opacity:0 !important}` eller `body {visibility:hidden !important}`, beroende på vilket som fungerar bäst för dina specifika omständigheter.

Följande bild visar anropet Dölj brödtext och Visa brödtext i båda at.js 1.*x* och at.js 2.x.

**at.js 2.x**

(Klicka på bilden för att expandera till full bredd.)

![Målflöde: at.js page load request](/help/dev/implement/client-side/assets/atjs-20-flow-page-load-request.png "Målflöde: at.js page load request"){zoomable="yes"}

**at.js 1.*x***

(Klicka på bilden för att expandera till full bredd.)

![Målflöde: Automatiskt skapad global mbox](/help/dev/implement/client-side/atjs/how-atjs-works/assets/target-flow2.png "målflöde: Automatiskt skapad global mbox"){zoomable="yes"}

Mer information om åsidosättningen av `bodyHiddenStyle` finns i [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

## Hantera flimmer vid inläsning av at.js asynkront

Att läsa in at.js asynkront är ett bra sätt att undvika att blockera webbläsaren från återgivning, men den här tekniken kan leda till att webbsidan flimrar.

Du kan undvika flimmer genom att använda ett fragment som är synligt i förväg när de relevanta HTML-elementen har anpassats av Target.

at.js kan läsas in asynkront, antingen direkt inbäddad på sidan eller via en tagghanterare (till exempel Adobe Experience Platform Launch).

Om at.js är inbäddad på sidan måste fragmentet läggas till innan at.js läses in. Om du läser in at.js via en tagghanterare, som också läses in asynkront, måste du lägga till fragmentet innan du läser in tagghanteraren. Om tagghanteraren läses in synkront kan skriptet inkluderas i tagghanteraren före at.js.

Så här döljer du kodfragment:

```
;(function(win, doc, style, timeout) {
  var STYLE_ID = 'at-body-style';

  function getParent() {
    return doc.getElementsByTagName('head')[0];
  }

  function addStyle(parent, id, def) {
    if (!parent) {
      return;
    }

    var style = doc.createElement('style');
    style.id = id;
    style.innerHTML = def;
    parent.appendChild(style);
  }

  function removeStyle(parent, id) {
    if (!parent) {
      return;
    }

    var style = doc.getElementById(id);

    if (!style) {
      return;
    }

    parent.removeChild(style);
  }

  addStyle(getParent(), STYLE_ID, style);
  setTimeout(function() {
    removeStyle(getParent(), STYLE_ID);
  }, timeout);
}(window, document, "body {opacity: 0 !important}", 3000));
```

Som standard döljs hela HTML BODY med fragmentet. I vissa fall kanske du bara vill dölja vissa element i HTML i förväg och inte hela sidan. Du kan uppnå det genom att anpassa style-parametern. Den kan ersättas med något som bara döljer vissa delar av sidan.

Du har till exempel två områden som identifieras av ID:n container-1 och container-2, och sedan kan formatet ersättas med följande:

```
#container-1, #container-2 {opacity: 0 !important}
```

I stället för standardinställningen:

```
body {opacity: 0 !important}
```

## Hantera flimmer i at.js 2.x för triggerView()

När du använder `triggerView()` för att visa målinnehåll i SPA anges hantering av flimmer direkt. Det innebär att fördold logik inte behöver läggas till manuellt. I stället döljer at.js 2.x platsen där din vy måste visas innan målinnehållet används.

## Hantera flimmer med getOffer() och applyOffer()

Eftersom både `getOffer()` och `applyOffer()` är lågnivå-API:er finns det ingen inbyggd flimmerkontroll. Du kan skicka en väljare eller ett HTML-element som ett alternativ till `applyOffer()`. I det här fallet lägger `applyOffer()` till aktivitetsinnehållet i det här specifika elementet. Du måste dock se till att elementet är fördolt innan du anropar `getOffer()` och `applyOffer()`.

```
document.documentElement.style.opacity = "0";
 
adobe.target.getOffer({
    mbox: 'target-global-mbox',
    success: function(offer) {
        adobe.target.applyOffer({
            mbox: 'target-global-mbox',
            offer: offer
        });
 
        document.documentElement.style.opacity = "1";
    },
    error: function() {
        document.documentElement.style.opacity = "1";        
    }
});
```

## Använda en regional mbox med mboxCreate() i at.js 1.x (stöds inte i at.js 2.x)

Om du använder en regional mbox-implementering kan du använda `mboxCreate()` med din sida som har etablerats på ungefär samma sätt som följande exempelkod:

```
<div class="mboxDefault">
Some default content
</div>
<script>
mboxCreate('some-mbox');
</script>
```

Om sidorna är korrekt konfigurerade hanterar at.js flimmer genom att ändra CSS-egenskapen &quot;visibility&quot; för elementet på rätt sätt med klassen mboxDefault.
