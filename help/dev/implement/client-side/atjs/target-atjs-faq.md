---
keywords: at.js faq, at.js Frågor och svar, faq, flimcker, loader, page loader, cross domain, file size, filesize, x-domain, at.js och mbox.js, x only, cross domain, safari, single page app, missing selectors, selectors, single page application, tt.omtrdc.net, spa, Adobe Experience Manager, AEM, ip address, httponly, http Endast, säker, ip, cookie-domän
description: Läs svar på vanliga frågor om [!DNL Adobe Target] at.js JavaScript-bibliotek.
title: Vad är vanliga frågor och svar om at.js?
feature: at.js
exl-id: 362ccc5b-8731-46c0-bc52-3e55c273e216
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '2884'
ht-degree: 0%

---

# at.js Frågor och svar

Svar på vanliga frågor om [!DNL Adobe Target] at.js JavaScript-bibliotek.

## Vilka är fördelarna med att använda at.js jämfört med mbox.js?

Library.js ersätter mbox.js. Biblioteket mbox.js stöds inte längre. För de flesta användare ger at.js dock fördelar jämfört med mbox.js.

Bland annat har at.js förbättrat sidinläsningstiden för webbimplementeringar, förbättrat säkerheten och erbjuder bättre implementeringsalternativ för enkelsidiga program.

I följande diagram visas sidinläsningsprestanda med mbox.js jämfört med at.js.

(Klicka på bilden för att expandera till full bredd.)

![Prestandadiagram som jämför mbox.js med at.js](/help/dev/implement/client-side/atjs/assets/atjs_versus_mboxjs.png "Prestandadiagram som jämför mbox.js med at.js"){zoomable=&quot;yes&quot;}

Som framgår ovan började inte sidinnehållet läsas in förrän efter [!DNL Target] samtalet är slutfört. Med at.js börjar sidinnehållet läsas in när [!DNL Target] anropet initieras och väntar inte tills anropet är slutfört.

## Hur påverkar at.js och mbox.js sidinläsningstiden?

Många kunder och konsulter vill veta hur at.js och mbox.js påverkar sidladdningstiden, särskilt när det gäller nya användare och användare som återvänder. Tyvärr är det svårt att mäta och ge konkreta siffror för hur at.js eller mbox.js påverkar sidladdningstiden på grund av varje kunds implementering.

Om besökar-API:t finns på sidan [!DNL Target] kan bättre förstå hur at.js och mbox.js påverkar sidinläsningstiden.

>[!NOTE]
>
>Visitor-API:t och at.js eller mbox.js påverkar bara sidinläsningstiden när du använder den globala mbox-filen (på grund av tekniken för att dölja brödtext). Regionala kryssrutor påverkas inte av integreringen med Visitor API.

I följande avsnitt beskrivs åtgärdssekvensen för nya och återkommande besökare:

### Nya besökare

1. Besökar-API:t läses in, tolkas och körs.
1. at.js / mbox.js läses in, tolkas och körs.
1. Om den globala mbox-autoskapa är aktiverad visas [!DNL Target] JavaScript-bibliotek:

   * Instansierar Visitor-objektet.
   * The [!DNL Target] biblioteket försöker hämta Experience Cloud Visitor-ID-data.
   * Eftersom den här besökaren är en ny besökare skickar besöks-API en korsdomänbegäran till demdex.net.
   * När data för besökar-ID för Experience Cloud har hämtats skickas en begäran till [!DNL Target] har fått sparken.

### Returnerande besökare

1. Besökar-API:t läses in, tolkas och körs.
1. at.js / mbox.js läses in, tolkas och körs.
1. Om den globala mbox-autoskapa är aktiverad visas [!DNL Target] JavaScript-bibliotek:

   * Instansierar Visitor-objektet.
   * The [!DNL Target] biblioteket försöker hämta Experience Cloud Visitor-ID-data.
   * Besökar-API:t hämtar data från cookies.
   * När data för besökar-ID för Experience Cloud har hämtats skickas en begäran till [!DNL Target] har fått sparken.

>[!NOTE]
>
>För nya besökare, när besökar-API:t finns, [!DNL Target] måste gå igenom tråden flera gånger för att säkerställa att [!DNL Target] begäranden innehåller data om Experience Cloud Visitor-ID. För återkommande besökare [!DNL Target] går bara över kabeln till [!DNL Target] för att hämta det personaliserade innehållet.

## Varför verkar det som om jag ser längre svarstider efter en uppgradering från en tidigare version av at.js till version 1.0.0?

