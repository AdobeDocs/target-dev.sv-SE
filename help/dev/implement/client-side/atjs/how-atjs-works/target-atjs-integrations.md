---
keywords: at.js-integrering, integreringar som stöds, integreringar som inte stöds, tredjepartsintegreringar
description: Se de integreringar som stöds (och inte stöds) av  [!DNL Adobe Target]  at.js, inklusive [!UICONTROL Analytics for Target] (A4T), [!UICONTROL Experience Cloud ID Service] med mera.
title: Vilka integreringar stöder .js?
feature: at.js
exl-id: d2c61e77-5fc7-4c35-905b-76b8c4f9df4b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 0%

---

# at.js-integreringar

Information om vanliga integreringar med [!DNL Adobe Target] och deras supportstatus med at.js.

Om du har ett övertygande behov av en integrering som inte stöds eller nämns här, kan du kontakta din kontorepresentant eller konsult.

## Integreringar som stöds

| Integrering | Information |
|--- |--- |
| [!UICONTROL Analytics for Target] (A4T) | Se [Adobe Analytics som Reporting Source för Adobe Target (A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html). |
| [!UICONTROL Profiles & Audiences] (P&amp;A) | Se [Publiker](https://experienceleague.adobe.com/docs/core-services/interface/audiences/audience-library.html) i *Användarhandboken för bastjänster*. |
| [!UICONTROL Experience Cloud ID Service] | Se [dokumentationen för Adobe Experience Cloud ID-tjänsten](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| [!UICONTROL Tags in Adobe Experience Platform] | [!UICONTROL Tags in Adobe Experience Platform] är nästa generation av tagghanteringsfunktioner från [!DNL Adobe]. [!UICONTROL Tags] ger kunderna ett enkelt sätt att driftsätta och hantera de analys-, marknadsförings- och annonstaggar som behövs för att driva relevanta kundupplevelser. Se [Implementera [!DNL Target] med Adobe Experience Platform](../how-to-deployatjs/implement-target-using-adobe-launch.md). |
| Cloud Servicen [!UICONTROL Adobe Experience Manager] (AEM) | Med [!UICONTROL AEM Cloud Service] kan du skapa [!UICONTROL A/B Test]- och [!UICONTROL Experience Targeting]-aktiviteter i AEM. Stöder at.js med [!UICONTROL Adobe Experience Manager] 6.2 med FP-11577 (eller senare). Mer information finns i [Integrera med [!DNL Adobe Target]](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html) och välja AEM. |
| [!UICONTROL AEM Experience Fragments] | Med upplevelsefragment som skapats i AEM i [!DNL Target]-aktiviteter kan du kombinera lättanvända och kraftfulla AEM med kraftfulla funktioner för Automated Intelligence (AI) och Machine Learning (ML) i [!DNL Target] för att testa och personalisera upplevelser i stor skala.  AEM samlar allt innehåll och alla resurser på en central plats för att understödja er personaliseringsstrategi. AEM gör det enkelt att skapa innehåll för datorer, surfplattor och mobila enheter på en och samma plats utan att behöva skriva kod. Du behöver inte skapa sidor för alla enheter. AEM justerar automatiskt varje upplevelse med ditt innehåll.  Se [AEM upplevelsefragment](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html). |

## Integreringar som inte stöds

| Integrering | Information |
|--- |--- |
| Äldre [!DNL Target] till [!DNL SiteCatalyst]-integrering | Det här var integreringen som skickade kampanj- och recept-ID till [!DNL SiteCatalyst] via sidanropet så att du kunde rapportera i [!DNL SiteCatalyst]-gränssnittet. Den här funktionen ersätts av A4T. |
| Äldre [!DNL Target] till [!DNL SiteCatalyst]-integrering | Det här var integreringen som gjorde mbox-anrop med namnen `"SiteCatalyst: Event"` och `"SiteCatalyst: Purchase"` så att du kunde skapa framgångsmått och användarprofiler baserade på variabler och props. Den här funktionen ersätts av A4T och P&amp;A. |
| Äldre [!DNL Audience Manager] (AAM) till [!DNL Target]-integrering | Det här var integreringen som gjorde ett front-end API-anrop för att hämta AAM segment och sedan skicka dem som mbox-parametrar för varje mbox-anrop på sidan. |

## Tredjepartsintegreringar

| Integrering | Information |
|--- |--- |
| Andra tagghanterare | at.js bör fungera med tagghanteringsplattformar som inte är Adobe, men var försiktig med att använda anpassade integreringsfunktioner som andra leverantörer har utvecklat. Deras integreringar kan vara beroende av interna mbox.js-funktioner som inte längre finns i at.js. |
| Tredjepartsleverantörer av data (t.ex. Demandbase, Bluekai, API:er för väder) | Många tredjepartsdataleverantörer som används som komplement till Target-användarprofilering kan integreras med funktionen at.js [Data Providers](../atjs-functions/targetglobalsettings.md#data-providers) . |
