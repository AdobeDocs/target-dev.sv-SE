---
keywords: guide för målutvecklare; översikt;hem
title: Adobe Target Developer Guide
description: Hur implementerar och administrerar jag  [!DNL Adobe Target]  och arbetar med dess API:er och SDK:er?
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: 655cff9b-fc04-45cf-9068-5c6c32b70d79
source-git-commit: dadc3804da4592dba4ad88b8c5c9f804c56e232b
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---

# Utvecklarhandbok för [!DNL Adobe Target]

**([Visa [!DNL Target] dokumentationsuppdateringar](https://experienceleague.adobe.com/docs/target/using/release-notes/doc-change.html?lang=sv-SE){target=_blank})**

Den här *[!DNL Adobe Target]utvecklarhandboken* innehåller resurser och guider för [!DNL Target]-utvecklare, inklusive API- och SDK-dokumentation för implementering och administration av [!DNL Target].

>[!NOTE]
>
>Förutom den här guiden finns även följande [!DNL Adobe Target] stödlinjer tillgängliga:
>
>* [*[!DNL Adobe Target] Business Practitioner Guide *](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=sv-SE){target=_blank}
>
>* [*[!DNL Adobe Target] Tutorials *](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html?lang=sv-SE){target=_blank}
>
>Versionsinformation finns i [Versionsinformation för mål (aktuell)](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html?lang=sv-SE){target=_blank} i *[!DNL Adobe Target]Business Practitioner Guide* .

## Komma igång med implementering

**[Innan du implementerar](/help/dev/before-implement/considerations-before-you-implement-target.md)**: Du bör tänka på att åtgärda innan du implementerar [!DNL Adobe Target].

## Implementering på klientsidan

[**Adobe Experience Platform Web SDK**](/help/dev/implement/client-side/aep-web-sdk.md): Med [!DNL Adobe Experience Platform Web SDK] kan du interagera med de olika tjänsterna i [!DNL Experience Cloud] (inklusive [!DNL Target]) via [!UICONTROL Adobe Experience Edge Network].

[**Använd JavaScript-biblioteket at.js som mål**](/help/dev/implement/client-side/overview.md): JavaScript-biblioteket at.js förbättrar sidinläsningstiden för webblöplementeringar, förbättrar säkerheten och erbjuder bättre implementeringsalternativ för ensidiga program.

## Implementering på serversidan

[**Översikt över mål-SDK**](implement/server-side/server-side-overview.md): Kom igång med [!DNL Adobe Target] SDK:er, inklusive beslut på enheten.

[**Node.js SDK**](implement/server-side/node-js/overview.md): Så här använder du [!DNL Target] Node.js SDK.

[**Java SDK**](implement/server-side/java/overview.md): Så här använder du Java SDK [!DNL Target] .

[**.NET SDK**](implement/server-side/net/overview.md): Så här använder du [!DNL Target] .NET SDK.

[**Python SDK**](implement/server-side/python/overview.md): Så här använder du [!DNL Target] Python SDK.

## Hybrid-implementering

[**Hybrid-distribution**](implement/hybrid/hybrid-overview.md): Implementera [!DNL Target] med en kombination av implementering på klient- och serversidan.

## Implementering av Recommendations

[**Recommendations-implementering**](implement/recommendations/recommendations.md): Planera och implementera [!DNL Adobe Target Recommendations].

## Implementering av mobilappar

[**Översikt över AEP Mobile SDK**](implement/mobile/overview.md): Översikt över hur du implementerar [!DNL Adobe Target] med [!DNL Adobe Experience Platform] Mobile SDK:er.

[**AEP Mobile SDK-referens**](https://developer.adobe.com/client-sdks/documentation/): Implementera [!DNL Adobe Target] med [!DNL Adobe Experience Platform] Mobile SDK:er.

## E-postimplementering

[**E-postöversikt**](implement/email/overview.md): Översikt över hur du implementerar [!DNL Adobe Target] i e-postmeddelanden.

## API-guider

[**Introduktion**](before-administer/target-api-overview.md): Översikt över [!DNL Adobe Target] API:er.

[**[!DNL Target Delivery API]**](/help/dev/implement/delivery-api/overview.md): Använd [!DNL Adobe Target]-leverans-API:erna för att leverera upplevelser över webb- och mobilkanaler samt icke-webbläsarbaserade IoT-enheter som en ansluten TV, kioskdator eller en digital butiksskärm.

[**[!DNL Target Admin API]**](administer/admin-api/admin-api-overview-new.md): Använd [!DNL Adobe Target] Admin-API:t för att hantera egenskaper, aktiviteter, målgrupper, erbjudanden, egenskaper, rapporter, mbox, värdar, miljöer med mera.

[**[!DNL Target Profile API]**](/help/dev/administer/profile-api/profiles-api.md): Hämta [!DNL Adobe Target] användarprofilinformation.

[**[!DNL Target Reporting API]**](https://developer.adobe.com/target/administer/admin-api/#tag/Reports): Hämta [!UICONTROL A/B Test] och [!UICONTROL Automated Personalization] aktivitetsrapportdata.

[**[!DNL Target Recommendations API]**](https://developer.adobe.com/target/administer/recommendations-api/): Använd API:t [!DNL Target Recommendations].

[**[!DNL Target Models API]**](administer/models-api/models-api-overview.md): Hantera blockeringslista för att definiera funktionerna som används i [!DNL Target] maskininlärningsmodeller.

[**Admin Console API:er**](https://developer.adobe.com/umapi/): Hantera användare och produktbehörigheter via API:erna för användarhantering och användarsynkronisering i Adobe.

[**[!DNL Adobe Experience Platform Edge Network Server API]**](https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/overview.html?lang=sv-SE): Använd API:t [!DNL Adobe Experience Platform Edge Network Server] för en mängd olika användningsfall för datainsamling, personalisering, annonsering och marknadsföring.

## Resurs

* [Adobe, öppen källkodsrepo](https://github.com/adobe)
* [JS SDK-målnod, Source](https://github.com/adobe/target-nodejs-sdk)
* [JS SDK-exempelrepo för målnod](https://github.com/adobe/target-nodejs-sdk-samples)
* [Aktivera Java SDK Source](https://github.com/adobe/target-java-sdk)
* [Java SDK-exempelrepo](https://github.com/adobe/target-java-sdk-samples) som mål
* [Målimplementering](./before-implement/prepare-to-implement-target.md)
* [Måladministration](./before-administer/target-api-overview.md)
* [Adobe Target Dev Docs GitHub Repo](https://github.com/AdobeDocs/target-developers)
* [Versionsinformation för Adobe Target](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html?lang=sv-SE)
* [Adobe Target Business User Guide](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=sv-SE)

