---
keywords: Översikt och referens, webbkit, cookies, first-party, third-party, 1st-party, 3rd-party, $8
description: Lär dig mer om Target-cookies (cookie från första part, cookie från tredje part med cookie från första part eller cookie från tredje part enbart).
title: Var hittar jag information om målcookies?
feature: at.js
role: Developer
source-git-commit: 34e8625798121e236a04646dfcf049f9c2b6f9d0
workflow-type: tm+mt
source-wordcount: '1590'
ht-degree: 0%

---

# Målcookies

Cookie-beteendet beror på om det är en cookie från en annan leverantör, en cookie från en annan tillverkare med en cookie från en annan leverantör eller en cookie från en annan tillverkare.

>[!NOTE]
>
>Det här avsnittet innehåller information om `mboxSession` och `mboxPC`. Bästa praxis för implementering rekommenderar att du inte länkar eller lagrar känslig information med cookie-data: `mboxSession` eller `mboxPC`.

Se även [Ta bort målcookien](/help/dev/before-implement/privacy/cookie-deleting.md).

## När cookies från första eller tredje part ska användas

Platskonfigurationen avgör vilka cookies du vill använda. Det är praktiskt att förstå hur Target fungerar när man försöker förstå cookies från första och tredje part. Se [Så här fungerar Adobe Target](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html) för mer information.

Det finns tre huvudsakliga användningsområden för cookies:

1. En domän.

   Alla testerna utförs i en toppnivådomän (`www.domain.com`, `store.domain.com`, `anysub.domain.com`och så vidare).

   Metod: Använd endast cookies från första part (standard).

1. Användare överskrider domäner och du vill spåra och testa deras beteende i dessa domäner.

   Exempel: En användare kommer till din webbplats för att handla men checkar ut genom Yahoo-butiker. Tre metoder (samarbeta med din kontorepresentant för att fastställa det bästa tillvägagångssättet):

   * Aktivera cookies från första och tredje part.
   * Aktivera endast tredjepartscookie (sällsynt, men har fördelen att inte ta bort mbox-cookien från din domän).
   * Aktivera endast cookies och pass från första part `mboxSession` parameter när domänen korsas.

     The `mboxSession` -parametern måste skickas till en landningssida och refereras från JavaScript-biblioteket (Adobe Experience Platform Web SDK eller at.js). Det kan inte vara en mellanliggande omdirigeringssida.

1. Du använder bara adboxar eller FlashBox på en tredjepartswebbplats.

   Två metoder (tillsammans med din kontorepresentant för att fastställa det bästa tillvägagångssättet):

   * Aktivera cookies från första och tredje part.

     För FlashBox och dynamiska kreatörer krävs cookies från första och tredje part.

   * Aktivera endast cookies från tredje part.

     Detta tillvägagångssätt gäller endast i de sällsynta fall där AdBox-implementeringar används utan målinriktning på plats.

## Cookie-beteende från första part

Den första partens cookie lagras i clientdomain.com, där `clientdomain` är din domän.

JavaScript-biblioteket genererar en `mboxSession ID` och lagrar den i Target-cookien. Det första mbox-svaret innehåller erbjudandet och JavaScript-koden som lagrar `mboxPC ID` som genereras av programmet, i mbox-cookien.

>[!NOTE]
>
>AMCV_###@AdobeOrg cookie för första part anges alltid med Experience Cloud Visitor-ID:t.

## Cookie-beteende från tredje part

Tredjepartscookie lagras i clientcode.tt.omtrdc.net och den första partens cookie lagras i clientdomain.com, där `clientdomain` är din domän.

JavaScript-biblioteket genererar en `mboxSession ID`. Den första platsbegäran returnerar HTTP-svarshuvuden som försöker ange cookies från tredje part med namnet `mboxSession` och `mboxPC` och en omdirigeringsbegäran skickas tillbaka med en extra parameter ( `mboxXDomainCheck=true`).

Om webbläsaren accepterar cookies från tredje part inkluderar omdirigeringsbegäran dessa cookies och erbjudandet returneras.

Om webbläsaren avvisar cookies från tredje part inkluderar omdirigeringsbegäran inte dessa cookies, och standardinnehåll visas för alla platser på sidan. Eftersom det inte finns några angivna cookies sker samma process ovan igen vid varje sidbegäran.

