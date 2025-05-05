---
keywords: implementering, javascript-bibliotek, js, atjs, enhetsbeslut, enhetsbeslut, enhetsbeslut, funktioner som stöds, $8
description: Lär dig vilka funktioner som stöds för [!UICONTROL on-device decisioning].
title: Vilka funktioner som stöds i Enhetsbeslut
feature: at.js
exl-id: bdd65658-6c4a-41ae-a222-59c00a11bdac
source-git-commit: 79ffa3f58d780f587fe1202b82d3860395504dfe
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# Funktioner som stöds för [!UICONTROL on-device decisioning]

JS SDK [!DNL Adobe Target] ger kunderna flexibilitet att välja mellan prestanda och aktualitet för data för beslut. Med andra ord, om det viktigaste är att leverera det mest relevanta och engagerande personaliserade innehållet via maskininlärning, bör du ringa ett live-serversamtal. Men när prestanda är viktigare bör man fatta beslut på enheten och i minnet. För att [!UICONTROL on-device decisioning] ska fungera, se följande avsnitt som listar de funktioner som stöds.

## Aktivitetstyper som stöds

Tabellen nedan visar vilka [aktivitetstyper](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=sv-SE) som skapats av [formulärbaserade Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=sv-SE) eller [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=sv-SE) (VEC) som stöds eller som inte stöds för [!UICONTROL on-device decisioning].

| Typ av aktivitet | Stöds? |
| --- | --- |
| [A/B-test](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=sv-SE) | Ja |
| [Automatisk allokering](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=sv-SE) | Nej |
| [Automatiskt mål](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=sv-SE) ![Premium](../../../assets/premium.png) | Nej |
| [Multivariata tester](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html?lang=sv-SE) (MVT) | Nej |
| [Experience Targeting](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=sv-SE) (XT) | Ja |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=sv-SE) ![Premium](../../../assets/premium.png) | Nej |
| [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=sv-SE) ![Premium](../../../assets/premium.png) | Nej |
| [Aktiviteter som använder analys för mål](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=sv-SE&) (A4T) | Ja |

## Målgruppsanpassning

Tabellen nedan visar vilka målgruppsregler som stöds eller inte stöds för [!UICONTROL on-device decisioning].

| Målgruppsregel | Stöds? |
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
| Adobe Experience Cloud-målgrupper<P>([!DNL Audiences from Adobe Analytics], [!DNL Adobe Audience Manager] och [!DNL Adobe Experience Manager]) | Nej |

### Geografisk målinriktning för [!UICONTROL on-device decisioning]

Adobe rekommenderar att du anger geovärdena själv i anropet till [getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) för att behålla minimal fördröjning för [!UICONTROL on-device decisioning]-aktiviteter med geobaserade målgrupper. Ange Geo-objektet i sammanhanget för begäran. Det innebär att webbläsaren ska avgöra var besökarna befinner sig. Du kan till exempel utföra en IP-till-Geo-sökning med en tjänst som du konfigurerar. Vissa värdtjänstleverantörer, som Google Cloud, tillhandahåller den här funktionen via anpassade rubriker i varje `HttpServletRequest`.

```javascript {line-numbers="true"}
window.adobe.target.getOffers({ 
    decisioningMethod: "on-device", 
    request: { 
        context: { 
            geo: { 
                city: "SAN FRANCISCO", 
                countryCode: "US", 
                stateCode: "CA", 
                latitude: 37.75, 
                longitude: -122.4 
            } 
        }, 
        execute: { 
            pageLoad: {} 
        } 
    } 
})
```

Om du inte kan utföra IP-till-Geo-sökningar på servern, men ändå vill utföra [!UICONTROL on-device decisioning] för [ getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)-begäranden som innehåller geobaserade målgrupper, stöds detta också. Nackdelen med det här tillvägagångssättet är att det använder en fjärr-IP-till-Geo-sökning, som lägger till fördröjning för varje `getOffers`-anrop. Denna fördröjning bör vara lägre än ett `getOffers`-anrop med beslut på serversidan, eftersom den träffar ett CDN som ligger nära servern. Ange endast fältet&quot;ipAddress&quot; i Geo-objektet i kontexten för din begäran om att SDK ska hämta geoplatsen för din besökares IP-adress. Om något annat fält förutom ipAddress anges kommer SDK:n för [!DNL Target] inte att hämta metadata för geopositionering för upplösning.

```javascript {line-numbers="true"}
window.adobe.target.getOffers({ 
    decisioningMethod: "on-device", 
    request: { 
        context: { 
            geo: { 
                ipAddress: "127.0.0.1" 
            } 
        }, 
        execute: { 
            pageLoad: {} 
        } 
    } 
})
```

### Allokeringsmetod

Följande tabell visar vilka allokeringsmetoder som stöds eller inte stöds för [!UICONTROL on-device decisioning].

| Allokeringsmetod | Stöds? |
| --- | --- |
| Manuell | Ja |
| [Autoallokera till bästa upplevelse](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=sv-SE) | Nej |
| [Automatisk målgruppsanpassning för personaliserade upplevelser](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=sv-SE) | Nej |
