---
title: Meddela mål
description: Se till att alla händelser som behöver spåras av [!DNL Target] skickas med metoden trackEvent.
feature: APIs/SDKs
level: Experienced
role: Developer
source-git-commit: 723bb2f33a011995757009193ee9c48757ae1213
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---

# Meddela [!DNL Target]

När du slutför det här steget ser du till att alla händelser som måste skickas till [!DNL Adobe Target] skickas med `trackEvent` -metod.

Alla händelser som behöver spåras [!DNL Target] kan vara en primär konverteringshändelse eller ett framgångsmått.

>[!TIP]
>
>Klicka på bilderna i det här avsnittet för att utöka till helskärm.

## Meddela [!DNL Target] diagram {#diagram}

Stegnumret i följande bild motsvarar avsnittet nedan.

![Meddela måldiagram](/help/dev/patterns/recs-atjs/assets/diagram-notify-target.png){width="600" zoomable="yes"}

## 4.1: Eld [!DNL Adobe Target] Spår-API

Det här steget hjälper dig att se till att alla händelser som måste skickas till [!DNL Target] skickas med `trackEvent` -metod.

+++Se information

![Fire Adobe Target Track API-diagram](/help/dev/patterns/recs-atjs/assets/fire-adobe-target-track-api-diagram-combined.png){width="400" zoomable="yes"}

Du skickar de attribut för orderkonvertering som anges i *Förutsättningar* nedan. Namnet på mbox spelar ingen roll, men konverteringen ska användas `orderConfirmPage`.

Du behöver inte inkludera attribut för orderkonvertering i det här samtalet. Dessa anrop registrerar idealiskt framgångsmått som kan ses som mini-conversion-händelser före de huvudsakliga konverteringshändelserna. `CardIds` måste ingå i kundvagnsbaserade rekommendationer baserade på `Add to Cart` -händelse.

**Förutsättningar**

* Träffa ert affärsteam för att identifiera alla händelser som kan anses vara konverterings- eller framgångsmått. Du måste också identifiera konverteringshändelsen som genererar intäkter så att informationen kan skickas till [!DNL Target] tillsammans med händelsedata.
* Kontrollera att följande attribut är tillgängliga i datalagret så att du kan skicka dem med konverteringshändelsen. Konverteringshändelsen genererar intäkter, till exempel ett produktköp eller Lägg i kundvagnen.

   * `productPurchaseId`: Produkt-ID som köptes som en del av ordern. Separera flera produkter med kommatecken.
   * `orderTotal`: Ordersumma för köpet.
   * `orderId`: Inköpets beställnings-ID.

  Följande bild visar en [regel för [!DNL tags] in [!DNL Experience Platform]](https://experienceleague.adobe.com/docs/tags.html){target=_blank} som bara ska aktiveras på [!UICONTROL Confirmation] sida.

  ![Sidan Åtgärdskonfiguration](/help/dev/patterns/recs-atjs/assets/action-configuration.png){width="400" zoomable="yes"}

* Om du spårar en händelse för kundvagnstillägg skickar du `cartIds` som en parameter. En kommaavgränsad lista med produkt-ID:n kan skickas för `cardIds`.

**Läser**

* [adobe.target.trackEvent(), metod](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
* [cartIds för kundvagnsbaserade kriterier](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=en#cart-based){target=_blank}

**Åtgärder**

* Använd `adobe.target-trackEvent()` metod för att skicka alla data som måste skickas till [!DNL Target].