at.js version 1.0.0 och senare utlöser alla begäranden parallellt. I tidigare versioner körs förfrågningarna sekventiellt, vilket innebär att förfrågningarna placeras i kö och [!DNL Target] Väntar på att den första begäran ska slutföras innan du går vidare till nästa begäran.

Det sätt som tidigare versioner av at.js kör begäranden på är känsligt för den så kallade &quot;head of line blockering&quot;. in at.js 1.0.0 och senare, [!DNL Target] växlade till parallell körning av begäran.

Om du till exempel kontrollerar vattenfallet på nätverksfliken för at.js 0.9.1 ser du det nästa [!DNL Target] begäran startar inte förrän den föregående har slutförts. Den här sekvensen är inte fallet med at.js 1.0.0 och senare, där alla begäranden börjar samtidigt.

Från ett svarstidsperspektiv, matematiskt, kan den här sekvensen summeras så här

<ul class="simplelist"> 
 <li> at.js 0.9.1: Svarstid för alla [!DNL Target] begäranden = summan av begäranden och svarstid </li> 
 <li> at.js 1.0.0 och senare: svarstid för alla [!DNL Target] begäranden = maximal svarstid för begäranden </li> 
</ul>

at.js-biblioteket version 1.0.0 slutför förfrågningarna snabbare. Dessutom är at.js-begäranden asynkrona, så [!DNL Target] blockerar inte sidåtergivning. Även om en begäran tar några sekunder att slutföra, ser du fortfarande den återgivna sidan, så är det bara vissa delar av sidan som döljs tills [!DNL Target] får ett svar från [!DNL Target] kant.

## Kan jag läsa in [!DNL Target] biblioteket asynkront?

Med versionen at.js 1.0.0 kan du läsa in [!DNL Target] bibliotek asynkront.

Så här läser du in at.js asynkront:

* Vi rekommenderar att du använder taggar i Adobe Experience Platform.
* Du kan också läsa in at.js asynkront genom att lägga till attributet async i script-taggen som läser in at.js. Använd något liknande:

  ```
  <script src="<URL to at.js>" async></script>
  ```

* Du kan också läsa in at.js asynkront med den här koden:

  ```
  var script = document.createElement('script'); 
  script.async = true; 
  script.src = "<URL to at.js>"; 
  document.head.appendChild(script);
  ```

Att läsa in at.js asynkront är ett bra sätt att undvika att blockera webbläsaren från återgivning, men den här tekniken kan leda till att webbsidan flimrar.

Du kan undvika flimmer genom att använda ett fragment som döljer sidan (eller vissa delar) och sedan visar den efter at.js och den globala begäran har lästs in. Utdraget måste läggas till innan at.js läses in.

Om du distribuerar at.js via en asynkron [!UICONTROL Adobe Experience Platform] implementeringen måste du se till att du inkluderar det fragment som döljs direkt på sidorna, innan implementeringen [!DNL Target] använda [!UICONTROL Adobe Experience Platform] Bädda in kod.

Om du distribuerar at.js via en synkron DTM-implementering kan det fördolda fragmentet läggas till via en sidinläsningsregel som aktiveras högst upp på sidan.

Mer information finns i [Hur at.js hanterar flimmer](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md).

## Är at.js kompatibel med [!DNL Adobe Experience Manager] integrering (Experience Manager)?

[!DNL Adobe Experience Manager] 6.2 med FP-11577 (eller senare) har nu stöd för at.js-implementeringar med dess [!UICONTROL Adobe Target Cloud Services] integrering.

## Hur förhindrar jag sidinläsningsflimmer med at.js?

[!DNL Target] innehåller flera sätt att förhindra sidinläsningsflimmer. Mer information finns i [Förhindra flimmer med at.js](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md).

## Vilken är filstorleken för at.js?

Filen at.js är ungefär 109 kB när den hämtas. Eftersom de flesta servrar automatiskt komprimerar filer så att de blir mindre, är at.js ungefär 34 kB vid komprimering (med GZIP eller någon annan metod) på servern och laddas när användarna besöker webbplatsen. Komprimeringsinställningarna på servern där du installerade at.js avgör den faktiska komprimerade storleken.

## Varför är at.js större än mbox.js?

Vid implementering av at.js används ett bibliotek ( at.js), medan implementeringar av mbox.js faktiskt använder två bibliotek ( mbox.js och target.js). En rättvisare jämförelse är at.js jämfört med mbox.js *och* `target.js`. Jämförelse av de gzippade storlekarna för de två versionerna är at.js version 1.2 34 kB och mbox.js version 63 26,2 kB. &quot;

