---
title: Prenumerera på händelser i  [!DNL Adobe Target] Python SDK
description: Lär dig hur du prenumererar på olika händelser som inträffar i Python SDK med objektet [!UICONTROL OnDeviceDecisioningHandler].
feature: APIs/SDKs
exl-id: 4e32e3b5-6072-4703-b09d-abb467aa1304
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# SDK-händelser (Python)

## Beskrivning

När [SDK](initialize-sdk.md) initieras är `options["events"]`-ordlistan ett valfritt objekt med händelsenamnnycklar och värden för återanropsfunktionen. Det kan användas för att prenumerera på olika händelser som inträffar i SDK. Händelsen `client_ready` kan till exempel användas med en callback-funktion som anropas när SDK är redo för metodanrop.

När funktionen `callback` anropas skickas ett händelseobjekt. Varje händelse har en `type` som motsvarar händelsenamnet, och vissa händelser innehåller ytterligare egenskaper med relevant information.

## Händelser

| Händelsenamn (typ) | Beskrivning | Ytterligare händelseegenskaper |
| --- | --- | --- |
| client_ready | Skickas när artefakten har hämtats och SDK är redo för get_offers-anrop. Rekommenderas vid användning | beslutsmetod på enheten. | Ingen |
| artifact_download_success | Skickas varje gång en ny artefakt hämtas. | artifact_payload, artifact_location |
| artifact_download_failed | Skickas varje gång en artefakt inte kan hämtas. | artifact_location, fel |

## Exempel

### Python

```python {line-numbers="true"}
def client_ready_callback():
    # make get_offers requests

def artifact_download_succeeded(event):
    print("The artifact was successfully downloaded from {}".format(event.artifact_location))
    # optionally do something with event.artifact_payload, like persist it

def artifact_download_failed(event):
    print("The artifact failed to download from {} with the following error: {}"
          .format(event.artifact_location, str(event.error)))

client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback,
        "artifact_download_succeeded": artifact_download_succeeded,
        "artifact_download_failed": artifact_download_failed
    }
}
target_client = target_client.create(client_options)
```
