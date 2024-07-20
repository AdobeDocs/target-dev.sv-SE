---
keywords: qa, förhandsgranska, förhandsgranskningslänk, mobil, mobilförhandsgranskning
description: Använd länkar för förhandsgranskning av mobiler för att utföra QA-åtgärder från början till slut för mobilappsaktiviteter.
title: Hur använder jag Mobile Preview-länkar i  [!DNL Adobe Target] Mobile?
feature: Implement Mobile
exl-id: c0c4237a-de1f-4231-b085-f8f1e96afc13
source-git-commit: 15e42d0fb049f9243ff5468ff5f22a8e79c55c79
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# [!DNL Target] mobilförhandsgranskning

Använd länkar för förhandsgranskning av mobiler för att enkelt skapa heltäckande QA för mobilappsaktiviteter och registrera dig för olika upplevelser med din enhet utan några särskilda testenheter.

Med funktionen för mobilförhandsgranskning kan du testa mobilappsaktiviteterna innan du startar dem live.

## Förutsättningar

1. **Använd en version av SDK som stöds:** Mobilförhandsvisningsfunktionen kräver att du hämtar och installerar rätt version av [!DNL Adobe Mobile SDK] i dina motsvarande program.

   Anvisningar om hur du hämtar rätt SDK finns i [Aktuella SDK-versioner](https://developer.adobe.com/client-sdks/documentation/current-sdk-versions/){target=_blank} i *[!DNL Adobe Experience Platform Mobile SDK]*-dokumentationen.

1. **Konfigurera ett URL-schema:** Förhandsgranskningslänken använder ett URL-schema för att öppna ditt program. Ange ett unikt URL-schema för förhandsgranskningen.

   Mer information finns i [Visuell förhandsgranskning](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank} i *Konfigurera måltillägget i användargränssnittet för dataanslutning* i dokumentationen för *[!DNL Mobile SDK]*.

   Följande länkar innehåller mer information:

   * **iOS**: Mer information om hur du anger URL-scheman för iOS finns i [Definiera ett anpassat URL-schema för din app](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app){target=_blank} på webbplatsen *Apple Developer* .
   * **Android**: Mer information om hur du anger URL-scheman för Android finns i [Skapa djuplänkar till appinnehåll](https://developer.android.com/training/app-links/deep-linking){target=_blank} på webbplatsen *Android-utvecklare* .

1. **Konfigurera `collectLaunchInfo` API (endast i0S)**

   Mer information finns i [Visuell förhandsgranskning](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank} i *Konfigurera måltillägget i användargränssnittet för dataanslutning* i dokumentationen för *[!DNL Mobile SDK]*.

## Skapa en förhandsgranskningslänk

1. Klicka på ikonen **[!UICONTROL More Options]** (den lodräta ellipsen) i användargränssnittet för [!DNL Target] och välj sedan **[!UICONTROL Create Mobile Preview Link]**.

   ![alt-bild](assets/mobile-preview-create.png)

1. Markera de aktiviteter som du vill förhandsgranska och klicka sedan på **[!UICONTROL Generate Mobile Preview Link]**.

   >[!NOTE]
   >
   >Du kan bara välja formulärbaserade [!UICONTROL A/B Test]- och [!UICONTROL Experience Targeting] (XT)-aktiviteter.

   ![alt-bild](assets/mobile-preview-select-activities.png)

1. Ange appens URL-schema.

   URL-schemat måste vara samma som det som finns i din iOS- eller Android-app. Upprepa vid behov den här processen separat för iOS och Android.

   ![alt-bild](assets/mobile-preview-enter-url-scheme.png)

1. Klicka på **[!UICONTROL Generate Mobile Preview Link]** och kopiera sedan länken.

   ![alt-bild](assets/mobile-preview-generate-and-copy.png)

## Förhandsgranska på din enhet

Öppna länken i en mobilwebbläsare på en enhet där appen är installerad. Den här appen kan vara den produktionsapp som du hämtade från [!DNL Apple App Store] eller [!DNL Google Play Store]. Appen behöver inte vara en specialversion. Om du har en aktiv förhandsgranskningslänk kan du visa upplevelserna på enheten.

1. Öppna länken i din webbläsare.

   Dela länken som du kopierade i föregående avsnitt från användargränssnittet i [!DNL Target] till din mobila enhet på ett bekvämt sätt, till exempel med text, e-post eller [!DNL Slack].

   |![förhandsgranska djup länk 1](assets/mobile-preview-open-deeplink.png)|![förhandsgranska djup länk 2](assets/mobile-preview-open-app.png)|

   Din app öppnas och [!DNL Target] [!UICONTROL Mobile Preview Mode] startas.

1. Välj den kombination av upplevelser som du vill se och klicka sedan på **[!UICONTROL Launch Experiences]**.

   |![mobilförhandsvisning 1](assets/mobile-preview-experience-selection-1.png)|![mobilförhandsvisning 2](assets/mobile-preview-experience-result-1-france.png)|![mobilförhandsvisning 3](assets/mobile-preview-experience-result-1-shipfree.png)|
|![mobilförhandsvisning 4](assets/mobile-preview-experience-selection-2.png)|![mobilförhandsvisning 5](assets/mobile-preview-experience-result-2-aus.png)|![mobilförhandsvisning 6](assets/mobile-preview-experience-result-2-10off.png)|

## Begränsningar

* Vyn måste läsas in igen för att det nya innehållet ska visas när användaren klickar på knappen **[!UICONTROL Launch Experiences]**. Det enklaste sättet är att växla till en annan skärm och sedan gå tillbaka till skärmen där du väntar dig att ändringen ska ske.
* Mobilförhandsvisning stöds inte för tidigare Android-versioner än API-19 (KitKat).
