---
keywords: implementera, implementera, at.js, adobe experience platform web sdk, aep web sdk
description: Lär dig hur du implementerar  [!DNL Adobe Target] för klientwebben med hjälp av JavaScript-biblioteket  [!DNL Adobe Experience Platform Web SDK]  (AEP Web SDK) eller at.js.
title: Hur implementerar jag  [!DNL Target] för klientwebben?
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
source-git-commit: 315e8fbe67938588c3c9a0135e0cd85fa1f12187
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Översikt: implementera [!DNL Target] för klientwebben

I en implementering på klientsidan av [!DNL Adobe Target] levererar [!DNL Target] upplevelserna som är kopplade till en aktivitet direkt till klientwebbläsaren. Webbläsaren avgör vilken upplevelse som ska visas och visar den. Med en implementering på klientsidan kan du använda en WYSIWYG-redigerare, [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) (VEC) eller ett icke-visuellt gränssnitt, [formulärbaserad Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html), för att skapa din aktivitet och dina personaliseringsupplevelser.

Om du vill implementera [!DNL Target] på klientsidan måste du använda ett av följande JavaScript-bibliotek:

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)

  Med [!UICONTROL Adobe Experience Platform Web SDK] kan du interagera med de olika tjänsterna i [!DNL Adobe Experience Cloud] (inklusive [!DNL Target]) via [!UICONTROL Adobe Experience Edge Network]. Om du väljer att migrera till [!UICONTROL Adobe Experience Platform Web SDK] läser du [Vad är [!UICONTROL Adobe Experience Platform Web SDK]](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md).

* [[!DNL Target] at.js JavaScript-bibliotek](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)

  JavaScript-biblioteket at.js förbättrar sidinläsningstiderna för webbimplementeringar, förbättrar säkerheten och erbjuder bättre implementeringsalternativ för enkelsidiga program. Om du väljer att migrera till at.js ska du läsa [How At.js Works](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) och [[!DNL Adobe Target] SDödBuilder: Developer chat, migrera Adobe Target mbox.js till at.js](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true).


Mer information om skillnaderna mellan de två implementeringsmetoderna finns i [Jämföra at.js-biblioteket med Web SDK](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/adobe-target/web-sdk-atjs-comparison){target=_blank}.