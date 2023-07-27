---
title: Använd getOffers() i [!DNL Adobe Target] när .NET SDK används
description: Lär dig hur du använder getOffers() för att köra ett beslut och hämta en upplevelse från [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 4d1d1cbd-c7e5-4146-9fea-08e01923874d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 0%

---

# Erbjudanden (.NET)

## Beskrivning

`GetOffers()` används för att verkställa ett beslut och hämta en upplevelse från [!DNL Adobe Target].

## Metod

`TargetClient.GetOffers` metodsignatur.

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.GetOffers(TargetDeliveryRequest request)
```

`TargetDeliveryRequest` skapas med `TargetDeliveryRequest.Builder`.

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryRequest.Builder TargetDeliveryRequest.Builder()
```

## Parametrar

The `TargetDeliveryRequest.Builder` objektet har följande struktur:

| Namn | Typ | Obligatoriskt | Beskrivning |
| --- | --- | --- | --- |
| kontext | Kontext | Ja | Anger kontexten för begäran |
| sessionId | Sträng | Nej | Används för att länka flera [!DNL Target] förfrågningar |
| thirdPartyId | Sträng | Nej | Ditt företags-ID för användaren som du kan skicka med varje samtal |
| cookies | Lista | Nej | Lista över cookies som returnerats i tidigare [!DNL Target] begäran från samma användare. |
| customerIds | Karta | Nej | Kund-ID i VisitorId-kompatibelt format |
| execute | ExecuteRequest | Nej | PageLoad eller mboxes begär att köras. Utvärderingen av servern kommer att göras omedelbart |
| förhämtning | PrefetchRequest | Nej | Vyer, PageLoad eller mboxes begär att få förhämtning. Returnerar med en meddelandetoken som ska returneras vid konvertering. |
| meddelanden | Lista | Nej | Används för att skicka meddelanden om vilket förhämtat innehåll som visades |
| requestId | Sträng | Nej | Begärande-ID som returneras i svaret. Genereras automatiskt om den inte finns. |
| scriptId | Sträng | Nej | Om det finns en andra och efterföljande begäran med samma ID ökar inte intrycket av aktiviteter/mått. Genereras automatiskt om den inte finns. |
| environmentId | Lång | Nej | Giltigt ID för klientmiljö. Om ingen angiven värd anges bestäms den utifrån den angivna värden. |
| property | Egenskap | Nej | Anger egenskapen at_property via tokenfältet. Den kan användas för att styra leveransomfånget. |
| trace | Kalkera | Nej | Aktiverar spårning för leverans-API. |
| qaMode | QAMode | Nej | Använd det här objektet för att aktivera QA-läget i begäran. |
| locationHint | Sträng | Nej | [!DNL Target] tips för edge-klusterplats. Används för att ange angivet edge-kluster som mål för denna begäran. |
| besökare | Besökare | Nej | Används för att tillhandahålla anpassade Visitor API-objekt. |
| id | VisitorId | Nej | Objekt som innehåller identifierarna för besökaren. T.ex. tntId, thirdParyId, mcId, customerIds. |
| experienceCloud | ExperienceCloud | Nej | Anger integreringar med Audience Manager och Analytics. Fylls i automatiskt med hjälp av cookies, om detta inte anges. |
| tntId | Sträng | Nej | Primär identifierare i [!DNL Target] för en användare. Hämtat från targetCookies. Automatiskt genererad om inte. |
| mcId | Sträng | Nej | Används för att sammanfoga och dela data mellan olika Adobe-lösningar (ECID). Hämtat från targetCookies. Automatiskt genererad om inte. |
| trackingServer | Sträng | Nej | Adobe Analytics Server för att [!DNL Adobe Target] och [!DNL Adobe Analytics] för att sammanfoga data på ett korrekt sätt. |
| trackingServerSecure | Sträng | Nej | The [!UICONTROL Adobe Analytics Secure Server] för att [!DNL Adobe Target] och [!DNL Adobe Analytics] för att sammanfoga data på ett korrekt sätt. |
| decisioningMethod | DecisioningMethod | Nej | Kan användas för att explicit ange ON_DEVICE eller HYBRID-beslutsmetod för enhetsbeslut |

Värdena för varje fält bör överensstämma med [Målleverans-API](/help/dev/implement/delivery-api/overview.md) begärandespecifikation.

## Svar

The `TargetDeliveryResponse` returneras av `TargetClient.GetOffers()` har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| Begäran | TargetDeliveryRequest &#x200B; | [Målleverans-API](/help/dev/implement/delivery-api/overview.md) förfrågan |
| Svar | DeliveryResponse &#x200B; | [Målleverans-API](/help/dev/implement/delivery-api/overview.md)* svar |
| Status | HttpStatusCode | HTTP-statuskod för svar |
| Meddelande | string | Svarsstatusmeddelande eller felmeddelande |
| Platser | Platser | [!DNL Target] platsnamn, inklusive globala mbox-namn och mbox/views (mbox-namn)/views (mbox-namn) där endast fjärrbeslut är tillgängligt |
| GetCookies | Ordlista | Returnerar en ordlista med sessionsmetadata för den här användaren. Detta måste skickas in nästa gång [!DNL Target] begäran för den här användaren. |
| VisitorState | IDictionary | Besökartillstånd som ska anges på klientsidan för initiering av JavaScript-bibliotek i Visitor API |

The `TargetCookie` objekt som används för att spara data för användarsession har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| Namn | string | Kaknamn |
| Värde | string | Cookie-värde |
| MaxAge | int | The `MaxAge` är ett bekvämt alternativ för att ange förfallodatum i förhållande till den aktuella tiden i sekunder |

Du behöver inte oroa dig för att cookies ska upphöra. [!DNL Target] handtag `MaxAge` i SDK.

## Exempel

## \.NET

```dotnet {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

var targetClient = TargetClient.Create(targetClientConfig);

var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .SetExecute(new ExecuteRequest(mboxes: mboxRequests))
    .Build();

var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
```
