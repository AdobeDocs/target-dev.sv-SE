---
description: Lär dig hur du använder  [!DNL Adobe Mobile SDK]  för att visa de optimala upplevelserna för dina mobilappsbesökare.
title: Hur fungerar  [!DNL Target] i mobilappar?
feature: Implement Mobile
exl-id: 33001f01-fde6-48cb-ac02-d1a632b2150d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---

# Så här fungerar [!DNL Target] i mobilappar

[!DNL Adobe Mobile SDK] kontaktar [!DNL Target]-servern för att hämta innehållet tillsammans med andra datapunkter för att visa rätt upplevelse för användaren.

>[!IMPORTANT]
>
>Stöd för version 4 av [!DNL Adobe Mobile].*x* SDK:er har upphört den 31 augusti 2021 och rekommenderas inte längre för [!DNL Adobe Target] mobila användare.
>
>[Adobe Experience Platform SDK för mobilappar](https://developer.adobe.com/client-sdks/documentation/){target=_blank} är den rekommenderade lösningen för att driva [!DNL Adobe Experience Cloud] lösningar och tjänster i dina mobilappar.

## [!DNL Target] platser och framgångsmått

En *målplats* kallas också för en mbox. En identifierad plats i appen är aktiverad för testning eller personalisering (till exempel välkomstmeddelandet på startskärmen). Dessa platser identifieras när testet skapas.

Ett *[framgångsmått](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html?lang=sv-SE)* är en åtgärd som utförs av användaren och som identifierar om en viss aktivitet lyckades (t.ex. registrering, köp, bokning av biljett).

![alt-bild](assets/mobile-target-location.png)

* **[!DNL Target]plats:** Innehållet som visas under registreringsknappen.

  Den här användaren erbjuds fri frakt till och med 18.00. Den här platsen kan återanvändas i flera [!DNL Target]-aktiviteter för att köra A/B-tester och personalisering.

* **Resultatmått:** Den åtgärd som utfördes av användaren där användaren trycker på registerknappen.

**Förstå hur [!DNL Target] fungerar i SDK**

![alt-bild](assets/how-target-mobile-works.png)
