---
title: Rekommendationer implementeringsmönster med at.js
description: Förstå hur implementeringsmönstret används för rekommendationer med at.js
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: d568cd1d-acc3-42e0-ae2c-5787e6f361f8
source-git-commit: 3b0bc0b67800ed4b1da6ba2bfa05c677147a78ba
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# [!DNL Recommendations] implementeringsmönster med översikten at.js

Det här implementeringsmönstret hjälper dig att förstå och skapa din [!DNL Adobe Target Recommendations]-implementering när du använder JavaScript-biblioteket [&#x200B; at.js](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md).

Klicka på bilden för att expandera till helskärm.

![Adobe Target-arkitekturdiagram](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

Observera att siffrorna i bilden inte anger sekvensen av åtgärder:

1. SDK för klientsidan för [!DNL Adobe Target] och [!DNL Experience Cloud ID Service]
1. [!DNL Target Delivery API] samtal
1. [!UICONTROL Experience Cloud ID] (ECID) förvärvssamtal
1. API för gruppprofiluppdatering och tjänsten [!DNL Customer Attributes] (CA)
1. Inmatning av profildata från kundens datakällor till profilarkivet [!DNL Target]
1. Samla in profil- och beteendedata och bestämma vilken upplevelse som ska visas för besökaren
1. Erfarenheter återges på sidan
1. at.js återger upplevelserna på sidan

Varje mönster består av olika delar, där varje del motsvarar ett kritiskt implementeringskrav för din [!DNL Target]-implementering.

Varje del förklaras i en separat artikel i den här handboken:

* [Initiera SDKS](/help/dev/patterns/recs-atjs/initialize-sdk.md)
* [Konfigurera datainsamling](/help/dev/patterns/recs-atjs/data-collection.md)
* [Återge upplevelser](/help/dev/patterns/recs-atjs/render-experiences.md)
* [Meddela mål](/help/dev/patterns/recs-atjs/notify-target.md)
