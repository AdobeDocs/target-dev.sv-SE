---
title: Om Adobe Target Delivery API och kända begränsningar
description: Vilka överväganden och kända begränsningar bör jag tänka på när jag använder [!UICONTROL Adobe Target Delivery API]?
keywords: leverans-API
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: 94a4122244065384f487ca9a29dfa1b414168cb8
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# Överväganden och kända begränsningar

I följande information visas överväganden och kända begränsningar för användning av [!DNL Adobe Target] [!DNL Delivery API].

* Det finns ingen autentisering för [!DNL Target] leverans-API:er.
* Detta API bearbetar inte cookies eller omdirigeringsanrop.
* Både HTTP/1.1- och HTTP/2-rubriknamn är skiftlägeskänsliga, men HTTP/2 använder gemena rubriknamn. Mer information finns i [Hypertext Transfer Protocol version 2 (HTTP/2) ](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank} -dokumentationen.

  Om du använder en slutpunkt som dirigerar besökare genom vår nya infrastruktur för belastningsutjämning uppgraderas deras deras anslutningar automatiskt till HTTP/2. Den här uppgraderingsprocessen konverterar begäranrubriker till gemena rubriker så att de inte betraktas som felformaterade.

  Detta kan vara ett problem för kunderna om deras bibliotek är inställda på att söka efter skiftlägeskänsliga (inte små bokstäver) begärande-/svarshuvuden.
