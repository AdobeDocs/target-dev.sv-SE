---
title: Översikt över implementeringsmönster
description: Förstå hur implementeringsmönster används
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: e15513f5c52240536ccf41f16ba7f4dc6dbf9a04
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Översikt över implementeringsmönster

[!DNL Adobe Target] implementeringsmönster hjälper dig att förstå och skapa följande arkitektur för [!DNL Target] implementering.

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

Varje del förklaras i ett separat avsnitt i den här handboken. Till exempel [[!DNL Recommendations] implementeringsmönster med at.js](/help/dev/patterns/recs-atjs/recs-implementation-pattern-atjs.md) innehåller följande ämnen:

* Initiera SDK
* Konfigurera datainsamling
* Återge upplevelser
* Meddela [!DNL Target]

