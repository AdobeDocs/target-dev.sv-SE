---
title: Logga A4T-data på serversidan i Experience Platform Web SDK
description: Lär dig hur du aktiverar serverloggning för Adobe Analytics for Target (A4T) med Experience Platform Web SDK.
seo-title: Server-side logging for A4T data in Experience Platform Web SDK
seo-description: Learn how to enable server-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: a4t;target;web;sdk;platform;log;
feature: Implementation
source-git-commit: b1b0424bfe61fb8b4e88723e6bb2c565d75f8351
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Logga A4T-data på serversidan i [!DNL Experience Platform Web SDK]

Med [!DNL Adobe Experience Platform Web SDK] kan du implementera [!UICONTROL Adobe Analytics for Target] (A4T)-funktioner på [!UICONTROL Experience Platform Edge Network]. När loggning på serversidan är aktiverad utökas alla [!DNL Analytics] träffar som skickas via Edge Network med [!DNL Target] information på serversidan, utan att du behöver gå igenom träffsammanfogningsprocessen.

Loggning på serversidan för [!DNL Analytics] aktiveras när [!DNL Analytics] aktiveras i datastream-konfigurationen:

![Dataströmskonfiguration för analys har aktiverats](/help/dev/implement/a4t/assets/enable-analytics-datastream.png)

I följande diagram visas hur data flödar genom systemet när loggning på serversidan [!DNL Analytics] är aktiverad:

![Loggningsflöde på serversidan](/help/dev/implement/a4t/assets/analytics-server-side-logging.png)

## Nästa steg

Den här guiden täcker serverloggning av A4T-data i Web SDK. Mer information om hur du hanterar A4T-data på klientsidan finns i handboken om [klientloggning](/help/dev/implement/a4t/client-side-logging.md).