>[!NOTE]
>
>demdex.net cookie anges om cookies från tredje part inte blockeras.

## Cookie-beteende för tredje part och första part

Tredjepartscookie lagras i clientcode.tt.omtrdc.net och den första partens cookie lagras i clientdomain.com, där `clientdomain` är din domän.

JavaScript-biblioteket genererar en `mboxSession ID`. Den första platsbegäran returnerar HTTP-svarshuvuden som försöker ange cookies från tredje part med namnet `mboxSession` och `mboxPC`och en omdirigeringsbegäran skickas tillbaka med en extra parameter (`mboxXDomainCheck=true`).

Om webbläsaren accepterar cookies från tredje part inkluderar omdirigeringsbegäran dessa cookies och erbjudandet returneras.

Vissa webbläsare avvisar cookies från tredje part. Om cookie-filen från tredje part är blockerad fungerar fortfarande cookie-filen från den första parten. Målet försöker ange cookie-filen från tredje part, och om den inte kan det kan Target bara spåra på klientens specifika domän. Spårning över domäner fungerar inte om cookie-filen från tredje part blockeras, såvida inte `mboxSession` läggs till i länken som korsar domäner. I det här fallet ställs en annan cookie in och synkroniseras med den tidigare domänens cookie.

## Cookie-inställningar

Cookien har flera standardinställningar. Du kan ändra de här inställningarna om det behövs, förutom cookie-varaktighet. Kontakta din kontorepresentant när du ändrar cookie-inställningarna.

| Inställning | Information |
|--- |--- |
| Kaknamn | mbox. |
| Cookie-domän | Den andra och den översta nivån i de domäner som du underhåller innehållet från. Eftersom cookie används av ditt företags domän är den en cookie från första part.<br />Exempel: `mycompany.com`. |
| Serverdomän | `clientcode.tt.omtrdc.net`, med klientkoden för ditt konto. |
| Cookie-varaktighet | Cookien finns kvar i besökarens webbläsare två veckor efter den senaste inloggningen. Du kan inte ändra varaktighet för cookie-filen. |
| P3P-princip | Cookien publiceras med en P3P-princip, vilket krävs enligt standardinställningen i de flesta webbläsare. En P3P-profil anger för en webbläsare som skickar cookien och hur informationen används. |

Denna cookie har olika värden för att hantera hur besökarna upplever kampanjer:

| Värde | Definition |
|--- |--- |
| sessions-ID | Ett unikt ID för en användarsession. Detta ID varar som standard i 30 minuter. |
| pc-ID | Ett halvpermanent ID för en besökares webbläsare. Varar 14 dagar. |
| check | Ett enkelt testvärde som används för att avgöra om en besökare stöder cookies. Ange varje gång en besökare begär en sida. |
| disable | Ange om besökarens inläsningstid överskrider den timeout som har konfigurerats i JavaScript-biblioteksfilen. Som standard varar det här värdet en timme. |

## Påverkan på Target för Safari-besökare på grund av förändringar i Apple WebKit-spårning

**Hur fungerar Adobe Target tracking?**

| Cookies | Information |
|--- |--- |
| Första parts domäner | Standardimplementeringen för Target-kunder. &quot;mbox&quot;-cookies anges i kundens domän. |
| Spårning från tredje part | Spårning från tredje part är viktigt för reklam och riktad marknadsföring i Target och i Adobe Audience Manager (AAM). Spårning från tredje part kräver serveröverskridande skripttekniker. Målet använder två cookies, &quot;mboxSession&quot; och &quot;mboxPC&quot;, som anges i `clientcode.tt.omtrd.net` domän. |
**Vad är Apple tillvägagångssätt?**

Från Apple:

&quot;Intelligent Tracking Prevention är en ny WebKit-funktion som minskar spårningen mellan webbplatser genom att ytterligare begränsa cookies och andra webbplatsdata.&quot;

&quot;Det här kallas spårning på olika webbplatser och cookie-filen som används av `example-tracker.com` kallas en cookie från tredje part. I våra tester fann vi populära webbplatser med över 70 sådana spårare, som alla i tysthet samlar in data om användarna.&quot;

