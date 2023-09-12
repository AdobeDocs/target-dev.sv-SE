---
title: Recommendations implementeringsmönster med at.js
description: Förstå hur implementeringsmönstret för Recommendations används med at.js
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 752c52c0db5173f49fd828c297fa7afd7c53c6ce
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# [!DNL Recommendations] implementeringsmönster med hjälp av översikten at.js

Detta implementeringsmönster hjälper dig att förstå och skapa [!DNL Adobe Target Recommendations] implementering när du använder [at.js JavaScript-bibliotek](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md).

Klicka på bilden för att expandera till helskärm.

![Adobe Target - arkitektur](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

Observera att siffrorna i bilden inte anger sekvensen av åtgärder:

1. SDK för klientsidan för [!DNL Adobe Target] och [!DNL Experience Cloud ID Service]
1. [!DNL Target Delivery API] ring
1. [!UICONTROL Experience Cloud ID] (ECID) kundvärvningssamtal
1. API för gruppprofilsuppdatering och [!DNL Customer Attributes] (CA)-tjänst
1. Intag av profildata från kundens datakällor till [!DNL Target] profilarkiv
1. Samla in profil- och beteendedata och bestämma vilken upplevelse som ska visas för besökaren
1. Erfarenheter återges på sidan
1. at.js återger upplevelserna på sidan

Varje mönster består av olika delar, där varje del motsvarar ett kritiskt implementeringskrav för din [!DNL Target] implementering.

Varje del förklaras i en separat artikel i den här handboken:

* [Initiera SDKS](/help/dev/patterns/recs-atjs/initialize-sdk.md)
* [Konfigurera datainsamling](/help/dev/patterns/recs-atjs/data-collection.md)
* [Återge upplevelser](/help/dev/patterns/recs-atjs/render-experiences.md)
* [Meddela mål](/help/dev/patterns/recs-atjs/notify-target.md)