at.js är större eftersom det gör mycket mer DOM-analys jämfört med mbox.js. Detta är obligatoriskt eftersom at.js hämtar&quot;raw&quot;-data i JSON-svaret och måste förstå det. mbox.js used `document.write()` och all tolkning gjordes av webbläsaren.

Trots den större filstorleken visar vår testning att sidorna läses in snabbare med at.js jämfört med mbox.js. At.js är dessutom överlägset ur säkerhetsperspektiv eftersom det inte läser in ytterligare filer dynamiskt eller använder `document.write`.

## Har At.js jQuery? Kan jag ta bort den här delen av at.js eftersom jag redan har jQuery på min webbplats?

at.js använder för närvarande delar av jQuery och därför visas MIT-licensmeddelandet högst upp i at.js. jQuery visas inte och påverkar inte det jQuery-bibliotek som du redan har på sidan, vilket kan vara en annan version. Det går inte att ta bort jQuery-koden i at.js.

## Stöder at.js Safari och korsdomäner som är inställda på endast x?

Nej, om korsdomänen är inställd på endast x och Safari har inaktiverade cookies från tredje part, anger både mbox.js och at.js en inaktiverad cookie och inga mbox-begäranden körs för den aktuella klientens domän.

För att stödja Safari-besökare är en bättre X-Domain inaktiverad (anger endast en cookie från första part) eller aktiverad (anger endast en cookie från första part i Safari, medan cookies från första och tredje part anges i andra webbläsare).

## Kan jag använda målet [!UICONTROL Visual Experience Composer] (VEC) i mina ensidiga program?

