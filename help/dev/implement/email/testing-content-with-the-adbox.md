---
keywords: Implementering, ej javascript, mbox, adbox
description: Använd en AdBox för att leverera bilder i en implementering utanför webbplatsen med  [!DNL Adobe Target]. En AdBox är som en mbox, men styrs av en URL i stället för JavaScript.
title: Hur skapar jag en Adbox för en bild?
feature: Implement Email
exl-id: ad1eb6c4-7a16-4054-ae76-57971261e931
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Skapa en Adbox för en bild

Använd en AdBox för att leverera bilder i en implementering utanför webbplatsen med [!DNL Adobe Target].

En AdBox är som en mbox, men den styrs av en URL i stället för JavaScript. AdBox skapas med en särskild AdBox-URL som läser in en &quot;ad&quot;-ruta (eller AdBox) i ditt Adobe-konto. Använd den här AdBox istället för mbox i dina aktiviteter. Använd AdBox-URL:en i stället för en direktbildreferens i e-postmeddelanden eller andra icke-JavaScript-implementeringar.

Hjälp med att välja rätt konfiguration finns i [Icke-JavaScript-baserade implementeringar](/help/dev/implement/email/overview.md).

1. Skapa AdBox-URL:

   ```
   https://myClientCode.tt.omtrdc.net/m2/myClientCode/ubox/
   image?mbox=emailHeroImage123_320x200&
   mboxDefault=http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2Fimg%2Flogo%2Egif
   ```

   * Där `myClientCode` är ditt företags klientkod. Ditt företags klientkod är endast liten och har inga specialtecken.

     Klientkoden finns längst upp på sidan **[!UICONTROL Administation]** > **[!UICONTROL Implementation]** i gränssnittet [!DNL Target].

   * Där `image` är anropstypen. I det här fallet är det en bild.

   * Där `emailHeroImage123_320x200` är namnet på AdBox.

   * Där `http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2Fimg%2Flogo%2Egif` är mbox standardinnehåll. Det här måste vara en bild.

     Detta måste vara URL-kodat och måste vara en absolut referens. Du kan använda [HTML URL-kodningsreferens](https://www.w3schools.com/tags/ref_urlencode.asp) för att snabbt koda dina URL-adresser.

1. Skapa [Omdirigeringserbjudanden](https://experienceleague.adobe.com/docs/target/using/experiences/offers/offer-redirect.html) för varje alternativ bild.

   >[!NOTE]
   >
   >AdBoxes måste läsas in med ett omdirigeringserbjudande eller standarderbjudandet. Andra erbjudandetyper fungerar inte. Eftersom AdBox är en URL kan den bara visa de URL:er som den tar emot, så endast omdirigeringserbjudandet fungerar.

1. Skapa aktiviteten.

   Se [Icke-JavaScript-baserade implementeringar](/help/dev/implement/email/overview.md) för rätt inställningar för att uppnå dina mål.

1. Slutför Frågor och svar om aktiviteten.

   Det bästa är att skapa en dummy-sida och verifiera att alla upplevelser, standardinnehåll och rapporter fungerar som förväntat i alla webbläsartyper, för alla dina miljöer.

1. Starta aktiviteten.
