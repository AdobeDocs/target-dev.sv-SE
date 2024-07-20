---
keywords: at.js, funktioner, javascript-bibliotek
description: Visa en lista över funktioner som kan användas med 1.x- och 2.x-versionerna av JavaScript-biblioteket at.js i  [!DNL Adobe Target].
title: Vilka funktioner kan jag använda med at.js?
feature: at.js
exl-id: 1efed365-8a74-4c85-bdb1-8daaaf53d642
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 0%

---

# Funktionerna at.js

Lista över funktioner som kan användas med JavaScript-biblioteket [!DNL Adobe Target] at.js. Klicka på länkarna i kolumnen Funktion för mer information och exempel.

| Funktion | Information |
| --- | --- | 
| [[!UICONTROL adobe.target.getOffer(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) | Den här funktionen utlöser en begäran om att få ett [!DNL Target]-erbjudande. Använd med `adobe.target.applyOffer()` om du vill bearbeta svaret eller använda din egen hantering av lyckade åtgärder. |
| [[!UICONTROL adobe.target.getOffers(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)<P>(at.js 2.x) | Med den här funktionen kan du hämta flera erbjudanden genom att skicka in flera rutor. Dessutom kan flera erbjudanden hämtas för alla vyer i aktiva aktiviteter.<P>**Obs!** Den här funktionen introducerades med at.js 2.x. Den här funktionen är inte tillgänglig för at.js version 1.*x*. |
| [[!UICONTROL adobe.target.applyOffer(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md) | Den här funktionen används för att tillämpa svarsinnehållet. |
| [[!UICONTROL adobe.target.applyOffers(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)<P>(at.js 2.x) | Med den här funktionen kan du tillämpa mer än ett erbjudande som har hämtats av [!UICONTROL adobe.target.getOffers()].<P>**Obs!** Den här funktionen introducerades med at.js 2.x. Den här funktionen är inte tillgänglig för at.js version 1.*x*. |
| [[!UICONTROL adobe.target.triggerView (viewName, options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)<P>(at.js 2.x) | Den här funktionen kan anropas när en ny sida läses in eller när en komponent på en sida återges på nytt.<P> Den här funktionen bör implementeras för program med en sida (SPA) för att [!UICONTROL Visual Experience Composer] (VEC) ska kunna användas för att skapa [!UICONTROL A/B Test]- och [!UICONTROL Experience Targeting] (XT)-aktiviteter. |
| [[!UICONTROL adobe.target.trackEvent(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) | Den här funktionen utlöser en begäran om att rapportera användaråtgärder, till exempel klickningar och konverteringar. Den levererar inte någon verksamhet som svar. |
| [[!UICONTROL mboxCreate(mbox,params)]](/help/dev/implement/client-side/atjs/atjs-functions/mboxcreate-atjs.md)<P>(at.js 1.x) | Kör en begäran och tillämpar erbjudandet på närmaste DIV med mboxDefault-klassnamn.<P>**Obs!** Den här funktionen är tillgänglig för at.js version 1.Endast *x*. Den här funktionen har ersatts med versionen av at.js 2.x. Den här funktionen returnerar standardinnehåll om den används med at.js 2.x. |
| [[!UICONTROL mboxDefine(options)] och [!UICONTROL mboxUpdate(options)]](/help/dev/implement/client-side/atjs/atjs-functions/mboxdefine-mboxupdate-atjs-1x.md)<P>(at.js 1.x) | Definiera och uppdatera en mbox.<P>**Obs!** Den här funktionen är tillgänglig för at.js version 1.Endast *x*. Den här funktionen har ersatts med versionen av at.js 2.x. Den här funktionen returnerar standardinnehåll om den används med at.js 2.x. |
| [[!UICONTROL targetGlobalSettings(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) | Du kan åsidosätta inställningarna i at.js-biblioteket med `[!UICONTROL targetGlobalSettings()]` i stället för att konfigurera dem i [!DNL Target Standard/Premium]-gränssnittet eller med REST API:er.<ul><li>Dataleverantörer: Med den här inställningen kan kunder samla in data från tredjepartsleverantörer av data, som Demandbase, BlueKai och anpassade tjänster, och skicka data till Target som mbox-parametrar i den globala mbox-begäran.</li></ul> |
| [[!UICONTROL targetPageParams(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) | Med den här metoden kan du bifoga parametrar till den globala mbox utanför begärandekoden. |
| [[!UICONTROL targetPageParamsAll(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparamsall.md) | Med den här metoden kan du bifoga parametrar till alla rutor utanför begärandekoden. |
| [[!UICONTROL registerExtension(options)]](/help/dev/implement/client-side/atjs/atjs-functions/registerextension-atjs-1x.md)<P>(at.js 1.x) | Tillhandahåller ett standardsätt att registrera ett specifikt tillägg.<P>**Obs!** Den här funktionen är tillgänglig för at.js version 1.Endast *x*. Den här funktionen har ersatts med versionen av at.js 2.x. Den här funktionen returnerar standardinnehåll om den används med at.js 2.x. |
| [[!UICONTROL at.js custom events]](/help/dev/implement/client-side/atjs/atjs-functions/atjs-custom-events.md) | anpassade at.js-händelser talar om när en mbox-begäran eller ett erbjudande misslyckas eller lyckas. |
| [[!UICONTROL adobe.target.sendNotifications(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md)<P>(at.js 2.1.0) | Den här funktionen skickar ett meddelande till [!DNL Target] när en upplevelse återges utan att använda `[!UICONTROL adobe.target.applyOffer()]` eller `[!UICONTROL adobe.target.applyOffers()]`.<P>**Obs!** Den här funktionen har introducerats i at.js 2.1.0 och är tillgänglig för alla versioner över 2.1.0. |
