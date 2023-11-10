---
title: Hämta Recommendations med leverans-API
description: I den här artikeln får utvecklare hjälp med att hämta rekommendationer för innehåll med Adobe Target Delivery API.
feature: APIs/SDKs, Recommendations, Administration & Configuration
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 9b391f42-2922-48e0-ad7e-10edd6125be6
source-git-commit: d98c7b890f7456de0676cadce5d6c70bc62d6140
workflow-type: tm+mt
source-wordcount: '1506'
ht-degree: 0%

---

# Hämta Recommendations med leverans-API

API:erna för Adobe Target och Adobe Target Recommendations kan användas för att leverera svar på webbsidor, men kan också användas i upplevelser som inte är HTML, inklusive appar, skärmar, konsoler, e-post, kioskdatorer och andra visningsenheter. När Target-bibliotek och JavaScript inte kan användas är det alltså [Målleverans-API](/help/dev/implement/delivery-api/overview.md) ger fortfarande tillgång till alla Target-funktioner för att leverera personaliserade upplevelser.

>[!NOTE]
>
>Använd Target Delivery API när du begär innehåll som innehåller faktiska rekommendationer (rekommenderade produkter eller objekt).

Om du vill hämta rekommendationer skickar du ett Adobe Target Delivery API POST-anrop med lämplig sammanhangsberoende information, som kan innehålla ett användar-ID (som kan användas med profilspecifika rekommendationer som användarens nyligen visade objekt), relevant mbox-namn, mbox-parametrar, profilparametrar eller andra attribut. Svaret innehåller rekommenderade entity.ids (och kan inkludera andra entitetsdata) i JSON- eller HTML-format, som sedan kan visas i enheten.

The [Leverans-API](/help/dev/implement/delivery-api/overview.md) för Adobe Target visar alla befintliga funktioner som finns i en standardbegäran från Target.

Leverans-API:

* Gör att ni kan hämta upplevelser eller erbjudanden för en plats och en målgrupp på ett RESTful-sätt.
* Ingen autentisering krävs.
* Endast POST.
* Bearbetar inte cookies eller omdirigeringssamtal.
* Kräver eller känner inte igen&quot;användarroller&quot;. Det hämtar bara innehåll eller rapporterar händelser till edge-servrar.

Följ de här stegen för att använda Delivery API för att leverera Target-upplevelser, inklusive rekommendationer:

1. Skapa en Target-aktivitet (A/B, XT, AP eller Recommendations) med den formulärbaserade dispositionen (inte Visual Experience Composer).
1. Använd leverans-API:t för att få ett svar på begäranden som genereras av den Target-aktivitet som du just skapade.

&lt;!— F: Varför krävs BÅDA stegen för detta? Om du har definierat en formulärbaserad rekommendation för en mbox, vad är poängen med att ALSO har steget för leverans-API för att hämta resultat? Varför kan du inte bara få formulärbaserade kopior att leverera resultaten i målenheten..? S: Se användningsexempel nedan ... det är när du vill avbryta väntande resultat för att kunna göra mer innan du visar resultaten. Exempel på jämförelser i realtid med lagernivåer. --->

## Skapa en rekommendation med den formulärbaserade Experience Composer

Om du vill skapa rekommendationer som kan användas med leverans-API:t använder du [Formulärbaserad disposition](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html).

1. Först skapar och sparar du en JSON-baserad design som du kan använda i dina rekommendationer. Exempel-JSON, plus bakgrundsinformation om hur JSON-svar kan returneras när en formulärbaserad aktivitet konfigureras, finns i dokumentationen för [Skapa rekommendationsdesigner](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html). I det här exemplet heter designen *Enkel JSON.*
   ![server-side-create-recs-json-design.png](assets/server-side-create-recs-json-design.png)

1. Navigera till i Mål **[!UICONTROL Activities]** > **[!UICONTROL Create Activity]** > **[!UICONTROL Recommendations]** väljer **[!UICONTROL Form]**.

   ![server-side-create-recs.png](assets/server-side-create-recs.png)

1. Markera en egenskap och klicka på **[!UICONTROL Next]**.
1. Definiera den plats där du vill att användarna ska få rekommendationens svar. I exemplet nedan används en plats med namnet *api_charter*. Välj din JSON-baserade design, skapad tidigare, namngiven *Enkel JSON.*
   ![server-side-create-recs-form.png](assets/server-side-create-recs-form1.png)
