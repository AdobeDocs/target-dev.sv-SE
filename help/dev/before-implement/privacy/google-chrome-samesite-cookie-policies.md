---
keywords: google, samesite, cookies, chrome 80, ietf
description: Upptäck hur [!DNL Adobe Target] hanterar den IETF-standard för SameSite som introducerades i Google Chrome version 80 och det du behöver göra för att följa dessa principer.
title: Hur [!DNL Target] Vill du hantera cookie-principer för Google Samesite?
feature: Privacy & Security
exl-id: 58a83def-9625-4d44-914f-203509c6c434
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1950'
ht-degree: 0%

---

# Cookiprinciper för Google Chrome SameSite

Google börjar som standard införa nya cookie-principer för användare som börjar med Chrome 80, som kommer att släppas i början av 2020. I den här artikeln förklaras allt du behöver veta om de nya reglerna för cookie-filer för samma webbplats, hur [!DNL Adobe Target] stöder dessa profiler och hur du kan använda [!DNL Target] för att följa Google Chrome nya SameSite Cookie-principer.

Från och med Chrome 80 måste webbutvecklare uttryckligen ange vilka cookies som kan användas på olika webbplatser. Detta är den första av många meddelanden som Google planerar att göra för att förbättra integriteten och säkerheten på webben.

Med tanke på att Facebook har varit på frammarsch när det gäller sekretess och säkerhet har andra stora aktörer som Apple, och nu Google, snabbt dragit nytta av möjligheten att skapa nya identiteter som integritets- och säkerhetsstyrkor. Apple ledde paketet genom att först meddela ändringar av cookie-reglerna tidigt i år via ITP 2.1 och nyligen ITP 2.2. I ITP 2.1 blockerar Apple helt cookies från tredje part och sparar cookies som skapats i webbläsaren i endast sju dagar. I ITP 2.2 sparas cookies i endast en dag. Google tillkännagivande är inte alls lika aggressivt som Apple, men det är det första steget mot samma slutmål. Mer information om Apple policy finns i [Apple Intelligent Tracking Prevention (ITP) 2.x](/help/dev/before-implement/privacy/apple-itp-2x.md).

## Vad är cookies och hur används de?

Innan vi börjar dyka upp för Google när det gäller att ändra deras cookies-policy måste vi ta reda på vilka cookies som är och hur de används. Kakor är enkelt uttryckt små textfiler som lagras i webbläsaren och som används för att komma ihåg användarattribut.

Cookies är viktiga eftersom de förbättrar användarens upplevelse när de surfar på webben. Om du till exempel handlar på en e-handelswebbplats och lägger till något i kundvagnen, men inte loggar in eller köper på det besöket, kommer cookies att spara dina artiklar i kundvagnen för nästa besök. Eller tänk om du tvingades ange ditt användarnamn och lösenord på nytt varje gång du besöker din sociala medias webbplats. Cookies löser det problemet också eftersom de lagrar information som hjälper webbplatser att identifiera vem du är. Den här typen av cookies kallas cookies från första part eftersom de skapas och används av webbplatsen som du besökte.

Det finns även cookies från tredje part. Låt oss titta på följande exempel för att förstå dem bättre:

Låt oss säga att ett hypotetiskt företag för sociala medier som kallas &quot;Kompisar&quot; har en Dela-knapp som andra sajter implementerar för att göra det möjligt för Kompisar-användare att dela webbplatsens innehåll i Vänner-flödet. Nu läser en användare en nyhetsartikel på en nyhetswebbplats som använder knappen Dela och klickar på den för att automatiskt publicera den på sitt vänkonto.

För att detta ska ske hämtar webbläsaren knappen Dela vänner från `platform.friends.com` när nyhetsartikeln läses in. I den här processen bifogar webbläsaren cookies, som innehåller användarens inloggade uppgifter, till begäran till vänservrar. Detta gör att vänner kan publicera nyhetsartikeln i sin feed för användaren utan att användaren behöver logga in.

Allt detta är möjligt genom att använda cookies från tredje part. I det här fallet sparas cookien från tredje part i webbläsaren för `platform.friends.com`så att `platform.friends.com` Du kan göra inlägget i appen Vänner åt användaren.

Om du för ett ögonblick tänker dig hur du ska uppnå detta utan cookies från tredje part måste användaren följa många manuella steg. Först måste användaren kopiera länken till nyhetsartikeln. För det andra måste användaren logga in i Friends-appen separat. Användaren klickar sedan på knappen Skapa inlägg. Användaren kopierar och klistrar sedan in länken i textfältet och klickar slutligen på Publicera. Som du ser kan cookies från tredje part hjälpa användaren att uppleva manuella steg som kan minskas drastiskt.

