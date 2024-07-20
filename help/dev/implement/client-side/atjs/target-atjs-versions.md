---
keywords: at.js releases, at.js versions, release notes
description: Visa information om ändringarna i varje version av JavaScript-biblioteket  [!DNL Adobe Target]  at.js.
title: Vad ingår i varje version av at.js?
feature: at.js
exl-id: 609dacba-2ab8-45e9-b189-928d59938c98
source-git-commit: 9999c1b5f603e6607bd81f6ad6a06a7f74e76acb
workflow-type: tm+mt
source-wordcount: '4904'
ht-degree: 0%

---

# versionsinformation för at.js

Information om ändringar i varje version av JavaScript-biblioteket [!DNL Adobe Target] at.js.

>[!IMPORTANT]
>
>[!DNL Adobe Target] stöder både at.js 1.*x* och at.js 2.*x*.
>
>at.js 1.*x* har försatts i underhållsläge. [!DNL Target]-teamet släpper felkorrigeringar och säkerhetsuppdateringar när det behövs.
>
>[!DNL Target]-teamet har fullt stöd för at.js 2.*x* och fortlöpande släpper felkorrigeringar, säkerhetsuppdateringar, funktioner och prestandaoptimering.
>
>Du bör uppgradera till de senaste versionerna av antingen 1.*x* eller 2.*x* om du vill få buggfixar och säkerhetsuppdateringar för problem som upptäckts i en tidigare mindre version av motsvarande större version.

