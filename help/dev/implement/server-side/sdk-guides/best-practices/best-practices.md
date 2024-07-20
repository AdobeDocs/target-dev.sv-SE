---
title: Bästa tillvägagångssätt vid användning av enhetsbeslut
description: Lär dig bästa praxis när du använder [!UICONTROL on-device decisioning] i [!DNL Adobe Target]
feature: Implement Server-side
exl-id: a0ca014d-ad9f-4ecc-961d-cb7ba236507f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# God praxis

[!DNL Adobe] rekommenderar följande metodtips när du använder [!UICONTROL on-device decisioning]:

## Bästa tillvägagångssätt vid beslut är &quot;på enheten&quot;

När du använder&quot;på enheten&quot; som beslutsmetod hämtas artefakten när besökaren läser in webbsidan för första gången. Alla aktivitetskvalificeringar som behöver göras på första sidan läses in (inget cacheminne) inträffar först när artefakten har hämtats helt. Det finns vissa tips du kan följa för att se till att aktivitetskvalifikationerna sker snabbt för en ny anonym besökare.

* Inaktivera aktiviteter som inte ska ingå i artefakten och som kan användas på en enhet.
* Om du har Target Premium kan du använda [egenskaper/arbetsytor](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html) för att skapa olika artefaktfiler för olika arbetsytor.
* Om dina artefaktfiler blir mycket stora av legitima skäl kan du använda den&quot;hybridmetoden&quot; för beslut. Med den här metoden kan du hämta artefakten parallellt och alla Target API-anrop går över tråden tills artefakten har hämtats. Läs avsnittet om de effektivaste strategierna i hybridläget nedan om du vill veta mer om detta.
* Om du har ett enkelsidigt program (SPA) rekommenderar [!DNL Adobe] att du läser in och initierar at.js innan du läser in programmets JavaScript-huvudfil under första sidinläsningen. Den här metoden initierar artefaktnedladdningen mycket tidigare, vilket ger en snabbare upplevelserendering.

## Bästa tillvägagångssätt vid beslut av metod är &quot;hybrid&quot;

När du använder&quot;hybrid&quot; som beslutsmetod hämtas artefakten parallellt. Tills artefakten har hämtats går alla [!DNL Target] API-anrop igenom tråden även om &quot;platserna&quot; är kompatibla på enheten. Detta beteende är standard för alla `getOffers()`-anrop och ger bästa prestanda i de flesta situationer. Om du ändrar standardbeteendet för `getOffers()` genom att ställa in `decisioningMethod` på `on-device` bör du följa dessa metodtips för att undvika fel och för att säkerställa bästa prestanda.

* Om du bestämmer dig för att anropa `getOffers()` med `decisioningMethod` som `on-device` när sidan läses in för första gången måste du göra det i händelsehanteraren &quot;ARTIFACT_DOWNLOAD_SUCCEEDED&quot; at.js för att undvika fel. Om din artefakt är mycket stor återges alla&quot;platser&quot; som använder den här metoden först när artefakten har hämtats helt, vilket kan fördröja återgivningen. [!DNL Adobe] rekommenderar att du sällan använder den här metoden. Följ de bästa sätten att minska artefaktstorleken i avsnittet&quot;På enhet&quot; ovan när du använder den här metoden.
