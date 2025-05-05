---
keywords: serversida, server-side, sdk, sdks, on-device, beslut, on device, ondevice, zero latency, latency, near-zero, node.js, server side3
description: Lär dig hur du använder [!UICONTROL [!UICONTROL on-device decisioning]] för att cachelagra dina  [!DNL Target] A/B- och MVT-aktiviteter på servern och utföra minnesbaserad decimering vid nästan noll-fördröjning.
title: Vad är On-Device Decision?
feature: Implement Server-side
exl-id: 22ed3072-56f0-4075-9d1a-d642afe3b649
source-git-commit: ff0becf3fe3a6fd6694e13243b6a93b910316434
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# Översikt över beslut på enheter

Nästa generations [!DNL Adobe Target] SDK:er erbjuder nu [!UICONTROL on-device decisioning], som gör det möjligt att cachelagra dina A/B- och Experience Targeting-kampanjer (XT) på servern och utföra minnesbaserad beslutsfattande med en latens som är nära noll, utan att blockera nätverksförfrågningar till [!DNL Adobe Target]s Edge Network.

[!DNL Adobe Target] erbjuder även flexibiliteten att leverera den mest relevanta och aktuella upplevelsen från era experimentella och ML-drivna personaliseringskampanjer via ett live-serversamtal. Med andra ord, när prestanda är viktigast kan du välja att använda [!UICONTROL on-device decisioning], men när den mest relevanta och aktuella upplevelsen behövs kan du göra ett serveranrop i stället. Se [när du ska använda enhetsspecifik kontra kantavkänning](../../sdk-guides/on-device-decisioning/supported-features.md) om du vill veta mer om användningsfall som kräver att du använder den ena över den andra.

>[!NOTE]
>
>Enhetsbeslut är tillgängligt både för implementeringar på klientsidan och på serversidan. I den här artikeln beskrivs [!UICONTROL on-device decisioning] för serversidan. Mer information om [!UICONTROL on-device decisioning] för klientsidan finns i implementeringsdokumentationen på klientsidan [här](../../../client-side/atjs/on-device-decisioning/on-device-decisioning.md).

## Hur fungerar det?

När du installerar och initierar en [!DNL Adobe Target] SDK med [!UICONTROL on-device decisioning] aktiverat hämtas en *regelartefakt* och cachelagras lokalt på servern från det Akamai CDN som ligger närmast servern. När en begäran om att hämta en [!DNL Adobe Target]-upplevelse görs i programmet på serversidan, bestäms vilket innehåll som ska returneras i minnet utifrån de metadata som är kodade i den cachelagrade regelartefakten, som definierar alla dina [!UICONTROL on-device decisioning] A/B- och XT-aktiviteter.

I följande diagram visas arkitekturen för [!UICONTROL on-device decisioning]. Klicka för att expandera bilden.

(Klicka på bilden för att expandera till full bredd.)

![Arkitektur för enhetsbeslut](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/assets/asset-sdk-local-decisioning-architecture-diagram.png "Arkitektur för enhetsbeslut"){zoomable="yes"}

## Vilka är fördelarna?

* **Ta fram svarstidsbeslut som är nära noll.** Buckring och beslut utförs i minnet och på enheten för att undvika blockering av nätverksbegäranden.
* **Förbättra programmets prestanda.** Kör experiment och leverera personalisering till kunder och användare utan att kompromissa med slutanvändarupplevelserna.
* **Förbättra Google Site Quality Score.** I och med att beslut fattas i minnet och på serversidan kan du förbättra Google Site Quality-poängen för ditt onlineföretag så att det blir lättare att hitta för kunderna.
* **Lär dig av realtidsanalyser.** Få insikter från aktivitetsprestanda i realtid via [!DNL Adobe Target]- eller A4T-rapportering, vilket gör att du kan styra din strategi i kritiska ögonblick.

## Funktioner som stöds

### Verksamhet

Enhetsspecifikt beslutsfattande stöder följande aktivitetstyper som skapats av [formulärbaserad Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=sv-SE):

* [!UICONTROL A/B Test]
* [!UICONTROL Experience Targeting] (XT)

### Allokeringsmetod

Enhetsbeslut har stöd för följande allokeringsmetod:

* Manuell

### Målgruppsanpassning

Enhetsbeslut stöder följande målgruppsregler:

| Målgruppsregel | Beslut på enheten |
| --- | --- |
| [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=sv-SE) | Ja<P>Följande geo-attribut stöds när enhetsbeslut används:<ul><li>Land/region</li><li>Ort</li><li>Latitude</li><li>Longitud</li></ul> |
| [Nätverk](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=sv-SE) | Nej |
| [Mobil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=sv-SE) | Nej |
| [Egna parametrar](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=sv-SE) | Ja |
| [Operativsystem](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=sv-SE) | Ja |
| [Webbplatssidor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=sv-SE) | Ja |
| [Webbläsare](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=sv-SE) | Ja |
| [Besökarprofil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=sv-SE) | Nej |
| [Trafikkällor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=sv-SE) | Nej |
| [Tidsram](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=sv-SE) | Ja |
| [Experience Cloud-målgrupper](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=sv-SE) (målgrupper från Adobe Audience Manager, Adobe Analytics och Adobe Experience Manager) | Nej |

## Hur etablerar jag min klient för att använda [!UICONTROL on-device decisioning]?

Enhetsbeslut är tillgängligt för alla [!DNL Adobe Target]-kunder som använder [!DNL Adobe Target] SDK:er på serversidan. Om du vill aktivera den här funktionen går du till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** i [!DNL Adobe Target]-gränssnittet och aktiverar växlingsknappen **[!UICONTROL On-Device Decisioning]**.