Ja, du kan använda VEC för din SPA om du använder at.js 2.x. Mer information finns i [Visual Experience Composer med en sida (SPA)](https://experienceleague.adobe.com/docs/target/using/experiences/spa-visual-experience-composer.html).

## Kan jag använda Adobe Experience Cloud Debugger med at.js-implementeringar?

Ja. Du kan också använda mboxTrace för felsökning eller webbläsarens utvecklingsverktyg för att granska nätverksförfrågningar och filtrera till mbox för att isolera mbox-anrop.

## Kan jag använda specialtecken i mbox-namn med at.js?

Ja, precis som med mbox.js.

## Varför skjuter inte mina lådor på mina webbsidor?

[!DNL Target] kunder använder ibland molnbaserade instanser med [!DNL Target] för testning eller enkla konceptbevis. Dessa domäner, och många andra, ingår i [Lista över offentliga suffix](https://publicsuffix.org/list/public_suffix_list.dat).

I moderna webbläsare sparas inte cookies om du använder dessa domäner om du inte anpassar `cookieDomain` ange med targetGlobalSettings(). Mer information finns i [Använda molnbaserade instanser med [!DNL Target]](/help/dev/implement/client-side/target-debugging-atjs/targeting-using-cloud-based-instances.md).

## Kan IP-adresser användas som cookie-domän när du använder at.js?

Ja, om du använder [at.js version 1.2 eller senare](/help/dev/implement/client-side/atjs/target-atjs-versions.md). Adobe rekommenderar dock att du håller dig uppdaterad med den senaste versionen.

>[!NOTE]
>
>Följande exempel är inte nödvändiga om du använder at.js version 1.2 eller senare.

Beroende på hur du använder [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)kan du behöva göra ytterligare ändringar i koden efter att ha laddat ned at.js. Om du t.ex. behövde lite olika inställningar för [!DNL Target] implementeringar på olika webbplatser och det gick inte att definiera dessa inställningar dynamiskt med anpassat JavaScript, gör anpassningarna manuellt efter att filen har laddats ned och innan den överförs till respektive webbplats.

I följande exempel kan du använda `targetGlobalSettings()` funktionen at.js för att infoga ett kodfragment som stöder IP-adresser:

Det här exemplet gäller en enda IP-adress:

```
if (window.location.hostname === '123.456.78.9') { 
    window.targetGlobalSettings = window.targetGlobalSettings || {}; 
    window.targetGlobalSettings.cookieDomain = window.location.hostname; 
}
```

Det här exemplet gäller ett intervall av IP-adresser:

```
if (/^123\.456\.78\..*/g.test(window.location.hostname)) { 
    window.targetGlobalSettings = window.targetGlobalSettings || {}; 
    window.targetGlobalSettings.cookieDomain = window.location.hostname; 
}
```

## Varför visas varningsmeddelanden, till exempel&quot;åtgärder med väljare som saknas&quot;?

Dessa meddelanden är inte relaterade till funktionen at.js. Biblioteket at.js försöker rapportera något som inte går att hitta i DOM.

Följande är möjliga rotorsaker om du ser det här varningsmeddelandet:

* Sidan byggs dynamiskt och at.js kan inte hitta elementet.
* Sidan byggs långsamt (på grund av ett långsamt nätverk) och at.js kan inte hitta väljaren i DOM.
* Sidstrukturen som aktiviteten körs på har ändrats. Om du öppnar aktiviteten igen i Visual Experience Composer (VEC) bör du få ett varningsmeddelande. Uppdatera aktiviteten så att alla nödvändiga element kan hittas.
* Den underliggande sidan är en del av ett Single Page-program (SPA) eller sidan innehåller element som visas längre ned på sidan och väljaravsökningsfunktionen at.js kan inte hitta dessa element. Öka `selectorsPollingTimeout` kanske kan hjälpa. Mer information finns i [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).
* Alla klickspårningsmått försöker lägga till sig själv på varje sida, oavsett vilken URL som måttet har ställts in på. Även om det är ofarligt visas många av dessa meddelanden.

  Du får bäst resultat om du laddar ned och använder [senaste versionen av at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md). Mer information om nedladdning på .js finns i [Hämta at.js med [!DNL Target] gränssnitt](how-to-deployatjs/implement-target-without-a-tag-manager.md#download-atjs-using-the-target-interface) i [*Så här distribuerar du at.js* > *Implementera [!DNL Target] utan tagghanterare*](how-to-deployatjs/implement-target-without-a-tag-manager.md) artikel.

## Vad är domänen tt.omtrdc.net som [!DNL Target] går serversamtal till?

tt.omtrdc.net är domännamnet för Adobe EDGE-nätverket, som används för att ta emot alla serveranrop för [!DNL Target].

## Varför använder inte at.js alltid flaggorna HTTPOnly och Secure cookie?

HttpOnly kan bara anges via kod på serversidan. [!DNL Target] cookies, t.ex. mbox, skapas och sparas via JavaScript-kod, så [!DNL Target] kan inte använda HTML-cookie-flaggan HttpOnly. [!DNL Target] använder set HttpOnly för cookies från tredje part som anges från serversidan när korsdomänen är aktiverad.

Säker kan bara ställas in via JavaScript när sidan har lästs in via HTTPS. Om sidan först läses in via HTTP kan JavaScript inte ange den här flaggan. Om du dessutom använder flaggan Secure är cookien bara tillgänglig på HTTPS-sidor. För sidor som läses in via HTTPS [!DNL Target] anger attributen Secure och SameSite=None.

Se till att [!DNL Target] kan spåra användare och eftersom cookies genereras på klientsidan, [!DNL Target] Ingen av dessa flaggor används förutom i de situationer som nämns ovan.

## Hur hanterar at.js säkerhetsproblem som XSS- och MITM-attacker?

Kommunikation med Adobe Edge-nätverket, aktiverat av at.js, sker endast via HTTPS så länge som `secureOnly` är inställt på true i funktionen targetGlobalSettings() ([targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)), annars kan at.js växla mellan HTTP och HTTPS baserat på sidprotokollet.

Följande rubriker används som standard:
* HTTP Strict Transport Security (HSTS)
* X-XSS-skydd
* Alternativ för X-innehållstyp
* Referensprincip

Alla rubriker som redan används på klientsidor kan framtvingas. Ett vanligt sätt att göra detta är genom&quot;HTTP Request Header Authorization&quot;. Adobe kundtjänst kan ge ytterligare råd om bästa metoder och praxis.

Begäranden till Adobe Edge Network är dessutom publika (eftersom de är utformade att göras av besökarens webbläsare) och de innehåller inte synlig besöksinformation (de innehåller bara ett besökar-ID). Dessa förfrågningar levererar upplevelser till besökare och innehåller information om vad en besökare ska se på sidan.

Observera att för svarstoken och sessions-ID som skickas i dessa begäranden:

* De spårar kommunikationssessioner
* De består av slumpmässiga tecken
* Sessions-ID är giltiga i 30 minuter
* Svarstoken kan inaktiveras ([Svarstoken](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html))
* De är bara användbara i den miljö där Adobe lösningar finns.

Det förväntas se `Access-Control-Allow-Origin` header med värdet &quot;*&quot; i at.js-begäranden, eftersom de är publika, autentisering krävs inte och Adobe Edge Network måste nås från alla domäner via JavaScript-anrop.

Däremot måste CSP (Content Security Policy) tillämpas på sidan. Mer information om CSP-krav för at.js finns i [Skyddsprincip för innehåll](/help/dev/before-implement/privacy/content-security-policy.md) och [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

## Hur ofta skickar at.js en nätverksbegäran?

[!DNL Target] verkställer alla sina beslut på serversidan. Det innebär att at.js utlöser en nätverksbegäran varje gång sidan läses in igen eller att ett at.js public API anropas.

## I det bästa fallet, kan vi förvänta oss att användaren inte upplever några synliga effekter på sidinläsningen som relaterar till att dölja, ersätta och visa innehåll?

at.js försöker undvika att HTML BODY eller andra DOM-element döljs i förväg under en längre period, men detta beror på nätverksvillkoren och aktivitetsinställningarna. at.js innehåller [inställningar](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) du kan använda för att anpassa CSS-formatet BODY för att dölja, så att du bara kan dölja vissa delar av HTML BODY i förväg i stället för att dölja hela sidans BODY. Förväntningen är att dessa delar innehåller DOM-element som måste vara&quot;personaliserade&quot;.

## Vilken händelsesekvens i ett genomsnittligt scenario där en användare kvalificerar sig för en aktivitet?

AT.js-begäran är en asynkron `XMLHttpRequest`så vi utför följande steg:

1. Sidan läses in.
1. at.js fördöljer HTML BODY. Det finns en inställning för att dölja en viss behållare i förväg i stället för HTML BODY.
1. AT.js-begäran utlöses.
1. Efter [!DNL Target] svar tas emot, [!DNL Target] extraherar CSS-väljarna.
1. Använda CSS-väljare [!DNL Target] skapar STYLE-taggar för att i förväg dölja de DOM-element som ska anpassas.
1. HTML BODY-FORMATMALLEN som döljs före döljningen tas bort.
1. [!DNL Target] börjar avfråga DOM-element.
1. Om ett DOM-element hittas [!DNL Target] använder DOM-ändringar och elementet före döljning av STYLE tas bort.
1. Om DOM-element inte hittas döljs elementen med en global tidsgräns, så att ingen bruten sida hittas.

## Hur ofta är sidans innehåll helt inläst och synligt när at.js äntligen tar bort elementet som aktiviteten ändras från?

Med tanke på ovanstående scenario, hur ofta är sidans innehåll helt inläst och synligt när at.js äntligen döljer elementet som aktiviteten ändras? Med andra ord är sidan helt synlig förutom aktivitetens innehåll, som sedan visas något efter resten av innehållet.

at.js blockerar inte sidan från återgivning. En användare kan märka att vissa tomma områden på sidan representerar element som är anpassade av [!DNL Target]. Om innehållet som ska användas inte innehåller många fjärrresurser, som SCRIPT eller IMG, bör allt återges snabbt.

## Hur skulle en helt cachelagrad sida påverka scenariot ovan? Skulle det vara troligare att aktivitetens innehåll blir synligt när resten av sidans innehåll har lästs in?

Om en sida cachas på ett CDN som ligger nära användarens plats, men inte nära den [!DNL Target] kan användaren se vissa förseningar. [!DNL Target] Kanterna är väl fördelade över hela världen, så det här är oftast inget problem.

## Kan en hjältebild visas och sedan bytas ut efter en kort fördröjning?

Tänk på följande scenario:

The [!DNL Target] timeout är fem sekunder. En användare läser in en sida som har en aktivitet för att anpassa en hjältebild. at.js skickar begäran för att avgöra om det finns en aktivitet att tillämpa, men det finns inget initialt svar. Anta att användaren ser det vanliga innehållet i hjältebilden eftersom inget svar togs emot från [!DNL Target] om det finns någon associerad aktivitet. Efter fyra sekunder [!DNL Target] returnerar ett svar med aktivitetsinnehållet.

Skulle det i det här skedet vara möjligt att visa den alternativa versionen? Så efter fyra sekunder kan hjältebilden bytas ut och användaren kan märka att bilden byts ut?

Till att börja med är bildhjälteelementet DOM dolt. Efter ett svar från [!DNL Target] tas emot, at.js tillämpar DOM-ändringarna, som att ersätta IMG-filen och visa den anpassade hjältebilden.

## Vilken HTML doctype kräver at.js?

at.js kräver dokumenttypen HTML 5.

Syntaxen:

`<!DOCTYPE html>`

Dokumenttypen HTML 5 ser till att sidan läses in i standardläge. När du läser in i felsökningsläge inaktiveras vissa JS-API:er som at.js är beroende av. [!DNL Target] inaktiverar at.js i felsökningsläge.
