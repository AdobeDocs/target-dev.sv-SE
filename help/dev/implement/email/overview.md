---
keywords: Implementering, at.js non javascript, adbox, redirector, mbox
description: Lär dig hur du implementerar  [!DNL Adobe Target]  i icke-JavaScript-scenarier, till exempel med en AdBox eller Redirector.
title: Hur implementerar jag  [!DNL Target] för e-post?
feature: Implement Email
exl-id: dda00b75-5d58-4405-ae58-75e7883a30ed
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 0%

---

# E-post: implementera [!DNL Target]

Information om hur du implementerar [!DNL Target] i icke-JavaScript-scenarier, som att använda en AdBox eller Redirector.

Ni kan spåra besök i annonser och annat externt innehåll. Ni kan också identifiera samma användare både på och utanför er webbplats och leverera en enhetlig upplevelse genom hela deras webbupplevelse. Med en enda URL kan AdBox testa utan JavaScript eller at.js.

En AdBox är användbar för webbplatser som inte har at.js, till exempel filialer. Om din aktivitet behöver dynamisk kreativ (du till exempel behöver visa en produkt i annonsen som övergavs i kundvagnen) kan du inte använda en AdBox.

Annonser och omdirigerare kan användas med alla typer av aktiviteter. I följande tabell jämförs Adbox och Redirector samt när de ska användas:

| | Syfte | När ska användas | URL-struktur | Erbjudandetyp | Erbjudandeinnehåll |
|--- |--- |--- |--- |--- |--- |
| AdBox | Returnerar olika bilder till annonsen | Ändra innehållet i en annons | `clientcode&#x200B;.tt.&#x200B;omtrdc&#x200B;.net/&#x200B;m2&#x200B;/&#x200B;clientcode/ubox/&#x200B;image?` | omdirigeringserbjudande | URL för en bild |
| Omdirigerare | Omdirigerar en besökare till en annan webbsida | Ändra en annons landningssida | `clientcode&#x200B;.tt.omtrdc.net/&#x200B;m2/clientcode&#x200B;/ubox/page?` | omdirigeringserbjudande | URL för en sida |

## Bästa praxis för säkerhet

Observera att med Redirector kan du utsättas för en risk för ett Open Redirect-fel. För att undvika obehörig användning av omdirigeringslänkar av tredje part rekommenderar vi att du använder &quot;auktoriserade värdar&quot; för att tillåtslista standarddomänerna för omdirigering av URL. [!DNL Target] använder värdar för att tillåtslista domäner som du vill tillåta omdirigeringar till. Mer information finns i [Skapa Tillåtelselista som anger värdar som har behörighet att skicka mbox-anrop till [!DNL Target]](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=sv-SE#allowlist) i *Värdar*.

## Begränsningar

* Det finns ingen tidsgräns på klientsidan som med standardrutor. Om [!DNL Target] är helt nedtryckt kommer besökare på annonsen inte att se innehåll, inte ens standard.
* Tredjepartscookies används för att spåra besöken. Om PCIds är annorlunda sammanfogas besökarens tredje part som standard med befintliga förstapartsprofiler.
* Om du vill använda cookies från första part på själva AdBox måste du skicka mBox-sessionen i URL:en. Tala med din kontorepresentant om du vill göra detta.
* Om du vill använda cookies från första part för att spåra annonsklickningar skickar du mbox-sessionen i URL:en. Tala med din kontorepresentant om du vill göra detta.
* Om du vill använda mer än en AdBox på samma sida måste du skicka Mbox-sessionen i URL:en. Tala med din kontorepresentant om du vill göra detta. Du kan ha en AdBox- och en Redirector-länk på samma sida (eftersom Redirector finns på en andra sida).
