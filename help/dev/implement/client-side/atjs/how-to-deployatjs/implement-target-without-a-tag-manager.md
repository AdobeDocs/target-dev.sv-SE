---
keywords: implementera mål, implementera, implementera at.js, tagghanterare, enhetsbeslut, vid enhetsbeslut
description: Lär dig hur du anger inställningar (kontoinformation, implementeringsmetoder osv.) för att implementera biblioteket  [!DNL Adobe Target]  at.js utan att använda en tagghanterare.
title: Kan jag implementera  [!DNL Target] utan en tagghanterare?
feature: Implement Server-side
exl-id: f675ae21-105d-4aa3-9926-59291f1136b5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1693'
ht-degree: 2%

---

# Implementera [!DNL Target] utan tagghanterare

Information om hur du implementerar [!DNL Adobe Target] utan att använda en eller flera taggar i [!DNL Adobe Experience Platform].

>[!NOTE]
>
>Taggar i [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) är den bästa metoden för implementering av [!DNL Target] och biblioteket at.js. Följande information gäller inte när du använder taggar i [!DNL Adobe Experience Platform] för att implementera [!DNL Target].

Klicka **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** för att komma åt implementeringssidan.

Du kan ange följande inställningar på den här sidan:

* Kontoinformation
* Implementeringsmetoder
* Profil-API
* Felsökningsverktyg
* Integritet

