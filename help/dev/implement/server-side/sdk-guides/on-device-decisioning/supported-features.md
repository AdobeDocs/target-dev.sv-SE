---
title: Vilka funktioner stöds i enhetsbeslut?
description: Lär dig hur du levererar det mest relevanta och engagerande personaliserade innehållet via maskininlärning via ett live-serversamtal.
feature: APIs/SDKs
exl-id: 15d9870f-6c58-4da0-bfe5-ef23daf7d273
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 0%

---

# Översikt över funktioner som stöds

[!DNL Adobe Target]Med SDK:er på serversidan kan utvecklare välja mellan prestanda och aktualitet för data vid beslut. Med andra ord, om det viktigaste är att leverera det mest relevanta och engagerande personaliserade innehållet via maskininlärning, bör du ringa ett live-serversamtal. Men när prestanda är viktigare bör man fatta ett beslut på enheten. För [!UICONTROL on-device decisioning] om du vill arbeta kan du läsa följande lista över funktioner som stöds:

* Typ av aktivitet
* Målgruppsanpassning
* Allokeringsmetod

## Typ av aktivitet

I följande tabell visas vilka [aktivitetstyper](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) som skapats med [Formulärbaserad Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?) stöds eller stöds inte för [!UICONTROL on-device decisioning].

| Typ av aktivitet | Stöds |
| --- | --- |
| [A/B-test](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html) | Ja |
| [Automatisk allokering](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | Nej |
| [Automatiskt mål](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) | Nej |
| [Analyser för Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) (A4T) | Ja |
| [Multivariata tester](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html) (MVT) | Nej |
| [Experience Targeting](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html) (XT) | Ja |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) (AP) | Nej |
| [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html) | Nej |


## Målgruppsanpassning

Tabellen nedan visar vilka målgruppsregler som stöds eller inte stöds för [!UICONTROL on-device decisioning].

| Målgruppsregel | Beslut på enheten |
| --- | --- |
| [Geo](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | Ja |
| [Nätverk](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | Nej |
| [Mobil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | Nej |
| [Egna parametrar](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | Ja |
| [Operativsystem](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | Ja |
| [Webbplatssidor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | Ja |
| [Webbläsare](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | Ja |
| [Besökarprofil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | Nej |
| [Trafikkällor](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | Nej |
| [Tidsram](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | Ja |
| [Experience Cloud målgrupper](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) (Målgrupper från Adobe Audience Manager, Adobe Analytics och Adobe Experience Manager | Nej |

### Geografisk anpassning för [!UICONTROL on-device decisioning]

För att behålla latenstid nära noll för [!UICONTROL on-device decisioning] Adobe rekommenderar att du själv anger geovärdena i anropet till `getOffers`. Gör detta genom att ställa in `Geo` -objektet i `Context` om begäran. Det innebär att servern behöver ett sätt att avgöra var varje slutanvändare befinner sig. Servern kan till exempel utföra en IP-till-Geo-sökning med en tjänst som du konfigurerar. Vissa värdtjänstleverantörer, som Google Cloud, tillhandahåller den här funktionen via anpassade rubriker i varje `HttpServletRequest`.

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

Om du inte kan utföra IP-till-Geo-sökningar på servern, men ändå vill utföra sökningen [!UICONTROL on-device decisioning] for `getOffers` -förfrågningar som innehåller geobaserade målgrupper stöds också. Nackdelen med detta är att det kommer att använda en fjärransluten IP-till-Geo-sökning som lägger till fördröjning för varje `getOffers` ring. Den här fördröjningen bör vara lägre än en fjärranslutning `getOffers` eftersom det träffar ett CDN som ligger nära servern. Du får bara ange `ipAddress` fältet i `Geo` -objektet i `Context` för att SDK ska kunna hämta den geografiska platsen för användarens IP-adress. Om något annat fält förutom `ipAddress` tillhandahålls, [!DNL Target] SDK hämtar inte metadata för geopositionering för upplösning.


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
| [Autoallokera till bästa upplevelse](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | Nej |
| [Automatisk målgruppsanpassning för personaliserade upplevelser](https://experienceleague.adobe.com/docs/target/using/activities/auto-target-to-optimize.html) | Nej |
