---
title: Målgruppsanpassning
description: Målgrupper kan användas för att målinrikta era experiment- och personaliseringsaktiviteter. [!DNL Adobe Target] har stöd för en mängd kraftfulla målgruppsanpassningsfunktioner.
exl-id: df1bd856-e848-452c-90a0-abf29e7a2313
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# Målgruppsanpassning

## Ökning

Målgrupper kan användas för att målinrikta era experiment- och personaliseringsaktiviteter. [!DNL Adobe Target] har stöd för en mängd kraftfulla målgruppsanpassningsfunktioner. Följande attribut är tillgängliga för [målgrupp](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/create-audience.html?lang=sv-SE):

### Bibliotek för [!DNL Target]

Mer information finns i [[!DNL Target] Bibliotek](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-library.html?lang=sv-SE).
&#x200B;
* Refereras från Bing
* Chrome Browser
* Firefox-webbläsare
* Refererad från Google
* Internet Explorer
* Linux-operativsystem
* Operativsystemet Mac OS
* Nya besökare
* Returnerande besökare
* Safari Browser
* Surfplatteenhet
* Windows-operativsystem
* Refererad från Yahoo

### Geo

Mer information finns i [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=sv-SE).
&#x200B; &#x200B;
* Land/region
* Läge
* Ort
* Postnummer
* Latitude
* Longitud
* DMA
* Mobiloperatör

### Nätverk

Mer information finns i [Nätverk](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=sv-SE).

* ISP
* Domännamn
* Anslutningshastighet

### Mobil

Mer information finns i [Mobile](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=sv-SE).

* Namn på enhetsmarknadsföring
* Enhetsmodell
* Enhetsleverantör
* Är mobil enhet
* Är mobiltelefon
* Är bärbar dator
* OS
* Skärmhöjd (px)
* Skärmbredd (px)

### Egen

Mer information finns i [Egna parametrar](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=sv-SE).

* alla nyckel-/värdepar

### Operativsystem

Mer information finns i [Operativsystem](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=sv-SE).

* Linux
* Macintosh
* Windows

### Webbplatssidor

Mer information finns i [Webbplatssidor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=sv-SE).

* Aktuell sida
* Föregående sida
* Landningssida
* HTTP-huvud

### Webbläsare

Mer information finns i [Webbläsare](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=sv-SE).

* Typ
* Språk
* Version

### Besökarprofil

Mer information finns i [Besökarprofil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=sv-SE).

* alla nyckel-/värdepar som är beständiga

### Trafikkällor

Mer information finns i [Trafikkällor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=sv-SE).

* Från Baidu
* Från Bing
* Från Google
* Från Yahoo
* Refererande landningssida: URL
* Referenslandningssida: Domän
* Referenslandningssida: Fråga

### Tidsram

Mer information finns i [Tidsram](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=sv-SE).

* Startdatum/slutdatum

## Klienttips

[!DNL Adobe Target] kräver klienttips för korrekt segmentering av attribut för webbläsare, operativsystem och mobila målgrupper samt vissa instanser av profilskript. Mer bakgrundsinformation finns i [Användaragent och Klienttips](../../../client-side/atjs/user-agent-and-client-hints.md).

### Skicka klienttips till [!DNL Adobe Target]

Från och med Node.js SDK v2.4.0 och Java SDK v2.3.0 kan klienttips skickas till [!DNL Target] via `getOffers()` anrop. Klienttips bör inkluderas på objektet `request.context`, tillsammans med användaragenten.

>[!BEGINTABS]

>[!TAB Node.js SDK]

```js {line-numbers="true"}
targetClient.getOffers({ 
    request: { 
        context: { 
            channel: "mobile" 
            userAgent: "Mozilla/5.0 (Linux; Android 12; Pixel 4a) AppleWebKit/537.36 (KHTML, like Gecko) Mobile Safari/537.36", 
            clientHints: { 
                mobile: "true", 
                platform: "Linux", 
                platformVersion: "12.1", 
                model: "Pixel 4a", 
                browserUAWithMajorVersion: "\"Not A;Brand\";v=\"98\", \"Chromium\";v=\"98\", \"Google Chrome\";v=\"98\"", 
                browserUAWithFullVersion: "\" Not A;Brand\";v=\"98.0.0.0\", \"Chromium\";v=\"98.0.4844.83\", \"Google Chrome\";v=\"98.0.4758.101\"", 
                bitness: "64", 
                architecture: "x86" 
            } 
        }, 
        execute: { 
            mboxes: [{ 
                name: "home", 
                index: 1 
            }] 
        } 
    } 
});
```

>[!TAB Java SDK]

```javascript {line-numbers="true"}
import com.adobe.target.delivery.v1.model.ClientHints; 
import com.adobe.target.delivery.v1.model.Context; 
import com.adobe.target.delivery.v1.model.ExecuteRequest; 
import com.adobe.target.edge.client.model.TargetDeliveryRequest; 

 
ClientHints clientHints = new ClientHints(); 
clientHints.setMobile(true); 
clientHints.setPlatform("macOS"); 
clientHints.setArchitecture("x86"); 
clientHints.setPlatformVersion("11.3.1"); 
clientHints.setBrowserUAWithMajorVersion( 
  "\" Not A;Brand\";v=\"99\", \"Chromium\";v=\"99\", \"Google Chrome\";v=\"99\""); 
String userAgent = 
  "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Safari/537.36"; 

 
TargetDeliveryRequest request = TargetDeliveryRequest.builder() 
        .execute(new ExecuteRequest().pageLoad(pageLoad)) 
        .context(new Context().clientHints(clientHints).userAgent(userAgent)) 
        .build(); 
```

