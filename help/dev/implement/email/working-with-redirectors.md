---
keywords: Implementering, mbox.js non javascript, redirector, cost per click, intäkt per klick
description: Lär dig hur du använder regissörer i e-postimplementeringar, ungefär som hur du använder en mbox i dina [!DNL Adobe Target] aktiviteter.
title: Hur arbetar jag med Redirectors?
feature: Implement Email
exl-id: 072368ff-9f17-4709-ac2d-c9e1f0d888bb
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 0%

---

# Arbeta med regissörer

Använd en omdirigerare på liknande sätt som du använder en mbox i dina tester.

Omdirigeringar skapas med en särskild omdirigerings-URL som läser in en omdirigeringsruta (omdirigerare) till ditt konto. Använd den här omdirigeraren på liknande sätt som du använder en mbox i dina tester. Skicka URL:en för omdirigering till annonsnätverket som annonsens mållänk.

Använd omdirigeraren för att göra följande:

* Spåra klick från era webbannonser till er webbplats
* Skapa en central rapport för att spåra klick för att visa annonser i flera annonsnätverk
* Variera displayannonser och destinationer

  Samma banderoller visas till exempel på din hemsida, kategorisida och produktsida.

* Hitta den landningssida som leder till flest konverteringar

Hjälp med att välja rätt konfiguration finns i [Icke-JavaScript-baserade implementeringar](/help/dev/implement/email/overview.md).

## Skapa en omdirigering

Innan du kan använda en omdirigering måste du skapa den.

1. Bestäm annonsens målvariationer, inklusive standarddestinationen.
1. Skapa URL:en för omdirigeraren.

   ```
   https://<your_testandtarget_clientcode>.tt.omtrdc.net/​m2/yourclientcode/ubox
   /​page?mbox=redirectorlink_456
   &mboxDefault=http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2Fusualdestination%2Ehtm
   ```

   * Där `yourclientcode` är ditt företags klientkod. Ditt företags klientkod är endast liten och har inga specialtecken.

     Klientkoden finns längst upp på sidan **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** i gränssnittet [!DNL Target].

   * `redirectorlink_456` är namnet på omdirigeringsrutan som visas i ditt konto och som ska användas i kampanjer och tester.

     Omdirigeringar fungerar annorlunda jämfört med andra rutor, men visas precis som andra mbox-rutor i ditt konto. Namnge omdirigeraren så att den enkelt kan särskiljas från standardtypsrutorna i ditt konto.  Det bästa sättet är att börja med mbox-namnet med &quot;redirectorlink&quot;.

   * Där `http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2Fusualdestination%2Ehtm` är standardmålet.

     Detta måste vara URL-kodat och måste vara en absolut referens. Du kan använda [HTML URL-kodningsreferens](https://www.w3schools.com/tags/ref_urlencode.asp) för att snabbt koda dina URL-adresser.

   >[!WARNING]
   >
   >Observera att med Redirector kan du utsättas för en risk för ett Open Redirect-fel. För att undvika obehörig användning av omdirigeringslänkar av tredje part rekommenderar Adobe att du använder&quot;auktoriserade värdar&quot; för att tillåtslista standarddomänerna för omdirigering av URL. [!DNL Target] använder värdar för att tillåtslista domäner som du vill tillåta omdirigeringar till. Mer information finns i [Skapa Tillåtelselista som anger värdar som har behörighet att skicka mbox-anrop till [!DNL Target]](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html#allowlist) i *Värdar*.

1. Validera omdirigeringen.
   1. *Bästa praxis för säkerhet*: Kontrollera att domänen som används i omdirigeraren är tillåtslista, vilket anges ovan. Om du använder en domän som inte är tillåtslista kommer Adobe att blockera alla anrop till den domänen för att förhindra att skadliga aktörer använder omdirigeraren för att dirigera om till potentiellt skadliga domäner.
   2. Infoga URL:en för omdirigeraren i en webbläsare och uppdatera.
   3. Logga in på ditt konto, uppdatera din mbox-lista och verifiera att den nya omdirigeraren är listad som en mbox.
1. Om du vill testa olika mål för en annons skapar du [Omdirigeringserbjudanden](https://experienceleague.adobe.com/docs/target/using/experiences/vec/redirect-offer.html) för varje version.
1. Skapa kampanjen.

   Se [Icke-JavaScript-baserade implementeringar](/help/dev/implement/email/overview.md) för rätt konfiguration för att uppnå dina mål.
1. Fullständig kvalitetskontroll av kampanjen.

   Skapa en dummy-sida med en `<a href>` som innehåller URL:en för omdirigeraren. Exempel:

   ```
   <a href=https://<your_clientcode>.tt.omtrdc.net/​m2/yourclientcode/ubox/​page?mbox=
   
   redirectorlink_456&mboxDefault=http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2F​usualdestination%2Ehtm>
   ```

1. Kontrollera att alla upplevelser, standardinnehåll och rapporter fungerar som förväntat i alla webbläsartyper, för alla dina miljöer.

   >[!NOTE]
   >
   >Omdirigeringar stöds inte av funktionen Förhandsvisa erbjudande eller Bläddra efter mbox. Förhandsgranska upplevelser direkt i en webbläsare. `mboxDebug` fungerar inte heller med Redirectors.

1. Skicka den fullständiga URL:en för omdirigeraren till ditt Display Ad Network som annonsmål.

## Använd en omdirigering för att skicka kostnader per klick och intäkt per klick

Information om hur du använder en omdirigering för att plocka kostnader per klick och intäkter per klick.

### Löpande kostnader per klick

Använd en omdirigerare för att skicka kostnaden per klick.

>[!NOTE]
>
>Bästa sättet är att fastställa kostnadsvärdet med hjälp av **[!UICONTROL Score per visit]**-interaktionsmåttet.

Lägg till `&mboxPageValue=-value` i URL:en. Observera det negativa värdet.

Exempel: För en kostnad på 1,10 cent per klick:

```
https://<your_clientcode>.tt.omtrdc.net/​m2/yourclientcode/ubox/​page?mbox=redirectorlink_456
&mboxPageValue=-0.1&mboxDefault=​https://www.yourcompany.com/usualdestination.htm
```

### Passningsintäkt per klick

Använd en omdirigerare för att skicka intäkten per klick.

>[!NOTE]
>
>Bästa sättet är att fastställa intäktsvärdet med hjälp av **[!UICONTROL Score per visit]**-interaktionsmåttet.

Lägg till `&mboxPageValue=value` i URL:en.

Exempel: För en intäkt på 1,10 cent per klick.

```
https://<​your_clientcode>​​​​.tt​​.omtrdc​.net/​​m2/​yourclientcode/​ubox/​​​page?mbox=redirectorlink_456
&mboxPageValue=0.1​&mbox​Default=​​http%3A%2F%2Fwww%2E​yourcompany%2Ecom​%2Fusualdestination%2Ehtm
```
