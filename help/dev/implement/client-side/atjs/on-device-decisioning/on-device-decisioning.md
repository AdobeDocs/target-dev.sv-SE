---
keywords: implementering, javascript-bibliotek, js, atjs, on-device decisioning, on device decisioning, at.js, on device, on device, implementation0
description: Lär dig hur du utför [!UICONTROL on-device decisioning] med at.js-biblioteket
title: Hur fungerar On-device Decisioning med JavaScript-biblioteket at.js?
feature: at.js
exl-id: bd0e062f-c259-46f3-adba-e380af058ac8
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '3478'
ht-degree: 1%

---

# [!UICONTROL On-device decisioning] för at.js

Från och med version 2.5.0 erbjuder at.js [!UICONTROL on-device decisioning]. Med [!UICONTROL On-device decisioning] kan du cachelagra dina [A/B Test](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html) - och [Experience Targeting](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html) (XT)-aktiviteter i webbläsaren för att utföra minnesbaserad beslutsfattande utan någon blockerande nätverksbegäran till [!DNL Adobe Target] Edge Network.

>[!NOTE]
>
>[!UICONTROL On-device decisioning] är tillgängligt både för implementeringar på klientsidan och på serversidan. Den här artikeln beskriver [!UICONTROL on-device decisioning] för klientsidan. Mer information om [!UICONTROL on-device decisioning] för serversidan finns i implementeringsdokumentationen på serversidan [här](../../../server-side/sdk-guides/on-device-decisioning/overview.md).

[!DNL Target] erbjuder även flexibiliteten att leverera den mest relevanta och aktuella upplevelsen från dina experimentella och HTML-drivna personaliseringsaktiviteter (Machine Learning-driven) via ett live-serversamtal. Du kan med andra ord välja att använda [!UICONTROL on-device decisioning] när prestanda är viktigast. Men när den mest relevanta, aktuella och ML-drivna upplevelsen behövs kan ett serveranrop göras istället.

## Vilka är fördelarna med [!UICONTROL on-device decisioning]?

Fördelarna med [!UICONTROL on-device decisioning] är:

* **Leverera blixtsnabba beslut och upplevelser.** Buckling och beslut utförs i minnet och i webbläsaren för att undvika blockering av nätverksbegäranden.
* **Förbättra programmets prestanda.** Kör experiment och leverera personalisering till kunder och användare utan att kompromissa med slutanvändarupplevelserna.
* **Förbättra Google Site Quality Score.** I och med att beslut fattas i minnet kan du förbättra Google Site Quality-poängen för ditt onlineföretag så att det blir lättare att identifiera för kunderna.
* **Lär dig av realtidsanalyser.** Få insikter från aktivitetsprestanda i realtid via [Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) (A4T)-rapportering. Med A4T kan ni styra er strategi vid kritiska ögonblick.

## Funktioner som stöds

JS-SDK [!DNL Adobe Target] ger kunderna flexibilitet att välja mellan prestanda och aktualitet för data för beslut. Med andra ord, om det viktigaste är att leverera det mest relevanta och engagerande personaliserade innehållet via maskininlärning, bör du ringa ett live-serversamtal. Men när prestanda är viktigare bör man fatta beslut på enheten och i minnet. För att [!UICONTROL on-device decisioning] ska fungera, se listan över funktioner som stöds:

* Typ av aktivitet
* Målgruppsanpassning
* Allokeringsmetod

Mer information finns i [Funktioner som stöds för [!UICONTROL on-device decisioning]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md).

## Hur fungerar [!UICONTROL on-device decisioning]?

När du distribuerar och initierar at.js med [!UICONTROL on-device decisioning] aktiverat hämtas en [regelartefakt](/help/dev/implement/client-side/atjs/on-device-decisioning/rule-artifact.md) som innehåller din [!UICONTROL on-device decisioning] för A/B- och XT-aktiviteter, målgrupper och resurser från närmaste Akamai CDN till besökaren och cachelagras lokalt i besökarens webbläsare. När en begäran görs från at.js för att hämta en upplevelse, bestäms vilken upplevelse som ska returneras i minnet utifrån de metadata som är kodade i den cachelagrade regelartefakten.

## Beslutsmetod

Med [!UICONTROL on-device decisioning] introducerar [!DNL Target] en ny inställning med namnet Beslutsmetod. Inställningen Beslutsmetod styr hur at.js levererar dina upplevelser. Beslutsmetoden har tre värden:

