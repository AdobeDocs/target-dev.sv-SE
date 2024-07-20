---
keywords: serversida, server-side, api, sdk, node.js, nodats, node js, recommendations api, api, apis, server side1
description: Lär dig mer om  [!DNL Adobe Target] API:er för leverans på serversidan, SDK:er och [!DNL Target Recommendations] API:er.
title: Var kan jag läsa mer om  [!DNL Target] API:er och SDK:er för serversideleverans?
feature: Implement Server-side
exl-id: 3eb0a789-cf1a-4d02-acf7-3c895bcb662f
source-git-commit: 75af30045684b95d5989b0a1f877ba95bb8cd883
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---

# Serversida: implementera [!DNL Target]

Information om [!DNL Adobe Target] leverans-API:er på serversidan, SDK:er och [!DNL Target Recommendations] API:er.

>[!NOTE]
>
>Om implementeringen använder at.js och [!DNL AppMeasurement] på klientsidan bör du använda de SDK:er för [!UICONTROL Target Delivery API] och serversidan som beskrivs nedan.
>
>Om din implementering använder [!UICONTROL Adobe Experience Platform Web SDK] bör du använda [[!UICONTROL Adobe Experience Platform] [!UICONTROL Edge Network Server API]](https://experienceleague.adobe.com/en/docs/experience-platform/edge-network-server-api/overview){target=_blank}.

Följande process utförs i en implementering på serversidan av [!DNL Target]:

1. En klientenhet begär en upplevelse via servern.
1. Servern skickar den begäran till [!DNL Target].
1. [!DNL Target] skickar tillbaka svaret till servern.
1. Servern bestämmer vilken upplevelse som ska levereras till klientenheten för att den ska kunna återges.

Upplevelsen behöver inte visas i en webbläsare. Upplevelsen kan visas i ett e-postmeddelande eller i en kioskdator, via en röstassistent eller via någon annan icke-visuell upplevelse eller icke-webbläsarbaserad enhet. Eftersom servern finns mellan klienten och [!DNL Target] är den här typen av implementering också idealisk om du behöver större kontroll och säkerhet eller har komplexa serverprocesser som du vill köra på servern.

>[!NOTE]
>
>En förstagångsbesökare kan bara initieras på klientsidan. En förstagångsbesökare *kan inte* initieras på serversidan. Detta beror på ECID, som är beroende av en tredjeparts demdex-cookie och därför måste initieras via Visitor API.js på en implementering där webbläsaren är inblandad.

I följande avsnitt finns mer information om de olika API:erna och SDK:erna på serversidan:

## API:er för leverans på serversidan

Länk: [API:er för leverans på serversidan](/help/dev/implement/delivery-api/overview.md)

`/rest/v1/delivery`

Via leverans-API:t för [!DNL Target] kan du:

* Leverera upplevelser via webben, inklusive SPA, mobilkanaler och icke-webbläsarbaserade IoT-enheter, som anslutna tv-apparater, kioskdatorer eller digitala butiksskärmar.
* Leverera upplevelser från alla plattformar eller applikationer på serversidan som kan ringa HTTP/s-samtal.
* Leverera enhetliga och personaliserade upplevelser till en besökare oavsett vilken kanal eller vilka enheter besökaren använde för att interagera med ert företag.
* Cachelagra upplevelser för en besökare i en session på servern så att flera API-anrop kan undvikas, vilket ger bättre prestanda.
* Integrera smidigt med Adobe Experience Cloud-produkter som Adobe Analytics, Adobe Audience Manager (AAM) och Experience Cloud ID-tjänsten från serversidan.

## SDK på serversidan

Med hjälp av SDK-dokumentationen på serversidan [!DNL Adobe Target] kan du implementera [!DNL Target] på dina servrar på det språk du föredrar.

* [Node.js](node-js/overview.md)
* [Java](java/overview.md)
* [.NET](net/overview.md)
* [Python](python/overview.md)

Genom serversidessDK:er för [!DNL Adobe Target] kan du:

* Kör och kör **funktionsflaggning**, **rollouts** och **A/B-experiment** vid **nära-noll-fördröjning**.
* Leverera upplevelser på **webben**, inklusive **SPA** och **mobila kanaler**, liksom icke-webbläsarbaserade **sakernas Internet-enheter**, till exempel en ansluten TV, kioskskärm eller en digital butiksskärm.
* Leverera **ML-drivna personaliserade upplevelser** till en användare, oavsett vilken kanal eller enhet användaren har interagerat med företaget.
* **Integrera sömlöst med Adobe Experience Cloud**-produkter som **Adobe Analytics**, **Adobe Audience Manager** och **Experience Cloud ID-tjänsten** från serversidan.

Gå till sidan [Komma igång](sdk-guides/getting-started/getting-started.md) om du vill lära dig hur du kör en enkel funktion som flaggar användningsfall via [enhetsbeslut](sdk-guides/on-device-decisioning/overview.md).

Kolla in våra [exempelappar](sdk-guides/sample-apps/sample-apps.md) för att ha kul och leka med!

## [!DNL Target Recommendations] API:er

Länk: [Mål-API:er för Recommendations](https://developers.adobetarget.com/api/recommendations) och [Adobe Recommendations API - översikt](../../before-administer/recs-api/overview.md).

Med Recommendations API:er kan du programmässigt interagera med [!DNL Target] rekommendationsservrar. Dessa API:er kan integreras med en rad programstackar för att utföra funktioner som du vanligtvis gör via användargränssnittet i [!DNL Target].
