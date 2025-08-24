---
title: Om Adobe Target Delivery API och kända begränsningar
description: Vilka överväganden och kända begränsningar bör jag tänka på när jag använder [!UICONTROL Adobe Target Delivery API]?
keywords: leverans-API
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: 413b16ed0b098de6914558fa29b9ca59aaba958e
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# Överväganden och kända begränsningar

I följande information visas överväganden och kända begränsningar för användning av [!DNL Adobe Target] [!DNL Delivery API].

* Det finns ingen autentisering för [!DNL Target] leverans-API:er.
* Detta API bearbetar inte cookies eller omdirigeringsanrop.
* Både HTTP/1.1- och HTTP/2-rubriknamn är skiftlägeskänsliga, men HTTP/2 använder gemena rubriknamn. Mer information finns i [Hypertext Transfer Protocol version 2 (HTTP/2) ](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank} -dokumentationen.

  Om du använder en slutpunkt som dirigerar besökare genom vår nya infrastruktur för belastningsutjämning uppgraderas deras deras anslutningar automatiskt till HTTP/2. Den här uppgraderingsprocessen konverterar begäranrubriker till gemena rubriker så att de inte betraktas som felformaterade.

  Detta kan vara ett problem för kunderna om deras bibliotek är inställda på att söka efter skiftlägeskänsliga (inte små bokstäver) begärande-/svarshuvuden.

* Var försiktig när du uppdaterar [!DNL Recommendations] [!UICONTROL Catalog] via [!DNL Delivery API]. [!DNL Delivery API] är offentlig, så undvik att använda den för att fylla i klickbara objekt i din rekommendationskatalog. Om du gör det kan innehållet bli ogiltigt och katalogen förorenas.

  **God praxis**:

  Använd bara [!DNL Delivery API] för att uppdatera katalogattribut som:
   * Ändra ofta (till exempel pris, aktienivå).
   * Använd ett fördefinierat format som enkelt kan valideras på webbplatsen.
   * Använd den inte för att lägga till eller ändra klickbara objekt eller annat overifierat innehåll.
   * Vid behov kan du begära kundsupport för att inaktivera kataloguppdateringar via leverans-API:t.

  Mer information finns i dokumentationen för [[!UICONTROL Adobe Target Delivery API]](https://developer.adobe.com/target/implement/delivery-api/){target=_blank}.
