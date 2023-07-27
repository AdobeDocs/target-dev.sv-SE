---
keywords: offer, prefetch, iOS, android, sdk, mobile, mobile sdk, $8
description: Använd [!DNL Adobe Target] förhämtningsfunktionen i iOS och Android Mobile SDK:er för att hämta erbjudandeinnehåll så få gånger som möjligt genom att cachelagra serversvaren.
title: Kan jag förhämta erbjuda innehåll för mobilappar?
feature: Implement Mobile
exl-id: 6f8e8298-f1e9-46f0-828f-717c7d632077
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---

# Förhämta erbjudandeinnehåll

The [!DNL Target] förhämtningsfunktionen använder iOS och Android Mobile SDK:er för att hämta erbjudandeinnehåll så få gånger som möjligt genom att cachelagra serversvaren.

>[!IMPORTANT]
>
>Stöd för [!DNL Adobe Mobile] version 4.*x* SDK har upphört den 31 augusti 2021 och rekommenderas inte längre för [!DNL Adobe Target] mobilanvändare.
>
>The [Adobe Experience Platform SDK för mobilappar](https://developer.adobe.com/client-sdks/documentation/){target=_blank} är den rekommenderade lösningen för strömförsörjning [!DNL Adobe Experience Cloud] lösningar och tjänster i era mobilappar.

Den här processen minskar inläsningstiden, förhindrar flera nätverksanrop och tillåter [!DNL Target] för att få ett meddelande om vilken mbox som mobilappsanvändaren har besökt. Allt innehåll hämtas och cachelagras under förhämtningsanropet, och det här innehållet hämtas från cachen för alla framtida anrop som innehåller cachelagrat innehåll för det angivna mbox-namnet.

Tänk på följande begränsningar när du använder förhämtningsmetoden med iOS och Android Mobile SDK:er:

* Förhämtningsinnehåll bevaras inte vid starter. Det förhämtade innehållet cachelagras så länge som programmet finns eller tills `clearPrefetchCache()` -metoden anropas.
* Förhämtningsfunktionen stöds inte för [!UICONTROL Auto-Allocate] och [!UICONTROL Auto-Target] trafiktilldelningsmetoder, för [!UICONTROL Automated Personalization] eller [!UICONTROL Recommendations] aktivitetstyper, eller för [rekommendationer i A/B- eller XT-aktivitet](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-as-an-offer.html).

Mer information, inklusive förhämtningsmetoder, publika klasser och kodexempel finns i:

* **iOS:**  [Förhämta erbjudandeinnehåll i iOS](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-mob-target-prefetch-ios.html) i *Mobile Services iOS SDK - hjälp*.
* **Android:**  [Förhämta erbjudandeinnehåll i Android](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html) i *Hjälp för Mobile Services Android SDK*.
