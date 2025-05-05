---
keywords: global mbox, implementera at.js
description: Lär dig mer om den globala mbox-implementeringen i [!DNL Adobe Target], a name used to refer to the single server call made at the top of each web page in your [!DNL Target] .
title: Vad är en global mbox?
feature: at.js
exl-id: 572c1dc6-5cdd-427a-9458-e5ec49990cf8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 0%

---

# Förstå den globala mbox

Information om den globala mbox, ett namn som används för att referera till det enskilda serveranropet som görs högst upp på varje webbsida i din [!DNL Adobe Target]-implementering.

Som standard har den globala mbox namnet `target-global-mbox`. Den kan vid behov få ett nytt namn för ditt konto.

Det finns flera skillnader mellan en vanlig mbox (icke-global mbox) och den globala mbox, inklusive:

| Vanlig mbox | Global mbox |
|--- |--- |
| En vanlig mbox radbryts vanligtvis runt innehåll med en `<DIV>`-tagg. | Den globala rutan är tom och figursätts inte runt något innehåll. |
| Innehåll från endast en aktivitet kan levereras i en vanlig mbox. | Innehåll från flera aktiviteter kan levereras som ett svar på en global mbox. |

Om flera aktiviteter levereras via den globala mbox eller via flera vanliga mbox-rutor, avgör mål [prioriteten](https://experienceleague.adobe.com/docs/target/using/activities/priority.html?lang=sv-SE) som aktiviteten (eller aktiviteterna) levereras med till en webbsida.

Ytterligare data på sidnivå kan skickas till [!DNL Target] tillsammans med den globala mbox-filen med funktionen `[!UICONTROL targetPageParams]`. Detta liknar mbox-parameterfunktionen. Mer information finns i [Överföra parametrar till en global mbox](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md).
