---
title: Hämta profiler
description: Lär dig hur du använder Adobe Target Profile API:er för att hämta besöksdata som ska användas i  [!DNL Target].
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: b422ae68-49b3-4d60-9ea4-0fa67b6934b0
source-git-commit: b8ccfdcaff6aa17a325727df0a9ffd716e44519b
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 0%

---

# Hämta profiler

En [!DNL Target]-profil kan hämtas på tre sätt: med en `[!DNL Experience Cloud Visitor ID]` (`ECID`), `tntid` eller `thirdPartyId`.

## Använda en [!DNL Experience Cloud Visitor ID] (ECID)

Du kan hämta en profil baserat på `ECID`. HTTP-metoden måste vara GET.

URL:en ser ut som i följande exempel:

```
https://<clientCode>.tt.omtrdc.net/rest/v1/profiles/marketingCloudVisitorId/<ECID>?client=<clientCode>
```

Ersätt `<clientCode>` med [!DNL Target] [!UICONTROL Client Code] och `<ECID>` med [!DNL Experience Cloud Visitor ID] ([!DNL Marketing Cloud Visitor ID]).

## Använda en ttid

[!DNL Target] tilldelar automatiskt en `tntid` för varje begäran.

I följande exempel visas begärandeformatet för att hämta en profil med hjälp av en `tntid`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/your-tnt-id?client=<your-client-code>
```

Ersätt `<your-client-code>` och `your-tnt-id` och starta en GET-förfrågan. Här är ett exempel på ett profilhämtningsanrop med en `tntid`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/111492025094307-353046?client=<your-client-code>
```

## Använda ett thirdPartyId

[!DNL Adobe Target] profiler kan utökas med din egen identifierare (till exempel CRM-ID, `uuid`, medlemsnummer och så vidare).

Se [Uppdatera profiler](/help/dev/administer/profile-api/profile-api-overview.md) om du vill veta hur du kan koppla en `thirdPartyId` till din profil.

I följande exempel visas begärandeformatet för att hämta en profil med hjälp av en `thirdPartyId`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/your-thirdpartyid?client=<your-client-code>
```

Ersätt `<your-client-code>` och `your-thirdpartyid` och starta en GET-förfrågan. Här är ett exempel på ett profilhämtningsanrop med en [!UICONTROL thirdpartyid]:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/a1-mbox3rdPartyId?client=<your-client-code>
```

När det här anropet görs försöker [!DNL Target] att hitta profilen först i klustret som anges i edge-begäran, eller var profilen finns, och returnera innehållet. Profilinnehållet returneras i JSON-format.

## Autentisering

[!DNL Target Profile API] kan skyddas genom att autentisering aktiveras från användargränssnittet i [!DNL Target] enligt beskrivningen här. När autentiseringen har aktiverats måste alla profil-API-begäranden ha profilautentiseringstoken angiven i begärandehuvudena. Själva token kan genereras med användargränssnittet för [!DNL Target] eller med de steg som beskrivs ovan i avsnittet [Profilautentiseringstoken](https://developers.adobetarget.com/api/#authentication-tokens){target=_blank}.

## Mätning

Dessa samtal räknas inte med i dina mbox-samtal.

## Felhantering

Om det finns ett anrop till `/thirdPartyId` med en ogiltig eller utgången `thirdPartyId` angiven:

```
{"status" : 404, "message" : "No profile found for client <client_code> with third party id=<third_party_id>"}
```

Om profilen inte kan hittas eller har gått ut:

```
{"status" : 404, "message" : "No profile found for client <client_code> with mboxPC=<mbox_pc>"}
```
