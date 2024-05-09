---
keywords: implementera, implementera, at.js, adobe experience platform web sdk, aep web sdk
description: Lär dig implementera [!DNL Adobe Target] för webben på klientsidan med [!DNL Adobe Experience Platform Web SDK] (AEP Web SDK) eller JavaScript-biblioteket at.js.
title: Hur implementerar jag [!DNL Target] för webben på klientsidan
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
source-git-commit: 2d2a593df661c7e6c6e6384af6042e8aa4575fdb
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# Översikt: implementera [!DNL Target] för webben på klientsidan

I en implementering på klientsidan av [!DNL Adobe Target], [!DNL Target] levererar upplevelserna som är kopplade till en aktivitet direkt till klientwebbläsaren. Webbläsaren avgör vilken upplevelse som ska visas och visar den. Med en implementering på klientsidan kan du använda en WYSIWYG-redigerare, [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) (VEC), eller ett icke-visuellt gränssnitt, [Formulärbaserad Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html), för att skapa din aktivitet och dina personaliseringsupplevelser.

Att implementera [!DNL Target] på klientsidan måste du använda något av följande JavaScript-bibliotek:

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)

  The [!UICONTROL Adobe Experience Platform Web SDK] kan du interagera med de olika tjänsterna i [!DNL Adobe Experience Cloud] (inklusive [!DNL Target]) via [!UICONTROL Adobe Experience Edge Network]. Om du migrerar till [!UICONTROL Adobe Experience Platform Web SDK], se [Vad är [!UICONTROL Adobe Experience Platform Web SDK]](/help/dev/implement/client-side/aep-web-sdk.md).

* [[!DNL Target] at.js JavaScript-bibliotek](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)

  JavaScript-biblioteket at.js förbättrar sidinläsningstiderna för webbimplementeringar, förbättrar säkerheten och erbjuder bättre implementeringsalternativ för enkelsidiga program. Om du väljer att migrera till at.js, se [How At.js Works](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) och [[!DNL Adobe Target] Kunskapsbyggaren: Utvecklarchatt, migrera Adobe Target mbox.js till at.js](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true).


Se [Jämföra biblioteket at.js med Web SDK](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/adobe-target/web-sdk-atjs-comparison){target=_blank} för att lära sig om skillnaderna mellan de två implementeringsmetoderna.