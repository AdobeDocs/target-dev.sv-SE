---
keywords: apple, ITP, intelligent tracking prevent, experience cloud id, ecid, itp
description: Läs mer om [!DNL Adobe Target] och effekterna av Apple Intelligent Tracking Prevention (ITP) som syftar till att skydda Safaris integritet.
title: Hur [!DNL Target] Hantera stödet för Apple ITP?
feature: Privacy & Security
exl-id: 6deee03b-df86-4d0d-999c-b11855ddfda5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 0%

---

# Apple Intelligent Tracking Prevention (ITP) 2.x

ITP (Intelligent Tracking Prevention) är Apple initiativ för att skydda Safaris användarintegritet. Den första releasen av ITP, som var 2017, var inriktad på användning av cookies från tredje part. Faktum är att Apple blockerade cookies från tredje part i sin helhet, vilket i sin tur orsakade stor huvudvärk för marknadsförings- och reklamteknikföretag, eftersom cookies från tredje part i allmänhet används för att spåra besökare och samla in besöksdata. Nu är Apple på väg att införa begränsningar och begränsningar för hur cookies från första part används i Safari.

Dessa versioner av ITP innehåller följande begränsningar:

| Version | Information |
| --- | --- |
| [ITP 2.1](https://webkit.org/blog/8613/intelligent-tracking-prevention-2-1/) | Klickade cookies på klienten som placeras i webbläsaren med `document.cookie` API till en sjudagars utgång.<br />Sändes 21 februari 2019. |
| [ITP 2.2](https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/) | Drastiskt reducerade 7- dagars utgångsvärdet till en dag.<br />Utgiven 24 april 2019. |
| [ITP 2.3](https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/) | Eliminerade flera tillfälliga lösningar, som att använda localStorage eller JavaScript `Document.referrer property`.<br />Utgiven 23 september 2019.<br />CNAME-klädda försvarsfunktioner till ITP som lanserades i Safari 14, macOS Big Sur, Catalina, Mojave, iOS 14 och iPadOS 14. Alla cookies som skapas av ett CNAME-insvept HTTP-svar från tredje part kommer att upphöra om sju dagar.<br />Den 12 november 2020. |

## Vad har jag för effekt som [!DNL Target] kund?

Target innehåller JavaScript-bibliotek som du kan distribuera på dina sidor, så att [!DNL Target] kan leverera personalisering i realtid till era besökare. Det finns tre [!DNL Target] JavaScript-bibliotek vid.js 1.*x*, at.js 2.*x*, [!DNL Adobe Experience Cloud Web SDK] som placerar klientsidan [!DNL Target] cookies i besökarnas webbläsare via `document.cookie` API. Detta resulterar i [!DNL Target] cookies påverkas av Apple ITP 2.1, 2.2 och 2.3 och upphör att gälla efter sju dagar (med ITP 2.1) och efter en dag (med ITP 2.2 och ITP 2.3).

Apple ITP 2.x-effekter [!DNL Target] inom följande områden:

| Effekt | Information |
| --- | --- |
| Möjlig ökning av antalet unika besökare | Eftersom förfallotiden är inställd på sju dagar (med ITP 2.1) och en dag (med ITP 2.2 och ITP 2.3) kan det hända att antalet unika besökare i Safari ökar. Om besökarna återbesöker din domän efter sju dagar (ITP 2.1) eller en dag (ITP 2.2 och ITP 2.3), [!DNL Target] tvingas att placera en ny [!DNL Target] cookie på din domän istället för den utgångna cookien. Den nya [!DNL Target] cookie innebär en ny unik besökare även om användaren är densamma. |
| Minskade uppslagsperioder för [!DNL Target] verksamhet | Besöksprofiler för [!DNL Target] aktiviteter kan ha en reducerad uppslagsperiod för beslut. [!DNL Target] cookies används för att identifiera en besökare och lagra användarprofilattribut för personalisering. Med tanke på [!DNL Target] cookies kan upphöra på Safari efter sju dagar (ITP 2.1) eller en dag (ITP 2.2 och 2.3), det vill säga användarprofildata som var knutna till rensningen [!DNL Target] cookie kan inte användas för beslut. |
| Profilskript baserade på 3rdPartyID | På grund av att utgångsfönstret är inställt på sju dagar (med ITP 2.1) och en dag (med ITP 2.2 och ITP 2.3), [profilskript](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html) baserat på cookie-filen från tredje partID slutar fungera när den upphör att gälla. |
| QA/Preview URLs in iOS devices | På grund av att utgångsfönstret är inställt på sju dagar (med ITP 2.1) och en dag (med ITP 2.2 och ITP 2.3), [URL för frågor och svar/förhandsgranskning](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) kommer att sluta fungera när de upphör att gälla eftersom URL:erna är baserade på cookie-filen för 3rdPartyID. |

## Är min nuvarande implementering av [!DNL Target] påverkas?

Om du använder Experience Cloud ID-biblioteket (ECID) förutom [!DNL Target] JavaScript-bibliotek kommer implementeringen att påverkas på de sätt som anges i den här artikeln: [Safari ITP 2.1 Impact on Adobe Experience Cloud and Experience Platform Customers](https://medium.com/adobetech/safari-itp-2-1-impact-on-adobe-experience-cloud-customers-9439cecb55ac).