| Metod | Information |
|--- |--- |
| Förebyggande av intelligent spårning | Mer information finns i [Intelligent spårningsförebyggande](https://webkit.org/blog/7675/intelligent-tracking-prevention/) på webbplatsen WebKit Open Source Web Browser Engine. |
| Cookies | Hur Safari hanterar cookies:<ul><li>Cookies från tredje part som inte finns på en domän som användaren kommer åt direkt sparas aldrig. Det här beteendet är inte nytt. Cookies från tredje part stöds redan i Safari.</li><li>Tredjepartscookies som anges för en domän som användaren kommer åt direkt rensas efter 24 timmar.</li><li>cookies från första part rensas efter 30 dagar om den förstahandsdomänen har klassificerats som spårning av användare på olika webbplatser. Problemet kan gälla stora företag som skickar användare till olika domäner online. Apple har inte klargjort hur exakt dessa domäner klassificeras, eller hur en domän kan avgöra om de har klassificerats som spårning av användare på olika webbplatser.</li></ul> |
| Maskininlärning för att identifiera domäner som är olika platser | Från Apple:<br />Maskininlärningsklassificering: En maskininlärningsmodell används för att klassificera vilka av de främsta privatkontrollerade domänerna som kan spåra användarens webbplats, baserat på insamlad statistik. Av de olika insamlade statistiken visade det sig att tre vektorer har en stark signal för klassificering baserat på aktuella spårningsmetoder: underresurs under antalet unika domäner, underbildruta under antalet unika domäner och antal unika domäner som omdirigeras till. All datainsamling och klassificering sker på enheten.<br />Men om användaren interagerar med `example.com` Som den översta domänen, som ofta kallas förstahandsdomän, anser Intelligent Tracking Prevention att det är en signal om att användaren är intresserad av webbplatsen och tillfälligt anpassar sitt beteende enligt den här tidslinjen:<br />Om användaren interagerade med `example.com` de senaste 24 timmarna är cookies tillgängliga när `example.com` är en tredje part. Den här metoden tillåter inloggning med mitt X-konto på Y.<ul><li>Domäner som besöktes som toppnivådomäner påverkas inte. Exempel på platser som OKTA</li><li>Identifierar domäner som är underdomäner eller underramar till den aktuella sidan över flera unika domäner.</li></ul> |

**Hur påverkas Adobe?**

| Funktioner som påverkas | Information |
|--- |--- |
| Stöd för avanmälan | Stöd för avanmälan om ändringsbrytningar för Apple WebKit-spårning.<br />Målavanmälan använder en cookie i `clientcode.tt.omtrdc.net` domän. Mer information finns i [Integritet](/help/dev/before-implement/privacy/privacy.md).<br />Målet har stöd för två avanmälningar:<ul><li>En per klient (klienten hanterar länken för avanmälan).</li><li>Ett via Adobe som gör att användaren slipper alla Target-funktioner för alla kunder.</li></ul>Båda metoderna använder cookie-filen från tredje part. |
| Verksamhetens syfte? | Kunderna kan själva välja [livslängd för profil](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html) för målkonton (upp till 90 dagar). Orsaken är att om kontots profillivslängd är längre än 30 dagar och cookie-filen rensas eftersom kundens domän har markerats som spårning av användare på alla webbplatser påverkas beteendet för Safari-besökare i följande områden i Target:<br />**[!UICONTROL Target reports]**: Om en Safari-användare går in i en aktivitet returneras efter 30 dagar och konverteras sedan räknas användaren som två besökare och en konvertering.<br />Detta beteende är detsamma för aktiviteter som använder Analytics som rapportkälla (A4T).<br />**[!UICONTROL Profile & activity membership]**:<ul><li>Profildata raderas när cookie-filen från första part förfaller.</li><li>Aktivitetsmedlemskapet raderas när cookie-filen från första part förfaller.</li><li> Målet fungerar inte i Safari för konton som använder en cookie-implementering från tredje part eller en cookie-implementering från första och tredje part. Det här beteendet är inte nytt. Safari har inte tillåtit cookies från tredje part på ett tag.</li></ul><br />**[!UICONTROL Suggestions]**: Om det finns en oro för att kunddomänen kan markeras som en spårningsbesökare som korssession är det säkraste att ange profilens livstid till 30 dagar eller mindre i Target. Den här gränsen gör att användare spåras på liknande sätt i Safari och alla andra webbläsare. |
