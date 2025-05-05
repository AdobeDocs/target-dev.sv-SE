---
title: Vilka funktioner stöds i enhetsbeslut?
description: Lär dig hur du levererar det mest relevanta och engagerande personaliserade innehållet via maskininlärning via ett live-serversamtal.
feature: APIs/SDKs
exl-id: 15d9870f-6c58-4da0-bfe5-ef23daf7d273
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 0%

---

# Översikt över funktioner som stöds

Serversidesbaserade SDK:er för [!DNL Adobe Target] ger utvecklare flexibilitet att välja mellan prestanda och aktualitet för data för beslut. Med andra ord, om det viktigaste är att leverera det mest relevanta och engagerande personaliserade innehållet via maskininlärning, bör du ringa ett live-serversamtal. Men när prestanda är viktigare bör man fatta ett beslut på enheten. För att [!UICONTROL on-device decisioning] ska fungera, se följande lista över funktioner som stöds:

* Typ av aktivitet
* Målgruppsanpassning
* Allokeringsmetod

## Typ av aktivitet

Tabellen nedan visar vilka [aktivitetstyper](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=sv-SE) som skapats med [formulärbaserade Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=sv-SE&) som stöds eller inte stöds för [!UICONTROL on-device decisioning].

| Typ av aktivitet | Stöds |
| --- | --- |
| [A/B-test](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=sv-SE) | Ja |
| [Automatisk allokering](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=sv-SE) | Nej |
| [Automatiskt mål](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=sv-SE) | Nej |
| [Analyser för mål](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=sv-SE) (A4T) | Ja |
| [Multivariata tester](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html?lang=sv-SE) (MVT) | Nej |
| [Experience Targeting](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=sv-SE) (XT) | Ja |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=sv-SE) (AP) | Nej |
| [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=sv-SE) | Nej |


## Målgruppsanpassning

Tabellen nedan visar vilka målgruppsregler som stöds eller inte stöds för [!UICONTROL on-device decisioning].

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

### Geografisk målinriktning för [!UICONTROL on-device decisioning]

Adobe rekommenderar att du anger geovärdena själv i anropet till `getOffers` för att behålla nästan noll-fördröjning för [!UICONTROL on-device decisioning]-aktiviteter med geobaserade målgrupper. Gör detta genom att ställa in objektet `Geo` i `Context` för begäran. Det innebär att servern behöver ett sätt att avgöra var varje slutanvändare befinner sig. Servern kan till exempel utföra en IP-till-Geo-sökning med en tjänst som du konfigurerar. Vissa värdtjänstleverantörer, som Google Cloud, tillhandahåller den här funktionen via anpassade rubriker i varje `HttpServletRequest`.

>[!BEGINTABS]

>[!TAB Node.js]

```csharp {line-numbers="true"}
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

>[!TAB Java]

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

Om du inte kan utföra IP-till-Geo-sökningar på servern, men ändå vill utföra [!UICONTROL on-device decisioning] för `getOffers`-begäranden som innehåller geobaserade målgrupper, stöds detta också. Nackdelen med den här metoden är att den kommer att använda en fjärr-IP-till-Geo-sökning, som lägger till fördröjning till varje `getOffers`-samtal. Den här fördröjningen bör vara lägre än ett `getOffers`-fjärranrop, eftersom den träffar ett CDN som ligger nära servern. Du får bara ange fältet `ipAddress` i objektet `Geo` i `Context` i din begäran för att SDK ska kunna hämta den geografiska platsen för användarens IP-adress. Om något annat fält förutom `ipAddress` anges kommer [!DNL Target] SDK inte att hämta metadata för geopositionering för upplösning.


>[!BEGINTABS]

>[!TAB Node.js]

```csharp {line-numbers="true"}
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

>[!TAB Java]

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

## Allokeringsmetod

Följande tabell visar vilka allokeringsmetoder som stöds eller inte stöds för [!UICONTROL on-device decisioning].

| Allokeringsmetod | Stöds |
| --- | --- |
| Manuell | Ja |
| [Autoallokera till bästa upplevelse](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=sv-SE) | Nej |
| [Automatisk målgruppsanpassning för personaliserade upplevelser](https://experienceleague.adobe.com/docs/target/using/activities/auto-target-to-optimize.html?lang=sv-SE) | Nej |
