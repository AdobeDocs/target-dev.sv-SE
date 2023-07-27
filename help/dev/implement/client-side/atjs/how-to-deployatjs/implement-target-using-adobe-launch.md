---
keywords: implementera, implementera, implementera, adobe launch, launch, race, redirect, experience platform launch, platform launch, taggar, adobe platform, implement2
description: Lär dig implementera [!DNL Adobe Target] at.js-bibliotek med [!DNL Adobe Experience Platform], den metod som ska användas för att implementera Target.
title: Hur implementerar jag [!DNL Target] använda [!DNL Adobe Experience Platform]?
feature: Implement Server-side
exl-id: 0a325871-194a-479c-a3bf-294e3dde3e9a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 0%

---

# Implementera [!DNL Target] använda [!DNL Adobe Experience Platform]

Taggar i [!DNL Adobe Experience Platform] är nästa generations tagghanteringsfunktioner från [!DNL Adobe]. Taggar ger kunderna ett enkelt sätt att driftsätta och hantera de analys-, marknadsförings- och reklamtaggar som behövs för att skapa relevanta kundupplevelser.

>[!NOTE]
>
>Adobe Experience Platform Launch har omklassificerats som en serie datainsamlingstekniker i [!DNL Adobe Experience Platform]. Som ett resultat av detta har flera terminologiska förändringar införts i produktdokumentationen. Se följande [dokument](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html?) för en konsoliderad hänvisning till terminologiska förändringar.

I följande tabell visas de olika källor där du kan få mer information:

| Resurs | Information |
|--- |--- |
| [Lägg till Adobe Target](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/implement-solutions/target.html#implement-solutions) | Den här självstudiekursen innehåller stegvisa instruktioner för att implementera [!DNL Target] på en webbplats med taggar i [!DNL Adobe Experience Platform]. Du kan lägga till JavaScript-biblioteket at.js, bränna den globala mbox, lägga till parametrar och integrera med andra lösningar. Artikeln ingår i en större självstudiekurs som visar hur du implementerar Adobe Experience Platform och andra Adobe Experience Cloud-lösningar. |
| [Snabbstartguide](https://experienceleague.adobe.com/docs/experience-platform/tags/get-started/quick-start.html) | Information om driftsättning och hantering av de analys-, marknadsförings- och annonstaggar som krävs för relevanta kundupplevelser. |
| [Adobe [!DNL Target] tilläggsöversikt](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/target/overview.html) | Information om implementering [!DNL Target] använda [!DNL Adobe Experience Platform]. |

## Fördelar med att implementera at.js med [!DNL Target] extension

Följande fördelar gäller bara om du använder taggar i [!DNL Adobe Experience Platform] att implementera at.js. Därför rekommenderar Adobe starkt att du använder taggar i [!DNL Adobe Experience Platform] i stället för att implementera at.js manuellt.

* **Lösningar [!DNL Adobe Analytics] och [!DNL Target] tävlingsvillkor:** På grund av [!DNL Analytics] samtalet kunde utlösas innan [!DNL Target] ring, [!DNL Target] anropet är inte fäst vid [!DNL Analytics] ring. Den här sekvenseringen kan leda till felaktiga data. The [!DNL Target] tillägget säkerställer att [!DNL Analytics] telefonsamtal väntar tills [!DNL Target] samtalet slutförs, slutförs eller inte. Använda taggar i [!DNL Adobe Experience Platform] löser de inkonsekvenser i datan som kunderna kan uppleva när de implementerar manuellt.

  >[!NOTE]
  >
  >Använd åtgärden Skicka signal i dialogrutan [!DNL Adobe Analytics] så att [!DNL Analytics] samtal väntar på [!DNL Target] ring. Om du ringer `s.t()` eller `s.tl()` med egen kod, [!DNL Analytics] samtal inte väntar tills [!DNL Target] samtalen är slutförda.

* **Förhindrar felaktig hantering av omdirigeringserbjudanden:** Om du har [!DNL Target] och [!DNL Analytics] på sidan, och det finns ett omdirigeringserbjudande från Target, kan du uppleva en situation där [!DNL Analytics] spåraren utlöser en begäran när den inte ska utföras (eftersom användaren omdirigeras till en annan URL). Om du implementerar [!DNL Target] och [!DNL Analytics] via taggar i [!DNL Adobe Experience Platform], kommer du inte att uppleva det här problemet. Använda taggar i [!DNL Adobe Experience Platform], [!DNL Target] handledare [!DNL Analytics] för att avbryta [!DNL Analytics] beacon request.
