---
keywords: mboxCreate, mboxCreate, mbox create, at.js, functions, function
description: Använd funktionen [!UICONTROL mboxCreate()] för JavaScript-biblioteket  [!DNL Adobe Target]  at.js för att tillämpa erbjudanden på närmaste DIV med klassnamnet mboxDefault. (at.js 1.x)
title: Hur använder jag funktionen [!UICONTROL mboxCreate()]?
feature: at.js
exl-id: 86eba1fc-4e1d-4793-94e7-898bf81f8945
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# [!UICONTROL mboxCreate(mbox,params)] - at.js 1.x

Kör en begäran och tillämpar erbjudandet på närmaste DIV med mboxDefault-klassnamn.

>[!NOTE]
>
>Den här funktionen är tillgänglig för version 1 av at.js.Endast *x*. Den här funktionen har ersatts med versionen av at.js 2.x. Den här funktionen returnerar standardinnehåll om den används med at.js 2.x.

Den här funktionen är till största delen inbyggd i at.js för att underlätta övergången från mbox.js (nu borttagen) till at.js. Ett nyare alternativ till `[!UICONTROL mboxCreate()]` är `[!UICONTROL adobe.target.getOffer()]`/ `[!UICONTROL adobe.target.applyOffer()]` eller Angularna.

## Exempel

```javascript {line-numbers="true"}
<div class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  mboxCreate('mboxName','param1=value1','param2=value2'); 
</script>
```

## Anteckningar

`mboxCreate()` använder nu slutpunkten json i stället för standardslutpunkten och aktiveras asynkront. På grund av detta:

* [Felsökning](/help/dev/implement/client-side/target-debugging-atjs/target-debugging-atjs.md) är lite annorlunda.
* Undvik erbjudandekod som kräver synkrona, blockerande anrop.

  I erbjudanden anges till exempel JavaScript-variabler som används av webbplatskoden eller andra rutor som kommer senare på sidan.

* Se till att du har en `<div class="mboxDefault"></div>` innan du anropar `[!UICONTROL mboxCreate()]` eftersom at.js inte kommer att lägga till en åt dig.

* Tomma, övre delen av sidan `[!UICONTROL mboxCreate()]` rekommenderas inte som en global mbox.

  Den automatiskt skapade globala mbox i at.js är ett bättre alternativ eftersom den utlöses från `<head>` och kan returnera innehåll tidigare.