* Endast serversidan
* Endast på enheten
* Hybrid

### Endast serversidan

Endast på serversidan är den standardmetod för beslut som anges i rutan när at.js 2.5.0+ implementeras och distribueras på dina webbegenskaper.

Om endast serversidan används som standardkonfiguration innebär det att alla beslut fattas i gränsnätverket [!DNL Target], vilket innebär ett blockerande serveranrop. Den här metoden kan innebära inkrementell fördröjning, men den ger också avsevärda fördelar, som att du kan använda [!DNL Target]s maskininlärningsfunktioner som inkluderar [Rekommendationer](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html), [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) (AP) och [Automatiskt mål](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) -aktiviteter.

Dessutom kan förbättringar av dina personaliserade upplevelser genom att använda användarprofilen för [!DNL Target], som är beständig i olika sessioner och kanaler, ge ett kraftfullt resultat för ditt företag.

Slutligen kan du bara använda Adobe Experience Cloud på serversidan och finjustera målgrupper som kan riktas mot dem via segmenten Audience Manager och Adobe Analytics.

Följande diagram visar interaktionen mellan din besökare, webbläsaren, at.js 2.5.0+, och [!DNL Adobe Target] Edge-nätverket. Det här flödesdiagrammet fångar nya besökare och returnerar besökare.

(Klicka på bilden för att expandera till full bredd.)

![Endast flödesdiagram på serversidan](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/server-side-only.png "Endast flödesdiagram på serversidan"){zoomable="yes"}

Följande lista motsvarar siffrorna i diagrammet:

| Steg | Beskrivning |
| --- | --- |
| 1 | Experience Cloud Visitor-ID hämtas från [Adobe Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/home.html?). |
| 2 | At.js-biblioteket läses in synkront och döljer dokumentets brödtext.<br />   At.js-biblioteket kan också läsas in asynkront med ett valfritt fördolt fragment som implementerats på sidan. |
| 3 | At.js-biblioteket döljer kroppen för att förhindra flimmer. |
| 4 | En sidinläsningsbegäran görs som innehåller alla konfigurerade parametrar, t.ex. (ECID, Kund-ID, Anpassade parametrar, Användarprofil osv.) |
| 5 | Profilskript körs och matas sedan in i profilarkivet.<br />Profilarkivet begär kvalificerade målgrupper från målgruppsbiblioteket (till exempel målgrupper som delas från Adobe Analytics, Adobe Audience Manager och så vidare).<br />Kundattribut skickas till profilarkivet i en gruppbearbetning. |
| 6 | Profilarkivet används för att filtrera aktiviteter genom att kvalificera och klippa målgrupper. |
| 7 | Det resulterande innehållet väljs efter att upplevelsen har bestämts från aktiva [!DNL Target]-aktiviteter. |
| 8 | Med biblioteket at.js döljs motsvarande element på sidan som är kopplade till upplevelsen som måste återges. |
| 9 | I biblioteket at.js visas brödtexten så att resten av sidan kan läsas in så att besökaren kan se den. |
| 10 | At.js-biblioteket ändrar DOM för att återge upplevelsen från [!DNL Target] Edge Network. |
| 11 | Upplevelsen renderar för besökaren. |
| 12 | Hela webbsidan läses in. |
| 13 | Analysdata skickas till datainsamlingsservrar. |
| 14 | Målinriktade data matchas mot analysdata via SDID och bearbetas till lagringsplatsen för analysrapporter. Analysdata kan sedan visas både i Analytics-rapporter och i [!DNL Target] via [!UICONTROL Analytics for Target]-rapporter (A4T). |

### Endast på enheten

Enbart på enheten är den beslutsmetod som måste anges i at.js 2.5.0+ när [!UICONTROL on-device decisioning] endast ska användas på dina webbsidor.

[!UICONTROL On-device decisioning] kan leverera dina upplevelser och personaliseringsaktiviteter med blixtsnabb hastighet eftersom besluten fattas från en cache-lagrad regelartefakt som innehåller alla dina aktiviteter som är kvalificerade för [!UICONTROL on-device decisioning].

Mer information om vilka aktiviteter som är kvalificerade för [!UICONTROL on-device decisioning] finns i [Funktioner som stöds i [!UICONTROL on-device decisioning]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md).

