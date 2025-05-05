---
keywords: apple, ITP, intelligent tracking prevent, experience cloud id, ecid, itp
description: Lär dig mer om [!DNL Adobe Target] och effekten av Apple ITP (Intelligent Tracking Prevention) som syftar till att skydda Safaris användares integritet.
title: Hur hanterar  [!DNL Target] supporten för Apple ITP?
feature: Privacy & Security
exl-id: 6deee03b-df86-4d0d-999c-b11855ddfda5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# Apple Intelligent Tracking Prevention (ITP) 2.x

ITP (Intelligent Tracking Prevention) är Apple initiativ för att skydda Safaris användarintegritet. Den första releasen av ITP, som var 2017, var inriktad på användning av cookies från tredje part. Faktum är att Apple blockerade cookies från tredje part i sin helhet, vilket i sin tur orsakade stor huvudvärk för marknadsförings- och reklamteknikföretag, eftersom cookies från tredje part i allmänhet används för att spåra besökare och samla in besöksdata. Nu är Apple på väg att införa begränsningar och begränsningar för hur cookies från första part används i Safari.

Dessa versioner av ITP innehåller följande begränsningar:

| Version | Information |
| --- | --- |
| [ITP 2.1](https://webkit.org/blog/8613/intelligent-tracking-prevention-2-1/) | Klientsidans cookies som placeras i webbläsaren med `document.cookie`-API har fästs vid ett sjudagars förfallodatum.<br />Utgiven 21 februari 2019. |
| [ITP 2.2](https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/) | Drastiskt reducerade 7- dagars utgångsvärdet till en dag.<br />Utgiven 24 april 2019. |
| [ITP 2.3](https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/) | Flera tillfälliga lösningar har tagits bort, t.ex. användning av localStorage eller användning av JavaScript `Document.referrer property`.<br />Utgiven 23 september 2019.<br />CNAME-insvepande försvarsfunktion för ITP som släpptes i Safari 14, macOS Big Sur, Catalina, Mojave, iOS 14 och iPadOS 14. Alla cookies som skapas av ett CNAME-insvept HTTP-svar från tredje part kommer att upphöra om sju dagar.<br />Introducerad 12 november 2020. |

## Vad har jag för effekt som [!DNL Target]-kund?

Target tillhandahåller JavaScript-bibliotek som du kan distribuera på dina sidor så att [!DNL Target] kan leverera personalisering i realtid till dina besökare. Det finns tre [!DNL Target] JavaScript-bibliotek i .js 1.*x*, at.js 2.*x*, den [!DNL Adobe Experience Cloud Web SDK] som placerar [!DNL Target]-cookies på klientsidan i dina besökares webbläsare via `document.cookie` API. Därför påverkas [!DNL Target] cookies av Apple ITP 2.1, 2.2 och 2.3 och upphör att gälla efter sju dagar (med ITP 2.1) och efter en dag (med ITP 2.2 och ITP 2.3).

Apple ITP 2.x påverkar [!DNL Target] inom följande områden:

| Effekt | Information |
| --- | --- |
| Möjlig ökning av antalet unika besökare | Eftersom förfallotiden är inställd på sju dagar (med ITP 2.1) och en dag (med ITP 2.2 och ITP 2.3) kan det hända att antalet unika besökare i Safari ökar. Om dina besökare återbesöker din domän efter sju dagar (ITP 2.1) eller en dag (ITP 2.2 och ITP 2.3) tvingas [!DNL Target] att placera en ny [!DNL Target]-cookie på din domän istället för den utgångna cookien. Den nya [!DNL Target]-cookien översätts till en ny unik besökare även om användaren är densamma. |
| Minskade uppslagsperioder för [!DNL Target] aktiviteter | Besöksprofiler för [!DNL Target] aktiviteter kan ha en reducerad uppslagsperiod för beslut. [!DNL Target] cookies används för att identifiera en besökare och lagra användarprofilattribut för personalisering. Eftersom [!DNL Target] cookies kan upphöra på Safari efter sju dagar (ITP 2.1) eller en dag (ITP 2.2 och 2.3), kan användarprofildata som var knutna till den rensade [!DNL Target]-cookien inte användas för beslut. |
| Profilskript baserade på 3rdPartyID | På grund av att förfallotiden är inställd på sju dagar (med ITP 2.1) och en dag (med ITP 2.2 och ITP 2.3), kommer [profilskript](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html?lang=sv-SE) som baseras på cookien 3rdPartyID att sluta fungera när det går ut. |
| QA/Preview URLs in iOS devices | På grund av att förfallotiden är inställd på sju dagar (med ITP 2.1) och en dag (med ITP 2.2 och ITP 2.3), kommer [QA/Preview URL:er](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=sv-SE) sluta fungera när de upphör att gälla eftersom URL:erna baseras på cookien för tredje parts-ID. |

## Påverkas min nuvarande implementering av [!DNL Target]?

Om du använder Experience Cloud ID-biblioteket (ECID) förutom JavaScript-biblioteket [!DNL Target] påverkas implementeringen av de sätt som listas i den här artikeln: [Safari ITP 2.1 Impact on Adobe Experience Cloud and Experience Platform Customers](https://medium.com/adobetech/safari-itp-2-1-impact-on-adobe-experience-cloud-customers-9439cecb55ac).
