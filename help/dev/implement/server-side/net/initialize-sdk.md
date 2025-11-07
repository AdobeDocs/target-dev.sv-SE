---
title: Initiera .NET SDK med metoden create
description: Lär dig hur du använder metoden create för att initiera Java SDK och instansiera [!UICONTROL TargetClient] för att ringa till  [!DNL Adobe Target] för experiment och personaliserade upplevelser.
feature: APIs/SDKs
exl-id: 501010c3-22f4-49a8-b2ac-c7307232d180
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# Initiera .NET SDK

## Beskrivning

Använd metoden `Create` för att initiera .NET SDK och instansiera [!UICONTROL Target Client] för att anropa [!DNL Adobe Target] för experiment och personaliserade upplevelser.

När du använder .NET Dependency Injection lägger du bara till SDK i tjänstkonfigurationssteget genom att anropa `services.AddTargetLibrary()`, och sedan matar du in `ITargetClient targetClient` i appens konstruktor.

Därefter använder du metoden `Initialize` i SDK för att konfigurera SDK och därmed slutföra initieringssteget.

## Metod

`TargetClient` skapas med `TargetClient.Create`.

## C\#

```csharp {line-numbers="true"}
TargetClient TargetClient.Create(TargetClientConfig clientConfig)
```

`ClientConfig` skapas med ClientConfig.Builder.

## C\#

```csharp {line-numbers="true"}
TargetClientConfig.Builder TargetClientConfig.Builder()
```

## Parametrar

`TargetClientConfig.Builder` har följande struktur:

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- | --- | --- | --- | --- |
| Klient | string | Ja | Ingen | [!UICONTROL Target Client Id] |
| OrganizationId | string | Ja | Ingen | [!UICONTROL Experience Cloud Organization ID] |
| Timeout | int | Nej | 10000 | Timeout för alla begäranden i millisekunder |
| Proxy | WebProxy | Nej | null | Proxy för alla [!DNL Target]-begäranden |
| RetryPolicy | Policy | Nej | null | Prova principen igen för alla [!DNL Target]-begäranden |
| AsyncRetryPolicy | AsyncPolicy | Nej | null | Asynkron återförsöksprincip för alla [!DNL Target]-begäranden |
| Logger | ILogger | Nej | null | Används för felsökningsloggning av [!DNL Target] begäranden och svar |
| ServerDomain | string | Nej | `client.tt.omtrdc.net` | Åsidosätter standardvärdnamn |
| Säker | bool | Nej | true | Avmarkerad för att tillämpa HTTP-schema |
| DefaultPropertyToken | string | Nej | null | Anger standardegenskapstoken för varje `getOffers`-anrop |
| TelemetryEnabled | bool | Nej | true | Skicka telemetridata för att förbättra SDK användarupplevelse |
| DecisioningMethod | DecisioningMethod enum | Nej | ServerSide | Måste anges till OnDevice eller Hybrid för att enhetsbeslut ska kunna aktiveras |
| OnDeviceDecisioningReady | Åtgärd | Nej | null | Delegera för Ready-händelse för enhetsbeslut (anropas en gång när enhetsbeslut är klart) |
| ArtifactDownloadSucceeded | Åtgärd | Nej | null | Delegera för slutförd hämtning av artikelfelaktighet på enheter (anropas för varje slutförd artefakt-hämtning) |
| ArtifactDownloadFailed | Åtgärd | Nej | null | Delegera för fel vid hämtning av artefakt på enheter (anropas för varje misslyckad artefakt-hämtning) |
| OnDeviceEnvironment | string | Nej | produktion | Kan användas för att ange en annan enhetsmiljö, t.ex. mellanlagring |
| OnDeviceConfigHostname | string | Nej | `assets.adobetarget.com` | Kan användas för att ange en annan värd som ska användas för att ladda ned filen med beslutsartefakt på enheten |
| OnDeviceDecisioningPollingIntSecs | int | Nej | 300 (5 min) | Antal sekunder mellan hämtningarna av enhetsspecifik beslutsartefaktfil |
| OnDeviceArtifactPayload | string | Nej | null | Tillhandahåller enhetsspecifik beslutsfattande med en lokal artefaktnyttolast som tillåter omedelbar körning |

## Exempel

## C\#

```csharp {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeclient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

targetClient = TargetClient.Create(targetClientConfig);

// make calls to Adobe Target
```
