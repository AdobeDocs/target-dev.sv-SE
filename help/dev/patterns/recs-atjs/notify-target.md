---
title: Meddela mål
description: Se till att alla händelser som behöver spåras av [!DNL Target] skickas med metoden trackEvent.
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 7a79eb1d263cf42529a5a1b1ca1f9de4db218a49
workflow-type: tm+mt
source-wordcount: '335'
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

## Eld [!DNL Adobe Target] Spår-API

Det här steget hjälper dig att se till att alla händelser som måste skickas till [!DNL Target] skickas med `trackEvent` -metod.

+++Se information

![Fire Adobe Target Track API-diagram](/help/dev/patterns/recs-atjs/assets/fire-adobe-target-track-api-diagram.png){width="300" zoomable="yes"}

Du skickar de attribut för orderkonvertering som anges i *Förutsättningar* nedan. Namnet på mbox spelar ingen roll, men konverteringen ska användas `orderConfirmPage`.

Du behöver inte inkludera attribut för orderkonvertering i det här samtalet. Dessa anrop registrerar idealiskt framgångsmått som kan ses som mini-conversion-händelser före de huvudsakliga konverteringshändelserna. `CardIds` måste ingå i kundvagnsbaserade rekommendationer baserade på `Add to Cart` -händelse.

**Förutsättningar**

* Träffa ert affärsteam för att identifiera alla händelser som kan anses vara konverterings- eller framgångsmått. Du måste också identifiera konverteringshändelsen som genererar intäkter så att informationen kan skickas till [!DNL Target] tillsammans med händelsedata.
* Kontrollera att följande attribut är tillgängliga i datalagret så att du kan skicka dem med konverteringshändelsen. Konverteringshändelsen genererar intäkter, till exempel ett produktköp eller Lägg i kundvagnen.

   * `productPurchaseId`: Produkt-ID som köptes som en del av ordern. Separera flera produkter med kommatecken.
   * `orderTotal`: Ordersumma för köpet.
   * `orderId`: Inköpets beställnings-ID.

* Om du spårar en händelse för kundvagnstillägg skickar du `cartIds` som en parameter. En kommaavgränsad lista med produkt-ID:n kan skickas för `cardIds`.

**Läser**

* [adobe.target.trackEvent(), metod](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
* [cartIds för kundvagnsbaserade kriterier](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=en#cart-based){target=_blank}

**Åtgärder**

* Använd `adobe.target-trackEvent()` metod för att skicka alla data som måste skickas till [!DNL Target].







