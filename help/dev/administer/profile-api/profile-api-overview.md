---
title: Uppdatera profiler
description: Lär dig hur du använder Adobe Target Profile API:er för att skicka besöksdata till [!DNL Target].
contributors: https://github.com/icaraps
exl-id: 482a4175-1d02-47e9-a5c0-dd00e8560773
feature: APIs/SDKs
source-git-commit: 9707680ddcf0c373c635aa9f3cb5ba1b74cf90a3
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---

# Uppdatera profiler

En användarprofil innehåller demografisk information och beteendeinformation om en besökare på en webbsida, t.ex. ålder, kön, köpta produkter, sista besökstillfälle osv. [!DNL Adobe Target] använder den här informationen för att anpassa det innehåll som används för varje besökare.

Profilinformationen för varje besökare lagras antingen i cookies eller i tredjepartsappar.

Om webbsidan implementerar [!DNL Target] kod ([at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) eller [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)) skickas profilinformationen från cookies till [!DNL Target] med profilparametrar. [!DNL Target] identifierar varje besökare unikt via en `pcID` som genereras i besökarens cookies. Du kan dock skicka profilparametrar från en extern app via mbox-anrop med `mbox3rdPartyIds`.

Använd [!DNL Adobe Target] profil-API:er när du har profildata om besökarna att skicka till [!DNL Target] som du antingen inte kan eller inte vill skicka som en del av din sidbaserade integrering med [!DNL Target]. Det kan vara data från ett CRM- eller POS-system (Customer Relationship Management) som inte är tillgängligt på sidan. Eller så kan dessa data vara mer känsliga och inte vettiga att skicka vidare på sidan.

Det finns två sätt att uppdatera profiler via API:

* [API för enkel profiluppdatering](/help/dev/administer/profile-api/profile-single-api.md)
* [Massprofiluppdatering via batch](/help/dev/administer/profile-api/profile-bulk-api.md)

Den äldre API-dokumentationen för profiler finns här: [https://developers.adobetarget.com/api/#profiles](https://developers.adobetarget.com/api/#profiles){target=_blank}
