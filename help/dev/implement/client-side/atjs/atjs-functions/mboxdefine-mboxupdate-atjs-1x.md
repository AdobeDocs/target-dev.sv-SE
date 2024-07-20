---
keywords: mboxDefine, mboxdefine, mbox define, mboxUpdate, mbox update, mbox update, at.js, functions, function, mboxDefine0
description: Använd funktionerna [!UICONTROL mboxDefine()] och [!UICONTROL mboxUpdate()] för JavaScript-biblioteket  [!DNL Adobe Target]  at.js för att definiera eller uppdatera en mbox. (at.js 1.x)
title: Hur använder jag funktionerna [!UICONTROL mboxDefine()] och [!UICONTROL mboxUpdate()]?
feature: at.js
exl-id: 0a7dbea2-1cbd-4a5b-ba68-4c76a88d65c4
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# [!UICONTROL mboxDefine()] och [!UICONTROL mboxUpdate()] - at.js 1.x

Definiera och uppdatera en mbox i [!DNL Adobe Target].

>[!NOTE]
>
>Dessa funktioner är tillgängliga för version 1 av at.js.Endast *x*. Dessa funktioner har ersatts med version 2 av at.js.*x*. Dessa funktioner returnerar standardinnehåll om de används med at.js 2.*x*.

`[!UICONTROL mboxDefine()]` och `[!UICONTROL mboxCreate()]` är kopplade till HTML DIV-element där erbjudandet ska visas. Dessa HTML DIV-element ska ha klassen `mboxDefault`. Om den här klassen inte är kopplad till elementen i HTML kan du se en viss märkbar flimmer.

## mboxDefine

Skapar en intern mappning mellan ett nodeId och ett mbox-namn, men kör inte begäran. Används tillsammans med `[!UICONTROL mboxUpdate()]`. Inbyggd i at.js oftast för att underlätta övergången från mbox.js (nu borttagen) till at.js.

## mboxUpdate

Kör begäran och tillämpar erbjudandet på elementet som identifieras av `nodeId` i `mboxDefine()`. Kan också användas för att uppdatera en mbox som initierats av `[!UICONTROL mboxCreate]`. Inbyggd i at.js oftast för att underlätta övergången från mbox.js (nu borttagen) till at.js. `mboxDefine()`/ `mboxUpdate()` kan ersättas av [adobe.target.getOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) och [adobe.target.applyOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md) med hjälp av väljaralternativet.

## Exempel

```javascript {line-numbers="true"}
<div id="someId" class="mboxDefault"></div> 
<script> 
 mboxDefine('someId','mboxName','param1=value1','param2=value2'); 
 mboxUpdate('mboxName','param3=value3','param4=value4'); 
</script>
```
