---
keywords: at.js, 2.0, 1.x, cookies
description: Information om hur [!DNL Adobe Target] hantera cookies i at.js 2.x och at.js 1.x
title: at.js Cookies
feature: at.js
exl-id: 154a844a-6855-4af7-8aed-0719b4c389f5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1711'
ht-degree: 0%

---

# at.js cookies

Information om at.js 2.x och at.js 1.*x* cookie-beteende.

## cookie-beteende i at.js 2.x

För at.js version 2.x (upp till, men inte inklusive, version 2.10.0), *endast cookies från första part stöds*. Precis som i at.js 1.*x*, den första partens cookie,&quot;mbox&quot;, lagras i `clientdomain.com`, där `clientdomain` är din domän.

at.js genererar ett sessions-ID och lagrar det i cookien. Det första svaret innehåller all aktivitetsinformation samt `TNT` eller `PC ID` genereras av [!DNL Target] servrar. at.js skriver sedan `TNT/PC ID` till kakan.

The `AMCV_###@AdobeOrg` cookie för första part anges alltid av Experience Cloud ID-tjänsten, även om `ECID` skickas in [!DNL Target] förfrågningar.

>[!NOTE]
>
>För at.js version 2.10.0 och senare stöds cookies både från första part och från korsdomäner.

### Stöd för cookies från tredje part och domänövergripande spårning

Spårning mellan domäner gör det möjligt att se sessioner på två relaterade webbplatser, men med olika domäner, som en enda session. Du kan skapa en [!DNL Target] aktivitet som spänner över `siteA.com` och `siteB.com` och besökaren skulle förbli i samma upplevelse när de korsade domäner. Den här funktionen är kopplad till at.js 1.*x* cookie-beteende från tredje part och från första part.

>[!NOTE]
>
>För at.js version 2.10.0 och senare stöds både cookies från tredje part och domänövergripande spårning.


## at.js 1.*x* cookie-beteende

För at.js version 1.*x* Men cookie-beteendet beror på om det är en cookie från första part, en cookie från tredje part med en cookie från första part eller en cookie från tredje part.

### När cookies från tredje part ska användas

Platskonfigurationen avgör vilka cookies du vill använda. Det är till hjälp att förstå hur [!DNL Target] fungerar när du försöker förstå cookies från första part och tredje part. Se [Hur [!DNL Adobe Target] verk](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html) för mer information.

Det finns tre huvudsakliga användningsområden för cookies:

1. En domän.

   Alla testerna utförs inom en toppnivådomän (`www.domain.com`, `store.domain.com`, `anysub.domain.com`, osv.).

   Använd endast cookies från första part. Det här är standardinställningen.

1. Användare överskrider domäner och du vill spåra och testa deras beteende i dessa domäner.

   Exempel: En användare kommer till din webbplats för att handla men checkar ut genom Yahoo-butiker. Tre metoder (samarbeta med din kontorepresentant för att fastställa det bästa tillvägagångssättet):

   * Aktivera cookies från första och tredje part.
   * Aktivera endast tredjepartsprodukter (mycket sällsynt, men har fördelen att inte ta bort cookien at.js från domänen).
   * Aktivera endast cookies och pass från första part `mboxSession` parameter när domänen korsas.

     The `mboxSession` -parametern måste skickas till en landningssida med referenser till at.js. Det kan inte vara en mellanliggande omdirigeringssida.

1. Du använder bara adboxar eller FlashBox på en tredjepartswebbplats.

   Två metoder (arbeta med kundtjänsthanteraren för att fastställa det bästa sättet):

   * Aktivera cookies från andra tillverkare.

     För FlashBox och dynamiska kreatörer krävs cookies från andra leverantörer.

   * Aktivera endast cookies från tredje part.

     Detta tillvägagångssätt gäller endast i de sällsynta fall där adbox-implementeringar används utan anpassning till webbplatsen.

### cookie-beteende från första part

Den första partens cookie lagras i `clientdomain.com`, där `clientdomain` är din domän.

at.js genererar en `mboxSession ID` och lagrar den i kakan. Det första svaret innehåller erbjudandet samt JavaScript-koden som lagrar `mboxPC ID` som genereras av programmet, i cookien.

>[!NOTE]
>
>The `AMCV_###@AdobeOrg` cookie för första part anges alltid med Experience Cloud Visitor-ID:t.

### Cookie-beteende från tredje part

Tredjepartskakan lagras i `clientcode.tt.omtrdc.net` och cookie-filen från första part lagras i `clientdomain.com`, där `clientdomain` är din domän.

