---
keywords: cookie, cookies, delete cookie, delete [!DNL Target] cookie, google chrome, chrome, mozilla firefox, firefox, microsoft edge, safari, cookie1
description: Lär dig hur du tar bort dina  [!DNL Target] webbläsarcookies så att du kan validera dina upplevelser.
title: Hur tar jag bort  [!DNL Target] cookien?
feature: Privacy & Security
exl-id: c975c47f-8d81-4abe-aa89-f65275a73002
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '355'
ht-degree: 1%

---

# Ta bort cookien [!DNL Target]

Du kan ta bort din [!DNL Adobe Target]-webbläsarcookie (mbox) så att du kan validera alla dina upplevelser under testningen.

Om det inte finns någon [!DNL Target]-cookie (mbox) betraktas du som en ny besökare och får en ny upplevelse. Det finns flera sätt att ta bort din mbox utan att ta bort alla webbläsarcookies.

>[!NOTE]
>
>Följande instruktioner är korrekta för de webbläsare och versioner som visas. Sök på Internet efter instruktioner om din webbläsare eller version.

## Ta bort cookien [!DNL Target] från Google Chrome

Version 84.0.4147.105

1. Klicka på menyn **[!UICONTROL Chrome]** > **[!UICONTROL Preferences]**.
1. Klicka på fliken **[!UICONTROL Privacy and Security]**.
1. Klicka på **[!UICONTROL Cookies and other site data]**.
1. Klicka på **[!UICONTROL See all cookies and site data]**.
1. Expandera avsnittet `adobe.com`, markera cookien **mbox** och klicka sedan på ikonen Ta bort (X).

## Ta bort cookien [!DNL Target] från Mozilla Firefox

Version 79.0

### Ta bort alla cookies som är associerade med `adobe.com`

1. Klicka på menyn **[!UICONTROL Firefox]** > **[!UICONTROL Preferences]**.
1. Klicka på fliken **[!UICONTROL Privacy and Security]**.
1. Klicka på **[!UICONTROL Manage Data]** under **Cookies och Site Data*.
1. Markera `adobe.com`-webbplatsen och klicka sedan på **[!UICONTROL Remove Selected]**.

>[!WARNING]
>
>Detta tar bort alla cookies som är associerade med webbplatsen `adobe.com`. Om du vill ta bort en enskild cookie för en webbplats följer du instruktionerna nedan.

### Ta bort en enskild cookie (mbox)

1. I Firefo klickar du på **[!UICONTROL Tools]** > **[!UICONTROL Web Developer]** > **[!UICONTROL Storage Inspector]**.
1. Klicka på fliken **[!UICONTROL Advanced]**.
1. Navigera till den webbsida som innehåller den cookie som du vill ta bort.
1. Expandera avsnittet **[!UICONTROL Cookies]** och klicka sedan på `https://experience.adobe.com`.
1. Högerklicka på **[!UICONTROL mbox]**-cookien och klicka sedan på **[!UICONTROL Delete]**.

## Ta bort cookien [!DNL Target] från Microsoft Edge

Version 84.0.522.52

1. Klicka på menyn **[!UICONTROL Microsoft Edge]** > **[!UICONTROL Preferences]**.
1. Klicka på fliken **[!UICONTROL Site Permissions]**.
1. Klicka på **[!UICONTROL Cookies and site data]**.
1. Klicka på **[!UICONTROL See all cookies and site data]**.
1. Expandera avsnittet `adobe.com`, markera cookien **mbox** och klicka sedan på ikonen Ta bort (X).

## Ta bort cookien [!DNL Target] från Apple Safari

Version 13.1.2

### Ta bort alla cookies som är associerade med `adobe.com`

1. Klicka på menyn **[!UICONTROL Safari]** > **[!UICONTROL Preferences]**.
1. Klicka på fliken **[!UICONTROL Privacy]**.
1. Klicka på **[!UICONTROL Manage Website Data]**.
1. Markera webbplatserna för de cookies som du vill ta bort och klicka sedan på **[!UICONTROL Remove]**.

>[!WARNING]
>
>Detta tar bort alla cookies som är associerade med webbplatsen `adobe.com`. Om du vill ta bort en enskild cookie för en webbplats följer du instruktionerna nedan.

### Ta bort en enskild cookie (mbox)

1. Klicka på menyn **[!UICONTROL Safari]** > **[!UICONTROL Preferences]**.
1. Klicka på fliken **[!UICONTROL Advanced]**.
1. Välj alternativet **[!UICONTROL Show Develop menu in menu bar]**.
1. Navigera till den webbsida som innehåller den cookie som du vill ta bort.
1. Klicka på menyn **[!UICONTROL Develop]** > **[!UICONTROL Show Web Inspector]**.
1. Klicka på fliken **[!UICONTROL Storage]**.
1. Expandera avsnittet **[!UICONTROL Cookies]** och klicka sedan på `www.adobe.com`.
1. Högerklicka på **mbox**-cookien och klicka sedan på **[!UICONTROL Delete]**.
