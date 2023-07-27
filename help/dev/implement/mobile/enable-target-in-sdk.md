---
keywords: mobilapp, sdk för mobilapp, målmobilapp, sdk för mobilmål, sdk för mobilapp, aktivera mål i sdk
description: Lär dig hur du lägger till Adobe Mobile Services SDK i din mobilapp.
title: Hur aktiverar jag [!DNL Target] i [!DNL Adobe Mobile SDK]?
feature: Implement Mobile
exl-id: 4263b96a-23c8-4513-8302-00080122181d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# Aktivera [!DNL Target] i SDK

Lägg till [!UICONTROL Adobe Mobile Services SDK] till appen.

>[!IMPORTANT]
>
>Stöd för [!DNL Adobe Mobile] version 4.*x* SDK har upphört den 31 augusti 2021 och rekommenderas inte längre för [!DNL Adobe Target] mobilanvändare.
>
>The [Adobe Experience Platform SDK för mobilappar](https://developer.adobe.com/client-sdks/documentation/){target=_blank} är den rekommenderade lösningen för strömförsörjning [!DNL Adobe Experience Cloud] lösningar och tjänster i era mobilappar.

1. Om du inte har installerat Adobe Mobile Services SDK i din app använder du inloggningsuppgifterna för Analytics eller Experience Cloud och hämtar SDK från [Adobe Mobile Services](https://mobilemarketing.adobe.com/) webbplats.

1. Lägg till [!DNL Adobe Mobile Services SDK] till appen.

   Instruktionerna finns under [Kärnimplementering och livscykel](https://experienceleague.adobe.com/docs/mobile-services/ios/getting-started-ios/dev-qs.html).

1. Lägg till klientkod, tidsgräns och aktivera SSL.

   Öppna Mobile Services i Experience Cloud och gå sedan till **[!UICONTROL Manage App Settings]** > **[!UICONTROL SDK Target Options]**.

   Lägg till [!DNL Target] klientkod och timeout. Klientkoden är unik för ditt konto eller företag. Tidsgränsen är den tid i antal sekunder till vilken [!DNL Target] väntar på ett svar innan standardinnehållet visas. Se till att **[!UICONTROL Use HTTPS]** alternativet är markerat på sidan Hantera programinställningar i Adobe Mobile Services. Om HTTPS inte är aktiverat blockeras alla samtal i iOS9+ såvida du inte tillåtslista [!DNL Target] server.

   ![alt-bild](assets/mobile-clientcode.png)

1. När du har skapat/hittat programmet kan du hitta programinställningarna och hämta önskat SDK.

   ![alt-bild](assets/download-sdk.png)

>[!WARNING]
>
> Om du inte har tillgång till gränssnittet för mobilmarknadsföring kan du göra ändringar direkt i konfigurationsfilen i appkoden, men den synkroniseras inte med inställningssidan i användargränssnittet.
