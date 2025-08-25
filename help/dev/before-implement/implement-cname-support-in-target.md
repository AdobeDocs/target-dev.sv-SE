---
keywords: kundtjänst, namn, certifikatprogram, kanoniskt namn, cookies, certifikat, amc, adobe managed certificate, digicert, domänkontrollsvalidering, dcv, klienttjänst2
description: Arbeta med [!UICONTROL Adobe Client Care] för att implementera stöd för CNAME (Canonical Name) i  [!DNL Adobe Target]  för att hantera annonsblockeringsproblem.
title: Hur använder jag CNAME i Target?
feature: Privacy & Security
exl-id: 5709df5b-6c21-4fea-b413-ca2e4912d6cb
source-git-commit: 17caf4e54d2efa372ebc6f3916f120a37d39d4a5
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 0%

---

# CNAME och Target

Instruktioner för hur du arbetar med [!DNL Adobe Client Care] för att implementera stöd för CNAME (Canonical Name) i [!DNL Adobe Target]. Använd CNAME för att hantera annonsblockeringsproblem eller ITP-relaterade (Intelligent Tracking Prevention) cookie-principer. Med CNAME sker samtal till en domän som ägs av kunden i stället för till en domän som ägs av Adobe.

## Begär CNAME-stöd i Target

1. Bestäm listan med värdnamn som du behöver för ditt SSL-certifikat (se Vanliga frågor och svar nedan).

1. För varje värdnamn skapar du en CNAME-post i din DNS som pekar på ditt vanliga [!DNL Target] värdnamn `clientcode.tt.omtrdc.net`.

   Om klientkoden till exempel är &quot;namnkund&quot; och det föreslagna värdnamnet är `target.example.com` ser din DNS CNAME-post ut ungefär så här:

   ```
   target.example.com.  IN  CNAME  cnamecustomer.tt.omtrdc.net.
   ```

   >[!WARNING]
   >
   >Adobe certifikatutfärdare, DigiCert, kan inte utfärda ett certifikat förrän det här steget har slutförts. Därför kan Adobe inte fullfölja din begäran om CNAME-implementering förrän det här steget är klart.

