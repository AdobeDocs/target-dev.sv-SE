---
keywords: offer, prefetch, iOS, android, sdk, mobile, mobile sdk, $8
description: Använd funktionen  [!DNL Adobe Target] prefetch i iOS och Android Mobile SDK:er för att hämta erbjudandeinnehåll så få gånger som möjligt genom att cachelagra serversvaren.
title: Kan jag förhämta erbjuda innehåll för mobilappar?
feature: Implement Mobile
exl-id: 6f8e8298-f1e9-46f0-828f-717c7d632077
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Förhämta erbjudandeinnehåll

Förhämtningsfunktionen [!DNL Target] använder iOS och Android Mobile SDK:er för att hämta innehåll så få gånger som möjligt genom att cachelagra serversvaren.

>[!IMPORTANT]
>
>Stöd för version 4 av [!DNL Adobe Mobile].*x* SDK:er har upphört den 31 augusti 2021 och rekommenderas inte längre för [!DNL Adobe Target] mobila användare.
>
>[Adobe Experience Platform SDK för mobilappar](https://developer.adobe.com/client-sdks/documentation/){target=_blank} är den rekommenderade lösningen för att driva [!DNL Adobe Experience Cloud] lösningar och tjänster i dina mobilappar.

Den här processen minskar inläsningstiden, förhindrar flera nätverksanrop och gör att [!DNL Target] kan meddelas vilken mbox som mobilappsanvändaren har besökt. Allt innehåll hämtas och cachelagras under förhämtningsanropet, och det här innehållet hämtas från cachen för alla framtida anrop som innehåller cachelagrat innehåll för det angivna mbox-namnet.

Tänk på följande begränsningar när du använder förhämtningsmetoden med iOS och Android Mobile SDK:er:

* Förhämtningsinnehåll bevaras inte vid starter. Förhämtningsinnehållet cachelagras så länge som programmet finns eller tills metoden `clearPrefetchCache()` anropas.
* Förhämtningsfunktionen stöds inte för [!UICONTROL Auto-Allocate]- och [!UICONTROL Auto-Target]-trafikallokeringsmetoder, för aktivitetstyperna [!UICONTROL Automated Personalization] eller [!UICONTROL Recommendations], eller för [rekommendationer i en A/B- eller XT-aktivitet](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-as-an-offer.html).

Mer information, inklusive förhämtningsmetoder, publika klasser och kodexempel finns i:

* **iOS:** [Förhämta erbjudandeinnehåll i iOS](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-mob-target-prefetch-ios.html) i *Hjälp om iOS SDK för mobila tjänster*.
* **Android:** [Förhämta erbjudandeinnehåll i Android](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html) i *Hjälp om Android SDK för mobila tjänster*.
