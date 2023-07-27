---
description: Lär dig använda [!DNL Adobe Mobile SDK] för att visa de optimala upplevelserna för era mobilappsbesökare.
title: Hur [!DNL Target] Arbeta i mobilappar?
feature: Implement Mobile
exl-id: 33001f01-fde6-48cb-ac02-d1a632b2150d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# Hur [!DNL Target] fungerar i mobilappar

The [!DNL Adobe Mobile SDK] kontaktar [!DNL Target] servern för att hämta innehållet tillsammans med andra datapunkter för att visa rätt upplevelse för användaren.

>[!IMPORTANT]
>
>Stöd för [!DNL Adobe Mobile] version 4.*x* SDK har upphört den 31 augusti 2021 och rekommenderas inte längre för [!DNL Adobe Target] mobilanvändare.
>
>The [Adobe Experience Platform SDK för mobilappar](https://developer.adobe.com/client-sdks/documentation/){target=_blank} är den rekommenderade lösningen för strömförsörjning [!DNL Adobe Experience Cloud] lösningar och tjänster i era mobilappar.

## [!DNL Target] platser och framgångsmått

A *målplats* kallas också en mbox. En identifierad plats i appen är aktiverad för testning eller personalisering (till exempel välkomstmeddelandet på startskärmen). Dessa platser identifieras när testet skapas.

A *[framgångsmått](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html)* är en åtgärd som utförs av användaren och som identifierar om en viss aktivitet lyckades (som att registrera sig, göra ett köp, boka en biljett och så vidare).

![alt-bild](assets/mobile-target-location.png)

* **[!DNL Target]plats:** Innehållet som visas under knappen Register.

  Den här användaren erbjuds fri frakt till och med 18.00. Den här platsen kan återanvändas i flera [!DNL Target] aktiviteter för att köra A/B-tester och personalisering.

* **Resultatmått:** Den åtgärd som utförs av användaren där användaren trycker på knappen Register.

**Förstå hur [!DNL Target] fungerar i SDK**

![alt-bild](assets/how-target-mobile-works.png)
