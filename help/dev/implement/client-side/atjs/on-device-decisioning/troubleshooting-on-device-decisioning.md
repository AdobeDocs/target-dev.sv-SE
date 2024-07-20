---
keywords: implementering, javascript-bibliotek, js, atjs, on-device decisioning, on device decisioning, at.js, on device, on device, troubleshooting, trouble shots capture, implementation2
description: Lär dig hur du felsöker [!UICONTROL on-device decisioning] med biblioteket at.js.
title: Hur felsöker jag Enhetsbeslut med JavaScript-biblioteket at.js?
feature: at.js
exl-id: b9530cc7-5e83-4fdf-bde9-b2492e0861ff
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Felsökning av [!UICONTROL on-device decisioning] för at.js

Utför följande steg för att felsöka [!UICONTROL on-device decisioning] i [!UICONTROL Adobe Target] med JavaScript-biblioteket at.js:

## Steg 1: Aktivera konsolloggen för at.js

Om URL-parametern `mboxDebug=1` läggs till aktiveras at.js för att skriva ut meddelanden i webbläsarens konsol.

Alla meddelanden innehåller prefixet&quot;AT:&quot; för praktisk översikt. För att säkerställa att en artefakt har lästs in bör konsolloggen innehålla meddelanden som liknar följande:

```
AT: LD.ArtifactProvider fetching artifact - https://assets.adobetarget.com/your-client-cide/production/v1/rules.json
AT: LD.ArtifactProvider artifact received - status=200
```

Följande bild visar dessa meddelanden i konsolloggen:

(Klicka på bilden för att expandera till full bredd.)

![Konsollogg med artefaktmeddelanden](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/browser-console.png "Konsollogg med artefaktmeddelanden"){zoomable="yes"}

## Steg 2: Verifiera regelartefaktnedladdningen på fliken Nätverk i webbläsaren

Öppna fliken Nätverk i webbläsaren.

Om du till exempel vill öppna DevTools i Google Chrome:

1. Tryck på Ctrl+Skift+J (Windows) eller Kommando+Alt+J (Mac).
1. Gå till fliken Nätverk.
1. Filtrera dina anrop med nyckelordet &quot;rules.json&quot; för att säkerställa att endast artefaktregelfilen visas.

   Dessutom kan du filtrera efter &quot;/delivery|rules.json/&quot; för att visa alla Target-anrop och artefaktregler.json.

   ![Fliken Nätverk i Google Chrome](assets/rule-json.png)

## Steg 3: Verifiera regelartefaktnedladdningen med anpassade at.js-händelser

At.js-biblioteket skickar två nya anpassade händelser som stöder [!UICONTROL on-device decisioning].

* `adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED`
* `adobe.target.event.ARTIFACT_DOWNLOAD_FAILED`

Du kan prenumerera för att lyssna på dessa anpassade händelser i ditt program för att agera när artefaktregelfilen har hämtats eller inte.

I följande exempel visas ett exempel på kod som avlyssnar lyckade artefaktnedladdningar och felhändelser:

```javascript {line-numbers="true"}
document.addEventListener(adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED, function(e) { 
  console.log("Artifact successfully downloaded", e.detail);
}, false);

document.addEventListener(adobe.target.event.ARTIFACT_DOWNLOAD_FAILED, function(e) { 
  console.log("Artifact failed to download", e.detail);
}, false);
```
