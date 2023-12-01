---
keywords: implementera, implementera, konfigurera, konfigurera, uppdatera en profil
description: Hämta data till [!DNL Target] med API:t för uppdatering av en profil.
title: Hur får jag in data på [!DNL Target] Använda [!UICONTROL Single Profile Update API]?
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
source-git-commit: 43f4fb8345a77ccb0e112fe196e7e0944cc468c9
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 0%

---

# [!UICONTROL Single Profile Update API]

The [!DNL Adobe Target] [!UICONTROL Single Profile Update API] Med kan du skicka en profiluppdatering för en enskild användare. The [!UICONTROL Single Profile Update API] är nästan identisk med [!UICONTROL Bulk Profile Update API], men en besökarprofil uppdateras åt gången, i linje med API-anropet i stället för med en .cvs-fil.

The [!UICONTROL Single Profile Update API] och används vanligtvis när en uppdatering måste ske i relation till en transaktion som inträffar i en kanal som inte har implementerats [!DNL Target]. Du vill till exempel uppdatera profilen för en enskild besökare som utför en offlineåtgärd. Åtgärderna kan omfatta att nå en kundtjänst, ett lån finansieras, använda ett förmånskort i butik, få tillgång till en kioskdator och så vidare.

## Resurser:

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)
