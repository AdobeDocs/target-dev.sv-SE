---
keywords: felsökning, vanliga frågor, vanliga frågor, frågor och svar, global, mbox
description: Frågor och svar om Adobe [!DNL Target] globala lådor.
title: Vilka är de vanliga frågorna om den globala rutan?
feature: at.js
exl-id: 7bcd1b67-809a-466a-b648-6e0e44386157
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 0%

---

# Vanliga frågor och svar om Global Mbox

Lista med vanliga frågor och svar om globala kryssrutor.

## Kan jag ha fler än en global mbox om [!DNL Target] anges kontot över flera domäner?

Endast en global mbox stöds för hela kontot.

Du kan begränsa var dina aktiviteter körs genom att lägga till URL-regler i dina aktiviteter. Mer information finns i [Inkludera samma upplevelse på liknande sidor](https://experienceleague.adobe.com/docs/target/using/experiences/vec/temtest.html).

Du kan också skicka en parameter på sidan med [targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) och sedan välja parametrarna i avsnittet &quot;Konfigurera URL&quot; i [!UICONTROL Visual Experience Composer] (VEC) eller genom att lägga till parametrarna som&quot;förbättringar&quot; i [!UICONTROL Form-Based Experience Composer].

## Hur skickar jag intäktsdata till en [!DNL Target] global mbox?

Om du vill samla in information om intäkter och beställningar på mbox-målets globala mbox måste du skicka &quot;mbox parameters&quot; till [!DNL Target]. Dessa parametrar är namn/värde-par som används för att skicka mer information till [!DNL Target]. [!DNL Target] söker automatiskt efter dessa parametrar (reserverade namn) för att fylla i intäktsdata.

För `orderConfirmPage`bör du skicka in `orderTotal`, `orderId`och `productPurchasedId`.

Dessa parametrar måste skickas till target-global-mbox via `targetPageParams()`. Mer information finns i [Skicka parametrar till en global mbox](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md).

Du vill också lägga till målinriktning i konverteringsdelen så att [!DNL Target] räknar bara konverteringar på målversionen av mbox när orderbekräftelsesidan har visats, vilket visas nedan:

![alt-bild](assets/revenue1.png)

Avsnittet Webbplatssidor som visas ovan innehåller följande val: Aktuell sida, URL, innehåller, orderbekräftelse.

![alt-bild](assets/revenue2.png)

Alternativen i ovanstående bild omfattar följande inställningar:

* **Vad vill du mäta med den här aktiviteten:** Intäkter
* **Standardvy för rapportering:** Intäkter per besökare
* **Vilka åtgärder vidtogs av er målgrupp för att ange att ni har nått ert mål?** Visad mbox, target-global-mbox
