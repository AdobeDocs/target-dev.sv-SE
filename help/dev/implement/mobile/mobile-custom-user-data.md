---
keywords: mobilapp, skicka mobilapp, målmobilapp, anpassade användardata för mobilapp, anpassade data för mobilapp
description: Lär dig hur du skickar ytterligare information om platsen eller användaren till [!DNL Adobe Target] som namnvärdespar för att hjälpa er att skapa anpassade målgrupper.
title: Hur skickar jag anpassade användardata i en iOS-app?
feature: Implement Mobile
exl-id: 9cf8e8fd-1898-43b1-b339-d7a21cb35d57
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 0%

---

# iOS - skicka anpassade användardata

Du kan skicka ytterligare information om platsen eller användaren till [!DNL Target] som namnvärdespar.

>[!IMPORTANT]
>
>Stöd för [!DNL Adobe Mobile] version 4.*x* SDK har upphört den 31 augusti 2021 och rekommenderas inte längre för [!DNL Adobe Target] mobilanvändare.
>
>The [Adobe Experience Platform SDK för mobilappar](https://developer.adobe.com/client-sdks/documentation/){target=_blank} är den rekommenderade lösningen för strömförsörjning [!DNL Adobe Experience Cloud] lösningar och tjänster i era mobilappar.

Den här informationen kan användas för att skapa anpassade målgrupper (till exempel användare med fler än 2500 mil) och för rapportering.

Det finns två typer av parametrar som du kan skicka med en [!DNL Target] ring:

* **mbox-parametrar**: Mbox-parametrar är inte beständiga mellan sessioner.
* **Profilparametrar**: Profilparametrar lagras i besökarprofilens arkiv och är beständiga mellan sessioner. mbox-parametrar finns inte kvar. Vissa nycklar är reserverade, men både profil- och mbox-parametrar kan vara anpassade nyckel/värde-par.

Även om det finns reserverade nycklar kan både profil- och mbox-parametrar innehålla anpassade nyckel/värde-par.

1. Skapa ordlista.

   Skapa först ett lexikon med de värden som du skickar till [!DNL Target]. För enkelhetens skull bör du lägga till det här inuti `welcomeMessageCampaign` så att du inte behöver bekymra dig om omfattningen.

   Här följer ett exempel på en ordlista. Du kan kopiera och klistra in den här inuti `(void)welcomeMessageCampaign`. Värdena för tangenter som `userLevel` och `userMiles` är hårdkodade i detta exempel. Vanligtvis skickar du in motsvarande variabler.

   ```
   NSDictionary *targetParams = [[NSDictionary alloc] initWithObjectsAndKeys: 
                                 @"platinum",@"userLevel", 
                                 @26500,@"userMiles", 
                                 @"1067007",@"entity.id", 
                                 @"dealsapp.qa", @"host", 
                                 @"fashion",@"entity.categoryId", 
                                 @"millenial", @"profile.persona", 
                                 @"cohort_5", @"profile.cohort", 
                                 nil];
   ```

   * Tangenter med prefixprofilen (till exempel `profile.persona`) lagras i användarens profil.

     Dessa profilattribut kan användas för olika aktiviteter och kanaler.

   * Tangenter som inte har något prefix (till exempel `userMiles`) är mbox-parametrar.

     De här parametrarna är bara tillgängliga under sessionen.

   * Tangenter med prefixentiteten (till exempel `entity.category.id`) används för produktrekommendationer.

1. Verifiera data.
   1. I programmet `didFinishLaunchingWithOptions`, ta bort kommentarer eller lägga till `[ADBMobile setDebugLogging:YES];`.

      Detaljerade felsökningsloggar skrivs ut.
   1. Skapa appen.
   1. Kontrollera att parametrarna skickas i målanropet.

      Sök efter målplatsens namn i felsökningskonsolen. Du kommer att se ett samtal till `YOUR-CLIENT-CODE.tt.omtrdc.net`med alla parametrar som du just har passerat.

      (Klicka på bilden för att expandera till full bredd.)

      ![Målplats i felsökningskonsolen](/help/dev/implement/mobile/assets/mobile-debug.png "Målplats i felsökningskonsolen"){zoomable=&quot;yes&quot;}

   Du kan bygga målgrupper och begränsa eller rikta in visningen av innehåll med dessa parametrar i [!DNL Target].
