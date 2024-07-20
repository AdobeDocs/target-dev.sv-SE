---
keywords: mobilapp, sdk för mobilapp, målmobilapp, sdk för mobilmål, sdk för mobilapp, aktivera mål i sdk
description: Lär dig hur du lägger till Adobe Mobile Services SDK i din mobilapp.
title: Hur aktiverar jag  [!DNL Target]  i  [!DNL Adobe Mobile SDK]?
feature: Implement Mobile
exl-id: 4263b96a-23c8-4513-8302-00080122181d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 0%

---

# Aktivera [!DNL Target] i SDK

Lägg till [!UICONTROL Adobe Mobile Services SDK] i din app.

>[!IMPORTANT]
>
>Stöd för version 4 av [!DNL Adobe Mobile].*x* SDK:er har upphört den 31 augusti 2021 och rekommenderas inte längre för [!DNL Adobe Target] mobila användare.
>
>[Adobe Experience Platform SDK för mobilappar](https://developer.adobe.com/client-sdks/documentation/){target=_blank} är den rekommenderade lösningen för att driva [!DNL Adobe Experience Cloud] lösningar och tjänster i dina mobilappar.

1. Om du inte har installerat Adobe Mobile Services SDK i din app använder du dina inloggningsuppgifter för Analytics eller Experience Cloud och hämtar SDK från webbplatsen [Adobe Mobile Services](https://mobilemarketing.adobe.com/).

1. Lägg till [!DNL Adobe Mobile Services SDK] i din app.

   Instruktionerna finns under [Core Implementation och Lifecycle](https://experienceleague.adobe.com/docs/mobile-services/ios/getting-started-ios/dev-qs.html).

1. Lägg till klientkod, tidsgräns och aktivera SSL.

   Öppna Mobile Services i Experience Cloud och gå sedan till **[!UICONTROL Manage App Settings]** > **[!UICONTROL SDK Target Options]**.

   Lägg till din [!DNL Target]-klientkod och tidsgräns. Klientkoden är unik för ditt konto eller företag. Tidsgränsen är den tid i antal sekunder som [!DNL Target] väntar på ett svar innan standardinnehållet visas. Kontrollera att alternativet **[!UICONTROL Use HTTPS]** är markerat på sidan Hantera appinställningar i Adobe Mobile Services. Om HTTPS inte är aktiverat blockeras alla anrop i iOS9+ om du inte tillåtslista [!DNL Target]-servern.

   ![alt-bild](assets/mobile-clientcode.png)

1. När du har skapat/hittat programmet kan du hitta programinställningarna och hämta önskat SDK.

   ![alt-bild](assets/download-sdk.png)

>[!WARNING]
>
> Om du inte har tillgång till gränssnittet för mobilmarknadsföring kan du göra ändringar direkt i konfigurationsfilen i appkoden, men den synkroniseras inte med inställningssidan i användargränssnittet.