Taggar i [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) är den metod du föredrar att uppgradera at.js. Tilläggsutvecklare lägger ständigt till nya funktioner i sina tillägg och åtgärdar ofta fel. Dessa uppdateringar paketeras i nya versioner av ett tillägg och görs tillgängliga i Adobe Experience Platform-katalogen som uppgraderingar. Mer information finns i [Tilläggsuppgraderingar](https://experienceleague.adobe.com/docs/experience-platform/tags/ui/extensions/extension-upgrade.html) i *Översikt över taggar*.6+

## at.js version 2.11.4 (24 januari 2024)

* Uppdaterat at.js för att förhindra att ogiltiga geodata skickas till leverans-API:t.

## at.js version 2.11.3 (21 november 2023)

* Korrigerade ett problem som förhindrade att svarstoken skickades för `at-content-rendering-failed` händelser.

## at.js version 2.11.2 (26 oktober 2023)

* Korrigerade ett problem som orsakade inkonsekvenser i svarstoken som skickades för anpassade händelser.

## at.js version 2.11.1 (13 oktober 2023)

* Ett problem som orsakade ohanterade fel när en sida som kör at.js är i felsäkert läge har åtgärdats.

## at.js version 2.11.0 (10 oktober 2023)

* Stöd har lagts till för inställning av anpassad [!DNL Adobe Experience Platform] (AEP) `sandboxId` och `sandboxName` i `targetGlobalSettings`, som skickas till leverans-API för `getOffer/getOffers`-anrop.
* Korrigering av skugg-DOM för kedja `:eq()` i väljare.

## at.js version 2.10.3 (12 september 2023)

* Korrigerade ett problem som felaktigt utlöste den anpassade `at-content-rendering-succeeded`-händelsen när inga erbjudanden återges. Den rätta händelsen, `at-content-rendering-no-offers`, har utlösts.
* `eventToken` och `responseTokens` har lagts till i felobjektet för den anpassade händelsen `at-content-rendering-failed`.

## at.js version 2.10.2 (7 mars 2023)

* Korrigerade ett problem som gjorde att funktionen `trackEvent` alltid returnerade ett fel.

## at.js version 2.10.1 (2 februari 2023)

* Korrigerade ett fel där aktiviteter som innehåller målgruppsregler som innehåller parametrar med punkter i namnen inte returnerade den förväntade upplevelsen för enhetsbeslut.
* Korrigerade ett fel som introducerades i at.js 2.6.0 där at.js utlöste ett leveransanrop, även när mboxDisable aktiverades.

## at.js version 2.10.0 (19 september 2022)

* Stöd för cookies från tredje part har lagts till.

## at.js version 2.9.0 (27 maj 2022)

* [Stöd för klienttips för användaragent](user-agent-and-client-hints.md) har lagts till.
* Korrigerade ett fel där flera mbox-förfrågningar på samma sida har olika ID:n för intrycket.

## at.js version 2.8.1 (28 januari 2022)

* Korrigerade att `pageLoad` inte mappas till måloblobal-mbox i hybridkörningsläget On Device Decisioning (ODD).
* Ett problem med analysinformation för mbox-begäran har korrigerats.
* Uppgraderade utvecklingsberoenden för att åtgärda säkerhetsproblem.

## at.js version 2.8.0 (7 januari 2022)

JavaScript-biblioteket [!DNL Target] at.js samlar nu in telemetridata för funktionsanvändning och prestanda. Personuppgifter samlas inte in. Avanmäl dig för den här funktionen genom att ange `telemetryEnabled` till false i `targetGlobalSettings`. Mer information finns i [telemetryEnabled i targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled).

## at.js version 2.7.0 (28 oktober 2021)

Den här versionen innehåller följande förbättringar:

* Stöd för [webbkomponenter](https://developer.mozilla.org/en-US/docs/Web/Web_Components) har lagts till. Den här versionen av at.js krävs för att skapa och testa personaliserade upplevelser och erbjudanden på anpassade element och på element inuti anpassade element. Den här funktionen ingår i [!DNL Target Standard/Premium] 21.10.5-versionen.

## kl. 1.8.3 (21 september 2021)

Den här versionen innehåller följande ändringar:

* Modulerna `reactor-window` och `reactor-document` Adobe Experience Platform Launch har tagits bort för att säkerställa att Platforma launchen fungerar korrekt för kunder som har `window.default` eller `document-default` angivna.
* at.js 1.8.3 anger nu explicit `Samesite=None` och `Secure` för att säkerställa att tredjepartsdomäncookies anges korrekt.

## kl. 2.6.1 (16 augusti 2021)

* Felkorrigering för &quot;Ingen cachelagrad artefakt tillgänglig för hybridläge&quot; vid användning av enhetsbeslut.

## at.js 2.6.0 (16 juli 2021)

* Ett säkert attribut har lagts till i cookies när at.js-inställningarna `secureOnly` är inställda på `true`.
* Svarstoken är nu tillgängliga när `triggerView()` används.
* Ett problem som rör händelsen `CONTENT_RENDERING_NO_OFFERS` har korrigerats. Den här händelsen utlöses korrekt när inget innehåll returneras från [!DNL Target].
* [!UICONTROL Analytics for Target] (A4T) klickmätningsinformation returneras korrekt när `prefetch`-begäranden används.
* UUID-generering använder inte längre `Math.random()`, utan är beroende av `window.crypto`.
* Utgångsdatumet för cookien `sessionId` har utökats korrekt för varje nätverksanrop.
* Cacheinitieringen för SPA-vyn (Single Page Application) hanteras nu korrekt och inställningarna för `viewsEnabled` följs. Om du anger `viewsEnabled` till värdet `false` inaktiveras funktionen `triggerView()`. Se [Åtgärdsordning för inledande sidinläsning](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md#order).

## at.js 2.5.0 (13 maj 2021)

Den här versionen av at.js innehåller följande förbättringar och ändringar:

* [Enhetsbeslut](/help/dev/implement/client-side/atjs/on-device-decisioning/on-device-decisioning.md) har stöd för at.js.
* [Förhandsgranska länkar](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) - stöd för Automated Personalization-aktiviteter

Den här versionen tar också bort stöd för Microsoft Internet Explorer 10 och senare versioner.

## kl. 2.4.1 (23 mars 2021)

Den här versionen av at.js är en underhållsrelease och innehåller följande förbättringar och korrigeringar:

* Ett problem med att `targetPageParams` togs med i mbox-begäranden har korrigerats. `targetPageParams` ska endast inkluderas i `pageLoad`-begäranden. (TNT-40247)
* Optimerade fönster- och dokumentgallerier som refererar i Adobe Experience Platform-tillägget. (TNT-37124)

## at.js 2.4.0 (14 januari 2021)

Den här versionen av at.js är en underhållsrelease och innehåller följande korrigeringar:

* Lägger till stöd för ett enhetligt profil-/plattforms-ID i Delivery API customerIds.
* Korrigerar ogiltig stiltaggsinmatning.

## kl. 2.3.3 (13 november 2020)

Den här versionen av at.js är en underhållsversion och innehåller följande korrigering:

* Korrigerade ett problem relaterat till lådklicksspårning och A4T. Med 0n-klick utlöste [!DNL Target] ett leverans-API-anrop med rätt mbox- och mbox-parametrar. SDID matchade emellertid inte det i [!DNL Analytics]-anropet, och därför fanns det ingen träffsammanfogning och konvertering. (TNT-38372)

## .js 2.3.2 (24 juli 2020)

Den här versionen av at.js är en underhållsversion och innehåller följande korrigering:

* Korrigerade ett fel när ett skript eller en kod lägger till en standardegenskap i fönstret eller dokumentet.

## kl. 1.8.2 (15 juni 2020)

Den här versionen av at.js är en underhållsversion och innehåller följande korrigering:

* Korrigerade ett problem vid användning av CNAME och kantåsidosättning, at.js 1.*x* kan skapa serverdomänen felaktigt, vilket resulterade i att [!DNL Target]-begäran misslyckades. (TNT-35064)

## kl. 2.3.1 (15 juni 2020)

Den här versionen av at.js är en underhållsrelease och innehåller följande förbättringar och korrigeringar:

* Inställningen `deviceIdLifetime` kunde åsidosättas via [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md). (TNT-36349)
* Korrigerade ett problem vid användning av CNAME och kantåsidosättning, at.js 2.*x* kan skapa serverdomänen felaktigt, vilket resulterade i att [!DNL Target]-begäran misslyckades. (TNT-35065)
* Korrigerade ett problem när tillägget [!DNL Target] v2 och tillägget [!UICONTROL Adobe Analytics Launch] användes, [!DNL Target] fördröjde anropet till [!DNL Analytics] `sendBeacon`. (TNT-36407, TNT-35990, TNT-36000)

## at.js version 2.3.0 (25 mars 2020)

Den här versionen av at.js är en underhållsrelease och innehåller följande förbättringar och korrigeringar:

* Stöd för inställning av innehållets säkerhetsprincip-noder för SCRIPT- och STYLE-taggar som läggs till på sidans DOM när levererade [!DNL Target] erbjudanden tillämpas. Kunder kan ange `targetGlobalSettings.cspScriptNonce` och `targetGlobalSettings.cspStyleNonce` så att at.js kan ställa in motsvarande skript- och stiltaggsnoder för tillämpade erbjudanden. Mer information finns i [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).
* Ett problem har korrigerats vid kompilering av at.js med Google Closure-kompilatorn för Google Tag Manager-distribution.
* Namnet på checkcookien at.js ändrades från `check` till `at_check` för att undvika kollisioner med kundernas implementeringar.

## at.js version 1.8.1 (25 mars 2020)

Den här versionen av at.js är en underhållsrelease och innehåller följande förbättringar och korrigeringar:

* Namnet på checkcookien at.js ändrades från `check` till `at_check` för att undvika kollisioner med kundernas implementeringar.

## at.js version 2.2.0 (10 oktober 2019)

Den här versionen av at.js innehåller följande förbättringar och korrigeringar:

* Korrigerade ett problem där klickspårning inte rapporterade konverteringar i [!DNL Analytics for Target] (A4T) när [!DNL Adobe Analytics] kod inte fanns på sidelementen.
* Förbättrade prestanda när du använder både Experience Cloud ID Service (ECID) v4.4 och at.js 2.2 på dina webbsidor.
* Tidigare gjorde ECID två blockerande anrop innan at.js kunde hämta upplevelser. Detta har reducerats till ett enda samtal, vilket avsevärt förbättrar prestandan.
* Korrigerade felaktig förhämtad vybearbetning, där händelsetoken från standarderbjudanden inte inkluderades i skickade meddelanden.

>[!NOTE]
>
>Uppgradera ditt ECID-tillägg till v4.4 för att utnyttja prestandaförbättringarna.

* at.js version 2.2 innehåller även en ny inställning med namnet `serverState`. Den här inställningen kan användas för att optimera sidprestanda när en hybridintegrering av [!DNL Target] implementeras. Hybrid-integrering innebär att du använder både at.js v2.2+ på klientsidan och Delivery API eller en [!DNL Target] SDK på serversidan för att leverera upplevelser. `serverState` ger at.js v2.2+ möjlighet att tillämpa upplevelser direkt från innehåll som hämtas på serversidan och returneras till klienten som en del av sidan som skickas. Mer information finns i &quot;serverState&quot; i [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverstate).

## at.js version 1.8.0 (10 oktober 2019)

Den här versionen av at.js innehåller följande förbättringar och korrigeringar:

* Förbättrade prestanda när du använder både Experience Cloud ID Service (ECID) v4.4 och at.js 1.8 på dina webbsidor.
* Tidigare gjorde ECID två blockerande anrop innan at.js kunde hämta upplevelser. Detta har reducerats till ett enda samtal, vilket avsevärt förbättrar prestandan.

>[!NOTE]
>
>Uppgradera ditt ECID-tillägg till v4.4 för att utnyttja prestandaförbättringarna.

## at.js version 2.1.1 (24 juli 2019)

Den här versionen av at.js är en underhållsrelease och innehåller följande förbättringar och korrigeringar:

(Numren inom parentes är avsedda för Adobe.)

* Korrigerade ett problem som gjorde att flera fyrar stacks när användaren använde metoden för klickspårning på sidan Mål och inställningar i Visual Experience Composer (VEC). (TNT-32812)
* Korrigerade ett problem som gjorde att `triggerView()` inte återgav erbjudanden mer än en gång. (TNT-32780)
* Ett problem med `triggerView()` har korrigerats för att kontrollera att begäran innehåller Marketing Cloud ID-information (MCID). (TNT-32776)
* Korrigerade ett problem som förhindrade att meddelandet `triggerView()` utlöstes även om det inte finns några sparade vyer. (TNT-32614)
* Korrigerade ett problem som orsakade ett fel på grund av användningen av decodeURIcomponent som orsakade problem när URL:en innehåller en felformaterad frågesträngsparameter. (TNT-32710)
* Beacon-flaggan är nu inställd på true i samband med leveransbegäranden som skickas via API:t `Navigator.sendBeacon()`. (TNT-32683)
* Korrigerade ett problem som hindrade Recommendations erbjudanden från att visas på webbplatser för ett fåtal kunder. Kunderna kunde se erbjudandeinnehållet i API-anropet, men erbjudandet tillämpades inte på webbplatsen. (TNT-32680)
* Korrigerade ett problem som gjorde att klickspårning över flera upplevelser inte fungerade som förväntat. (TNT-32644)
* Korrigerade ett problem som förhindrade at at at.js från att använda det andra måttet efter att återgivningen av det första måttet misslyckades. (TNT-32628)
* Korrigerade ett problem när `mbox3rdPartyId` skickades med funktionen `targetPageParams` som gjorde att nyttolasten för begäran inte fanns i frågeparametrarna eller i nyttolasten för begäran. (TNT-32613)
* Korrigerade ett problem som gjorde att visnings- och klickmeddelanderesvar blockerades i Chromium-baserade webbläsare (inklusive Google Chrome). (TNT-32290)

## at.js version 2.1.0 (3 juni 2019)

Den här versionen innehåller följande funktioner och förbättringar:

* **Stöd för deltagande i Adobe**: Adobe Opt-In är ett sätt att förenkla integreringen av Adobe-lösningar med plattformar för samtyckeshantering. Mer information om Adobe-deltagande finns i [Sekretess och allmänna dataskyddsförordningen (GDPR)](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md).

* **CSP-kompatibel enligt branschstandard**: at.js använder inte längre eval() för att köra JavaScript.

* **Loggning av analysdata på klientsidan**: Ge kunderna full kontroll över hur de vill skicka analysdata till [!DNL Adobe Analytics], oavsett om det gäller klient- eller serversidan.

  Mer information finns i [Logga [!DNL Analytics] på klientsidan](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/before-implement.html#client-side).

* **Skicka meddelanden**: Tillåt utvecklare att skicka meddelanden när en upplevelse återges av koden i stället för att använda `applyOffer()` eller `applyOffers()`.

  Mer information finns i [adobe.target.sendNotifications(options)](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md).

* **at.js-storleken reducerad med ~24%**: storleken på at.js reduceras med ~24%. Den mindre filstorleken förbättrar sidans inläsningsprestanda och minskar tiden för att ladda ned at.js på sidan.

## at.js version 2.0.1 (19 mars 2019)

Det här är en underhållsrelease och innehåller följande förbättringar och korrigeringar:

(Numren inom parentes är avsedda för Adobe.)

* Korrigerade ett konkurrensvillkor i DOM-avsökningskoden som orsakade JavaScript-undantag för vissa kunder. (TNT-31869)
* Meddelanden om att vyer har renderats har kopplats bort från händelsehanterare för klickspårning. Till att börja med skickade [!DNL Target] inga meddelanden om klickhändelsehanterare som tillhör en återgiven vy inte kunde bifogas. [!DNL Target] skickar nu ett vymeddelande även när klickelement inte hittas. (TNT-31969)
* Korrigerade ett problem som gjorde att omdirigeringsflaggan för händelsen som slutfördes alltid hade angetts till true. (TNT-31907)
* Korrigerade ett problem som gjorde att VEC-ändringsåtgärden loggades som lyckad, även när element saknades. (TNT-31924)
* Korrigerade ett problem som gjorde att meddelanden till vissa kunder inte innehöll egenskapstoken för företagsbehörigheter. (TNT-31999)

## at.js version 1.7.1 (19 mars 2019)

Det här är en underhållsrelease och innehåller följande korrigering:

(Numren inom parentes är avsedda för Adobe.)

* Korrigerade ett konkurrensvillkor i DOM-avsökningskoden som orsakade JavaScript-undantag för vissa kunder. (TNT-31869)

## at.js Version 2.0.0

at.js 2.x innehåller funktionsrika uppsättningar som gör det möjligt för företaget att utföra personalisering på nästa generations klienttekniker. Den nya versionen fokuserar på att uppgradera at.js för att få harmonisk interaktion med applikationer för en sida (SPA).

Här är några fördelar med att använda at.js 2.x som inte finns i tidigare versioner:

* Möjligheten att cachelagra alla erbjudanden på sidan för att minska antalet serveranrop till ett enda serveranrop.
* Förbättra slutanvändarnas upplevelser enormt på er webbplats, eftersom erbjudandena visas direkt via cachen utan den fördröjning som traditionella serversamtal ger.
* Enkel kodrad och engångsinstallation av utvecklare så att era marknadsförare kan skapa och köra A/B- och Experience-aktiviteter (XT) via Visual Experience Composer (VEC) i era single page-applikationer.

at.js 2.x innehåller följande nya funktioner:

* getOffers()
* applyOffers()
* triggerView()

Följande funktioner har tagits bort i och med introduktionen av at.js 2.x:

* mboxCreate()
* mboxDefine
* registerExtension()

Mer information finns i [Uppgradera från at.js 1.x till at.js 2.x](/help/dev/implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md) och [at.js-funktioner](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md).

>[!NOTE]
>
>Om du behöver stöd för Adobe Opt-in för [General Data Protection Regulation](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) (GDPR) måste du för närvarande använda at.js 1.7.0 eller at.js 2.1.0 eller senare.

## at.js Version 1.7.0

at.js 1.7.0 har stöd för Adobe Opt-In. Adobe Opt-In är ett sätt att förenkla integreringen av Adobe-lösningar med plattformar för samtyckeshantering.

Mer information om Adobe-deltagande finns i [Sekretess och allmänna dataskyddsförordningen](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) (GDPR).

Den här versionen åtgärdar också ett problem där [!DNL Target] kan åsidosätta omdirigerings-URL-parametrar med parametrar som kommer från omdirigerings-URL:en.

>[!NOTE]
>
>Om du behöver stöd för Adobe-deltagande för GDPR måste du för närvarande använda at.js 1.7.0 eller at.js 2.1.0 eller senare.

## at.js Version 1.6.4

at.js 1.6.4 är en underhållsrelease som åtgärdar följande problem:

* Korrigerade ett tävlingsvillkorsmanifest i Microsoft Internet Explorer 11 som gjorde att dubblerade erbjudanden tillämpades.

## at.js Version 1.6.3

at.js version 1.6.3 innehåller följande korrigeringar och förbättringar:

* Väljarna är nu CSS-escape-konverterade om de innehåller ID:n eller CSS-klasser som börjar med en siffra, två bindestreck eller ett bindestreck följt av en siffra (till exempel #-123). (TNT-31061)
* Korrigerade ett fel som introducerades i punkt 1.6.2 där VEC (Visual Experience Composer) erbjuder från olika aktiviteter som gäller för samma CSS-väljare inte respekterade aktivitetsprioriteten. (TNT-31052)
* Korrigerade ett problem med timing out a promise i miljöer där det inte fanns något inbyggt stöd för löften. (TNT-30974)
* Problem registreras nu korrekt och rapporteras via händelsen misslyckad innehållsåtergivning. Tidigare kunde man ha rapporterat att JavaScript fungerade som det skulle, även om så inte var fallet. (TNT-30599)

## at.js Version 1.6.2

Detta är en underhållsrelease som åtgärdar följande problem:

* Korrigerade ett problem som på vissa kundsajter ledde till en oändlig asynkron slinga.

>[!WARNING]
>
>Dessutom innehåller version 1.6.2 av at.js även alla förbättringar och korrigeringar som ingår i version 1.6.1 och 1.6.0 av at.js. Dessa versioner är inte längre tillgängliga för hämtning. Du bör uppgradera till version 1.6.2 om du använder 1.6.1 eller 1.6.0

Här är förbättringarna och korrigeringarna som ingick i at.js version 1.6.1:

* Korrigerade ett fel i at.js 1.6.0 som gjorde att rekommendationsupplevelserna duplicerades i Microsoft Internet Explorer 11. (TNT-30593)
* at.js ser nu till att logiken för att åsidosätta kanter kontrollerar om det finns en edge-klustercookie, så att ett annat kantnummer undviks om användaren trycker på kanterna under en session. (TNT-30563)
* Korrigerade ett problem som förhindrade at at.js från att utföra efterföljande åtgärder om HTML-innehållet innehöll ogiltig JS-kod. at.js loggar nu felet och återger de återstående åtgärderna utan problem. (TNT-30546)
* Ändras så att det finns ett undantag när en omdirigeringssida kvalificerar sig för en omdirigeringsaktivitet. (TNT-30532)
* Korrigerade ett problem som förhindrade att rätt tidsgräns för begäran spreds från API-begäran getOffer(). (TNT-30498)
* Korrigerade ett fel som förhindrade att cookies sparades i .js 1.6.0 när filprotokollet användes. (TNT-30454)
* Korrigerade ett problem som fick det att verka som om inte alla upplevelser levererades med omdirigeringar när [!DNL Analytics for Target] (A4T) användes. (TNT-30444)
* Korrigerade ett problem som gjorde att sidan doldes efter att [!DNL Target]-anropet lyckades. (TNT-30358)

Här är förbättringarna och korrigeringarna som ingår i at.js version 1.6.0:

* Omdirigeringserbjudanden stöds nu automatiskt i integreringen av [!UICONTROL Analytics for Target] (A4T). Klientsidans lösning har tagits bort. (TNT-30247)
* Klientsidans kantroutning är nu aktiverat som standard. (TNT-30261)
* Korrigerade ett problem med VEC-åtgärdsåtergivning (Visual Experience Composer) när det finns beroenden mellan åtgärder. (TNT-30248)

## at.js Version 1.5.0

at.js version 1.5.0 finns nu att köpa.

* Information om händelsen `at-request-succeeded` innehåller omdirigeringsflaggan. Den här flaggan kan användas för att avgöra om sidan kommer att omdirigeras till en annan URL. Om du vill veta URL:en prenumererar du på `at-content-rendering-redirect`. (TNT-29834)
* Korrigerade ett problem som gjorde att `window.targetGlobalSettings.enabled` misslyckades med ett körningsundantag om värdet var false. (TNT-29829)
* Korrigerade ett problem som gjorde att sidan inte kunde läsas in i Visual Experience Composer (VEC) om anpassad kod användes i en global mbox-begäran och brödtext gömdes. (TNT-29795)
* Stöd för `screenOrientation`, `devicePixelRatio` och `webGLRenderer` har lagts till. Dessa nya [!DNL Target]-begärandeparametrar används för iPhone X och annan modern enhetsidentifiering. Mer information finns i [Mobile](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html). (TNT-29781)
* Ett problem har korrigerats där platstipset för Adobe Audience Manager (AAM) inte alltid skickades. (TNT-29695)
* För webbläsare som har stöd för det växlar at.js 1.5.0 till MutationObserver för avsökning av väljare. Versioner före at.js 1.0.0 använde en MutationObserver-polyfill, som visade sig vara problematisk. För att undvika problem med polyfyllning använder version 1.5.0 följande pseudokod för att avgöra vilken schemaläggningsmekanism som ska användas:

  ```
  if MutationObserver is supported
    scheduler = MutationObserver
  else if document is visible
    scheduler = requestAnimationFrame
  else
    scheduler = setTimeout
  ```

## at.js Version 1.3.0

at.js version 1.3.0 finns nu att köpa.

* Följande nya händelser är tillgängliga för att hjälpa dig att spåra, felsöka och anpassa interaktion med at.js:

   * LIBRARY_LOADED
   * REQUEST_START
   * CONTENT_RENDERING_START
   * CONTENT_RENDERING_NO_OFFERS
   * CONTENT_RENDERING_REDIRECT

  Mer information finns i [at.js anpassade händelser](/help/dev/implement/client-side/atjs/atjs-functions/atjs-custom-events.md).

* Du kan utöka en at.js-begäran med ytterligare parametrar från dataleverantörer. Dataleverantörer bör läggas till i `window.targetGlobalSettings` under `dataProviders key`.

  Mer information finns i [Dataproviders](atjs-functions/targetglobalsettings.md#data-providers).

* at.js-begäranden använder nu GET, men kommer att växla till POST när URL-storleken överstiger 2 048 tecken. Det finns en ny egenskap med namnet `urlSizeLimit` där du kan öka storleksgränsen om det behövs. Med den här ändringen kan [!DNL Target] justera at.js mot AppMeasurementet, som använder samma teknik.
* [!DNL Target] tvingar nu `mbox`-nyckeln i funktionen `adobe.target.applyOffer(options)` att användas. Den här nyckeln har krävts tidigare, men [!DNL Target] använder den nu för att säkerställa att [!DNL Target] har korrekt validering och att kunderna använder funktionen korrekt.
* at.js har förbättrat funktionerna för händelsespårning och klickning. at.js använder `navigator.sendBeacon()` för att skicka händelsespårningsdata och kommer att återgå till synkron XHR när `navigator.sendBeacon()` inte stöds. Detta gäller oftast Internet Explorer 10 och 11 samt vissa versioner av Safari. Safari kommer att lägga till stöd för `navigator.sendBeacon()` i den kommande iOS 11.3-versionen.
* at.js kan nu återge erbjudanden även när en sida öppnas i bakgrundsflikar. Vissa [!DNL Target]-kunder stötte på ett problem när `requestAnimationFrame()` inaktiverades på grund av webbläsarbegränsningsbeteendet för bakgrundsflikar.
* Den här versionen innehåller många prestandaförbättringar, bland annat kortare anropsstackar vid kontroll av en Chrome CPU-profil.
* at.js 1.3.0 stöder inte längre innehållsleverans i Microsoft Internet Explorer 9. En lista över webbläsare som stöds finns i [Webbläsare som stöds](/help/dev/before-implement/supported-browsers.md). Framöver utförs alla begäranden via `XMLHttpRequest` med CORS-stöd utan JSONP-begäranden. Den här förändringen förbättrar säkerheten avsevärt.

## at.js Version 1.2.3

at.js version 1.2.3 är nu tillgänglig.

* Lägger till stöd för JSON-erbjudanden. JSON-erbjudanden stöds endast i aktiviteter som skapats med den formulärbaserade Experience Composer. För närvarande är det enda sättet att använda JSON-erbjudanden via direkta API-anrop. Se [Skapa JSON-erbjudanden](https://experienceleague.adobe.com/docs/target/using/experiences/offers/create-json-offer.html).

## at.js Version 1.2.2

at.js version 1.2.2 finns nu att köpa.

* Ett problem som returnerade ett JavaScript-fel när biblioteket [!DNL Target] lästes in på en sida i QUIRKS-läge har åtgärdats. (TNT-28312)
* Korrigerade ett problem som gjorde att [!DNL Target] klickningsspårning bröt [!DNL Analytics] datainsamlingsanrop. (TNT-28261)
* Korrigerade ett problem som gjorde att `getOffer() params` misslyckades när `targetPageParams()` returnerade en tom sträng. (TNT-28359)
* Ett problem med generering av sessions-ID har korrigerats när endast x användes. (TNT-28361)

## at.js Version 1.2.1

at.js version 1.2.1 finns nu att köpa.

* Ett problem som orsakade att [!DNL Target] inte kunde öppna länken på en ny flik korrigerades när användaren klickade på spårning på en länk med target=&quot;_blank&quot;.

## at.js Version 1.2.0

at.js version 1.2 finns nu som en underhållsrelease som innehåller de flesta felkorrigeringar.

* Korrigerade ett problem som förhindrade standardåtgärder för specialfall för klickspårning. (TNT-28089)
* Korrigerade ett problem när klickspårning på en länk med `target="_blank"` som förhindrade [!DNL Target] från att öppna länken på en ny flik. (TNT-28072)
* IP-adresser kan användas som cookie-domän. (TNT-28002)
* Korrigerade ett problem som orsakade flimmer i omdirigeringserbjudanden med en global mbox eller andra regionala mbox. (TNT-27978)
* Korrigerade ett problem i [!UICONTROL Experience Targeting] aktivitetskonfiguration som misslyckades i VEC vid växling mellan Browse och Compose. (TNT-27942)
* Felaktig hantering av flimmerstilklasser för klickspårselement har korrigerats. (TNT-27896)
* Korrigerade ett problem som gjorde att globala mbox-parametrar blandades ihop med alla mbox-parametrar. (TNT-27846)
* Ändringarna görs för att säkerställa att hanterarfält, Mustache och andra mallbibliotek på klientsidan hanteras korrekt av at.js. (TNT-27831)
* Ändringarna har gjorts för att se till att `sdidParamExpiry` initieras korrekt och skickas till besökar-API:t. Det här är en regression som har lagts till i `at.js 1.1.0`. Tidigare at.js-versioner påverkas inte. Detta påverkar bara klienter som använder omdirigeringserbjudanden och A4T. (TNT-27791)
* Utför ändringar för att säkerställa att `SCRIPT` körs oavsett vilket typattribut som används. (TNT-27865)

## at.js Version 1.1.0

**Datum:** 2 augusti 2017

Följande förbättringar och korrigeringar finns i version 1.1 av at.js:

* Förbättrad hantering av svarstoken. Mer information finns i [Svarstoken](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html).
* Problemet har lösts så att `document.currentScript polyfill` inte stör Angular 1.X.
* Ändringarna görs för att säkerställa att klickspårning inte stör synlighetsegenskapen. Klickspårningselement markeras med CSS-klassen `at-element-click-tracking` i stället för `at-element-marker`.

## at.js Version 1.0.0

**Datum:** 7 juli 2017

Följande förbättringar och korrigeringar ingår i .js version 1.0:

* Stöd för asynkron inläsning av at.js för snabbare sidinläsning.
* Stöd för att fördölja sidinnehåll när at.js läses in asynkront.
* Bättre felmeddelanden när innehållsleverans är inaktiverad.
* Prestandaförbättringar när du levererar flera aktiviteter.
* Stöd för YUI-kompressor.
* Fel-/felrapportering för anpassade händelser under aktivitetsleverans.
* Åtgärda prestandaproblem i Microsoft Internet Explorer 11.
* Korrigera funktionen `getOffer()` som genererar ett fel på vissa webbplatser.
* Läs in biblioteket [!DNL Target] asynkront. Mer information finns i [at.js Vanliga frågor ](/help/dev/implement/client-side/atjs/target-atjs-faq.md).

## at.js Version 0.9.7

**Datum:** 22 maj 2017

Följande förbättringar och korrigeringar finns i at.js version 0.9.7:

* Korrigerade ett problem relaterat till en resursnyckel som saknades i `insertAfter` och `insertBefore` åtgärder i Visual Experience Composer (VEC). Dessa problem gällde migreringen från visuella erbjudanden till mallar.

## at.js Version 0.9.6

**Datum:** 13 april 2017

Följande förbättringar och korrigeringar finns i at.js version 0.9.6:

* Stöd för omdirigeringserbjudande för A4T. När du har hämtat och installerat på .js version 0.9.6 kan du använda omdirigeringserbjudanden i aktiviteter som använder [!UICONTROL Adobe Analytics as the Reporting Source for Target] (A4T). Förutom at.js version 0.9.6 finns det andra minimikrav som din implementering måste uppfylla för att kunna använda omdirigeringserbjudanden och A4T. Mer information och ytterligare viktig information som du bör känna till finns i [Omdirigeringserbjudanden - A4T - frågor och svar](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html).
* Före kl. 0.9.6, när besökar-API:t fanns på sidan och inställningen `visitorApiTimeout` var för aggressiv, kunde [!DNL Target] hamna i en situation när inga MCID-data skickades i [!DNL Target]-begäran. Detta kan leda till problem som osydda träffar i [!DNL Analytics] när A4T används.

  Detta beteende har ändrats i at.js 0.9.6, även om `visitorApiTimeout` är inställt på 1 ms, försöker [!DNL Target] samla in SDID-data, spårningsservrar och kund-ID:n och skicka dessa i [!DNL Target]-begäran.

* Inställningen `selectorsPollingTimeout` har lagts till. Mer information finns i [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).
* Svarets format från `getOffer()` har ändrats. Mer information finns i [adobe.target.getOffer(options)](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md).
* Konsolloggning har lagts till för `<!DOCTYPE>`-deklarationer som inte stöds.
* Korrigerade ett problem där [!DNL Target Classic] plugin-program inte tillämpades korrekt när flera standarderbjudanden levererades till en enda mbox. (TGT-22664)
* Förbättrad cookie-inställning för två TLD-domäner (top-level-domains) för två bokstäver för att säkerställa att mbox-cookien är korrekt inställd för dessa domäner (till exempel test.no, autodrive.ca och så vidare).
* Algoritmen för extrahering av toppnivådomänen som ska användas när cookies sparas har ändrats i at.js version 0.9.6. På grund av den här ändringen kan cookies inte sparas i adresser som använder IP. För det mesta används IP-adresser i testsyfte, men som tillfälliga lösningar kan du använda DNS-poster eller justera värdfilen i en lokal ruta.
* Åtgärdade flytt- och omarrangeringsåtgärder när egenskaper är strängvärden i stället för heltal.

## at.js Version 0.9.4

**Datum:** 19 januari 2017

* Nu kan mbox-namn innehålla specialtecken, inklusive et-tecken ( &amp; ).

  En lista med tillåtna specialtecken finns i [at.js Configuration](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md).

* Inställningen `secureOnly` har lagts till som anger om at.js ska använda enbart HTTPS eller tillåtas växla mellan HTTP och HTTPS baserat på sidprotokollet. Det här är en avancerad inställning som har standardvärdet False och kan åsidosättas via `targetGlobalSettings`.
* Alternativet Stöd för äldre webbläsare finns i version 0.9.3 och tidigare av at.js. Det här alternativet togs bort i at.js version 0.9.4.

## at.js Version 0.9.3

**Datum:** 10 oktober 2016

* Ser till att mbox-anrop utlöses i Microsoft Internet Explorer 11 när äldre webbläsare är inaktiverade i at.js-inställningarna.
* Ser till att standardinnehåll återges om ett dynamiskt fjärrerbjudande misslyckas (till exempel om URL:en är felaktig och returnerar ett 404-fel).
* Ser till att element snabbt visas när VEC-klickningsspårningsväljare inte kan hittas i DOM.

## at.js Version 0.9.2

**Datum:** 21 september 2016

* En `optoutEnabled`-inställning har lagts till för att aktivera eller inaktivera avanmälan av enhetsdiagram. Om den här inställningen är `true` och besökaren har valt bort spårning, kommer besökarens webbläsare inte att göra några mbox-anrop. Device Graph finns för närvarande i Beta. Den här inställningen är inställd på `false` som standard, men måste anges till `true` om du använder Device Graph.
* `CustomEvent` stöd för meddelandefunktionen har lagts till. Tidigare kunde inte händelsemeddelandefunktionen at.js användas via vanliga DOM-API:er, som `document.addEventListener()`. Nu kan du använda `document.addEventListener()` för att prenumerera på at.js-händelser, som begäranhändelser och återgivningshändelser för innehåll.
* Ett problem som rör erbjudanden som har skapats i Visual Experience Composer (VEC) har korrigerats. Före den här versionen dolde [!DNL Target] väljarna och dolde dem bara när alla väljare matchade. I at.js 0.9.2 [!DNL Target] döljs väljarna så fort de matchar.

## at.js Version 0.9.1

**Datum:** 14 juli 2016

* Tillhandahåller en timeout för besökar-ID-tjänsten på at.js, som är oberoende av tjänstens egen timeout.
* Korrigerar ett fel i 0.9.0 som påverkade implementeringar med at.js på vissa sidor och mbox.js (nu borttaget) på andra sidor.
* Om du använder [!DNL Adobe Analytics] som aktivitetens rapportkälla behöver du inte ange en spårningsserver när du skapar aktiviteter om du använder mbox.js version 61 (eller senare) eller at.js version 0.9.1 (eller senare). at.js-biblioteket skickar automatiskt spårningsservervärden till [!DNL Target]. När du skapar en aktivitet kan du lämna fältet för spårningsserver tomt på sidan Mål och inställningar.

## at.js Version 0.9.0

**[!DNL Target]version:** 16.6.1

**Datum:** 23 juni 2016

* Korrigerar ett problem med vita skärmar när VEC-erbjudanden används. Alla som använder at.js bör uppgradera till den nya versionen.
* Nytt `registerExtension`-API.

  Detta nya API ger utvecklare tillgång till vissa jQuery-moduler som används i at.js för att utveckla tillägg (även plugin-program) för biblioteket. Den här förändringen har vissa konsekvenser. Detta påverkar endast de användare som använder dessa funktioner:

   * `getSettings()` API har tagits bort, men samma funktioner är tillgängliga med `registerExtension()`.
   * `getTracking()` API har tagits bort, men samma funktioner är tillgängliga med `registerExtension()`.

   * Befintliga tillägg (t.ex. AngularJS-tillägg) måste uppdateras för att metoden `registerExtension()` ska kunna användas.

* Nytt meddelande-API för at.js.

  Målet med det här meddelandesystemet är att ge mer insikt i vad at.js gör på sidan och när det finns problem. Ett vanligt problem med VEC är att en IT-release ändrar sidan, en VEC-väljare avbryter och att testet slutar leverera innehållet korrekt. Ett mål med det här meddelandesystemet är att göra det här leveransproblemet känt på sidan, så att utvecklare kan komma åt den här informationen, skicka den till ett system som [!DNL Adobe Analytics] och varningar kan skickas till företagsägarna om att testet har avbrutits.

* Ny API-metod för `targetGlobalSettings()`.

  Du kan åsidosätta inställningarna i at.js-biblioteket i stället för att konfigurera dem i [!DNL Target Standard/Premium]-gränssnittet eller med REST API:er.

## at.js Version 0.8.0

**Datum:** 5 maj 2016.

Detta är den första officiella versionen av at.js-biblioteket.

at.js är ett nytt implementeringsbibliotek för [!DNL Target] som är utformat för både vanliga webbimplementeringar och enkelsidiga program.

at.js ersätter mbox.js för [!DNL Adobe Target] implementeringar.

Bland annat har at.js förbättrat sidinläsningstiderna för webbimplementeringar, förbättrat säkerheten och erbjuder bättre implementeringsalternativ för enkelsidiga program.

at.js innehåller komponenterna som ingick i target.js, så det finns inte längre något anrop till target.js.

Tänk på följande när du implementerar at.js:

* Tidigare versioner än 8 av Internet Explorer stöds inte.
* Asynkron implementering innebär att äldre integreringar som plugin-programmet [!UICONTROL Test&Target to SiteCatalyst] kanske inte fungerar.
* [!DNL Target] plugin-program som refererar till mbox.js-objekt och -metoder stöds inte.
* Alla anrop till [!DNL Target] görs via XMLHTTPRequest och innehållet returneras via JSON.