>[!NOTE]
>
>Du kan åsidosätta inställningarna i at.js-biblioteket i stället för att konfigurera dem i användargränssnittet för [!DNL Target] Standard/Premium eller genom att använda REST API:er. Mer information finns i [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

## Kontoinformation

Du kan visa följande kontoinformation. Dessa inställningar kan inte ändras.

| Inställning | Beskrivning |
| --- | --- |
| [!UICONTROL Client Code] | Klientkoden är en klientspecifik teckensekvens som ofta krävs när [!DNL Target] API:er används. |
| [!UICONTROL IMS Organization ID] | Detta ID kopplar implementeringen till ditt Adobe Experience Cloud-konto. |
| [!UICONTROL On-Device Decisioning] | Om du vill aktivera enhetsbeslut flyttar du växlingsknappen till positionen&quot;på&quot;.<p>Med enhetsbaserad beslutsfattande kan ni cachelagra era A/B- och Experience Targeting-kampanjer (XT) på servern och fatta beslut i minnet med nästan noll fördröjning. Mer information finns i [Introduktion till enhetsbeslut](../../../server-side/sdk-guides/on-device-decisioning/overview.md). |
| [!UICONTROL Include all existing on-device decisioning qualified activities in the artifact] | (Villkorligt) Det här alternativet visas om du aktiverar enhetsbeslut.<p>Skjut växlingen till&quot;på&quot;-positionen om du vill att alla dina [!DNL Target]-aktiviteter som är kvalificerade för enhetsbeslut automatiskt ska inkluderas i artefakten.<p>Om du inte aktiverar det här alternativet måste du återskapa och aktivera alla enhetsspecifika beslutsaktiviteter för att de ska kunna inkluderas i den genererade regelartefakten. |

## Implementeringsmetoder

Följande inställningar kan konfigureras på panelen Implementeringsmetoder:

### Globala inställningar

>[!NOTE]
>
>De här inställningarna används för alla [!DNL Target] .js-bibliotek. När du har gjort ändringar i avsnittet Implementeringsmetoder måste du hämta biblioteket och uppdatera det i implementeringen.

| Inställning | Beskrivning |
| --- | --- |
| [!UICONTROL Page load enabled (Auto-create global mbox)] | Välj om du vill bädda in det globala mbox-anropet i filen at.js så att det automatiskt utlöses vid varje sidinläsning. |
| [!UICONTROL Global mbox] | Välj ett namn för den globala mbox-filen. Som standard är det här namnet target-global-mbox.<p>Specialtecken, inklusive et-tecken (&amp;), kan användas i mbox-namn med at.js. |
| [!UICONTROL Timeout (seconds)] | Om [!DNL Target] inte svarar med innehåll inom den definierade perioden, kommer serveranropet att ske i timeout och standardinnehållet visas. Ytterligare anrop fortsätter att utföras under besökarens session. Standardvärdet är 5 sekunder.<p>I at.js-biblioteket används timeoutinställningen i `XMLHttpRequest`. Tidsgränsen startar när begäran utlöses och avbryts när [!DNL Target] får ett svar från servern. Mer information finns i [XMLHttpRequest.timeout](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/timeout) i Mozilla Developer Network.<p>Om den angivna tidsgränsen inträffar innan svaret tas emot, visas standardinnehåll och besökaren kan räknas som deltagare i en aktivitet eftersom all datainsamling sker på kanten [!DNL Target]. Om begäran når kanten [!DNL Target] räknas besökaren.<p>Tänk på följande när du konfigurerar timeout-inställningen:<ul><li>Om värdet är för lågt kan användarna se standardinnehåll oftast, men besökaren kan räknas som deltagare i aktiviteten.</li><li>Om värdet är för högt kan besökarna se tomma områden på webbsidan eller tomma sidor om du använder dolt innehåll under längre tidsperioder.</li></ul>Om du vill få en bättre förståelse för svarstiderna i mbox kan du titta på fliken Nätverk i webbläsarens utvecklingsverktyg. Du kan också använda verktyg för övervakning av webbprestanda från tredje part, till exempel Catchpoint.<p>**Obs!**: Inställningen [visitorApiTimeout](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#visitorapitimeout) ser till att [!DNL Target] inte väntar på Visitor API-svar för länge. Den här inställningen och Timeout-inställningen för at.js som beskrivs här påverkar inte varandra. |
| [!UICONTROL Profile Lifetime] | Den här inställningen avgör hur långa besökarprofiler lagras. Som standard lagras profiler i två veckor. Den här inställningen kan ökas upp till 90 dagar.<p>Om du vill ändra inställningen för Profilens livstid kontaktar du [kundtjänst](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?lang=sv-SE#reference_ACA3391A00EF467B87930A450050077C). |

### Main implementation method

>[!NOTE]
>
>[!DNL Adobe Target] stöder både at.js 1.*x* och at.js 2.*x*. Uppgradera till den senaste uppdateringen av någon större version av at.js för att säkerställa att du kör en version som stöds.

Om du vill hämta en at.js-version klickar du på lämplig **Hämta**-knapp.

Om du vill redigera at.js-inställningen klickar du på **[!UICONTROL Edit]** bredvid den önskade at.js-versionen.

>[!WARNING]
>
>Innan du ändrar de här standardinställningarna bör du rådfråga [Client Care](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?lang=sv-SE#reference_ACA3391A00EF467B87930A450050077C) så att du inte påverkar den aktuella implementeringen.

Förutom de inställningar som förklaras ovan är följande specifika at.js-inställningar också tillgängliga:

| Inställning | Beskrivning |
|--- |--- |
| Domänövergripande | För at.js v1.*x*, ange om korsdomänfunktionerna är `disabled` (webbläsare anger cookies i din domän (endast cookies från första part)), `x only` (webbläsare anger cookies endast i måldomänen) eller både och, genom att välja `enabled` (webbläsare anger cookies från både första och tredje part). För at.js v2.10 och senare anger du om korsdomänfunktionerna är `enabled` (webbläsare anger både cookies från första och tredje part) eller `disabled` (webbläsare anger inte cookies från tredje part). |
| Anpassat bibliotekshuvud | Lägg till en anpassad JavaScript som du kan ta med högst upp i biblioteket. |
| Anpassa bibliotekets sidfot | Lägg till en anpassad JavaScript som du kan ta med längst ned i biblioteket. |

### Profil-API

Aktivera eller inaktivera autentisering för batchuppdateringar via API och generera en profilautentiseringstoken.

Mer information finns i [Inställningar för profil-API](/help/dev/before-implement/methods-to-get-data-into-target/profile-api-settings.md).

### Felsökningsverktyg

Generera en auktoriseringstoken för att använda avancerade felsökningsverktyg för [!DNL Target]. Klicka på **[!UICONTROL Generate New Authentication Token]**.

![Generera ny autentiseringstoken](../../../../before-implement/methods-to-get-data-into-target/assets/debugger-auth-token.png)

### Integritet

Med de här inställningarna kan du använda [!DNL Target] i enlighet med gällande datasekretesslagstiftning.

Välj önskad inställning i listrutan Förhindra besökarens IP-adress:

* Senaste oktettförvrängning
* Hela IP-förvrängningen
* Ingen

Mer information finns i [Sekretess](/help/dev/before-implement/privacy/privacy.md).

>[!NOTE]
>
>Alternativet Stöd för äldre webbläsare var tillgängligt i version 0.9.3 och tidigare av at.js. Det här alternativet togs bort i at.js version 0.9.4. En lista över webbläsare som stöds av at.js finns i [Webbläsare som stöds](/help/dev/before-implement/supported-browsers.md).<p>Äldre webbläsare är äldre webbläsare som inte har fullständigt stöd för CORS (Cross Origin Resource Sharing). Dessa webbläsare är bland annat: Internet Explorer-webbläsare tidigare än version 11 och Safari version 6 och tidigare. Om stöd för äldre webbläsare inaktiverades kunde [!DNL Target] inte leverera innehåll eller räkna besökare i rapporter för dessa webbläsare. Om det här alternativet är aktiverat rekommenderas kvalitetssäkring i äldre webbläsare för att säkerställa en bra kundupplevelse.

## Hämta på.js

Instruktioner för att hämta biblioteket med [!DNL Target]-gränssnittet eller hämtnings-API:t.

>[!NOTE]
>
>[Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) är den rekommenderade metoden för implementering av [!DNL Target] och biblioteket at.js. Följande information gäller inte när du använder taggar i [!DNL Adobe Experience Platform] för att implementera [!DNL Target].
>
>[!DNL Adobe Target] stöder både at.js 1.*x* och at.js 2.*x*. Uppgradera till den senaste uppdateringen av någon större version av at.js för att säkerställa att du kör en version som stöds. Mer information om vad som finns i respektive version finns i [at.js Versionsinformation](/help/dev/implement/client-side/atjs/target-atjs-versions.md).

### Hämta at.js med gränssnittet [!DNL Target]

Så här hämtar du at.js från gränssnittet [!DNL Target]:

1. Klicka på **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.
1. Klicka på knappen **[!UICONTROL Download]** bredvid den önskade at.js-versionen i avsnittet Implementeringsmetoder.

### Hämta at.js med [!DNL Target]-API:t för hämtning

Om du vill hämta at.js med API:t.

1. Hämta din klientkod.

   Klientkoden finns längst upp på sidan **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** i gränssnittet [!DNL Target].

1. Hämta ditt administratörsnummer.

   Läs in den här URL:en:

   ```
   https://admin.testandtarget.omniture.com/rest/v1/endpoint/<varname>client code</varname>
   ```

   Ersätt `client code` med klientkoden från steg 1.

   Resultatet av att läsa in den här URL:en ska se ut ungefär som i följande exempel:

   ```
   { 
     "api": "https://admin6.testandtarget.omniture.com/admin/rest/v1" 
   }
   ```

   I det här exemplet är &quot;6&quot; administratörsnumret.

1. Hämta på.js.

   Läs in den här URL:en med följande struktur. När du läser in den här URL:en börjar nedladdningen av din anpassade at.js-fil.

   ```
   https://admin<varname>admin number</varname>.testandtarget.omniture.com/admin/rest/v1/libraries/atjs/download?client=<varname>client code</varname>&version=<version number>
   ```

   * Ersätt `admin number` med ditt administratörsnummer.
   * Ersätt `client code` med klientkoden från steg 1.
   * Ersätt `version number` med det önskade at.js-versionsnumret (till exempel 2.2).

>[!WARNING]
>
>[!DNL Target]-teamet underhåller bara två versioner av at.js - den aktuella versionen och den andra senaste versionen. Uppgradera vid behov at.js för att säkerställa att du kör en version som stöds. Mer information om vad som finns i respektive version finns i [at.js Versionsinformation](/help/dev/implement/client-side/atjs/target-atjs-versions.md).

## at.js-implementering

at.js ska implementeras i elementet `<head>` på alla sidor på webbplatsen.

En typisk implementering av [!DNL Target] som inte använder en tagghanterare, som taggar i [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md), ser ut så här:

```
<!doctype html> 
<html> 
<head> 
    <meta charset="utf-8"> 
    <title>Title of the Page</title> 
    <!--Preconnect and DNS-Prefetch to improve page load time--> 
    <link rel="preconnect" href="//<client code>.tt.omtrdc.net"> 
    <link rel="dns-prefetch" href="//<client code>.tt.omtrdc.net"> 
    <!--/Preconnect and DNS-Prefetch--> 
    <!--Data Layer to enable rich data collection and targeting--> 
    <script> 
        var digitalData = { 
            "page": { 
                "pageInfo": { 
                    "pageName": "Home" 
                } 
            } 
        }; 
    </script> 
    <!--/Data Layer--> 
    <!-- targetPageParams(), targetPageParamsAll(), Data Providers or targetGlobalSettings() functions to enrich the visitor profile or modify the library settings--> 
    <script> 
        targetPageParams = function() { 
            return { 
                "a": 1, 
                "b": 2, 
                "pageName": digitalData.page.pageInfo.pageName, 
                "profile": { 
                    "age": 26, 
                    "country": { 
                        "city": "San Francisco" 
                    } 
                } 
            }; 
        }; 
    </script> 
    <!--/targetPageParams()--> 
 
    <!--jQuery or other helper libraries should be implemented before at.js if you would like to use their methods in Target--> 
    <script src="jquery-3.3.1.min.js"></script> 
    <!--/jQuery--> 
    <!--Target's JavaScript SDK, at.js--> 
    <script src="at.js"></script> 
    <!--/at.js--> 
</head>
<body> 
    The default content of the page 
</body> 
</html>
```

Tänk på följande viktiga punkter:

* Doctype för HTML5 (till exempel `<!doctype html>`) bör användas. Dokumenttyper som inte stöds eller äldre kan göra att [!DNL Target] inte kan göra en begäran.
* Föranslutning och Förhämtning är alternativ som kan hjälpa webbsidorna att läsas in snabbare. Om du använder dessa konfigurationer måste du ersätta `<client code>` med din egen klientkod, som du kan få från sidan **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.
* Om du har ett datalager är det optimalt att definiera så mycket som möjligt av det i `<head>` på sidorna innan at.js läses in. Den här placeringen ger maximal möjlighet att använda den här informationen i [!DNL Target] för personalisering.
* Särskilda [!DNL Target]-funktioner, som `targetPageParams()`, `targetPageParamsAll()`, Data Providers och `targetGlobalSettings()` bör definieras efter datalagret och innan at.js läses in. Dessa funktioner kan också sparas i bibliotekshuvudet på sidan Redigera at.js-inställningar och sparas som en del av biblioteket at.js. Mer information om de här funktionerna finns i [at.js-funktioner](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md).
* Om du använder hjälpbibliotek från JavaScript, t.ex. jQuery, bör du inkludera dem före [!DNL Target] så att du kan använda deras syntax och metoder när du skapar [!DNL Target]-upplevelser.
* Inkludera at.js i `<head>` på dina sidor.

## Spåra konverteringar

I rutan Orderbekräftelse registreras detaljer om beställningar på er webbplats och du kan rapportera baserat på intäkter och order. I rutan Orderbekräftelse kan du också skapa rekommendationsalgoritmer, till exempel&quot;Personer som köpt produkten x har också köpt produkten y&quot;.

>[!NOTE]
>
>Om användare gör inköp på din webbplats rekommenderar Adobe att du implementerar en orderbekräftelseruta även om du använder Analytics for [!DNL Target] (A4T) för din rapportering.

1. Infoga mbox-skriptet efter modellen nedan på sidan med orderinformation.
1. Ersätt ORD I VERSALA BOKSTÄVER med antingen dynamiska eller statiska värden från katalogen.

   >[!TIP]
   >
   >Du kan också skicka beställningsinformation i valfri mbox (den behöver inte ha namnet `orderConfirmPage`). Du kan också skicka beställningsinformation i flera rutor inom samma kampanj.

   ```
   <script type="text/javascript"> 
   adobe.target.trackEvent({ 
       "mbox": "orderConfirmPage", 
       "params":{  
           "orderId": "ORDER ID FROM YOUR ORDER PAGE",  
           "orderTotal": "ORDER TOTAL FROM YOUR ORDER PAGE",  
           "productPurchasedId": "PRODUCT ID FROM YOUR ORDER PAGE, PRODUCT ID2, PRODUCT ID3"  
       } 
   }); 
   </script> 
   ```

>[!NOTE]
>
>I rutan Orderbekräftelse använder du kommatecken som avgränsar flera produkt-ID:n.

I rutan Orderbekräftelse används följande parametrar:

| Parameter | Beskrivning |
|--- |--- |
| orderId | Unikt värde för att identifiera en order för konverteringsinventering.<p>Värdet `orderId` måste vara unikt. Dubblettorder ignoreras i rapporter. |
| orderTotal | Köpets monetära värde.<p>Skicka inte valutasymbolen. Använd ett decimaltecken (inte kommatecken) för att ange decimalvärden. |
| productPurchasedId (valfritt) | En kommaseparerad lista över produkt-ID som köpts i beställningen.<p>Dessa produkt-ID:n visas i granskningsrapporten som stöd för ytterligare rapportanalyser. |
