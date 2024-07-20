---
title: Om Adobe Target Delivery API och kända begränsningar
description: Vilka överväganden och kända begränsningar bör jag tänka på när jag använder [!UICONTROL Adobe Target Delivery API]?
keywords: leverans-API
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: 49acf92bbe06dbcee36fef2b7394acd7ce37baad
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 0%

---

# Överväganden och kända begränsningar

* Det finns ingen autentisering för [!DNL Target] leverans-API:er.
* Detta API bearbetar inte cookies eller omdirigeringsanrop.
* Både HTTP/1.1- och HTTP/2-rubriknamn är skiftlägeskänsliga, men HTTP/2 använder gemena rubriknamn. Mer information finns i [Hypertext Transfer Protocol version 2 (HTTP/2)-dokumentationen ](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}.

  Om du använder en slutpunkt som dirigerar besökare genom vår nya infrastruktur för belastningsutjämning uppgraderas deras deras anslutningar automatiskt till HTTP/2. Den här uppgraderingsprocessen konverterar begäranrubriker till gemena rubriker så att de inte betraktas som felformaterade.

  Detta kan vara ett problem för kunderna om deras bibliotek är inställda på att söka efter skiftlägeskänsliga (inte små bokstäver) begärande-/svarshuvuden.
