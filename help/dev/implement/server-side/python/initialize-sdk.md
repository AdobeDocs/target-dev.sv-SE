---
title: Initiera Python SDK med metoden create
description: Lär dig hur du använder metoden create för att initiera Python SDK och instansiera [!UICONTROL TargetClient] för att ringa till  [!DNL Adobe Target] för experiment och personaliserade upplevelser.
feature: APIs/SDKs
exl-id: 3e231e8e-696d-45c7-b733-79bf99da5bec
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 0%

---

# Initiera Python SDK

Beskrivning
Använd metoden `create` för att initiera Python SDK och instansiera [!UICONTROL Target Client] för att anropa [!DNL Adobe Target] för experiment och personaliserade upplevelser.

## Metod

### skapa

```python {line-numbers="true"}
TargetClient.create(options)
```

## Parametrar

`options` har följande struktur:

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- | --- | --- | --- | --- |
| klient | str | Ja | Ingen | [!UICONTROL Adobe Target client ID] |
| organisation_id | str | Ja | Ingen | [!UICONTROL Experience Cloud Organization ID] |
| timeout | int | Nej | 3000 | Timeout i millisekunder |
| server_domain | str | Nej | `client.tt.omtrdc.net` | Åsidosätter standardvärdnamn |
| säker | bool | Nej | true | Avmarkerad för att tillämpa HTTP-schema |
| loggare | object | Nej | INFO-logg | Ersätter standardINFO-loggaren |
| target_location_hint | str | Nej | Ingen | Platstips för [!DNL Target] |
| property_token | str | Nej | Ingen | [!DNL Target] egenskapstoken. Om det anges här kommer alla get_offers-anrop att använda det här värdet. |
| decisioning_method | str | Nej | server-side | Avgör vilken beslutsmetod som ska användas ([på enheten](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md), på serversidan, hybrid) |
| polling_interval | int | Nej | 300000 (5 minuter) | Avsökningsintervall för artefakten [för enhetsspecifik avlyssningsregel ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) (i ms) |
| artifact_location | str | Nej | Ingen | En fullständigt kvalificerad URL till [regeln för enhetsbeslut](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Åsidosätter internt bestämd plats. |
| artifact_payload | object | Nej | Ingen | JSON-nyttolasten för [enhetsregelartefakten ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Om den anges används den i stället för att begära en från en URL. |
| [händelser](sdk-events.md) | dict &lt;str, callable> | Nej | Ingen | Ett valfritt objekt med händelsenamnstangenter och callback-funktionsvärden |
| environment_id | int | Nej | produktion | Miljö-ID för [!DNL Target] |
| miljö | str | Nej | produktion | Miljönamnet [!DNL Target] |
| cdn_environment | str | Nej | produktion | CDN-miljönamnet |
| telemetry_enabled | bool | Nej | True | Om värdet är Falskt skickas inte telemetridata till [!DNL Adobe] |
| version | str | Nej | Ingen | Versionsnumret för denna SDK |

## Exempel

### Python

```python {line-numbers="true"}
from target_python_sdk import TargetClient

def client_ready_callback(ready_event):
    # make calls to Adobe Target

client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