>[!ENDTABS]

## Enhetsbeslut

Tabellen nedan visar vilka målgruppsregler som stöds eller inte stöds för enhetsbeslut.

| Målgruppsregel | Beslut på enheten |
| --- | --- |
| [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=sv-SE) | Ja |
| [Nätverk](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=sv-SE) | Nej |
| [Mobil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=sv-SE) | Nej |
| [Egna parametrar](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=sv-SE) | Ja |
| [Operativsystem](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=sv-SE) | Ja |
| [Webbplatssidor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=sv-SE) | Ja |
| [Webbläsare](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=sv-SE) | Ja |
| [Besökarprofil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=sv-SE) | Nej |
| [Trafikkällor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=sv-SE) | Nej |
| [Tidsram](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=sv-SE) | Ja |
| [Experience Cloud-målgrupper](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=sv-SE) (målgrupper från Adobe Audience Manager, Adobe Analytics och Adobe Experience Manager) | Nej |

### Målinriktning för geolokalisering för beslut på enheter

Adobe rekommenderar att du anger geovärdena själv i anropet till `getOffers` för att behålla nästan noll fördröjning för enhetsspecifika beslutsaktiviteter med geobaserade målgrupper. Gör detta genom att ställa in objektet `Geo` i `Context` för begäran. Det innebär att servern behöver ett sätt att avgöra var varje slutanvändare befinner sig. Servern kan till exempel utföra en IP-till-Geo-sökning med en tjänst som du konfigurerar. Vissa värdtjänstleverantörer, som Google Cloud, tillhandahåller den här funktionen via anpassade rubriker i varje `HttpServletRequest`.

>[!BEGINTABS]

>[!TAB Node.js SDK]

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
    request: {
        context: {
            geo: {
                city: "SAN FRANCISCO",
                countryCode: "US",
                stateCode: "CA",
                latitude: 37.75,
                longitude: -122.4
            }
        },
        execute: {
            pageLoad: {}
        }
    }
})
```

>[!TAB Java SDK]

```javascript {line-numbers="true"}
public class TargetRequestUtils {

    public static Context getContext(HttpServletRequest request) {
        Context context = new Context()
            .geo(ipToGeoLookup(request.getRemoteAddr()))
            .channel(ChannelType.WEB)
            .timeOffsetInMinutes(330.0)
            .address(getAddress(request));
        return context;
    }

    public static Geo ipToGeoLookup(String ip) {
        GeoResult geoResult = geoLookupService.lookup(ip);
        return new Geo()
            .city(geoResult.getCity())
            .stateCode(geoResult.getStateCode())
            .countryCode(geoResult.getCountryCode());
    }
}
```

>[!ENDTABS]

Men om du inte kan utföra IP-till-Geo-sökningar på servern, men ändå vill utföra enhetsbeslut för `getOffers`-begäranden som innehåller geobaserade målgrupper, stöds även detta. Nackdelen med den här metoden är att den kommer att använda en fjärr-IP-till-Geo-sökning, som lägger till fördröjning till varje `getOffers`-samtal. Den här fördröjningen bör vara lägre än ett `getOffers`-fjärranrop, eftersom den träffar ett CDN som ligger nära servern. Du får **endast** tillhandahålla fältet `ipAddress` i objektet `Geo` i `Context` i din begäran för att SDK ska kunna hämta den geografiska platsen för användarens IP-adress. Om något annat fält förutom `ipAddress` anges kommer [!DNL Target] SDK inte att hämta metadata för geopositionering för upplösning.

>[!BEGINTABS]

>[!TAB Node.js SDK]

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
    request: {
        context: {
            geo: {
                ipAddress: "127.0.0.1"
            }
        },
        execute: {
            pageLoad: {}
        }
    }
})
```

>[!TAB Java SDK]

```javascript {line-numbers="true"}
public class TargetRequestUtils {

    public static Context getContext(HttpServletRequest request) {
        Context context = new Context()
            .geo(new Geo().ipAddress(request.getRemoteAddr()))
            .channel(ChannelType.WEB)
            .timeOffsetInMinutes(330.0)
            .address(getAddress(request));
        return context;
    }

}
```

>[!ENDTABS]

## Beslut på serversidan

Tabellen nedan visar vilka målgruppsregler som stöds eller inte stöds för beslut på serversidan.

| Målgruppsregel | Beslut på serversidan |
| --- | --- |
| [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=sv-SE) | Ja |
| [Nätverk](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=sv-SE) | Ja |
| [Mobil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=sv-SE) | Ja |
| [Egna parametrar](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=sv-SE) | Ja |
| [Operativsystem](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=sv-SE) | Ja |
| [Webbplatssidor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=sv-SE) | Ja |
| [Webbläsare](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=sv-SE) | Ja |
| [Besökarprofil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=sv-SE) | Ja |
| [Trafikkällor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=sv-SE) | Ja |
| [Tidsram](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=sv-SE) | Ja |
| [Experience Cloud-målgrupper](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=sv-SE) (målgrupper från Adobe Audience Manager, Adobe Analytics och Adobe Experience Manager) | Ja |
