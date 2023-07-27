---
title: Om Adobe Target Delivery API och kända begränsningar
description: Vilka överväganden och kända begränsningar bör jag tänka på när jag använder [!UICONTROL Adobe Target Delivery API]?
keywords: leverans-API
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---

# Överväganden och kända begränsningar

* Det finns ingen autentisering för [!DNL Target] Leverans-API:er.
* Detta API bearbetar inte cookies eller omdirigeringsanrop.
* Stöd för [!UICONTROL Automated Personalization] (AP) och [!UICONTROL Recommendations] aktiviteter: Detta API har två lägen för att hämta innehåll: körnings- och förhämtningsläge. Förhämtningsläge kan bara användas för [!UICONTROL AB Test] och [!UICONTROL Experience Targeting] (XT) aktiviteter. Använd inte förhämtningsläge för [!UICONTROL Automated Personalization] (AP), [!UICONTROL Auto-Allocate], [!UICONTROL Auto-Target], eller [!UICONTROL Recommendations] aktivitetstyper.
* Både HTTP/1.1- och HTTP/2-rubriknamn är skiftlägeskänsliga, men HTTP/2 använder gemena rubriknamn. Mer information finns i [Hypertext Transfer Protocol version 2 (HTTP/2) - dokumentation](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}.

  Om du använder en slutpunkt som dirigerar besökare genom vår nya infrastruktur för belastningsutjämning uppgraderas deras deras anslutningar automatiskt till HTTP/2. Den här uppgraderingsprocessen konverterar begäranrubriker till gemena rubriker så att de inte betraktas som felformaterade.

  Detta kan vara ett problem för kunderna om deras bibliotek är inställda på att söka efter skiftlägeskänsliga (inte små bokstäver) begärande-/svarshuvuden.