1. Spara och aktivera rekommendationen. Det kommer att generera resultat. [När resultaten är klara](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/previewing-and-launching-your-recommendations-activity.html)kan du använda leverans-API:t för att hämta dem.

## Använda leverans-API

Syntaxen för [Leverans-API](/help/dev/implement/delivery-api/overview.md) är:

`POST https://{{CLIENT_CODE}}.tt.omtrdc.net/rest/v1/delivery`

1. Observera att klientkoden krävs. Du hittar din klientkod i Adobe Target genom att navigera till **[!UICONTROL Recommendations]** > **[!UICONTROL Settings]**. Anteckna **Klientkod** värdet i **Rekommendations-API-token** -avsnitt.
   ![client-code.png](assets/client-code.png)
1. När du har fått din klientkod konstruerar du ett leverans-API-anrop. Exemplet nedan börjar med **[!UICONTROL Web Batched Mboxes Delivery API Call]** som anges i [Postman-samling för leverans](../../implement/delivery-api/overview.md/#section/Getting-Started/Postman-Collection), och gör relevanta ändringar. Exempel:
   * den **webbläsare** och **adress** objekten togs bort från **Brödtext**, eftersom de inte krävs för andra ändamål än HTML
   * *api_charter* anges som platsnamn i det här exemplet
   * entity.id anges eftersom den här rekommendationen baseras på innehållets likhet, vilket kräver att en aktuell artikelnyckel skickas till Target.
     ![server-side-Delivery-API-call.png](assets/server-side-delivery-api-call2.png)
Kom ihåg att konfigurera frågeparametrarna korrekt. Se till att du anger `{{CLIENT_CODE}}` vid behov. &lt;!— Q: I den uppdaterade anropssyntaxen listas entity.id som en profileParameter i stället för en mboxParameter som i tidigare versioner. ---> &lt;!— Q: Gammal bild ![server-side-create-recs-post.png](assets/server-side-create-recs-post.png) Gammal medföljande text:&quot;Observera att den här rekommendationen baseras på innehåll som liknar produkter baserade på den entity.id som skickas via mboxParameters.&quot; —>
     ![client-code3](assets/client-code3.png)
1. Skicka begäran. Detta körs mot *api_charter* plats, som har en aktiv rekommendation som körs på den, definierad med din JSON-design som kommer att visa en lista över rekommenderade enheter.
1. Få ett svar baserat på JSON-designen.
   ![server-side-create-recs-json-response2.png](assets/server-side-create-recs-json-response2.png)
Svaret innehåller nyckel-ID samt enhets-ID för de rekommenderade entiteterna.

Om du använder API:t för leverans med Recommendations på det här sättet kan du utföra ytterligare steg innan du visar rekommendationer till besökaren på en annan enhet än HTML. Du kan till exempel ta svaret från leverans-API:t för att utföra en ytterligare sökning i realtid av information om entitetsattribut (lager, pris, klassificering och så vidare) från ett annat system (till exempel en CMS-, PIM- eller e-handelsplattform) innan du visar det slutliga resultatet.

Med den metod som beskrivs i den här guiden kan du få vilket program som helst som kan utnyttja Target-svaret för att ge personaliserade rekommendationer!

## Exempelimplementeringar

Följande resurser innehåller exempel på olika icke-HTML-inriktade implementeringar. Tänk på att varje implementering blir unik på grund av det system och de enheter som används.

| Resurs | Information |
| --- | --- |
| [Adobe Target Everywhere - Implementera serversidan eller i IoT](https://expleague.azureedge.net/labs/L733/index.html) | Adobe Summit 2019 Lab som ger dig en praktisk upplevelse av ett React-program som använder Adobe Target serversides-API:er. |
| [Adobe Target i en mobilapp utan Adobe SDK](https://community.tealiumiq.com/t5/Universal-Data-Hub/Adobe-Target-in-a-Mobile-App-Without-the-Adobe-SDK/ta-p/26753) | Den här guiden visar hur du konfigurerar Adobe Target i din mobilapp utan att installera Adobe SDK. Den här lösningen använder Tealium SDK-webbvyn och Remote Commands-modulen för att skicka och ta emot begäranden till Adobe Visitor API (Experience Cloud) och Adobe Target API. |
| [Konfigurera Target-tillägget i Experience Platform Launch och implementera Target-API:er](https://developer.adobe.com/client-sdks/documentation/adobe-target/) | Steg för att konfigurera Target-tillägget i Experience Platform Launch, lägga till Target-tillägget i programmet och implementera Target-API:er för att begära aktiviteter, förhämta erbjudanden och ange visuellt förhandsgranskningsläge. |
| [Adobe Target Node Client](https://www.npmjs.com/package/@adobe/target-nodejs-sdk) | Open-sourced Target Node.js SDK v1.0 |
| [Serversida - översikt](../../implement/server-side/server-side-overview.md) | Information om Adobe Target Server Side Delivery APIs, Server Side Batch Delivery APIs, Node.js SDK och Adobe Target Recommendations APIs. |
| [Adobe Campaign Content Recommendations in Email](https://medium.com/adobetech/adobe-campaign-content-recommendations-in-email-b51ced771d7f) | Blogg som beskriver hur du kan utnyttja innehållsrekommendationer i e-post via Adobe Target och Adobe I/O Runtime i Adobe Campaign. |

## Hantera Recommendations-inställningar med API:er

Rekommendationer konfigureras oftast i Adobe Target-gränssnittet och används eller nås via Target-API:erna, av skäl som de som nämns i avsnitten ovan. Denna API-samordning är vanlig. Ibland kanske användare vill utföra alla åtgärder via API:er, både konfiguration och användning av resultat. Även om det är mycket mindre vanligt att användare konfigurerar, kör, *och* utnyttja resultatet av rekommendationerna helt med API:erna.

Vi lärde oss [tidigare avsnitt](manage-catalog.md) hur ni hanterar Adobe Target Recommendations-enheter och levererar dem på serversidan. På samma sätt är [Adobe Developer Console](https://developer.adobe.com/console/home) gör att du kan hantera kriterier, kampanjer, samlingar och designmallar utan att behöva logga in på Adobe Target. En fullständig lista över alla Recommendations API:er finns [här](https://developer.adobe.com/target/administer/recommendations-api/), men här är en sammanfattning som du kan referera till.

| Resurs | Information |
| --- | --- |
| [Samlingar](https://developer.adobe.com/target/administer/recommendations-api/#tag/Collections) | Visa, skapa, hämta, redigera och ta bort samlingar. |
| [Kriterier](https://developer.adobe.com/target/administer/recommendations-api/#tag/Criteria) | Lista och hämta villkor. |
| [Designer](https://developer.adobe.com/target/administer/recommendations-api/#tag/Designs) | Lista, skapa, hämta, redigera, ta bort och validera designer. |
| [Enheter](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities) | Spara, ta bort och hämta enheter. |
| [Erbjudanden](https://developer.adobe.com/target/administer/recommendations-api/#tag/Promotions) | Visa, skapa, hämta, redigera och ta bort kampanjer. |
| [Kategorivillkor](https://developer.adobe.com/target/administer/recommendations-api/#tag/Category-Criteria) | Visa, skapa, hämta, redigera och ta bort kategorivillkor. |
| [Anpassade villkor](https://developer.adobe.com/target/administer/recommendations-api/#tag/Custom-Criteria) | Visa, skapa, hämta, redigera och ta bort anpassade villkor. |
| [Artikelvillkor](https://developer.adobe.com/target/administer/recommendations-api/#tag/Item-Criteria) | Visa, skapa, hämta, redigera och ta bort objektvillkor. |
| [Popularitetskriterier](https://developer.adobe.com/target/administer/recommendations-api/#tag/Popularity-Criteria) | Visa, skapa, hämta, redigera och ta bort popularitetskriterier. |
| [Kriterier för profilattribut](https://developer.adobe.com/target/administer/recommendations-api/#tag/Profile-Attribute-Criteria) | Visa, skapa, hämta, redigera och ta bort villkor för profilattribut. |
| [Senaste villkor](https://developer.adobe.com/target/administer/recommendations-api/#tag/Recent-Criteria) | Lista, skapa, hämta, redigera och ta bort senaste villkor. |
| [Sekvensvillkor](https://developer.adobe.com/target/administer/recommendations-api/#tag/Sequence-Criteria) | Visa, skapa, hämta, redigera och ta bort sekvensvillkor. |

## Referensdokumentation

* [Dokumentation för Adobe Target Delivery API](/help/dev/implement/delivery-api/overview.md)
* [Integrera Recommendations med e-post](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/integrating-recs-email.html)

## Sammanfattning och granskning

Grattis! Genom att fylla i den här guiden har du lärt dig att:
* [Hantera katalogen med Recommendations API](manage-catalog.md)
* [Hantera anpassade villkor med Recommendations API](manage-custom-criteria.md)
* [Använda leverans-API:t med Recommendations](fetch-recs-server-side-delivery-api.md)
