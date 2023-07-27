---
keywords: global mbox, customize global mbox, edit at.js, at.js, implement at.js
description: Lär dig anpassa en global mbox för at.js på [!UICONTROL Administration]-[!UICONTROL Implementation] sida in [!DNL Adobe Target].
title: Hur anpassar jag en global mbox?
feature: at.js
exl-id: f7809c3d-6e77-4bbe-8da3-4ab0a448c801
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# Anpassa en global mbox

Information som hjälper dig att anpassa en [!DNL Adobe Target] global mbox för at.js.

1. Klicka på **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.

1. Inaktivera **[!UICONTROL Page load enabled (Auto create global mbox)]** lägger du sedan till namnet på den anpassade globala mbox som du vill använda för att leverera aktiviteter från [!DNL Target].

>[!WARNING]
>
>Ändringen sparas automatiskt när du väljer en annan global ruta.

Den här anpassade globala rutan används även för klickspårning.

![custom-global-mbox](../../assets/custom-global-mbox.png)

1. Implementera at.js-biblioteket på din webbplats.

   Se [Så här distribuerar du at.js](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md) för mer information.

1. Tid för övergången med din release.

   När du är redo för [!DNL Target] Om du vill börja använda din globala mbox för alla aktiviteter i framtiden kan du fortsätta med det här steget.

   Uppdatera namnet på den anpassade globala mbox så att det matchar namnet som används i steg 2 ovan.


>[!WARNING]
>
>Alla aktiviteter i ditt konto synkroniseras med den här rutan. Kontrollera att den globala mbox finns på din webbplats så att aktiviteterna fortsätter att fungera. Var noga med att redigera och spara om aktiviteter som har skapats med [!UICONTROL Visual Experience Composer] (VEC) som synkroniseras med den här mbox. Du behöver inte spara om aktiviteter som skapats i [!UICONTROL Form-Based Experience Composer] eller via API.
