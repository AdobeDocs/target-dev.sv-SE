---
keywords: implementera, implementera, implementera, adobe launch, launch, race, redirect, experience platform launch, platform launch, taggar, adobe platform, implement2
description: Lär dig hur du implementerar biblioteket  [!DNL Adobe Target]  at.js med  [!DNL Adobe Experience Platform], den metod som rekommenderas för att implementera Target.
title: Hur implementerar jag  [!DNL Target] med  [!DNL Adobe Experience Platform]?
feature: Implement Server-side
exl-id: 0a325871-194a-479c-a3bf-294e3dde3e9a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---

# Implementera [!DNL Target] med [!DNL Adobe Experience Platform]

Taggar i [!DNL Adobe Experience Platform] är nästa generation av tagghanteringsfunktioner från [!DNL Adobe]. Taggar ger kunderna ett enkelt sätt att driftsätta och hantera de analys-, marknadsförings- och reklamtaggar som behövs för att skapa relevanta kundupplevelser.

>[!NOTE]
>
>Adobe Experience Platform Launch har omklassificerats som en serie datainsamlingstekniker i [!DNL Adobe Experience Platform]. Som ett resultat av detta har flera terminologiska förändringar införts i produktdokumentationen. I följande [dokument](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html?) finns en konsoliderad referens till de ändrade terminologin.

I följande tabell visas de olika källor där du kan få mer information:

| Resurs | Information |
|--- |--- |
| [Lägg till Adobe Target](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/implement-solutions/target.html#implement-solutions) | I den här självstudiekursen finns stegvisa instruktioner för hur du implementerar [!DNL Target] på en webbplats med taggar i [!DNL Adobe Experience Platform]. Du kan lägga till JavaScript-biblioteket at.js, bränna den globala mbox, lägga till parametrar och integrera med andra lösningar. Artikeln ingår i en större självstudiekurs som visar hur du implementerar Adobe Experience Platform och andra Adobe Experience Cloud-lösningar. |
| [Snabbstartguide](https://experienceleague.adobe.com/docs/experience-platform/tags/get-started/quick-start.html) | Information om driftsättning och hantering av de analys-, marknadsförings- och annonstaggar som krävs för relevanta kundupplevelser. |
| [Adobe [!DNL Target] tilläggsöversikt](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/target/overview.html) | Information om hur du implementerar [!DNL Target] med [!DNL Adobe Experience Platform]. |

## Fördelar med att implementera at.js med tillägget [!DNL Target]

Följande fördelar gäller bara om du använder taggar i [!DNL Adobe Experience Platform] för att implementera at.js. Därför föreslår Adobe starkt att du använder taggar i [!DNL Adobe Experience Platform] i stället för att manuellt implementera at.js.

* **Löser upp [!DNL Adobe Analytics]- och [!DNL Target]-konkurrensvillkoren:** Eftersom anropet [!DNL Analytics] kunde utlösas före anropet [!DNL Target] sammanfogas inte anropet [!DNL Target] med anropet [!DNL Analytics]. Den här sekvenseringen kan leda till felaktiga data. Tillägget [!DNL Target] ser till att [!DNL Analytics]-anropet väntar tills anropet [!DNL Target] har slutförts, eller inte. Om du använder taggar i [!DNL Adobe Experience Platform] löses de inkonsekvenser som kunderna kan uppleva när de implementerar manuellt.

  >[!NOTE]
  >
  >Använd åtgärden Skicka signal i tillägget [!DNL Adobe Analytics] så att anropet [!DNL Analytics] väntar på anropet [!DNL Target]. Om du anropar `s.t()` eller `s.tl()` direkt med anpassad kod väntar inte [!DNL Analytics] anrop förrän [!DNL Target] anrop har slutförts.

* **Förhindrar felaktig hantering av omdirigeringserbjudanden:** Om du har [!DNL Target] och [!DNL Analytics] på sidan och det finns ett omdirigeringserbjudande som körs av Target, kan du uppleva en situation där [!DNL Analytics]-spåraren utlöser en begäran när den inte ska det (eftersom användaren omdirigeras till en annan URL). Om du implementerar [!DNL Target] och [!DNL Analytics] via taggar i [!DNL Adobe Experience Platform] kommer du inte att märka det här problemet. Med hjälp av taggar i [!DNL Adobe Experience Platform] instruerar [!DNL Target] [!DNL Analytics] att avbryta [!DNL Analytics] Beacon-begäran.