Mer generellt gör cookies från tredje part det möjligt att lagra data i en användares webbläsare utan att användaren behöver besöka en webbplats explicit.

## Säkerhetsproblem

Även om cookies förbättrar användarupplevelser och strömannonsering kan de även medföra säkerhetsrisker som CSRF-attacker (Cross-Site Request Forgery). Om en användare till exempel loggar in på en bankwebbplats för att betala kreditkortsräkningar och lämnar webbplatsen utan att logga ut och sedan bläddrar till en skadlig webbplats i samma session, kan en CSRF-attack inträffa. Den skadliga webbplatsen kan innehålla kod som gör en förfrågan till bankwebbplatsen som verkställs när sidan läses in. Eftersom användaren fortfarande är autentiserad på bankwebbplatsen kan sessionscookie användas för att starta en CSRF-attack för att initiera en penningöverföringshändelse från användarens bankkonto. Detta beror på att när du besöker en webbplats bifogas alla cookies i HTTP-begäran. Och på grund av dessa säkerhetsproblem försöker Google nu mildra dem.

## Hur [!DNL Target] använder du cookies?

Med allt det där sagt, låt oss se hur [!DNL Target] använder cookies. För att kunna använda [!DNL Target] måste du först installera [!DNL Target] JavaScript-bibliotek på din webbplats. Detta gör att du kan placera en cookie från en annan leverantör i webbläsaren på den användare som besöker webbplatsen. När användaren interagerar med webbplatsen kan du skicka användarens beteendedata och intressedata till [!DNL Target] via JavaScript-biblioteket. The [!DNL Target] JavaScript-biblioteket använder cookies från första part för att extrahera identifieringsinformation om användaren för att mappa till användarens beteende och intressedata. Dessa data används sedan av [!DNL Target] för att driva personaliseringsaktiviteter.

Target använder även (ibland) cookies från tredje part. Om du har flera webbplatser som finns på olika domäner och du vill spåra användarresan över dessa webbplatser kan du använda cookies från tredje part genom att utnyttja spårning mellan domäner. Genom att aktivera spårning mellan domäner i dialogrutan [!DNL Target] JavaScript-bibliotek, ditt konto börjar använda cookies från tredje part. När en användare hoppar från en domän till en annan kommunicerar webbläsaren med backend-servern för Target, och i den här processen skapas en cookie från tredje part som placeras i användarens webbläsare. Genom den cookie som finns i användarens webbläsare kan [!DNL Target] kan leverera en enhetlig upplevelse över olika domäner för en enskild användare.

## Google nya cookie-recept

Google planerar att lägga till stöd för en IETF-standard som kallas SameSite, som kräver att webbutvecklare hanterar cookies med samma Site-attribut i Set-Cookie-huvudet, för att skydda webbplatser som skickar cookies så att användare skyddas.

Det finns tre olika värden som kan skickas till attributet SameSite: Strict, Lax eller None.

| Värde | Beskrivning |
| --- | --- |
| Strikt | Cookies med den här inställningen går bara att komma åt när du besöker den domän där den ursprungligen var inställd. Strict blockerar med andra ord helt cookie-filen från att användas på olika webbplatser. Det här alternativet passar bäst för program som kräver hög säkerhet, t.ex. banker. |
| Lax | Cookies med den här inställningen skickas endast på begäranden på samma plats eller navigering på högsta nivån med icke-idempotenta HTTP-begäranden, som `HTTP GET`. Det här alternativet skulle därför användas om cookien kan användas av tredje part, men med en extra säkerhetsförmån som skyddar användarna från att bli skadade av CSRF-attacker. |
| Ingen | Cookies med den här inställningen fungerar på samma sätt som cookies fungerar idag. |

Chrome 80 innehåller två oberoende inställningar för användare:&quot;SameSite som standard cookies&quot; och&quot;Cookies utan SameSite måste vara säkra.&quot; De här inställningarna aktiveras som standard i Chrome 80.

![Dialogrutan SameSite](../assets/samesite.png)

* **Samma webbplats som standard, cookies**: När detta anges kommer alla cookies som inte anger attributet SameSite automatiskt att tvingas använda `SameSite = Lax`.
* **Cookies utan SameSite måste vara säkra**: När detta anges används cookies utan attributet SameSite eller med `SameSite = None` måste vara säkra. Säkert i det här sammanhanget innebär att alla webbläsarförfrågningar måste följa HTTPS-protokollet. Cookies som inte uppfyller detta krav avvisas. Alla webbplatser bör använda HTTPS för att uppfylla detta krav.

## [!DNL Target] följer Google bästa säkerhetspraxis