>[!NOTE]
>
>Du måste ha administratörs- eller godkännarrollen *användare* för att aktivera eller inaktivera växlingsknappen [!UICONTROL On-Device Decisioning].

![alt-bild](assets/asset-odd-toggle.png)

När du har aktiverat växeln för On-Device Decisioning börjar [!DNL Adobe Target] generera och sprida *regelartefakter* för klienten.

>[!NOTE]
>
>Se till att du aktiverar växlingen innan du initierar SDK:n för [!DNL Adobe Target] för att använda [!UICONTROL on-device decisioning]. Regelartefakterna måste först generera och sprida till Akamai CDN:er för att [!UICONTROL on-device decisioning] ska fungera.

### Inkludera alla befintliga [!UICONTROL on-device decisioning] kvalificerade aktiviteter i artefaktväxeln

Växla **på** när du vill att alla dina [!DNL Target]-aktiviteter som är kvalificerade för [!UICONTROL on-device decisioning] automatiskt ska inkluderas i artefakten.

Om du lämnar den här växeln **inaktiverad** måste du återskapa och aktivera alla [!UICONTROL on-device decisioning]-aktiviteter för att de ska kunna inkluderas i den genererade regelartefakten.

## Hur vet jag att en aktivitet kan [!UICONTROL on-device decisioning]?

När du har skapat en aktivitet visas en etikett med namnet **[!UICONTROL Decisioning Method]** på aktivitetsdetaljsidan om aktiviteten är [!UICONTROL on-device decisioning]-kompatibel.

![alt-bild](assets/asset-odd9.png)

Du kan också se alla aktiviteter som är [!UICONTROL on-device decisioning] kapabla på sidan **[!UICONTROL Activities]** genom att lägga till kolumnen **[!UICONTROL Decisioning Method]** i listan över aktiviteter.

![alt-bild](assets/asset-odd7.png)

>[!NOTE]
>
>När du har skapat och aktiverat en aktivitet som är [!UICONTROL on-device decisioning]-kompatibel kan det ta upp till 20 minuter innan den ingår i den regelartefakt som genereras och sprids till Akamai CDN PoPs.

## Vad är sammanfattningen av de steg som jag måste följa för att se till att mina [!UICONTROL on-device decisioning]-aktiviteter kan levereras via serversidesSDK för [!DNL Adobe Target]?

1. Gå till användargränssnittet för [!DNL Adobe Target] och navigera till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** för att aktivera alternativet **[!UICONTROL On-Device Decisioning]**.
1. Aktivera växlingsknappen **[!UICONTROL Include all existing [!UICONTROL on-device decisioning] qualified activities in the artifact]**.
1. Skapa och aktivera en aktivitetstyp som stöds av [!UICONTROL on-device decisioning] och verifiera att **[!UICONTROL Decisioning Method]** är **[!UICONTROL On-Device Decisioning]** för den aktiviteten.
1. Installera och initiera [Node.js](../../node-js/overview.md) eller [Java](../../java/overview.md) SDK med `decisioningMethod = on-device`.
1. Implementera `getOffers()` eller `getAttributes()` i koden för att hämta en upplevelse på enheten.
1. Distribuera koden.

Exempel som visar hur du kommer igång med steg 1-3 ovan finns i avsnittet [Komma igång](../getting-started/getting-started.md).


## Ytterligare resurser

### Webbinarium: Anpassa och testa med nolltidsfördröjning med enhetsbeslut från [!DNL Adobe Target]

Nu mer än någonsin har marknadsförare, produktägare och utvecklare fått i uppgift att optimera den övergripande kundupplevelsen på webbplatser, i appar och på alla andra sätt de har kontakt med sina kunder. Det finns inte tillräckligt med flera verktyg med vattentäta skott och komplicerade implementeringar.

I det här inspelade webbinariet diskuterar [!DNL Adobe Target] produktexperter hur rörliga optimeringsbeslut för kritiska upplevelser på enheten som körs lokalt med nästan ingen latens kan öppna dörrar för spännande nya användningsfall samtidigt som webbplatsens prestanda förbättras för dina kunder.

>[!VIDEO](https://video.tv.adobe.com/v/328148/?quality=12)


### Självstudiekurs: Enhetsbeslut

[!DNL Adobe Target] [!UICONTROL on-device decisioning] aktiverar innehållsleverans med nästan noll latens.

Den här 7-minutersvideon:

* Beskriver [!UICONTROL on-device decisioning], inklusive hur den jämförs med andra metoder för [!DNL Target]-implementering
* Visar hur du aktiverar [!UICONTROL on-device decisioning] i mål
* Undersöker en formulärbaserad dispositionsaktivitet som har konfigurerats med JSON-innehåll
* Visar exempelkoden för Node.JS SDK som innehåller den nyckelkonfiguration som krävs för [!UICONTROL on-device decisioning]
* Visar resultat i en webbläsare

>[!VIDEO](https://video.tv.adobe.com/v/329032/?quality=12)

Fler videoklipp och självstudiekurser finns i [[!DNL Adobe Target] Tutorials](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html?lang=sv-SE).

### Adobe Tech Blog - Del 1: Kör [!DNL Adobe Target] NodeJS SDK för experiment och personalisering på kantplattformar (Akamai Edge Workers)

[Klicka här för att komma åt blogginlägget](https://medium.com/adobetech/part-1-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-4d8660964ed9).

### Adobe Tech Blog - Del 2: Kör [!DNL Adobe Target] NodeJS SDK för experiment och personalisering på kantplattformar (AWS Lambda@Edge)

[Klicka här för att komma åt blogginlägget](https://medium.com/adobetech/part-2-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-aws-4d6bdac24563).
