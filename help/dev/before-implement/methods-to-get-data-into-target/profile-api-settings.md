---
keywords: implementering, api, profil, profil-API-inställningar, autentiseringstoken
description: Lär dig hur du konfigurerar autentisering för batchuppdateringar via [!DNL Adobe Target] API:er och generera en profilautentiseringstoken.
title: Hur använder jag API-inställningar för profil för att aktivera eller inaktivera batchuppdateringar?
feature: APIs/SDKs
exl-id: 968f33d0-296b-4248-8c9a-8e6f3077bdfa
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# Profil-API-inställningar

Aktivera eller inaktivera autentisering för batchuppdateringar via [!DNL Adobe Target] API:er och generera en profilautentiseringstoken.

[!DNL Adobe Target] skapar och underhåller en profil för varje enskild användare. Den här profilen lagras på [!DNL Target] Edge Cluster och uppdateras i realtid efter varje besök. Du kan även uppdatera en profil individuellt eller gruppvis via API.

För ökad säkerhet kan du kräva att API-anropet för gruppuppdatering kräver att en giltig åtkomsttoken skickas i huvudet för begäran.

**För att begära autentisering och generera en åtkomsttoken med [!DNL Target] Gränssnitt:**

1. Klicka på **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.
1. Under **[!UICONTROL Profile API]** visa **[!UICONTROL Require Authentication]** växla till aktiverad eller inaktiverad position.

   ![alt-bild](assets/profile_api_settings.png)

1. (Villkorligt) Om du har aktiverat autentiseringskrav klickar du på **[!UICONTROL Generate New Profile Authentication Token]**.

   ![alt-bild](assets/profile_api_settings_2.png)

   Token förfaller enligt den tid som anges i rutan Förfaller i.

   Du måste ha någon av följande användarbehörigheter för att generera en autentiseringstoken:

   * Administratörsroll eller har minst godkännarbehörighet

     Mer information om kunder med Target Standard finns i [Ange roller och behörigheter](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/users/user-management.html#roles-permissions) in *Användare*. Mer information om [!DNL Target Premium] kunder, se [Konfigurera företagsbehörigheter](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html).

   * Administratörsroll på arbetsyta/produktprofilnivå

     Arbetsytor är tillgängliga för [!DNL Target Premium] endast kunder. Mer information finns i [Konfigurera företagsbehörigheter](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html).

   * Administratörsrättigheter (behörighet som systemadministratör) på [!DNL Adobe Target] produktnivå

Du kan också generera en profilautentiseringstoken via API. Mer information finns i Profiler i [Adobe Target Admin och Profile API Guide](../../administer/admin-api/admin-api-overview-new.md).

1. Kopiera variabeln och inkludera den i begärans huvud i formatet: &quot;Authorization&quot; : &quot;Bearer&quot;.

1. Klicka **[!UICONTROL Generate New Profile Authentication Token]** för att återskapa token efter behov.

>[!WARNING]
>
>Om du återställer den här token kommer API-anrop som använder den aktuella token att misslyckas. Detta kräver uppdatering av skript eller appar som använder denna token.