På Adobe vill vi alltid stödja branschens senaste metoder för säkerhet och integritet. Vi är glada att kunna meddela att [!DNL Target] har stöd för de nya säkerhets- och sekretessinställningarna som införs av Google.

För inställningen &quot;SameSite som standard cookies&quot; [!DNL Target] kommer att fortsätta leverera personalisering utan att ni behöver påverka och ingripa. [!DNL Target] använder cookies från första part och fortsätter att fungera som flaggan `SameSite = Lax` används av Google Chrome.

För alternativet&quot;Cookies without SameSite must be secure&quot; (Cookies utan SameSite måste vara säkra) måste du, om du inte väljer att använda funktionen för spårning mellan domäner i Target, cookies från första part i [!DNL Target] kommer att fortsätta arbeta.

Men när du väljer att använda spårning mellan domäner för att utnyttja [!DNL Target] över flera domäner, Chrome kräver `SameSite = None` och säkra flaggor som ska användas för cookies från tredje part. Det innebär att du måste se till att dina webbplatser använder HTTPS-protokollet. Bibliotek på klientsidan i [!DNL Target] använder HTTPS-protokollet automatiskt och bifogar `SameSite = None` och säkra flaggor för cookies från tredje part i [!DNL Target] säkerställa att alla aktiviteter fortsätter att leverera.

## Vad behöver du göra?

För att förstå vad du behöver göra för att få [!DNL Target] för användare av Google Chrome 80+, se tabellen nedan där du kan se följande kolumner:

* **JavaScript-målbibliotek**: Om du använder at.js 1.*x* eller at.js 2.*x* på era webbplatser.
* **SameSite som standard-cookies = Aktiverad**: Om dina användare har aktiverat &quot;SameSite som standard-cookies&quot;, hur påverkar det dig och finns det något du behöver göra för [!DNL Target] för att fortsätta arbeta.
* **Cookies utan SameSite måste vara säkra = Enabled**: Om dina användare har aktiverat&quot;Cookies without SameSite must be secure&quot;, hur påverkar det dig och finns det något du behöver göra för att [!DNL Target] fortsätta arbeta.

| JavaScript-målbibliotek | SameSite som standard-cookies = Aktiverad | Cookies utan SameSite måste vara säkra = Enabled |
| --- | --- | --- |
| at.js 1.*x* med cookie. | Ingen påverkan. | Ingen påverkan om du inte använder spårning mellan domäner. |
| at.js 1.*x* med spårning mellan domäner aktiverat. | Ingen påverkan. | Du måste aktivera HTTPS-protokollet för din plats.<br />Target använder en cookie från tredje part för att spåra användare och Google kräver att cookies från tredje part har `SameSite = None` och Säker flagga. För flaggan Secure måste dina webbplatser använda HTTPS-protokollet. |
| at.js 2.*x* | Ingen påverkan. | Ingen påverkan. |

## Vad gör [!DNL Target] behöver du göra?

Så vad behöver vi göra på vår plattform för att hjälpa dig att följa de nya cookie-reglerna för Google Chrome 80+ SameSite?

| JavaScript-målbibliotek | SameSite som standard-cookies = Aktiverad | Cookies utan SameSite måste vara säkra = Enabled |
| --- | --- | --- |
| at.js 1.*x* med cookie. | Ingen påverkan. | Ingen påverkan om du inte använder spårning mellan domäner. |
| at.js 1.*x* med spårning mellan domäner aktiverat. | Ingen påverkan. | at.js 1.*x* med spårning mellan domäner aktiverat. |
| at.js 2.*x* | Ingen påverkan. | Ingen påverkan. |

## Vilken är effekten om du inte går över till HTTPS-protokollet?

Det enda användningsfallet som påverkar dig är om du använder funktionen för domänövergripande spårning i [!DNL Target] via at.js 1.*x*. Utan att gå över till HTTPS, vilket är ett krav från Google, kommer ni att se en topp i unika besökare i alla domäner eftersom den cookie från tredje part som vi använder kommer att tas bort av Google. Och eftersom cookie-filen kommer att tas bort, [!DNL Target] kommer inte att kunna tillhandahålla en konsekvent och personaliserad upplevelse för den användaren när användaren navigerar från en domän till en annan. Den cookie som används av tredje part används främst för att identifiera en enskild användare som navigerar över domäner som du äger.

## Slutsats

I takt med att branschen strävar efter att skapa en säkrare webbsajt för konsumenterna är Adobe helt engagerat i att hjälpa våra kunder att leverera personaliserade upplevelser på ett sätt som säkerställer säkerhet och integritet för slutanvändarna. Allt du behöver göra är att följa de ovan nämnda bästa metoderna och dra nytta av [!DNL Target] för att följa Google Chrome nya SameSite Cookie-principer.