at.js genererar en `mboxSession ID`. Den första platsbegäran returnerar HTTP-svarshuvuden som försöker ange cookies från tredje part med namnet `mboxSession` och `mboxPC` och en omdirigeringsbegäran skickas tillbaka med en extra parameter (`mboxXDomainCheck=true`).

Om webbläsaren accepterar cookies från tredje part inkluderar omdirigeringsbegäran dessa cookies och erbjudandet returneras.

Om webbläsaren avvisar cookies från tredje part inkluderar omdirigeringsbegäran inte dessa cookies, och standardinnehåll visas för alla platser på sidan. Eftersom det inte finns några angivna cookies sker samma process ovan igen vid varje sidbegäran.

### cookie-beteende från tredje part och första part

Tredjepartskakan lagras i `clientcode.tt.omtrdc.net` och cookie-filen från första part lagras i `clientdomain.com`, där `clientdomain` är din domän.

at.js genererar en `mboxSession ID`. Den första platsbegäran returnerar HTTP-svarshuvuden som försöker ange cookies från tredje part med namnet `mboxSession` och `mboxPC`och en omdirigeringsbegäran skickas tillbaka med en extra parameter (`mboxXDomainCheck=true`).

Om webbläsaren accepterar cookies från tredje part inkluderar omdirigeringsbegäran dessa cookies och erbjudandet returneras.

Vissa webbläsare avvisar cookies från tredje part. Om cookie-filen från tredje part är blockerad fungerar fortfarande cookie-filen från den första parten. [!DNL Target] försöker ange cookie-filen från tredje part, och om den inte kan göra det [!DNL Target] kan bara spåra på klientens specifika domän. Spårning mellan domäner fungerar inte om tredje part blockeras, såvida inte `mboxSession` läggs till i länken som korsar domäner. I det här fallet ställs en annan cookie in och synkroniseras med den tidigare domänens cookie.

## Cookie-inställningar

Cookien har flera standardinställningar. Du kan ändra de här inställningarna om det behövs, med undantag för cookie-varaktighet. Kontakta din kontorepresentant när du ändrar cookie-inställningarna.

| Inställning | Information |
|--- |--- |
| Kaknamn | mbox. |
| Cookie-domän | Den andra och den översta nivån i de domäner som du underhåller innehållet från. Eftersom cookie används av ditt företags domän är den en cookie från första part. Exempel: `mycompany.com`. |
| Serverdomän | `clientcode.tt.omtrdc.net`, med klientkoden för ditt konto. |
| Cookie-varaktighet | Cookien finns kvar i besökarens webbläsare två år efter hans eller hennes senaste inloggning.<P>The `deviceIdLifetime` inställningen kan åsidosättas i [at.js version 2.3.1 eller senare](../atjs/target-atjs-versions.md). Mer information finns i [targetGlobalSettings()](../../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md). |
| P3P-princip | Cookien publiceras med en P3P-princip, vilket krävs enligt standardinställningen i de flesta webbläsare. En P3P-profil anger för en webbläsare som skickar cookien och hur informationen ska användas. |

Cookien har ett antal värden för att hantera hur besökarna upplever kampanjer:

| Värde | Definition |
|--- |--- |
| sessions-ID | Ett unikt ID för en användarsession. Som standard varar detta 30 minuter. |
| pc-ID | Ett halvpermanent ID för en besökares webbläsare. Varar 14 dagar. |
| check | Ett enkelt testvärde som används för att avgöra om en besökare stöder cookies. Ange varje gång en besökare begär en sida. |
| disable | Ange om besökarens inläsningstid överskrider den tidsgräns som konfigurerats i Adobe Experience Platform Web SDK eller filen at.js. Som standard varar detta 1 timme. |

## Påverkan på [!DNL Target] för Safari-besökare på grund av Apple WebKit-spårningsändringar

Tänk på följande:

### Hur [!DNL Adobe Target] Spåra arbetet?

| Cookies | Information |
|--- |--- |
| Första parts domäner | Det här är standardimplementeringen för [!DNL Target] kunder.  &quot;mbox&quot;-cookies anges i kundens domän. |
| Spårning från tredje part | Spårning från tredje part är viktigt för annonsering och målgruppsanvändning i [!DNL Target] och in [!DNL Adobe Audience Manager] AAM.  Spårning från tredje part kräver serveröverskridande skripttekniker.  [!DNL Target] använder två cookies, &quot;mboxSession&quot; och &quot;mboxPC&quot; som anges i `clientcode.tt.omtrd.net` domän. |

### Vad är Apple tillvägagångssätt?

Från Apple:

&quot;Intelligent Tracking Prevention är en ny WebKit-funktion som minskar spårningen mellan webbplatser genom att ytterligare begränsa cookies och andra webbplatsdata.&quot;

