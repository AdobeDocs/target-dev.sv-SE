---
keywords: serversida, server-side, api, sdk, node.js, nodats, node js, recommendations api, api, apis, server side1
description: Läs mer om [!DNL Adobe Target] server-side Delivery APIs, SDKs och [!DNL Target Recommendations] API.
title: Var kan jag läsa mer om [!DNL Target] Leverans-API:er och SDK på serversidan?
feature: Implement Server-side
exl-id: 3eb0a789-cf1a-4d02-acf7-3c895bcb662f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---

# Serversida: implementera [!DNL Target]

Information om [!DNL Adobe Target] server-side Delivery APIs, SDKs och [!DNL Target Recommendations] API.

Följande process utförs i en implementering på serversidan av [!DNL Target]:

1. En klientenhet begär en upplevelse via servern.
1. Servern skickar begäran till [!DNL Target].
1. [!DNL Target] skickar tillbaka svaret till servern.
1. Servern bestämmer vilken upplevelse som ska levereras till klientenheten för att den ska kunna återges.

Upplevelsen behöver inte visas i en webbläsare. Upplevelsen kan visas i ett e-postmeddelande eller i en kioskdator, via en röstassistent eller via någon annan icke-visuell upplevelse eller icke-webbläsarbaserad enhet. Eftersom servern är placerad mellan klienten och [!DNL Target]är den här typen av implementering också idealisk om du behöver större kontroll och säkerhet eller har komplexa serverprocesser som du vill köra på servern.

>[!NOTE]
>
>En förstagångsbesökare kan bara initieras på klientsidan. En förstagångsbesökare *inte* initieras på serversidan. Detta beror på ECID, som är beroende av en tredjeparts demdex-cookie och därför måste initieras via Visitor API.js på en implementering där webbläsaren är inblandad.

I följande avsnitt finns mer information om de olika API:erna och SDK:erna på serversidan:

## API:er för leverans på serversidan

Länk: [API:er för leverans på serversidan](/help/dev/implement/delivery-api/overview.md)

`/rest/v1/delivery`

Via [!DNL Target] Delivery API, du kan:

* Leverera upplevelser via webben, inklusive SPA, mobilkanaler och icke-webbläsarbaserade IoT-enheter, som anslutna tv-apparater, kioskdatorer eller digitala butiksskärmar.
* Leverera upplevelser från alla plattformar eller applikationer på serversidan som kan ringa HTTP/s-samtal.
* Leverera enhetliga och personaliserade upplevelser till en besökare oavsett vilken kanal eller vilka enheter besökaren använde för att interagera med ert företag.
* Cachelagra upplevelser för en besökare i en session på servern så att flera API-anrop kan undvikas, vilket ger bättre prestanda.
* Integrera smidigt med Adobe Experience Cloud-produkter som Adobe Analytics, Adobe Audience Manager (AAM) och Experience Cloud ID-tjänsten från serversidan.

## SDK på serversidan

The [!DNL Adobe Target] SDK-dokumentation på serversidan hjälper dig att implementera [!DNL Target] på valfritt språk.

* [Node.js](node-js/overview.md)
* [Java](java/overview.md)
* [.NET](net/overview.md)
* [Python](python/overview.md)

Via [!DNL Adobe Target]Med SDK:er på serversidan kan du

* Kör **funktionsflaggning**, **rollouts** och **A/B-experiment** på **latens nära noll**.
* Leverera upplevelser i **webb**, inklusive **SPA** och **mobilkanaler**, liksom icke-webbläsarbaserade **IoT-enheter (Internet of Things)** som en ansluten TV, kioskskärm eller digital butiksskärm.
* Leverera **Maskininlärningsdrivna, personaliserade upplevelser** för en användare, oavsett vilken kanal eller enhet användaren har interagerat med företaget.
* **Smidig integrering med Adobe Experience Cloud** produkter som **Adobe Analytics**, **Adobe Audience Manager** och **Experience Cloud ID-tjänst** från serversidan.

Se [Komma igång](sdk-guides/getting-started/getting-started.md) om du vill lära dig hur du kör en enkel funktion som flaggar [beslut på enheten](sdk-guides/on-device-decisioning/overview.md).

Kolla in vår [Exempelappar](sdk-guides/sample-apps/sample-apps.md) för att ha kul och leka!

## [!DNL Target Recommendations] API:er

Länk: [Ange Recommendations API:er](https://developers.adobetarget.com/api/recommendations) och [Adobe Recommendations API - översikt](../../before-administer/recs-api/overview.md).

Med Recommendations API:er kan du programmässigt interagera med [!DNL Target] rekommendationsservrar. Dessa API:er kan integreras med en rad programstackar för att utföra funktioner som du vanligtvis gör via [!DNL Target] användargränssnitt.
