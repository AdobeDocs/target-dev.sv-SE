---
title: Adobe Analytics for Target (A4T) Logga in på Experience Platform Web SDK
description: Lär dig hur du styr samlingen av Adobe Analytics for Target-data (A4T) med Experience Platform Web SDK.
seo-title: Adobe Analytics for Target (A4T) Logging in the Experience Platform Web SDK
seo-description: Learn how to control the collection of Adobe Analytics for Target (A4T) data using the Experience Platform Web SDK.
keywords: a4t;logga;analys;sdk;web sdk;
feature: Implementation
exl-id: 886588bf-4416-4f1e-aecc-467e48c8fb23
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# [!DNL Adobe Analytics for Target] (A4T) loggning in i [!DNL Experience Platform Web SDK]

När du använder [!DNL Adobe Target] för personalisering kan du välja vilket system du vill använda för prestandamätning. Med varje [målaktivitet](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) kan du välja mellan [!DNL Target]-rapportering och Adobe [!DNL Analytics]-rapportering.

Om du använder [!DNL Analytics]-rapportering måste [!DNL Target] förmedla följande till [!DNL Analytics]:

* Vilken [!DNL Target] aktivitet dina besökare har angett
* Vilken upplevelse de har sett
* Vilken konvertering har nåtts

[!DNL Adobe Experience Platform Web SDK] har stöd för två typer av [!DNL Analytics]-loggning för [!UICONTROL Analytics for Target] (A4T)-användningsfall:

| Loggningsmetod | Beskrivning |
| --- | --- |
| Loggning på serversidan [!DNL Analytics] | Alla [!DNL Analytics] träffar som skickas via Edge Network utökas med [!DNL Target] detaljer på serversidan, utan att du behöver gå igenom träffsammanfogningsprocessen. |
| Loggning på klientsidan [!DNL Analytics] | [!DNL Target] data returneras på klientsidan, vilket gör att du manuellt kan förstärka och skicka data till [!DNL Analytics] med [API:t för datainfogning](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html). |

Loggningsmetoden avgörs av om du har [!DNL Adobe Analytics] aktiverat på din konfigurerade [datastream](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/overview):

![Beslutsflöde för loggningsmetod](/help/dev/implement/a4t/assets/analytics-logging.png)

## Nästa steg

Det här dokumentet innehåller en kort introduktion till de olika loggningsmetoderna för A4T-data i Web SDK. Mer information om dessa metoder finns i följande dokumentation:

* [Logga A4T-data på serversidan i Experience Platform Web SDK](/help/dev/implement/a4t/client-side-logging.md)
* [Loggning på klientsidan för A4T-data i Experience Platform Web SDK](/help/dev/implement/a4t/client-side-logging.md)
