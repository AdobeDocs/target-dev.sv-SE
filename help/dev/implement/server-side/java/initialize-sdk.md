---
title: Initiera Java SDK med metoden create
description: Lär dig hur du använder metoden create för att initiera Java SDK och instansiera [!UICONTROL TargetClient] för att ringa [!DNL Adobe Target] för experiment och personaliserade upplevelser.
feature: APIs/SDKs
exl-id: 0e0ddead-7de8-4549-b81c-e72598558e4b
source-git-commit: 1d080b5e402e5d55039bf06611b44678cc6c36de
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 1%

---

# Initiera Java SDK

## Beskrivning

Använd `create` för att initiera Java SDK och instansiera [!UICONTROL Target Client] för att ringa [!DNL Adobe Target] för experiment och personaliserade upplevelser.

## Metod

[!UICONTROL TargetClient] skapas med `TargetClient.create`.

### skapa

```javascript {line-numbers="true"}
TargetClient TargetClient.create(ClientConfig clientConfig)
```

ClientConfig skapas med `ClientConfig.builder`.

```javascript {line-numbers="true"}
ClientConfigBuilder ClientConfig.builder()
```

## Parametrar

`ClientConfigBuilder` har följande struktur:

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- | --- | --- | --- | --- |
| klient | Sträng | Ja | Ingen | [!UICONTROL Target Client Id] |
| organizationId | Sträng | Ja | Ingen | [!UICONTROL Experience Cloud Organization ID] |
| connectTimeout | Nummer | Nej | 10000 | Anslutningens timeout för alla begäranden i millisekunder |
| socketTimeout | Nummer | Nej | 10000 | Sockettimeout för alla begäranden i millisekunder |
| maxConnectionsPerHost | Nummer | Nej | 100 | Max antal anslutningar per [!DNL Target] värd |
| maxConnectionsTotal | Nummer | Nej | 200 | Max antal anslutningar inklusive alla [!DNL Target] värdar |
| connectionTtlms | Nummer | Nej | -1 | TTL (Total time to live) definierar maximal livstid för beständiga anslutningar i millisekunder. Som standard hålls anslutningar levande i oändlighet |
| idleConnectionValidationms | Nummer | Nej | 1000 | Inaktivitetsperiod i millisekunder efter vilken beständiga anslutningar omvalideras innan de återanvänds |
| evictIdleConnectionsAfterSecs | Nummer | Nej | 20 | Tiden i sekunder för att ta bort inaktiva anslutningar från anslutningspoolen |
| enableRetries | Boolean | Nej | true | Automatiska försök för sockettimeout (max 4) |
| logRequests | Boolean | Nej | false | Logg [!DNL Target] förfrågningar och svar i felsökning |
| logRequestStatus | Boolean | Nej | false | Logg [!DNL Target] svarstid, status och URL |
| serverDomain | Sträng | Nej | `*client*.tt.omtrdc.net` | Åsidosätter standardvärdnamn |
| säker | Boolean | Nej | true | Avmarkerad för att tillämpa HTTP-schema |
| requestInterceptor | HttpRequestInterceptor | Nej | Null | Lägg till anpassad begärandespärr |
| defaultPropertyToken | Sträng | Nej | Ingen | Anger standardegenskapstoken för varje `getOffers` ring. **För beslut på enheten** hämtar SDK bara artefakten som innehåller de kvalificerade aktiviteterna för egenskapstoken som angetts i `defaultPropertyToken` |
| defaultDecisioningMethod | DecisioningMethod enum | Nej | SERVER_SIDE | Måste anges till ON_DEVICE eller HYBRID för att enhetsbeslut ska kunna aktiveras |
| telemetryEnabled | Boolean | Nej | true | Gör det möjligt för kunder att välja bort ytterligare datainsamling vid begäran om [!DNL Target] servrar |
| proxyConfig | ClientProxyConfig | Nej | Ingen | Klienten kan ange sin egen proxyinformation |
| exceptionHandler | TargetExceptionHandler | Nej | Ingen | Kan användas för att implementera anpassad undantagshantering under regelbearbetning |
| httpClient | HttpClient | Nej | Ingen | Tillåter användare att ersätta [!DNL Target] HTTP-klient med en anpassad HTTP-klient |
| onDeviceEnvironment | Sträng | Nej | produktion | Kan användas för att ange en annan enhetsmiljö, till exempel mellanlagring |
| onDeviceConfigHostname | Sträng | Nej | `assets.adobetarget.com` | Kan användas för att ange en annan värd som ska användas för att ladda ned filen med beslutsartefakt på enheten |
| onDeviceDecisioningPollingIntSecs | int | Nej | 300 (5 minuter) | Antal sekunder mellan hämtningarna av enhetsspecifik beslutsartefaktfil |
| onDeviceArtifactPayload | byte[] | Nej | Ingen | Tillhandahåller enhetsbeslut med föregående artefaktnyttolast för att möjliggöra omedelbar körning |
| onDeviceDecisionHandler | OnDeviceDecisionHandler | Nej | Ingen | Registrerar återanrop för enhets-ID-beslutshändelser |
| onDeviceAllMatchingRulesMboxes | Lista\&lt;string> | Nej | Ingen | Tillåter användare att ange kryssrutor för vilka allt matchande regelinnehåll returneras vid enhetsbeslut |

## Exempel

### Java

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient.create(clientConfig);

// make calls to Adobe Target
```
