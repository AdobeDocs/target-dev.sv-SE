---
title: Hämta profiler
description: Lär dig hur du använder Adobe Target Profile API:er för att hämta besöksdata som ska användas i [!DNL Target].
contributors: https://github.com/icaraps
feature: APIs/SDKs
source-git-commit: 49acf92bbe06dbcee36fef2b7394acd7ce37baad
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 0%

---

# Uppdatera profiler

A [!DNL Target] kan hämtas på tre sätt: med en `[!DNL Experience Cloud Visitor ID]` (`ECID`), `tntid` eller `thirdPartyId`.

## Använda [!DNL Experience Cloud Visitor ID] (ECID)

Du kan hämta en profil baserat på `ECID`. HTTP-metoden måste vara GET.

URL:en ser ut som i följande exempel:

```
https://<clientCode>.tt.omtrdc.net/rest/v1/profiles/marketingCloudVisitorId/<ECID>?client=<clientCode>
```

Ersätt `<clientCode>` med [!DNL Target] [!UICONTROL Client Code] och `<ECID>` med [!DNL Experience Cloud Visitor ID] ([!DNL Marketing Cloud Visitor ID]).

## Använda en ttid

[!DNL Target] tilldelar automatiskt en `tntid` för varje förfrågan.

I följande exempel visas begärandeformatet för att hämta en profil med en `tntid`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/your-tnt-id?client=<your-client-code>
```

Ersätt `<your-client-code>` och `your-tnt-id` och skicka en GET-förfrågan. Här är ett exempel på ett profilhämtningsanrop som använder en `tntid`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/111492025094307-353046?client=<your-client-code>
```

## Använda ett thirdPartyId

[!DNL Adobe Target] profiler kan utökas med din egen identifierare (till exempel CRM-id, `uuid`, medlemsnummer osv.).

Se [Uppdatera profiler](/help/dev/administer/profile-api/profile-api-overview.md) om du vill veta hur du kan bifoga en `thirdPartyId` till din profil.

I följande exempel visas begärandeformatet för att hämta en profil med en `thirdPartyId`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/your-thirdpartyid?client=<your-client-code>
```

Ersätt `<your-client-code>` och `your-thirdpartyid` och skicka en GET-förfrågan. Här är ett exempel på ett profilhämtningsanrop som använder en [!UICONTROL thirdpartyid]:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/a1-mbox3rdPartyId?client=<your-client-code>
```

När detta samtal görs, [!DNL Target] försöker att hitta profilen först i klustret som anges i edge-begäran, eller var profilen finns och returnera innehållet. Profilinnehållet returneras i JSON-format.

## Autentisering

The [!DNL Target Profile API] kan skyddas genom att autentisering aktiveras från [!DNL Target] Användargränssnittet som beskrivs här. När autentiseringen har aktiverats måste alla profil-API-begäranden ha profilautentiseringstoken angiven i begärandehuvudena. Själva token kan genereras med [!DNL Target] eller använda stegen ovan i [Profilautentiseringstoken](https://developers.adobetarget.com/api/#authentication-tokens){target=_blank} -avsnitt.

## Mätning

Dessa samtal räknas inte med i dina mbox-samtal.

## Felhantering

Vid en ansökningsomgång till `/thirdPartyId` med ogiltig eller utgången `thirdPartyId` anges:

```
{"status" : 404, "message" : "No profile found for client <client_code> with third party id=<third_party_id>"}
```

Om profilen inte kan hittas eller har gått ut:

```
{"status" : 404, "message" : "No profile found for client <client_code> with mboxPC=<mbox_pc>"}
```
