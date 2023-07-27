---
keywords: mboxCreate, mboxCreate, mbox create, at.js, functions, function
description: Använd [!UICONTROL mboxCreate()] funktionen för [!DNL Adobe Target] at.js JavaScript-bibliotek som ska tillämpa erbjudanden på närmaste DIV med mboxDefault-klassnamnet. (at.js 1.x)
title: Hur jag använder [!UICONTROL mboxCreate()] Funktion?
feature: at.js
exl-id: 86eba1fc-4e1d-4793-94e7-898bf81f8945
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# [!UICONTROL mboxCreate(mbox,params)] - at.js 1.x

Kör en begäran och tillämpar erbjudandet på närmaste DIV med mboxDefault-klassnamn.

>[!NOTE]
>
>Den här funktionen är tillgänglig för version 1 av at.js.*x* endast. Den här funktionen har ersatts med versionen av at.js 2.x. Den här funktionen returnerar standardinnehåll om den används med at.js 2.x.

Den här funktionen är till största delen inbyggd i at.js för att underlätta övergången från mbox.js (nu borttagen) till at.js. Ett nyare alternativ till `[!UICONTROL mboxCreate()]` är `[!UICONTROL adobe.target.getOffer()]`/ `[!UICONTROL adobe.target.applyOffer()]` eller Angular-direktivet.

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

`mboxCreate()` använder nu slutpunkten &quot;json&quot; i stället för standardslutpunkten och aktiveras asynkront. På grund av detta:

* [Felsökning](/help/dev/implement/client-side/target-debugging-atjs/target-debugging-atjs.md) är lite annorlunda.
* Undvik erbjudandekod som kräver synkrona, blockerande anrop.

  I erbjudanden anges till exempel JavaScript-variabler som används av platskod eller andra rutor som kommer senare på sidan.

* Se till att du har en `<div class="mboxDefault"></div>`före anrop `[!UICONTROL mboxCreate()]`, eftersom at.js inte lägger till en åt dig.

* Tom, överst på sidan `[!UICONTROL mboxCreate()]` funktioner rekommenderas inte som en global mbox.

  Den automatiskt skapade globala mbox i at.js är ett bättre alternativ eftersom den aktiveras från `<head>` och kan returnera innehåll tidigare.
