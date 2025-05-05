---
keywords: felsökning, vanliga frågor, vanliga frågor, frågor och svar, global, mbox
description: Läs vanliga frågor och svar om Adobe [!DNL Target] globala kryssrutor.
title: Vilka är de vanliga frågorna om den globala rutan?
feature: at.js
exl-id: 7bcd1b67-809a-466a-b648-6e0e44386157
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# Vanliga frågor och svar om Global Mbox

Lista med vanliga frågor och svar om globala kryssrutor.

## Kan jag ha fler än en global mbox om mitt [!DNL Target]-konto har angetts över flera domäner?

Endast en global mbox stöds för hela kontot.

Du kan begränsa var dina aktiviteter körs genom att lägga till URL-regler i dina aktiviteter. Mer information finns i [Inkludera samma upplevelse på liknande sidor](https://experienceleague.adobe.com/docs/target/using/experiences/vec/temtest.html?lang=sv-SE).

Du kan också skicka en parameter på sidan med [targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) och sedan välja parametrarna i avsnittet Konfigurera URL i [!UICONTROL Visual Experience Composer] (VEC) eller genom att lägga till parametrarna som &quot;finements&quot; i [!UICONTROL Form-Based Experience Composer] .

## Hur skickar jag intäktsdata till en [!DNL Target] global mbox?

Om du vill samla in information om intäkter och beställningar för målglobal-mbox måste du skicka &quot;mbox parameters&quot; till [!DNL Target]. Dessa parametrar är namn/värde-par som används för att skicka mer information till [!DNL Target]. [!DNL Target] söker automatiskt efter de här parametrarna (reserverade namn) för att fylla i intäktsdata.

För `orderConfirmPage` ska du skicka in `orderTotal`, `orderId` och `productPurchasedId`.

Dessa parametrar måste skickas till target-global-mbox via `targetPageParams()`. Mer information finns i [Överföra parametrar till en global mbox](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md).

Du kommer också att lägga till målinriktning till konverteringsdelen så att [!DNL Target] bara räknar konverteringar i målrutan global när orderbekräftelsesidan har visats, vilket visas nedan:

![alt-bild](assets/revenue1.png)

Avsnittet Webbplatssidor som visas ovan innehåller följande val: Aktuell sida, URL, innehåller, orderbekräftelse.

![alt-bild](assets/revenue2.png)

Alternativen i ovanstående bild omfattar följande inställningar:

* **Vad vill du mäta med den här aktiviteten:** Intäkter
* **Standardvy för rapportering:** Intäkter per besökare (RPV)
* **Vilka åtgärder har din målgrupp vidtagit för att ange att ditt mål har uppnåtts?** Visar en mbox, target-global-mbox
