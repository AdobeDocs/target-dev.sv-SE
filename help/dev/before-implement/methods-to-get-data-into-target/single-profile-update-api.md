---
keywords: implementera, implementera, konfigurera, konfigurera, uppdatera en profil
description: Hämta data till  [!DNL Target] med API:t för uppdatering av en profil.
title: Hur hämtar jag data till  [!DNL Target] med [!UICONTROL Single Profile Update API]?
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
source-git-commit: 946e9431e6bde30f564b4ba1a4cf0a78d8c5c6bf
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 0%

---

# [!UICONTROL Single Profile Update API]

Med [!DNL Adobe Target] [!UICONTROL Single Profile Update API] kan du skicka en profiluppdatering för en enskild användare. [!UICONTROL Single Profile Update API] är nästan identisk med [!UICONTROL Bulk Profile Update API], men en besökarprofil uppdateras åt gången, i linje med API-anropet i stället för med en .cvs-fil.

[!UICONTROL Single Profile Update API] och används vanligtvis när en uppdatering måste ske i relation till en transaktion som inträffar i en kanal som inte har implementerat [!DNL Target]. Du vill till exempel uppdatera profilen för en enskild besökare som utför en offlineåtgärd. Åtgärderna kan omfatta att nå en kundtjänst, ett lån finansieras, använda ett förmånskort i butik, få tillgång till en kioskdator och så vidare.

Kontrastera [!UICONTROL Single Profile Update API] med [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md).

## Resurs

Mer information finns i:

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)
