---
title: Översikt över implementeringsmönster
description: Förstå hur implementeringsmönster används
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: c4b9dfed19e5e4a56bfeae26c4310b997a2d617e
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# Översikt över implementeringsmönster

[!DNL Adobe Target] implementeringsmönster hjälper dig att förstå och skapa följande arkitektur för [!DNL Target] implementering.

Klicka på bilden för att expandera till helskärm.

![Adobe Target - arkitektur](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

Observera att siffrorna i bilden inte anger sekvensen av åtgärder:

1. SDK för klientsidan för [!DNL Adobe Target] och [!DNL Experience Cloud ID Service]
1. [!DNL Target Delivery API] ring
1. Anrop om ECID-förvärv
1. API för gruppprofilsuppdatering och [!DNL Customer Attributes] (CA)-tjänst
1. Intag av profildata från kundens datakällor till [!DNL Target] profilarkiv
1. Samla in profil-/beteendedata och bestämma vilken upplevelse som ska visas för slutanvändaren
1. Erfarenheter återges på sidan
1. at.js återger upplevelserna på sidan

Varje mönster består av olika delar. Varje del motsvarar ett kritiskt implementeringskrav för [!DNL Target] implementering.

Varje del förklaras på en separat sida i den här handboken. Till exempel [!DNL Target] implementeringsmönstret innehåller följande sidor:

* Initiera SDK
* Förbättra profil
* Återge upplevelser
* Meddela [!DNL Target]