&quot;Det här kallas spårning på olika webbplatser och cookie-filen som används av `example-tracker.com` kallas en cookie från tredje part. I våra tester fann vi populära webbplatser med över 70 sådana spårare, som alla i tysthet samlar in data om användarna.&quot;

| Metod | Information |
|--- |--- |
| Förebyggande av intelligent spårning | Mer information finns i [Intelligent spårningsförebyggande](https://webkit.org/blog/7675/intelligent-tracking-prevention/) på webbplatsen WebKit Open Source Web Browser Engine. |
| Cookies | Hur Safari hanterar cookies:<ul><li>Cookies från tredje part som inte finns på en domän som användaren kommer åt direkt sparas aldrig. Det här beteendet är inte nytt. Cookies från tredje part stöds redan i Safari.</li><li>Tredjepartscookies som anges för en domän som användaren kommer åt direkt rensas efter 24 timmar.</li><li>cookies från första part rensas efter 30 dagar om den förstahandsdomänen har klassificerats som spårning av användare på olika webbplatser. Problemet kan gälla stora företag som skickar användare till olika domäner online. Apple har inte klargjort hur dessa domäner kommer att klassificeras eller hur en domän kan avgöra om de har klassificerats som spårning av användare på olika webbplatser.</li></ul> |
| Maskininlärning för att identifiera domäner som är olika platser | Från Apple:<P>Maskininlärningsklassificering: En maskininlärningsmodell används för att klassificera vilka av de främsta privatkontrollerade domänerna som kan spåra användarens webbplats, baserat på insamlad statistik. Av de olika insamlade statistiken visade det sig att tre vektorer har en stark signal för klassificering baserat på aktuella spårningsmetoder: underresurs under antalet unika domäner, underbildruta under antalet unika domäner och antal unika domäner som omdirigeras till. All datainsamling och klassificering sker på enheten.<P>Men om användaren interagerar med example.com som toppdomän, vilket ofta kallas förstahandsdomän, är det en signal om att användaren är intresserad av webbplatsen och tillfälligt anpassar sitt beteende enligt den här tidslinjen:<P>Om användaren interagerade med example.com de senaste 24 timmarna är cookies tillgängliga när `example.com` är en tredje part. Detta gör att du kan logga in med mitt X-konto på Y-inloggningsscenarier.<ul><li>Domäner som besöktes som toppnivådomäner påverkas inte. Exempel på platser som OKTA</li><li>Identifierar domäner som är underdomäner eller underramar till den aktuella sidan över flera unika domäner.</li></ul> |

### Hur kommer Adobe att påverkas?

| Funktioner som påverkas | Information |
|--- |--- |
| Stöd för avanmälan | Stöd för avanmälan om ändringsbrytningar för Apple WebKit-spårning.<P>[!DNL Target] avanmälan använder en cookie i `clientcode.tt.omtrdc.net` domän. Mer information finns i [Integritet](/help/dev/before-implement/privacy/privacy.md).<P>[!DNL Target] har stöd för två avanmälningar:<ul><li>En per klient (klienten hanterar länken för avanmälan).</li><li>En via Adobe som tar användaren bort från alla [!DNL Target] för alla kunder.</li></ul>Båda metoderna använder cookie-filen från tredje part. |
| [!DNL Target] verksamhet | Kunderna kan själva välja [livslängd för profil](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html) för [!DNL Target] konton - upp till 90 dagar. Problemet är att om kontots profillivstid är längre än 30 dagar och cookie-filen rensas bort eftersom kundens domän har markerats som spårning av användare på alla webbplatser, påverkas beteendet för Safari-besökare i följande områden i [!DNL Target]:<P>**[!DNL Target]Rapporter**: Om en Safari-användare går in i en aktivitet returneras efter 30 dagar och konverteras sedan räknas användaren som två besökare och en konvertering.<P>Detta beteende är detsamma för aktiviteter som använder [!DNL Analytics] som rapportkälla (A4T).<P>**Profil- och aktivitetsmedlemskap**:<ul><li>Profildata raderas när cookie-filen från första part förfaller.</li><li>Aktivitetsmedlemskapet raderas när cookie-filen från första part förfaller.</li><li> [!DNL Target] fungerar inte i Safari för konton som använder en cookie-implementering från tredje part eller en cookie-implementering från första och tredje part. Observera att det här beteendet inte är nytt. Safari har inte tillåtit cookies från tredje part på ett tag.</li></ul><P>**Förslag**: Om det finns en oro för att kunddomänen kan markeras som en spårningsbesökare som korssession, är det säkraste att ange profilens livstid till 30 dagar eller mindre på [!DNL Target]. På så sätt spåras användare på samma sätt i Safari och alla andra webbläsare. |
