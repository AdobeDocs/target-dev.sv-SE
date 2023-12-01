---
title: Adobe Target Profile APIs - översikt
description: Lär dig hur du använder Adobe Target Profile API:er för att skicka besöksdata till [!DNL Target].
contributors: https://github.com/icaraps
exl-id: 482a4175-1d02-47e9-a5c0-dd00e8560773
feature: APIs/SDKs
source-git-commit: af9db32d59bdf32f2b9fade267922803250377dd
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---

# [!DNL Adobe Target Profile APIs overview]

En användarprofil innehåller demografisk information och beteendeinformation om en besökare på en webbsida, t.ex. ålder, kön, köpta produkter, sista besökstillfälle osv. [!DNL Adobe Target] använder den här informationen för att anpassa det innehåll som används för varje besökare.

Profilinformationen för varje besökare lagras antingen i cookies eller i tredjepartsappar.

Om webbsidan implementerar målkoden ([at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) eller [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)) skickas profilinformationen från cookies till [!DNL Target] med profilparametrar. [!DNL Target] identifierar varje besökare unikt via en `pcID` att det genererar besökarens cookies. Du kan dock skicka profilparametrar från en extern app via mbox-anrop med `mbox3rdPartyIds`.

Använd [!DNL Adobe Target] profil-API:er när du har profildata om besökarna att skicka till [!DNL Target] som du antingen inte kan eller inte vill skicka som en del av din sidbaserade integrering med [!DNL Target]. Det kan vara data från ett CRM- eller POS-system (Point of Sale) som inte finns på sidan, eller data av en mer känslig typ som inte går att skicka på sidan.

Det finns två sätt att uppdatera profiler via API:

* [API för enkel profiluppdatering](/help/dev/administer/profile-api/profile-single-api.md)
* [Massprofiluppdatering via batch](/help/dev/administer/profile-api/profile-bulk-api.md)
