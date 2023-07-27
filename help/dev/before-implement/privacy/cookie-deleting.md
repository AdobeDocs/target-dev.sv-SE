---
keywords: cookie, cookies, delete cookie, delete [!DNL Target] cookie, google chrome, chrome, mozilla firefox, firefox, microsoft edge, safari, cookie1
description: Lär dig hur du tar bort [!DNL Target] webbläsarcookies så att ni kan validera era upplevelser.
title: Hur tar jag bort [!DNL Target] Cookie?
feature: Privacy & Security
exl-id: c975c47f-8d81-4abe-aa89-f65275a73002
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 1%

---

# Ta bort [!DNL Target] cookie

Du kan ta bort [!DNL Adobe Target] webbläsar-cookie (mbox) så att du kan validera alla dina upplevelser under testningen.

Om det inte finns något [!DNL Target] cookie (mbox) betraktas du som en ny besökare och visas som en ny upplevelse. Det finns flera sätt att ta bort din mbox utan att ta bort alla webbläsarcookies.

>[!NOTE]
>
>Följande instruktioner är korrekta för de webbläsare och versioner som visas. Sök på Internet efter instruktioner om din webbläsare eller version.

## Ta bort [!DNL Target] cookie från Google Chrome

Version 84.0.4147.105

1. Klicka på **[!UICONTROL Chrome]** meny > **[!UICONTROL Preferences]**.
1. Klicka på **[!UICONTROL Privacy and Security]** -fliken.
1. Klicka på **[!UICONTROL Cookies and other site data]**.
1. Klicka på **[!UICONTROL See all cookies and site data]**.
1. Expandera `adobe.com` väljer du **mbox** cookie och klicka sedan på borttagningsikonen (X).

## Ta bort [!DNL Target] cookie från Mozilla Firefox

Version 79.0

### Ta bort alla cookies som är associerade med `adobe.com`

1. Klicka på **[!UICONTROL Firefox]** meny > **[!UICONTROL Preferences]**.
1. Klicka på **[!UICONTROL Privacy and Security]** -fliken.
1. Under **Cookies och webbplatsdata*, klicka **[!UICONTROL Manage Data]**.
1. Välj `adobe.com` plats och klicka sedan på **[!UICONTROL Remove Selected]**.

>[!WARNING]
>
>Detta tar bort alla cookies som är kopplade till `adobe.com` webbplats. Om du vill ta bort en enskild cookie för en webbplats följer du instruktionerna nedan.

### Ta bort en enskild cookie (mbox)

1. Klicka på **[!UICONTROL Tools]** > **[!UICONTROL Web Developer]** > **[!UICONTROL Storage Inspector]**.
1. Klicka på **[!UICONTROL Advanced]** -fliken.
1. Navigera till den webbsida som innehåller den cookie som du vill ta bort.
1. Expandera **[!UICONTROL Cookies]** -avsnittet och klicka sedan på `https://experience.adobe.com`.
1. Högerklicka på **[!UICONTROL mbox]** cookie, klicka sedan på **[!UICONTROL Delete]**.

## Ta bort [!DNL Target] cookie från Microsoft Edge

Version 84.0.522.52

1. Klicka på **[!UICONTROL Microsoft Edge]** meny > **[!UICONTROL Preferences]**.
1. Klicka på **[!UICONTROL Site Permissions]** -fliken.
1. Klicka på **[!UICONTROL Cookies and site data]**.
1. Klicka på **[!UICONTROL See all cookies and site data]**.
1. Expandera `adobe.com` väljer du **mbox** cookie och klicka sedan på borttagningsikonen (X).

## Ta bort [!DNL Target] cookie från Apple Safari

Version 13.1.2

### Ta bort alla cookies som är associerade med `adobe.com`

1. Klicka på **[!UICONTROL Safari]** meny > **[!UICONTROL Preferences]**.
1. Klicka på **[!UICONTROL Privacy]** -fliken.
1. Klicka på **[!UICONTROL Manage Website Data]**.
1. Markera webbplatserna för de cookies som du vill ta bort och klicka sedan på **[!UICONTROL Remove]**.

>[!WARNING]
>
>Detta tar bort alla cookies som är kopplade till `adobe.com` webbplats. Om du vill ta bort en enskild cookie för en webbplats följer du instruktionerna nedan.

### Ta bort en enskild cookie (mbox)

1. Klicka på **[!UICONTROL Safari]** meny > **[!UICONTROL Preferences]**.
1. Klicka på **[!UICONTROL Advanced]** -fliken.
1. Välj **[!UICONTROL Show Develop menu in menu bar]** alternativ.
1. Navigera till den webbsida som innehåller den cookie som du vill ta bort.
1. Klicka på **[!UICONTROL Develop]** meny > **[!UICONTROL Show Web Inspector]**.
1. Klicka på **[!UICONTROL Storage]** -fliken.
1. Expandera **[!UICONTROL Cookies]** -avsnittet och klicka sedan på `www.adobe.com`.
1. Högerklicka på **mbox** cookie, klicka sedan på **[!UICONTROL Delete]**.
