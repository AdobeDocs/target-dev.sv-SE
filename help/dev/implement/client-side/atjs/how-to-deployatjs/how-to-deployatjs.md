---
keywords: implementera, at.js, javascript-bibliotek
description: Lär dig hur du distribuerar [!DNL Adobe Target]  at.js JavaScript-bibliotek med taggar i [!DNL Adobe Experience Platform] eller utan en tagghanterare.
title: Hur distribuerar jag på .js?
feature: Implement Server-side
exl-id: e62cb27e-ea80-462b-90f8-0a033b128031
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# Så här distribuerar du at.js

Information om hur du distribuerar [!DNL Adobe Target]  JavaScript-bibliotek, at.js, använda taggar i [!DNL Adobe Experience Platform] eller utan en tagghanterare.

Du kan distribuera at.js på följande sätt:

* **[Implementera [!DNL Target] använda taggar i Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)**: Taggar i [!DNL Adobe Experience Platform] är nästa generation av tagghanteringsfunktioner från Adobe. Taggar ger kunderna ett enkelt sätt att driftsätta och hantera de analys-, marknadsförings- och reklamtaggar som behövs för att skapa relevanta kundupplevelser.

>[!NOTE]
>
> [!DNL Adobe Experience Platform Launch] har omklassificerats som en serie datainsamlingstekniker i [!DNL Adobe Experience Platform]. Som ett resultat av detta har flera terminologiska förändringar införts i produktdokumentationen. Se följande [dokument](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) för en konsoliderad hänvisning till terminologiska förändringar.

* **[Implementera [!DNL Target] utan en tagghanterare](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)**: Du kan implementera [!DNL Target] utan att använda en tagghanterare (till exempel taggar i [!DNL Adobe Experience Platform]).
* **Implementera [!DNL Target] använda en tagghanterare från tredje part**: [Taggar i Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) är den metod som ska implementeras [!DNL Target]Men du kan också implementera [!DNL Target] med en tagghanterare från tredje part, som Tealium, Ensighten och Google Tag. En lista över fördelarna med att använda Launch finns på [Fördelar med att implementera at.js med [!DNL Adobe Target]  extension](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md#advantages-of-implementing-atjs-using-the-target-extension).

  Om du vet hur man implementerar [!DNL Target] utan en tagghanterare kan du enkelt implementera med en tredjeparts tagghanterare i stället för att hårdkoda at.js i webbplatskoden.

  Här är två ämnen som kommer att hjälpa dig att implementera [!DNL Target] med en tagghanterare från tredje part:

   * [Innan du implementerar](/help/dev/before-implement/prepare-to-implement-target.md)
   * [Implementera [!DNL Target] utan tagghanterare](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

  Mer information finns i dokumentationen till tagghanteraren från tredje part.

Att implementera [!DNL Target] när du använder appar för en sida (SPA), se [Implementering av Single Page-program](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).
