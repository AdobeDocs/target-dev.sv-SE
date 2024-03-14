---
keywords: serversida, server-side, sdk, sdks, on-device, beslut, on device, ondevice, zero latency, latency, near-zero, node.js, server side3
description: Lär dig använda [!UICONTROL [!UICONTROL on-device decisioning]] för att cachelagra [!DNL Target] A/B- och MVT-aktiviteter på servern för att utföra minnesbaserad decimering med nästan noll latens.
title: Vad är On-Device Decision?
feature: Implement Server-side
exl-id: 22ed3072-56f0-4075-9d1a-d642afe3b649
source-git-commit: ff0becf3fe3a6fd6694e13243b6a93b910316434
workflow-type: tm+mt
source-wordcount: '1022'
ht-degree: 0%

---

# Översikt över beslut på enheter

Nästa generation [!DNL Adobe Target] SDK erbjuder nu [!UICONTROL on-device decisioning], som gör det möjligt att cachelagra era A/B- och Experience Targeting-kampanjer (XT) på servern och genomföra minnesbaserad beslutsfattande med nästan ingen fördröjning, utan att blockera nätverksförfrågningar till [!DNL Adobe Target]Edge Network.

[!DNL Adobe Target] erbjuder även flexibiliteten att leverera den mest relevanta och aktuella upplevelsen från era experiment och ML-drivna personaliseringskampanjer via ett live serversamtal. Med andra ord: när prestanda är viktigast kan du välja att använda [!UICONTROL on-device decisioning], men när den mest relevanta och aktuella upplevelsen behövs kan ett serveranrop göras i stället. Se [när enhetsspecifik kontra kantavräkning ska användas](../../sdk-guides/on-device-decisioning/supported-features.md) för att lära sig mer om användningsfall som motiverar att man använder det ena över det andra.

>[!NOTE]
>
>Enhetsbeslut är tillgängligt både för implementeringar på klientsidan och på serversidan. Den här artikeln beskriver [!UICONTROL on-device decisioning] för serversidan. För information om [!UICONTROL on-device decisioning] för klientsidan, se implementeringsdokumentationen på klientsidan [här](../../../client-side/atjs/on-device-decisioning/on-device-decisioning.md).

## Hur fungerar det?

När du installerar och initierar en [!DNL Adobe Target] SDK med [!UICONTROL on-device decisioning] aktiverad, en *regelartefakt* hämtas och cachelagras lokalt på servern från det Akamai CDN som ligger närmast servern. När en begäran om hämtning av en [!DNL Adobe Target] upplevelsen görs i serverprogrammet, vilket avgör vilket innehåll som ska returneras i minnet, baserat på de metadata som kodas i den cachelagrade regelartefakten, som definierar alla dina [!UICONTROL on-device decisioning] A/B- och XT-aktiviteter

I följande diagram visas [!UICONTROL on-device decisioning] arkitektur. Klicka för att expandera bilden.

(Klicka på bilden för att expandera till full bredd.)

![Arkitektur för beslutsfattande på enheter](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/assets/asset-sdk-local-decisioning-architecture-diagram.png "Arkitektur för beslutsfattande på enheter"){zoomable=&quot;yes&quot;}

## Vilka är fördelarna?

* **Leverera nästan-noll latensbeslut.** Buckling och beslut utförs i minnet och på enheten för att undvika blockering av nätverksbegäranden.
* **Förbättra programmets prestanda.** Kör experiment och leverera personalisering till kunder och användare utan att kompromissa med slutanvändarupplevelserna.
* **Förbättra Google Site Quality Score.** I och med att beslut fattas i minnet och på serversidan kan du förbättra Google Site Quality-poängen för ditt onlineföretag så att det blir lättare att hitta för kunderna.
* **Lär dig av realtidsanalyser.** Få insikter från aktivitetens resultat i realtid via [!DNL Adobe Target] eller A4T-rapportering, så att ni kan styra er strategi vid kritiska ögonblick.

## Funktioner som stöds

### Verksamhet

Enhetsbeslut stöder följande aktivitetstyper som skapas av [Formulärbaserad Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html):

* [!UICONTROL A/B Test]
* [!UICONTROL Experience Targeting] (XT)

### Allokeringsmetod

Enhetsbeslut har stöd för följande allokeringsmetod:

* Manuell

### Målgruppsanpassning

Enhetsbeslut stöder följande målgruppsregler:

| Målgruppsregel | Beslut på enheten |
| --- | --- |
| [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | Ja<P>Följande geo-attribut stöds när enhetsbeslut används:<ul><li>Land/region</li><li>Ort</li><li>Latitude</li><li>Longitud</li></ul> |
| [Nätverk](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | Nej |
| [Mobil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | Nej |
| [Egna parametrar](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | Ja |
| [Operativsystem](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | Ja |
| [Webbplatssidor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | Ja |
| [Webbläsare](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | Ja |
| [Besökarprofil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | Nej |
| [Trafikkällor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | Nej |
| [Tidsram](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | Ja |
| [Experience Cloud målgrupper](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) (Målgrupper från Adobe Audience Manager, Adobe Analytics och Adobe Experience Manager | Nej |

## Hur etablerar jag min kund att använda [!UICONTROL on-device decisioning]?

Enhetsbeslut är tillgängligt för alla [!DNL Adobe Target] kunder som använder [!DNL Adobe Target] SDK:er på serversidan. Om du vill aktivera funktionen går du till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** i [!DNL Adobe Target] och aktivera **[!UICONTROL On-Device Decisioning]** växla.

>[!NOTE]
>
>Du måste ha administratören eller godkännaren *användarroll* för att aktivera eller inaktivera [!UICONTROL On-Device Decisioning] växla.

![alt-bild](assets/asset-odd-toggle.png)

När du har aktiverat alternativet för val på enhet, [!DNL Adobe Target] börjar generera och sprida *regelartefakter* för kunden.

>[!NOTE]
>
>Se till att du aktiverar växlingsknappen innan du initierar [!DNL Adobe Target] SDK att använda [!UICONTROL on-device decisioning]. Regelartefakterna måste först generera och sprida till Akamai CDN:er för att [!UICONTROL on-device decisioning] till jobbet.

### Inkludera alla befintliga [!UICONTROL on-device decisioning] kvalificerade aktiviteter i verktygsfältet Artefakt

Växla detta **på** när du vill ha alla dina [!DNL Target] aktiviteter som uppfyller kraven för [!UICONTROL on-device decisioning] som automatiskt inkluderas i artefakten.

Lämnar denna växel **av** innebär att du måste återskapa och aktivera [!UICONTROL on-device decisioning] aktiviteter för att de ska kunna inkluderas i den genererade regelartefakten.

## Hur vet jag att en aktivitet är [!UICONTROL on-device decisioning] kapabel?

När du har skapat en aktivitet anropas en etikett **[!UICONTROL Decisioning Method]**, som visas på aktivitetsinformationssidan, anger om aktiviteten är [!UICONTROL on-device decisioning] kapabel.

![alt-bild](assets/asset-odd9.png)

Du kan även se alla aktiviteter som [!UICONTROL on-device decisioning] som klarar **[!UICONTROL Activities]** sida genom att lägga till kolumnen **[!UICONTROL Decisioning Method]** till förteckningen över verksamheter.

![alt-bild](assets/asset-odd7.png)

>[!NOTE]
>
>Efter att en aktivitet som [!UICONTROL on-device decisioning] kan det ta upp till 20 minuter innan det ingår i den regelartefakt som genereras och sprids till Akamai CDN PoPs.

## Vad är sammanfattningen av de steg jag behöver följa för att säkerställa [!UICONTROL on-device decisioning] aktiviteter kan levereras via [!DNL Adobe Target]SDK på serversidan?

1. Öppna [!DNL Adobe Target] Gränssnitt och navigera till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** för att aktivera **[!UICONTROL On-Device Decisioning]** växla.
1. Aktivera **[!UICONTROL Include all existing [!UICONTROL on-device decisioning] qualified activities in the artifact]** växla.
1. Skapa och aktivera en aktivitetstyp som stöds av [!UICONTROL on-device decisioning]och verifiera att **[!UICONTROL Decisioning Method]** är **[!UICONTROL On-Device Decisioning]** för den aktiviteten.
1. Installera och initiera [Node.js](../../node-js/overview.md) eller [Java](../../java/overview.md) SDK med `decisioningMethod = on-device`.
1. Implementera `getOffers()` eller `getAttributes()` i koden för att hämta en upplevelse på enheten.
1. Distribuera koden.

Exempel på hur du kommer igång med steg 1-3 ovan finns i [Komma igång](../getting-started/getting-started.md) -avsnitt.


## Ytterligare resurser

### Webbinarium: Personalisera och testa med nolltidsfördröjning med beslut på enheter från [!DNL Adobe Target]

Nu mer än någonsin har marknadsförare, produktägare och utvecklare fått i uppgift att optimera den övergripande kundupplevelsen på webbplatser, i appar och på alla andra sätt de har kontakt med sina kunder. Det finns inte tillräckligt med flera verktyg med vattentäta skott och komplicerade implementeringar.

I det här inspelade webbinariet [!DNL Adobe Target] produktexperter diskuterar hur flyttande beslut om optimering av kritiska upplevelser på enheter som körs lokalt med nästan nolltidsfördröjning kan öppna dörrar för spännande nya användningsfall samtidigt som webbplatsens prestanda förbättras för dina kunder.

>[!VIDEO](https://video.tv.adobe.com/v/328148/?quality=12)


### Självstudiekurs: Enhetsbeslut

[!DNL Adobe Target] [!UICONTROL on-device decisioning] möjliggör leverans av innehåll med nästan noll latens.

Den här 7-minutersvideon:

* Beskriver [!UICONTROL on-device decisioning], inklusive hur det jämförs med andra metoder för [!DNL Target] implementering
* Visar hur man aktiverar [!UICONTROL on-device decisioning] i mål
* Undersöker en formulärbaserad dispositionsaktivitet som har konfigurerats med JSON-innehåll
* Visar exempelkoden Node.JS SDK som innehåller den nyckelkonfiguration som krävs för [!UICONTROL on-device decisioning]
* Visar resultat i en webbläsare

>[!VIDEO](https://video.tv.adobe.com/v/329032/?quality=12)

Fler videoklipp och självstudiekurser finns i [[!DNL Adobe Target] Tutorials](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html).

### Adobe Tech Blog - Del 1: Kör [!DNL Adobe Target] NodeJS SDK för experiment och personalisering på edge-plattformar (Akamai Edge Workers)

[Klicka här för att komma åt blogginlägget](https://medium.com/adobetech/part-1-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-4d8660964ed9).

### Adobe Tech Blog - Del 2: Kör [!DNL Adobe Target] NodeJS SDK för experiment och personalisering på edge-plattformar (AWS Lambda@Edge)

[Klicka här för att komma åt blogginlägget](https://medium.com/adobetech/part-2-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-aws-4d6bdac24563).
