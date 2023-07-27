---
keywords: guide för målutvecklare; översikt
title: Adobe Target Developer Guide
description: Hur implementerar och administrerar jag  [!DNL Adobe Target]  och arbetar med dess API:er och SDK:er?
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: 655cff9b-fc04-45cf-9068-5c6c32b70d79
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 2%

---

# [!DNL Adobe Target] Utvecklarhandbok

![Adobe Target banner image](/help/dev/assets/target-home-banner-simple.png)

**([Visa [!DNL Target] dokumentuppdateringar](https://experienceleague.adobe.com/docs/target/using/release-notes/doc-change.html){target=_blank})**

Detta *[!DNL Adobe Target]Utvecklarhandbok* innehåller resurser och guider för [!DNL Target] utvecklare, inklusive API- och SDK-dokumentation för implementering och administration [!DNL Target].

>[!NOTE]
>
>Utöver den här guiden finns följande [!DNL Adobe Target] Det finns även stödlinjer:
>
>* [*[!DNL Adobe Target] Handbok för affärspersonalen *](https://experienceleague.adobe.com/docs/target/using/target-home.html){target=_blank}
>
>* [*[!DNL Adobe Target] Tutorials *](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html){target=_blank}
>
>Versionsinformation finns i [Versionsinformation för mål (aktuell)](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html){target=_blank} i *[!DNL Adobe Target]Handbok för affärspersonalen*.

## Komma igång med implementering

**[Innan du implementerar](/help/dev/before-implement/considerations-before-you-implement-target.md)**: Du bör tänka på saker innan du implementerar [!DNL Adobe Target].

## Implementering på klientsidan

[**Adobe Experience Platform Web SDK**](/help/dev/implement/client-side/aep-web-sdk.md): [!DNL Adobe Experience Platform Web SDK] kan du interagera med de olika tjänsterna i [!DNL Experience Cloud] (inklusive [!DNL Target]) via [!UICONTROL Adobe Experience Edge Network].

[**Target at.js JavaScript-bibliotek**](/help/dev/implement/client-side/overview.md): JavaScript-biblioteket at.js förbättrar sidinläsningstiderna för webbimplementeringar, förbättrar säkerheten och erbjuder bättre implementeringsalternativ för enkelsidiga program.

## Implementering på serversidan

[**Översikt över mål-SDK**](implement/server-side/server-side-overview.md): Kom igång med [!DNL Adobe Target] SDK:er, inklusive beslut på enheten.

[**Node.js SDK**](implement/server-side/node-js/overview.md): Använda [!DNL Target] Node.js SDK.

[**Java SDK**](implement/server-side/java/overview.md): Använda [!DNL Target] Java SDK.

[**.NET SDK**](implement/server-side/net/overview.md): Använda [!DNL Target] .NET SDK.

[**Python SDK**](implement/server-side/python/overview.md): Använda [!DNL Target] Python SDK.

## Hybrid-implementering

[**Hybrid-distribution**](implement/hybrid/hybrid-overview.md): Implementering [!DNL Target] med en kombination av implementering på klient- och serversidan.

## Implementering av Recommendations

[**Implementering av Recommendations**](implement/recommendations/recommendations.md): Planera och implementera [!DNL Adobe Target Recommendations].

## Implementering av mobilappar

[**AEP Mobile SDK - översikt**](implement/mobile/overview.md): Översikt över implementering [!DNL Adobe Target] med [!DNL Adobe Experience Platform] SDK för mobiler.

[**AEP Mobile SDK-referens**](https://developer.adobe.com/client-sdks/documentation/): Implementering [!DNL Adobe Target] med [!DNL Adobe Experience Platform] SDK för mobiler.

## E-postimplementering

[**E-postöversikt**](implement/email/overview.md): Översikt över implementering [!DNL Adobe Target] i e-postmeddelanden.

## API-guider

[**Introduktion**](before-administer/target-api-overview.md): Översikt över [!DNL Adobe Target] API.

[**[!DNL Target Delivery API]**](/help/dev/implement/delivery-api/overview.md): Använd [!DNL Adobe Target] Leverera API:er för att leverera upplevelser över webb- och mobilkanaler samt icke-webbläsarbaserade IoT-enheter som en ansluten TV, kioskdator eller digital butiksskärm.

[**[!DNL Target Admin API]**](administer/admin-api/admin-api-overview-new.md): Använd [!DNL Adobe Target] Admin-API för att hantera egenskaper, aktiviteter, målgrupper, erbjudanden, egenskaper, rapporter, mbox, värdar, miljöer med mera.

[**[!DNL Target Profile API]**](https://developers.adobetarget.com/api/#profiles): Hämta [!DNL Adobe Target] användarprofilinformation.

[**[!DNL Target Reporting API]**](https://developer.adobe.com/target/administer/admin-api/#tag/Reports): Hämta [!UICONTROL A/B Test] och [!UICONTROL Automated Personalization] aktivitetsrapportdata.

[**[!DNL Target Recommendations API]**](http://developers.adobetarget.com/api/recommendations/): Använd [!DNL Target Recommendations] API.

[**[!DNL Target Models API]**](administer/models-api/models-api-overview.md): Hantera blockeringslista för att definiera de funktioner som används i [!DNL Target] maskininlärningsmodeller.

[**Admin Console API:er**](https://developer.adobe.com/umapi/): Hantera användare och produktbehörigheter via Adobe API:er för användarhantering och användarsynkronisering.

[**[!DNL Adobe Experience Platform Edge Network Server API]**](https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/overview.html): Använd [!DNL Adobe Experience Platform Edge Network Server] API för en mängd olika användningsfall för datainsamling, personalisering, annonsering och marknadsföring.

## Resurs

* [Adobe öppen källkod](https://github.com/adobe)
* [JS SDK-källa för målnod](https://github.com/adobe/target-nodejs-sdk)
* [Exempelrepo för JS SDK för målnod](https://github.com/adobe/target-nodejs-sdk-samples)
* [Mål-Java SDK-källa](https://github.com/adobe/target-java-sdk)
* [Java SDK-exempelrepo](https://github.com/adobe/target-java-sdk-samples)
* [Målimplementering](./before-implement/prepare-to-implement-target.md)
* [Måladministration](./before-administer/target-api-overview.md)
* [Adobe Target Dev Docs GitHub Repo](https://github.com/AdobeDocs/target-developers)
* [Versionsinformation för Adobe Target](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html)
* [Adobe Target Business User Guide](https://experienceleague.adobe.com/docs/target/using/target-home.html)

