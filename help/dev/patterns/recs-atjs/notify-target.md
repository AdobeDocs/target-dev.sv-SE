---
title: Meddela mål
description: Kontrollera att alla händelser som måste spåras av  [!DNL Target]  skickas med metoden trackEvent.
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: efccadab-d139-4423-8613-c2743d87b3a0
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Meddela [!DNL Target]

När du slutför det här steget skickas alla händelser som måste skickas till [!DNL Adobe Target] med metoden `trackEvent`.

Alla händelser som behöver spåras i [!DNL Target] kan vara en primär konverteringshändelse eller ett framgångsmått.

>[!TIP]
>
>Klicka på bilderna i det här avsnittet för att utöka till helskärm.

## Meddela [!DNL Target]-diagram {#diagram}

Stegnumret i följande bild motsvarar avsnittet nedan.

![Meddela måldiagram](/help/dev/patterns/recs-atjs/assets/diagram-notify-target.png){width="600" zoomable="yes"}

## 4.1: Flash [!DNL Adobe Target] Track API

Det här steget hjälper dig att se till att alla händelser som måste skickas till [!DNL Target] skickas med metoden `trackEvent`.

+++Se information

![Branda Adobe Target Track API-diagram](/help/dev/patterns/recs-atjs/assets/fire-adobe-target-track-api-diagram-combined.png){width="400" zoomable="yes"}

Du skickar de attribut för orderkonvertering som anges i avsnittet *Förutsättningar* nedan. Namnet på mbox spelar ingen roll, men konverteringen ska använda `orderConfirmPage`.

Du behöver inte inkludera attribut för orderkonvertering i det här samtalet. Dessa anrop registrerar idealiskt framgångsmått som kan ses som mini-conversion-händelser före de huvudsakliga konverteringshändelserna. `CardIds` måste inkluderas i kundvagnsbaserade rekommendationer som baseras på händelsen `Add to Cart`.

**Förutsättningar**

* Träffa ert affärsteam för att identifiera alla händelser som kan anses vara konverterings- eller framgångsmått. Du måste också identifiera konverteringshändelsen som genererar intäkter så att informationen kan skickas till [!DNL Target] tillsammans med händelsedata.
* Kontrollera att följande attribut är tillgängliga i datalagret så att du kan skicka dem med konverteringshändelsen. Konverteringshändelsen genererar intäkter, till exempel ett produktköp eller Lägg i kundvagnen.

   * `productPurchaseId`: Produkt-ID som köptes som en del av ordern. Separera flera produkter med kommatecken.
   * `orderTotal`: Ordersumma för inköpet.
   * `orderId`: Inköpets order-ID.

  Följande bild visar en [regel för [!DNL tags] in [!DNL Experience Platform]](https://experienceleague.adobe.com/docs/tags.html?lang=sv-SE){target=_blank} som endast ska aktiveras på sidan [!UICONTROL Confirmation].

  ![Sidan Åtgärdskonfiguration](/help/dev/patterns/recs-atjs/assets/action-configuration.png){width="400" zoomable="yes"}

* Om du spårar en händelse för kundvagnstillägg skickar du `cartIds` som en parameter. En kommaavgränsad lista med produkt-ID:n kan skickas för `cardIds`.

**Läser**

* [adobe.target.trackEvent(), metod](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
* [cartIds för kundvagnsbaserade villkor](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=sv-SE#cart-based){target=_blank}

**Åtgärder**

* Använd metoden `adobe.target-trackEvent()` för att skicka alla data som måste skickas till [!DNL Target].
