---
title: Använd getOffers() i [!DNL Adobe Target] när du använder Java SDK
description: Lär dig hur du använder getOffers() för att köra ett beslut och hämta en upplevelse från [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 9d7bf956-9d6a-4b4f-a401-2e6814f17f3d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 0%

---

# Erbjudanden (Java)

## Beskrivning

`getOffers()` används för att verkställa ett beslut och hämta en upplevelse från [!DNL Adobe Target].

## Metod

### getOffers

The `TargetClient.getOffers` metodsignaturen visas så här.

**Begäran**

```javascript {line-numbers="true"}
TargetDeliveryResponse TargetClient.getOffers(TargetDeliveryRequest request)
```

TargetDeliveryRequest skapas med `TargetDeliveryRequest.builder`.

**Svar**

```javascript {line-numbers="true"}
TargetDeliveryRequestBuilder TargetDeliveryRequest.builder()
```

## Parametrar

The `[!UICONTROL TargetDeliveryRequestBuilder]` objektet har följande struktur:

| Namn | Typ | Obligatoriskt | Beskrivning |
| --- | --- | --- | --- |
| kontext | Kontext | Ja | Anger kontexten för begäran |
| sessionId |  | Sträng | Nej | Används för att länka flera [!DNL Target] förfrågningar |
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
| mcId | Sträng | Nej | Används för att sammanfoga och dela data mellan olika [!DNL Adobe] lösningar (ECID). Hämtat från targetCookies. Automatiskt genererad om inte. |
| trackingServer | Sträng | Nej | Adobe Analytics Server för att [!DNL Adobe Target] och [!DNL Adobe Analytics] för att sammanfoga data på ett korrekt sätt. |
| trackingServerSecure | Sträng | Nej | The [!UICONTROL Adobe Analytics Secure Server] för att [!DNL Adobe Target] och [!DNL Adobe Analytics] för att sammanfoga data på ett korrekt sätt. |
| decisioningMethod | DecisioningMethod | Nej | Kan användas för att explicit ange ON_DEVICE eller HYBRID-beslutsmetod för enhetsbeslut |

Värdena för varje fält bör överensstämma med *[!UICONTROL Target View Delivery API]* begärandespecifikation. Läs mer om *[!UICONTROL Target View Delivery API]*, se [http://developers.adobetarget.com/api/#view-delivery-overview](http://developers.adobetarget.com/api/#view-delivery-overview)


## Svar

The `TargetDeliveryResponse` returneras av `TargetClient.getOffers(`) har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| förfrågan | TargetDeliveryRequest &#x200B; | *[!DNL Target]* förfrågan |
| svar | Leveranssvar | *[!DNL Target]* svar |
| cookies | Lista | Lista över sessionsmetadata för den här användaren. Måste skickas i nästa målbegäran för den här användaren. |
| visitorState | Karta | Besökartillstånd som ska anges på klientsidan och användas av Visitor API |
| responseStatus | Status för svar | Ett objekt som representerar svarsstatus |

The `ResponseStatus` i svaret innehåller följande fält:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| status | int | HTTP-status returnerades från [!DNL Target] |
| message | Sträng | Statusmeddelande om HTTP-statusen inte är 200 |
| remoteMboxes | Lista över strängar | Används för enhetsbeslut. Innehåller en lista med kryssrutor som har fjärraktiviteter som inte kan bestämmas helt på enheten. |
| remoteViews | Lista över strängar | Används för enhetsbeslut. Innehåller en lista med vyer som har fjärraktiviteter som inte kan bestämmas helt på enheten. |

The `TargetCookie` objekt som används för att spara data för användarsession har följande struktur:

| Namn | Typ | Beskrivning |
| --- | --- | --- |
| name | Sträng | Kaknamn |
| value | Sträng | Cookie-värde, värdet konverteras till sträng |
| maxAge | Nummer | Alternativet maxAge är ett bekvämt sätt att ange förfallodatum i förhållande till den aktuella tiden i sekunder |

Du behöver inte oroa dig för att cookies ska upphöra. Målet hanterar maxAge inuti SDK.

## Exempel

**Begäran**

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);

List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("a1-serverside-ab").index(1));

TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
        .context(new Context().channel(ChannelType.WEB))
        .execute(new ExecuteRequest().setMboxes(mboxRequests))
        .build();
```

**Svar**

```javascript {line-numbers="true"}
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```
