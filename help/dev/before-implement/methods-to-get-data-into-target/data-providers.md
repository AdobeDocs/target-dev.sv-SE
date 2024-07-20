---
keywords: implementera, implementera, konfigurera, konfigurera, dataleverantörer
description: Hämta data till  [!DNL Target] med dataleverantörer.
title: Hur hämtar jag data till  [!DNL Target] Använda Data Providers?
feature: Implementation
exl-id: 9971bd96-f736-4965-afe2-b4901c12d006
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---

# Dataleverantörer

Dataleverantörer är en funktion som gör att du enkelt kan skicka data från tredje part till [!DNL Adobe Target].

>[!NOTE]
>
>Dataleverantörer kräver at.js 1.3 eller senare.

## Format

Inställningen `window.targetGlobalSettings.dataProviders` är en matris med dataleverantörer.

Mer information om strukturen för varje dataleverantör finns i [Data Providers](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers).

## Exempel på användningsområden

Samla in data från tredje part, t.ex. en vädertjänst, en datahanteringsplattform eller till och med din egen webbtjänst. Sedan kan ni använda dessa data för att skapa målgrupper, målinnehåll och berika besökarprofilen.

## Fördelar med metoden

Med den här inställningen kan kunder samla in data från tredjepartsleverantörer av data, som Demandbase, BlueKai och anpassade tjänster, och skicka data till [!DNL Target] som mbox-parametrar i den globala mbox-begäran.

Det stöder insamling av data från flera leverantörer via asynkrona och synkroniserade begäranden.

Med den här metoden blir det enkelt att hantera flimmer av standardsidinnehåll, samtidigt som oberoende tidsgränser för varje leverantör tas med för att begränsa effekten på sidans prestanda

## Caveats

Om dataleverantörerna som läggs till i `window.targetGlobalSettings.dataProviders` är asynkrona körs de parallellt. Visitor-API-begäran körs parallellt med funktioner som lagts till i `window.targetGlobalSettings.dataProviders` för att ge en minimal väntetid.

at.js försöker inte cachelagra data. Om dataleverantören bara hämtar data en gång, bör dataleverantören se till att data cachelagras och, när providerfunktionen anropas, hantera cachedata för det andra anropet.

## Exempel på koder

Flera exempel finns i [Data Providers](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers).

## Länkar till relevant information

Dokumentation: [Data Providers](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)

## Utbildningsvideor:

* [Använda Data Providers i Adobe Target](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html)
* [Implementera Data Providers i Adobe Target](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html)
