---
keywords: skyddsprofil för innehåll, csp, at.js, whitelist, tillåtelselista, flimmer, pre-hide, prehidden, content security policy, iFrame, iframe
description: Lär dig mer om CSP-direktiv (Content Security Policy) som du bör lägga till när du använder  [!DNL Adobe Target].
title: Hur hanterar  [!DNL Target] CSP (Content Security Policies)?
feature: Privacy & Security
exl-id: ec6942e5-36d8-4f88-b3d6-47f9eaca03a8
source-git-commit: c43c79b29768694eac534e22047b5ee6a3d0ccd5
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 1%

---

# CSP-direktiv (Content Security Policy)

Om du använder [Content Security Policy](https://en.wikipedia.org/wiki/Content_Security_Policy) (CSP) för din [!DNL Adobe Target] -implementering bör du lägga till följande CSP-direktiv när du använder [ at.js 2.1 eller senare](../../implement/client-side/atjs/target-atjs-versions.md):

* `connect-src` med `*.tt.omtrdc.net` tillåtslista. Nödvändig för att tillåta nätverksbegäran till kanten [!DNL Target].
* `style-src unsafe-inline`. Krävs för att dölja och flimra kontrollen.
* `script-src unsafe-inline`. Krävs för att tillåta exekvering av JavaScript som kan vara en del av ett HTML-erbjudande.

## Vanliga frågor och svar

Läs följande frågor och svar om säkerhetsprofiler:

### Finns det säkerhetsproblem i korsdomänprinciper för korsdomänsdelning (CORS) och Flash?

Det rekommenderade sättet att implementera CORS-policyn är att tillåta åtkomst till endast betrodda ursprung som kräver det via en tillåtelselista med betrodda domäner. Samma sak gäller för Flashens korsdomänspolicy. Vissa [!DNL Target]-kunder är oroade över användningen av jokertecken för domäner i Target. Problemet är att om en användare är inloggad på ett program och besöker en domän som tillåts enligt principen, kan allt skadligt innehåll som körs på den domänen potentiellt hämta känsligt innehåll från programmet och utföra åtgärder i säkerhetskontexten för den inloggade användaren. Denna situation kallas ofta CSRF (Cross-Site Request Forgery).

I en [!DNL Target]-implementering bör dessa principer inte utgöra ett säkerhetsproblem.

&quot;adobe.tt.omtrdc.net&quot; är en domän som ägs av Adobe. [!DNL Adobe Target] är ett test- och personaliseringsverktyg och det förväntas att [!DNL Target] kan ta emot och bearbeta begäranden från var som helst utan att någon autentisering krävs. Dessa begäranden innehåller nyckel/värde-par som används för A/B-testning, rekommendationer eller innehållspersonalisering.

Adobe lagrar inte PII (Personally Identiitable Information) eller annan känslig information på [!DNL Adobe Target]-edge-servrar, som&quot;adobe.tt.omtrdc.net&quot; pekar på.

[!DNL Target] förväntas kunna nås från alla domäner via JavaScript-anrop. Det enda sättet att tillåta den här åtkomsten är genom att använda &quot;Access-Control-Allow-Origin&quot; med jokertecken.

### Hur tillåter eller förhindrar jag att min webbplats bäddas in som en iFrame under utländska domäner?

Om du vill tillåta [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=sv-SE){target=_blank} (VEC) att bädda in din webbplats i en iFrame måste CSP (om den är inställd) ändras på webbserverinställningen. [!DNL Adobe] domäner måste vitlistas och konfigureras.

Av säkerhetsskäl kanske du vill förhindra att din plats bäddas in som iFrame under externa domäner.

I följande avsnitt beskrivs hur du tillåter eller förhindrar att VEC bäddar in din webbplats i en iFrame.

#### Tillåt att VEC bäddar in din webbplats i en iFrame

Den enklaste lösningen för att VEC ska kunna bädda in din webbplats i en iFrame är att tillåta `*.adobe.com`, som är det bredaste jokertecknet.

Exempel:

`Content-Security-Policy: frame-ancestors 'self' *.adobe.com`

Som på följande bild (klicka för att förstora):


![CSP med bredast jokertecken](/help/dev/before-implement/privacy/assets/csp-adobe.png){width="600" zoomable="yes"}

Du kanske bara vill tillåta den faktiska [!DNL Adobe]-tjänsten. Detta scenario kan uppnås med hjälp av `*.experiencecloud.adobe.com + https://experiencecloud.adobe.com`.

Exempel:

`Content-Security-Policy: frame-ancestors 'self' https://*.experiencecloud.adobe.com https://experiencecloud.adobe.com https://experience.adobe.com`

Som på följande bild (klicka för att förstora):

![CSP med Experience Cloud-omfång](/help/dev/before-implement/privacy/assets/csp-experiencecloud.png){width="600" zoomable="yes"}

Den mest restriktiva åtkomsten till ett företags konto kan uppnås med hjälp av `https://<Client Code>.experiencecloud.adobe.com https://experience.adobe.com`, där `<Client Code>` representerar din specifika klientkod.

Exempel:

`Content-Security-Policy: frame-ancestors 'self'  https://ags118.experiencecloud.adobe.com https://experience.adobe.com`

Som på följande bild (klicka för att förstora):

![CSP med klientkodsomfång](/help/dev/before-implement/privacy/assets/csp-clientcode.png){width="600" zoomable="yes"}

>[!NOTE]
>
>Om du har implementerat [Launch/Tag](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) måste den också vara olåst.
>
>Exempel:
>
> `Content-Security-Policy: frame-ancestors 'self' *.adobe.com *.assets.adobedtm.com;`

#### Hindra VEC från att bädda in webbplatsen i en iFrame

Om du inte vill att VEC ska kunna bädda in din webbplats i en iFrame kan du begränsa till enbart&quot;self&quot;.

Exempel:

`Content-Security-Policy: frame-ancestors 'self'`

Som på följande bild (klicka för att förstora):

![CSP-fel](/help/dev/before-implement/privacy/assets/csp-error.png){width="600" zoomable="yes"}

Följande felmeddelande visas:

`Refused to frame 'https://kuehl.local/' because an ancestor violates the following Content Security Policy directive: "frame-ancestors 'self'".`