Den här beslutsmetoden bör endast användas om prestanda är mycket kritiskt för alla sidor som kräver beslut från Target. Tänk dessutom på att när du väljer den här beslutsmetoden kommer [!DNL Target]-aktiviteter som inte är kvalificerade för [!UICONTROL on-device decisioning] inte att levereras eller köras. At.js-biblioteket 2.5.0+ är konfigurerat att endast söka efter den cachelagrade regelartefakten för att fatta beslut.

Följande diagram visar interaktionen mellan besökaren, webbläsaren, at.js 2.5.0+, och Akamai CDN. Akamai CDN cachar regelfelaktigheten vid besökarens första besök. Vid den första sidbesöket hos en ny besökare måste JSON-regelartefakten hämtas från Akamai CDN för att kunna cachas lokalt i besökarens webbläsare. När JSON-regelartefakten har hämtats fattas beslutet omedelbart utan ett blockerande nätverksanrop. I följande flödesdiagram visas nya besökare.

(Klicka på bilden för att expandera till full bredd.)

![Flödesdiagram endast på enheten](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/on-device-only.png "Flödesdiagram endast på enheten"){zoomable="yes"}

Följande lista motsvarar siffrorna i diagrammet:

>[!NOTE]
>
>[!DNL Adobe Target] Administratörsservrar kvalificerar alla dina aktiviteter som är berättigade till [!UICONTROL on-device decisioning], genererar JSON-regelartefakten och sprider den till Akamai CDN. Dina aktiviteter övervakas kontinuerligt för uppdateringar av en ny JSON-regelartefakt som ska spridas till Akamai CDN.