1. [Fyll i det här formuläret](assets/FPC_Request_Form.xlsx) och inkludera det när du [öppnar en Adobe Client Care-biljett som begär CNAME-stöd](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?lang=sv-SE&#reference_ACA3391A00EF467B87930A450050077C):

   * [!DNL Adobe Target]-klientkod:
   * SSL-certifikatvärdnamn (exempel: `target.example.com target.example.org`):
   * Inköpare av SSL-certifikat (Adobe rekommenderas varmt, se Frågor och svar): Adobe/kund
   * Om kunden köper certifikatet, även kallat &quot;Bring Your Own Certificate&quot; (BYOC), fyll i dessa ytterligare uppgifter:
      * Certifikatorganisation (exempel: Company Inc):
      * Organisationsenhet för certifikat (valfritt, exempel: Marknadsföring):
      * Certifikatland (exempel: USA):
      * Certifikatdeland (exempel: Kalifornien):
      * Certifikatstad (exempel: San Jose):

1. Om Adobe köper certifikatet arbetar Adobe med DigiCert för att köpa och distribuera certifikatet på Adobe produktionsservrar.

   Om kunden köper certifikatet (BYOC) skickar Adobe Client Care CSR-filen (Certificate Signing Request) till dig. Använd CSR när du köper certifikatet via den valfria certifikatutfärdaren. När certifikatet har utfärdats skickar du en kopia av certifikatet och eventuella mellanliggande certifikat till Adobe Client Care för distribution.

   Adobe Client Care meddelar dig när implementeringen är klar.

1. Uppdatera `serverDomain` [documentation](../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverdomain) till det nya CNAME-värdnamnet och ställ in `overrideMboxEdgeServer` på `false` [documentation](../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#overridemboxedgeserver) i at.js-konfigurationen.

## Vanliga frågor

Följande information besvarar vanliga frågor om att begära och implementera CNAME-stöd i Target:

### Kan jag tillhandahålla mitt eget certifikat (ta med ditt eget certifikat eller BYOC)?

Du kan ange ett eget certifikat. Adobe rekommenderar dock inte detta tillvägagångssätt. Hanteringen av SSL-certifikatets livscykel är enklare för både Adobe och dig om Adobe köper och kontrollerar certifikatet. SSL-certifikaten måste förnyas varje år. Därför måste Adobe Client Care kontakta dig varje år för att få ett nytt certifikat i tid. Vissa kunder kan få svårt att snabbt få fram ett förnyat certifikat. Implementeringen av [!DNL Target] äventyras när certifikatet upphör att gälla eftersom webbläsare nekar anslutningar.

>[!WARNING]
>
>Om du begär en CNAME-implementering av [!DNL Target]-ditt eget certifikat ansvarar du för att tillhandahålla förnyade certifikat till Adobe Client Care varje år. Om du tillåter att ditt CNAME-certifikat upphör att gälla innan Adobe kan distribuera ett förnyat certifikat uppstår ett driftstopp för din specifika [!DNL Target]-implementering.

### Hur länge till mitt nya SSL-certifikat upphör att gälla?

Alla certifikat som köpts av Adobe gäller i ett år. Mer information finns i [DigiCert-artikeln om ettårscertifikat](https://www.digicert.com/blog/position-on-1-year-certificates).

### Vilka värdnamn ska jag välja? Hur många värdnamn per domän ska jag välja?

För implementeringar av mål-CNAME krävs bara ett värdnamn per domän i SSL-certifikatet och i kundens DNS. Adobe rekommenderar ett värdnamn per domän. Vissa kunder kräver fler värdnamn per domän för sina egna syften (till exempel testning i mellanlagring), vilket stöds.

De flesta kunder väljer ett värdnamn som `target.example.com`. Adobe rekommenderar att du följer den här metoden, men i slutändan är det ditt val. Begär inte ett värdnamn för en befintlig DNS-post. Om du gör det uppstår en konflikt och det tar längre tid att lösa din [!DNL Target] CNAME-begäran.

### Jag har redan en CNAME-implementering för Adobe Analytics, kan jag använda samma certifikat eller värdnamn?

Nej, [!DNL Target] kräver ett separat värdnamn och certifikat.

### Påverkar min nuvarande implementering av [!DNL Target] av ITP 2.x?

Apple ITP (Intelligent Tracking Prevention) version 2.3 introducerade funktionen för hantering av CNAME-insvepning, som kan identifiera [!DNL Target] CNAME-implementeringar och minskar cookiens giltighetstid till sju dagar. För närvarande har [!DNL Target] ingen lösning för ITP:s CNAME-insvepningsåtgärder. Mer information om ITP finns i [Apple Intelligent Tracking Prevention (ITP) 2.x](../before-implement/privacy/apple-itp-2x.md).

### Vilken typ av tjänstavbrott kan jag förvänta mig när CNAME-implementeringen distribueras?

Det uppstår inga avbrott i tjänsten när certifikatet distribueras (inklusive certifikatförnyelse).

När du har ändrat värdnamnet i implementeringskoden för [!DNL Target] (`serverDomain` in at.js) till det nya CNAME-värdnamnet (`target.example.com`) behandlar webbläsare återkommande besökare som nya besökare. Returnerade besökares profildata går förlorade eftersom den tidigare cookien inte är tillgänglig under det gamla värdnamnet (`clientcode.tt.omtrdc.net`). Den tidigare cookien är inte tillgänglig på grund av säkerhetsmodeller i webbläsaren. Denna störning inträffar endast vid den första brytningen till den nya CNAME. Certifikatförnyelser har inte samma effekt eftersom värdnamnet inte ändras.

### Vilken nyckeltyp och certifikatsignaturalgoritm används för min CNAME-implementering?

Alla certifikat är RSA SHA-256 och nycklarna är RSA 2048-bitars som standard. Nyckelstorlekar som är större än 2 048 bitar ska begäras explicit via [!UICONTROL Customer Care].

### Hur verifierar jag att CNAME-implementeringen är klar för trafik?

Använd följande kommandouppsättning (i kommandoradsterminalen i macOS eller Linux, med bash och curl >=7.49):

1. Kopiera och klistra in den här basfunktionen i terminalen, eller klistra in funktionen i den grundläggande startskriptfilen (vanligen `~/.bash_profile` eller `~/.bashrc`) så att funktionen är tillgänglig för alla terminalsessioner:

   +++ Se detaljer

   ```bash {line-numbers="true"}
    function adobeTargetCnameValidation {
   
     local hostname="$1"
   
     if [ -z "$hostname" ]; then
       echo "ERROR: no hostname specified"
       return 1
     fi
   
     local service="Adobe Target CNAME implementation"
     local edges="41 42 44 45 46 47 48"
     local edgeDomain="tt.omtrdc.net"
     local edgeFormat="mboxedge%d%s.$edgeDomain"
     local poolDomain="pool.data.adobedc.net"
     local shards=5
     local shardsFoundCount=0
     local shardsFound=""
     local shardsFoundOutput=""
     local curlRegex="subject:.*CN=|expire date:|issuer:"
     local curlValidation="SSL certificate verify ok"
     local curlResponseValidation='"OK"'
     local curlEndpoint="/uptime?mboxClient=uptime3"
     local url="https://$hostname$curlEndpoint"
     local sslShopperUrl="https://www.sslshopper.com/ssl-checker.html#hostname=$hostname"
     local success="✅"
     local failure="🚫"
     local info="🔎"
     local rule="="
     local horizontalRule="$(seq ${COLUMNS:-30} | xargs printf "$rule%.0s")"
     local miniRule="$(seq 5 | xargs printf "$rule%.0s")"
     local curlVersion="$(curl --version | head -1 | cut -d' ' -f2)"
     local curlVersionRequired=7.49
     local edgeCount="$(wc -w <<< "$edges" | tr -d ' ')"
     local cnameExists=""
     local endToEndTestSucceeded=""
   
     for region in IRL1 IND1 SIN OR SYD VA TYO; do
       local currShard="${region}-${poolDomain}"
       local curlResult="$(curl -vsm20 --connect-to "$hostname:443:$currShard:443" "$url" 2>&1)"
   
       if grep -q "$curlValidation" <<< "$curlResult"; then
         shardsFound+=" $currShard"
   
         if grep -q "$curlResponseValidation" <<< "$curlResult"; then
           shardsFoundCount=$((shardsFoundCount+1))
           shardsFoundOutput+="\n\n$miniRule $success $hostname [edge shard: $currShard] $miniRule\n"
         else
           shardsFoundOutput+="\n\n$miniRule $failure $hostname [edge shard: $currShard] $miniRule\n"
         fi
   
         shardsFoundOutput+="$(grep -E "$curlRegex" <<< "$curlResult" | sort)"
   
         if ! grep -q "$curlResponseValidation" <<< "$curlResult"; then
           shardsFoundOutput+="\nERROR: unexpected HTTP response from this shard using $url"
         fi
       fi
     done
   
     echo
     echo "$horizontalRule"
     echo
     echo "$service validation for hostname $hostname:"
   
     local dnsOutput="$(dig -t CNAME +short "$hostname" 2>&1)"
     if grep -qFi ".$edgeDomain" <<< "$dnsOutput"; then
       echo "$success $hostname passes DNS CNAME validation"
       cnameExists=true
     else
       echo -n "$failure $hostname FAILED DNS CNAME validation -- "
       if [ -n "$dnsOutput" ]; then
         echo -e "$dnsOutput is not in the subdomain $edgeDomain"
       else
         echo "required DNS CNAME record pointing to <target-client-code>.$edgeDomain not found"
       fi
     fi
   
     for region in IRL1 IND1 SIN OR SYD VA TYO; do
       local curlResult="$(curl -vsm20 --connect-to "$hostname:443:${region}-pool.data.adobedc.net:443" "https://$hostname$curlEndpoint" 2>&1)"
   
       if grep -q "$curlValidation" <<< "$curlResult"; then
         if grep -q "$curlResponseValidation" <<< "$curlResult"; then
           echo -en "$success $hostname passes TLS and HTTP response validation for region $region"
           if [ -n "$cnameExists" ]; then
             echo
           else
             echo " -- the DNS CNAME is not pointing to the correct subdomain for ${service}s with Adobe-managed certificates" \
               "(bring-your-own-certificate implementations don't have this requirement), but this test passes as configured"
           fi
           endToEndTestSucceeded=true
         else
           echo -n "$failure $hostname FAILED HTTP response validation for region $region --" \
             "unexpected response from $url -- "
           if [ -n "$cnameExists" ]; then
             echo "DNS is NOT pointing to the correct shard, notify Adobe Client Care"
           else
             echo "the required DNS CNAME record is missing, see above"
           fi
         fi
       else
         echo -n "$failure $hostname FAILED TLS validation for region $region -- "
         if [ -n "$cnameExists" ]; then
           echo "DNS is likely NOT pointing to the correct shard or there's a validation issue with the certificate or" \
             "protocols, see curl output below and optionally SSL Shopper ($sslShopperUrl):"
           echo ""
           echo "$horizontalRule"
           echo "$curlResult" | sed 's/^/    /g'
           echo "$horizontalRule"
           echo ""
         else
           echo "the required DNS CNAME record is missing, see above"
         fi
       fi
     done
   
     if [ "$shardsFoundCount" -ge "$edgeCount" ]; then
       echo -n "$success $hostname passes shard validation for the following $shardsFoundCount edge shards:"
       echo -e "$shardsFoundOutput"
       echo
   
       if [ -n "$cnameExists" ] && [ -n "$endToEndTestSucceeded" ]; then
         echo "$horizontalRule"
         echo ""
         echo "  For additional TLS/SSL validation, see SSL Shopper:"
         echo ""
         echo "    $info  $sslShopperUrl"
         echo ""
         echo "  To check DNS propagation around the world, see whatsmydns.net:"
         echo ""
         echo "    $info  DNS A records:     https://whatsmydns.net/#A/$hostname"
         echo "    $info  DNS CNAME record:  https://whatsmydns.net/#CNAME/$hostname"
       fi
     else
       echo -n "$failure $hostname FAILED shard validation -- shards found: $shardsFoundCount," \
         "expected: $edgeCount"
       echo ""
     fi
   
     echo
     echo "$horizontalRule"
     echo
   }
   ```

   +++

1. Klistra in det här kommandot (ersätt `target.example.com` med ditt värdnamn):

   `adobeTargetCnameValidation target.example.com`

   Om implementeringen är klar visas utdata som nedan. Den viktiga delen är att alla valideringsstatusrader visar `✅` i stället för `🚫`. Varje CNAME-målserver ska visa `CN=target.example.com`, vilket matchar det primära värdnamnet på det begärda certifikatet (ytterligare SAN-värdnamn på certifikatet skrivs inte ut i dessa utdata).

+++ Se detaljer

```bash {line-numbers="true"}
  $ adobeTargetCnameValidation 
  target.example.com==========================================================Adobe Target CNAME implementation validation for hostname target.example.com:
  ✅ target.example.com passes DNS CNAME validation
  ✅ target.example.com passes TLS and HTTP response validation for region IRL1
  ✅ target.example.com passes TLS and HTTP response validation for region IND1
  ✅ target.example.com passes TLS and HTTP response validation for region SIN
  ✅ target.example.com passes TLS and HTTP response validation for region OR
  ✅ target.example.com passes TLS and HTTP response validation for region SYD
  ✅ target.example.com passes TLS and HTTP response validation for region VA
  ✅ target.example.com passes TLS and HTTP response validation for region TYO
  ✅ target.example.com passes shard validation for the following 7 edge shards:===== ✅ target.example.com [edge shard: IRL1-pool.data.adobedc.net] =====
  *  expire date: Feb 20 23:59:59 2026 GMT
  *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
  *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: IND1-pool.data.adobedc.net] =====
  *  expire date: Feb 20 23:59:59 2026 GMT
  *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
  *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: SIN-pool.data.adobedc.net] =====
  *  expire date: Feb 20 23:59:59 2026 GMT
  *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
  *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: OR-pool.data.adobedc.net] =====
  *  expire date: Feb 20 23:59:59 2026 GMT
  *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
  *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: SYD-pool.data.adobedc.net] =====
  *  expire date: Feb 20 23:59:59 2026 GMT
  *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
  *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: VA-pool.data.adobedc.net] =====
  *  expire date: Feb 20 23:59:59 2026 GMT
  *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
  *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: TYO-pool.data.adobedc.net] =====
  *  expire date: Feb 20 23:59:59 2026 GMT
  *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
  *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com==========================================================  For additional TLS/SSL validation, see SSL Shopper:    🔎  https://www.sslshopper.com/ssl-checker.html#hostname=target.example.com  To check DNS propagation around the world, see whatsmydns.net:    🔎  DNS A records:     https://whatsmydns.net/#A/target.example.com
      🔎  DNS CNAME record:  https://whatsmydns.net/#CNAME/target.example.com 
```

+++

>[!NOTE]
>
>Om det här verifieringskommandot misslyckas vid DNS-validering men du redan har gjort de nödvändiga DNS-ändringarna, kan du behöva vänta tills DNS-uppdateringarna har spridit sig helt. DNS-poster har en associerad [TTL (time-to-live)](https://en.wikipedia.org/wiki/Time_to_live#DNS_records) som anger cacheförfallotid för DNS-svar för dessa poster. Därför kan du behöva vänta åtminstone så länge som TTL:erna är klara. Du kan använda kommandot `dig target.example.com` eller [ G Suite-verktygslådan ](https://toolbox.googleapps.com/apps/dig/#CNAME) för att leta upp dina specifika TTL-filer. Information om hur du kontrollerar DNS-spridning över hela världen finns i [whatsmydns.net](https://whatsmydns.net/#CNAME).

### Hur använder jag en länk för avanmälan med CNAME?

Om du använder CNAME bör länken för avanmälan innehålla parametern &quot;client=`clientcode`, till exempel:
`https://my.cname.domain/optout?client=clientcode`.

Ersätt `clientcode` med din klientkod och lägg sedan till texten eller bilden som ska länkas till [avanmälnings-URL:en](privacy/privacy.md).

## Kända begränsningar

* QA-läget är inte fast när du har CNAME och at.js 1.x eftersom det baseras på en cookie från tredje part. Du kan komma runt problemet genom att lägga till förhandsgranskningsparametrarna i varje URL som du navigerar till. QA-läget är fast när du har CNAME och at.js 2.x.
* När du använder CNAME blir det mer sannolikt att storleken på cookie-huvudet för [!DNL Target] anrop ökar. Adobe rekommenderar att kakstorleken hålls under 8 kB.