| Steg | Beskrivning |
| --- | --- |
| 1 | Experience Cloud Visitor-ID hämtas från [Adobe Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | At.js-biblioteket läses in synkront och döljer dokumentets brödtext.<br />At.js-biblioteket kan också läsas in asynkront med ett valfritt fördolt fragment implementerat på sidan. |
| 3 | At.js-biblioteket döljer kroppen för att förhindra flimmer. |
| 4 | I at.js-biblioteket görs en begäran om att hämta JSON-regelartefakten från närmaste Akamai CDN till besökaren. |
| 5 | Akamai CDN svarar med JSON-regelartefakten. |
| 6 | JSON-regelartefakten cachelagras lokalt i besökarens webbläsare. |
| 7 | I biblioteket at.js tolkas JSON-regelartefakten och beslutet att hämta upplevelsen verkställs och de testade elementen döljs. |
| 8 | I biblioteket at.js visas brödtexten så att resten av sidan kan läsas in så att besökaren kan se den. |
| 9 | At.js-biblioteket ändrar DOM för att återge upplevelsen från den cachelagrade JSON-regelartefakten. |
| 10 | Upplevelsen renderar för besökaren. |
| 11 | Hela webbsidan läses in. |
| 12 | Analysdata skickas till datainsamlingsservrar. Målinriktade data matchas mot analysdata via SDID och bearbetas till lagringsplatsen för analysrapporter. Analysdata kan sedan visas både i Analytics-rapporter och i [!DNL Target] via [!UICONTROL Analytics for Target]-rapporter (A4T). |

I följande diagram visas interaktionen mellan besökaren, webbläsaren, at.js 2.5.0+, och den cachelagrade JSON-regelartefakten för besökarens efterföljande sidträff eller återbesök. Eftersom JSON-regelartefakten redan är cachelagrad och tillgänglig i webbläsaren, fattas beslutet omedelbart utan ett blockerande nätverksanrop. Det här flödesdiagrammet fångar efterföljande sidnavigering eller returnerar besökare.

(Klicka på bilden för att expandera till full bredd.)

![Flödesdiagram endast på enheten för efterföljande sidnavigering och återkommande besök](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/on-device-only-subsequent.png "Flödesdiagram endast på enheten för efterföljande sidnavigering och återkommande besök"){zoomable="yes"}

Följande lista motsvarar siffrorna i diagrammet:

>[!NOTE]
>
>[!DNL Adobe Target] Administratörsservrar kvalificerar alla dina aktiviteter som är berättigade till [!UICONTROL on-device decisioning], genererar JSON-regelartefakten och sprider den till Akamai CDN. Dina aktiviteter övervakas kontinuerligt för uppdateringar av en ny JSON-regelartefakt som ska spridas till Akamai CDN.

| Steg | Beskrivning |
| --- | --- |
| 1 | Experience Cloud Visitor-ID hämtas från [Adobe Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | At.js-biblioteket läses in synkront och döljer dokumentets brödtext.<br />At.js-biblioteket kan också läsas in asynkront med ett valfritt fördolt fragment implementerat på sidan. |
| 3 | At.js-biblioteket döljer kroppen för att förhindra flimmer. |
| 4 | I biblioteket at.js tolkas JSON-regelartefakten och beslutet i minnet verkställs för att hämta upplevelsen. |
| 5 | De testade elementen är dolda. |
| 6 | I biblioteket at.js visas brödtexten så att resten av sidan kan läsas in så att besökaren kan se den. |
| 7 | At.js-biblioteket ändrar DOM för att återge upplevelsen från den cachelagrade JSON-regelartefakten. |
| 8 | Upplevelsen renderar för besökaren. |
| 9 | Hela webbsidan läses in. |
| 10 | Analysdata skickas till datainsamlingsservrar. Målinriktade data matchas mot analysdata via SDID och bearbetas till lagringsplatsen för analysrapporter. Analysdata kan sedan visas både i Analytics-rapporter och i [!DNL Target] via [!UICONTROL Analytics for Target]-rapporter (A4T). |

### Hybrid

Hybrid är den beslutsmetod som måste anges i at.js 2.5.0+ när både [!UICONTROL on-device decisioning] och aktiviteter som kräver ett nätverksanrop till [!DNL Adobe Target] Edge-nätverket måste köras.

När du hanterar både [!UICONTROL on-device decisioning]-aktiviteter och aktiviteter på serversidan kan det vara lite komplicerat och tråkigt att fundera på hur du ska distribuera och distribuera [!DNL Target] på dina sidor. Med hybridmetoden som beslutsmetod vet [!DNL Target] när det måste göra ett serveranrop till Edge-nätverket [!DNL Adobe Target] för aktiviteter som kräver körning på serversidan och även när endast enhetsbeslut ska verkställas.

JSON-regelartefakten innehåller metadata som informerar at.js om en mbox har en aktivitet på serversidan som körs eller en [!UICONTROL on-device decisioning]-aktivitet. Den här beslutsmetoden ser till att aktiviteter som du avser att leverera snabbt utförs via [!UICONTROL on-device decisioning] och för aktiviteter som kräver mer kraftfull ML-driven personalisering utförs dessa aktiviteter via [!DNL Adobe Target] Edge-nätverket.

Följande diagram visar interaktionen mellan besökaren, webbläsaren, kl. 2.5.0+, Akamai CDN och [!DNL Adobe Target] Edge Network för en ny besökare som besöker din sida för första gången. Ingången från det här diagrammet är att JSON-regelartefakten hämtas asynkront medan besluten fattas via Edge-nätverket [!DNL Adobe Target].

Detta säkerställer att artefaktens storlek, som kan innehålla många aktiviteter, inte påverkar beslutets fördröjning negativt. Om du hämtar JSON-reglerna synkront och fattar beslut efter detta kan det också få negativa effekter på fördröjningen och kan vara inkonsekvent. Därför är hybridmetoden en rekommendation om att alltid göra ett anrop på serversidan för beslut om en ny besökare, och eftersom JSON-regelartefakten cachas parallellt. För efterföljande sidbesök och återbesök görs besluten från cacheminnet och i minnet via JSON-regelartefakten.

(Klicka på bilden för att expandera till full bredd.)

![Hybrid-flödesdiagram för förstagångsbesökare](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/hybrid-first-time-visitor.png "Hybrid-flödesdiagram för förstagångsbesökare"){zoomable="yes"}

Följande lista motsvarar siffrorna i diagrammet:

>[!NOTE]
>
>[!DNL Adobe Target] Administratörsservrar kvalificerar alla dina aktiviteter som är berättigade till [!UICONTROL on-device decisioning], genererar JSON-regelartefakten och sprider den till Akamai CDN. Dina aktiviteter övervakas kontinuerligt för uppdateringar av en ny JSON-regelartefakt som ska spridas till Akamai CDN.

| Steg | Beskrivning |
| --- | --- |
| 1 | Experience Cloud Visitor-ID hämtas från [Adobe Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | At.js-biblioteket läses in synkront och döljer dokumentets brödtext.<br />At.js-biblioteket kan också läsas in asynkront med ett valfritt fördolt fragment implementerat på sidan. |
| 3 | At.js-biblioteket döljer kroppen för att förhindra flimmer. |
| 4 | En sidinläsningsbegäran görs till [!DNL Adobe Target] Edge Network, inklusive alla konfigurerade parametrar som (ECID, Kund-ID, Anpassade parametrar, Användarprofil osv.). |
| 5 | Samtidigt begär at.js att JSON-regelartefakten ska hämtas från närmaste Akamai CDN till besökaren. |
| 6 | ([!DNL Adobe Target] Edge Network) Profilskript körs och matas sedan in i profilarkivet. Profile Store begär kvalificerade målgrupper från Audience Library (till exempel målgrupper som delas från Adobe Analytics, Adobe Audience Manager och så vidare). |
| 7 | Akamai CDN svarar med JSON-regelartefakten. |
| 8 | Profilarkivet används för att filtrera aktiviteter genom att kvalificera och klippa målgrupper. |
| 9 | Det resulterande innehållet väljs efter att upplevelsen har bestämts från aktiva [!DNL Target]-aktiviteter. |
| 10 | Med biblioteket at.js döljs motsvarande element på sidan som är kopplade till upplevelsen som måste återges. |
| 11 | I biblioteket at.js visas brödtexten så att resten av sidan kan läsas in så att besökaren kan se den. |
| 12 | At.js-biblioteket ändrar DOM för att återge upplevelsen från [!DNL Target] Edge Network. |
| 13 | Upplevelsen renderar för besökaren. |
| 14 | Hela webbsidan läses in. |
| 15 | Analysdata skickas till datainsamlingsservrar. Målinriktade data matchas mot analysdata via SDID och bearbetas till lagringsplatsen för analysrapporter. Analysdata kan sedan visas både i Analytics-rapporter och i [!DNL Target] via [!UICONTROL Analytics for Target]-rapporter (A4T). |

I följande diagram visas interaktionen mellan besökaren, webbläsaren, at.js 2.5.0+, och den cachelagrade JSON-regelartefakten för efterföljande sidnavigering eller ett returbesök. I det här diagrammet fokuserar du bara på det användningsfall som avgör om det är enheten som ska användas för den efterföljande sidnavigeringen eller det efterföljande besöket. Tänk på att beroende på vilka aktiviteter som är aktiva för vissa sidor kan ett anrop på serversidan göras för att köra beslut på serversidan.

(Klicka på bilden för att expandera till full bredd.)

![Hybrid-flödesdiagram för efterföljande sidnavigering och återkommande besök](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/hybrid-subsequent.png "Hybrid-flödesdiagram för efterföljande sidnavigering och återkommande besök"){zoomable="yes"}

Följande lista motsvarar siffrorna i diagrammet:

>[!NOTE]
>
>[!DNL Adobe Target] Administratörsservrar kvalificerar alla dina aktiviteter som är berättigade till [!UICONTROL on-device decisioning], genererar JSON-regelartefakten och sprider den till Akamai CDN. Dina aktiviteter övervakas kontinuerligt för uppdateringar av en ny JSON-regelartefakt som ska spridas till Akamai CDN.

| Steg | Beskrivning |
| --- | --- |
| 1 | Experience Cloud Visitor-ID hämtas från [Adobe Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| 2 | At.js-biblioteket läses in synkront och döljer dokumentets brödtext.<br />At.js-biblioteket kan också läsas in asynkront med ett valfritt fördolt fragment implementerat på sidan. |
| 3 | At.js-biblioteket döljer kroppen för att förhindra flimmer. |
| 4 | En begäran görs för att hämta en upplevelse. |
| 5 | At.js-biblioteket bekräftar att JSON-regelartefakten redan har cache-lagrats och kör beslutet i minnet för att hämta upplevelsen. |
| 6 | De testade elementen är dolda. |
| 7 | I biblioteket at.js visas brödtexten så att resten av sidan kan läsas in så att besökaren kan se den. |
| 8 | At.js-biblioteket ändrar DOM för att återge upplevelsen från den cachelagrade JSON-regelartefakten. |
| 9 | Upplevelsen renderar för besökaren. |
| 10 | Hela webbsidan läses in. |
| 11 | Analysdata skickas till datainsamlingsservrar. Målinriktade data matchas mot analysdata via SDID och bearbetas till lagringsplatsen för analysrapporter. Analysdata kan sedan visas både i Analytics-rapporter och i [!DNL Target] via [!UICONTROL Analytics for Target]-rapporter (A4T). |

## Hur aktiverar jag [!UICONTROL on-device decisioning]?

[!UICONTROL On-device decisioning] är tillgängligt för alla [!DNL Target]-kunder som använder At.js 2.5.0+.

Aktivera [!UICONTROL on-device decisioning]:

>[!NOTE]
>
>Du måste ha administratörs- eller godkännarrollen [användare](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) för att kunna aktivera eller inaktivera växlingsknappen för enhetsbeslut.

1. Klicka på **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]**.
1. Under **[!UICONTROL Account details]** flyttar du växlingsknappen **[!UICONTROL On-Device Decisioning]** till läget&quot;på&quot;.

   ![[!UICONTROL On-device decisioning] växla &#x200B;](assets/on-device-decisioning-toggle.png)

   Alternativet Inkludera alla befintliga [!UICONTROL on-device decisioning] kvalificerade aktiviteter i artefakten visas om du aktiverar [!UICONTROL on-device decisioning].
1. (Villkorligt) Skjut växeln till&quot;på&quot;-positionen om du vill att alla dina [!DNL Target]-aktiviteter som kvalificerar för [!UICONTROL on-device decisioning] automatiskt ska inkluderas i artefakten.

   Om du lämnar den här inaktiveringen måste du återskapa och aktivera alla [!UICONTROL on-device decisioning]-aktiviteter för att de ska inkluderas i den genererade regelartefakten. Det innebär att alla aktiviteter i live-läge innan du aktiverar reglaget för val på enhet inte ingår i regelartefakten.

När du har aktiverat växeln för On-Device Decisioning börjar [!DNL Target] generera och sprida [regelartefakter](/help/dev/implement/client-side/atjs/on-device-decisioning/rule-artifact.md) för klienten.

>[!WARNING]
>
>Se till att du aktiverar växlingsknappen innan du initierar SDK för [!DNL Adobe Target] för att använda [!UICONTROL on-device decisioning]. Regelartefakterna måste först generera och sprida till Akamai CDN:er för att [!UICONTROL on-device decisioning] ska fungera. Spridning kan ta fem till tio minuter innan den första regelartefakten genereras och sprids till Akamai CDN.

## Hur konfigurerar jag att at.js 2.5.0+ ska använda [!UICONTROL on-device decisioning]?

1. Klicka på **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]**.
1. Under **[!UICONTROL Implementation Methods]** > **[!UICONTROL Main Implementation Method]** klickar du på **[!UICONTROL Edit]** bredvid din at.js-version (måste vara at.js 2.5.0 eller senare).

   ![Redigera inställningar för huvudimplementeringsmetod](assets/main-implementation-method.png)

   >[!WARNING]
   >
   >Innan du ändrar dessa standardinställningar bör du kontakta kundtjänst för att undvika att den aktuella implementeringen påverkas.

1. Välj önskad beslutsmetod:

   * Endast serversidan
   * Endast på enheten
   * Hybrid

   ![Redigera på inställningspanelen at.js](assets/global-settings.png)

### Globala inställningar

Du kan konfigurera en standardmetod för beslutsfattande för alla [!DNL Target] beslut. De olika beslutsmetoderna är endast på serversidan, endast på enheten och hybrider. Beslutsmetoden som valts i användargränssnittet för [!DNL Target] är konfigurerad i `window.targetGlobalSettings` under fältet `decisioningMethod`. Läs mer om `decisioningMethod` i [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#decisioningmethod).

```javascript {line-numbers="true"}
<head> 
    <script type="text/javascript">

        window.targetGlobalSettings = { 
            clientCode: "yourClientCodeHere", 
            imsOrgId: "imsOrgId@AdobeOrg", 
            decisioningMethod: "on-device"

        }; 
    </script>

    <script type="text/javascript" src="at.js"></script> 
</head>
```

### Anpassad inställning

Om du anger `decisioningMethod` i `window.targetGlobalSettings`, men vill åsidosätta `decisioningMethod` för varje [!DNL Adobe Target]-beslut enligt ditt användningsexempel, kan du göra den här proceduren genom att ange `decisioningMethod` i [&#x200B; getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) -anropet för At.js2.5.0+.

```javascript {line-numbers="true"}
adobe.target.getOffers({ 

  decisioningMethod:"on-device", 
  request: { 
    execute: { 
      mboxes: [ 
        { 
          index: 0, 
          name: "homepage" 
        } 
      ] 
    } 
 } 
});
```

>[!NOTE]
>
>Om du vill använda&quot;on-device&quot; eller&quot;hybrid&quot; som en beslutsmetod i getOffers()-anropet kontrollerar du att den globala inställningen har `decisioningMethod` som&quot;on-device&quot; eller&quot;hybrid&quot;. At.js-biblioteket 2.5.0+ måste känna till om JSON-regelartefakten ska hämtas och cachelagras direkt efter att sidan har lästs in. Om beslutsmetoden för den globala inställningen är inställd på &quot;server-side&quot;, och &quot;on-device&quot; eller &quot;hybrid&quot;-beslutsmetoden skickas till getOffers()-anropet, skulle at.js 2.5.0+ inte ha JSON-regelartefakten cachelagrad för att köra dina enhetsbeslut.

### Cacheminne för felaktigheter

Målet representerar dina aktiviteter som är kvalificerade för [!UICONTROL on-device decisioning] som en artefakt som består av metadata, regler och villkor. Artefakten cachelagras på Akamai CDN. Under användarens första besök hämtas och cachelagras den artefakt som representerar dina [!UICONTROL on-device decisioning]-aktiviteter av användarens webbläsare.

Vid efterföljande besök på webbplatsen kontrollerar webbläsaren automatiskt om en nyare version av artefakten måste hämtas. Den här kontrollen lägger till fördröjning. TTL för artefaktcache anger hur många minuter du inte vill att webbläsaren ska kontrollera om det finns en uppdaterad artefakt sedan den senaste slutförda hämtningen. Ju längre tidsram, desto bättre prestanda. Ju kortare tidsram, desto bättre dataaktualitet, men till priset av extra latens.

## Hur vet jag att en aktivitet är [!UICONTROL on-device decisioning] berättigad?

När du har skapat en aktivitet som är [!UICONTROL on-device decisioning] berättigad visas en etikett som läser Berättigad att bestämma på enheten på aktivitetens översiktssida.

![Berättigad etikett för val på enheten på aktivitetens översiktssida.](assets/on-device-decisioning-eligible-label.png)

Den här etiketten betyder inte att aktiviteten alltid levereras via [!UICONTROL on-device decisioning]. Endast när at.js 2.5.0+ är konfigurerad att använda [!UICONTROL on-device decisioning] kommer den här aktiviteten att köras på enheten. Om at.js 2.5.0+ inte är konfigurerad att använda på enheten kommer aktiviteten fortfarande att levereras via ett serveranrop som görs från at.js.

Du kan filtrera efter alla aktiviteter som är [!UICONTROL on-device decisioning] berättigade på aktivitetssidan via filtret Berättigat till beslut på enheten.

![Berättigat filter för val på enhet på aktivitetssidan.](assets/on-device-decisioning-filter.png)

>[!NOTE]
>
>När du har skapat och aktiverat en aktivitet som är [!UICONTROL on-device decisioning] berättigad kan det ta fem till tio minuter innan den ingår i den regelartefakt som genereras och sprids till Akamai CDN-instansen.

## Sammanfattning av steg som säkerställer att mina [!UICONTROL on-device decisioning]-aktiviteter levereras via At.js 2.5.0+?

1. Gå till användargränssnittet för [!DNL Adobe Target] och navigera till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account Details]** för att aktivera alternativet **[!UICONTROL On-Device Decisioning]**.
1. Aktivera växlingsknappen **[!UICONTROL "Include all existing [!UICONTROL on-device decisioning] qualified activities in the artifact"]**.

   Den första genereringen av JSON-regelartefakter kan ta upp till 10 minuter.

1. Skapa och aktivera en [-aktivitetstyp som stöds av [!UICONTROL on-device decisioning]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md) och verifiera att den är [!UICONTROL on-device decisioning] giltig.
1. Ange **[!UICONTROL Decisioning Method]** till antingen **[!UICONTROL "Hybrid"]** eller **[!UICONTROL "On-device only"]** via användargränssnittet för at.js-inställningarna.
1. Ladda ned och driftsätt At.js 2.5.0+ på sidorna.
