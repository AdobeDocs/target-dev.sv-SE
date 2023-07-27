---
title: Prenumerera på händelser i [!DNL Adobe Target] Node.js SDK
description: Lär dig hur du prenumererar på olika händelser i Node.js SDK med hjälp av [!UICONTROL OnDeviceDecisioningHandler] -objekt.
feature: APIs/SDKs
exl-id: 40c53840-a560-4819-ae04-f527c36b22fe
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 0%

---

# SDK-händelser (Node.js)

## Beskrivning

När [initierar SDK](initialize-sdk.md), `options.events` är ett valfritt objekt med händelsenamnnycklar och värden för återanropsfunktionen. Det kan användas för att prenumerera på olika händelser som inträffar i SDK. Till exempel `clientReady` -händelsen kan användas med en callback-funktion som anropas när SDK är redo för metodanrop.

När återanropsfunktionen anropas skickas ett händelseobjekt. Varje händelse har en `type` motsvarar händelsenamnet. Vissa händelser innehåller ytterligare egenskaper med relevant information.

## Händelser

| Händelsenamn (typ) | Beskrivning | Ytterligare händelseegenskaper |
| --- | --- | --- |
| clientReady | Skickas när artefakten har hämtats och SDK är redo för `getOffers` samtal. Rekommenderas när du använder enhetsspecifik beslutsmetod. |
| artifactDownloadSucceeded | Skickas varje gång en ny artefakt hämtas. | artifactPayload, artifactLocation |
| artifactDownloadFailed | Skickas varje gång en artefakt inte kan hämtas. | artifactLocation, fel |

## Exempel

### Node.js

```js {line-numbers="true"}
const targetClient = TargetClient.create({
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device",
    events: {
        clientReady: onTargetClientReady,
        artifactDownloadSucceeded: onArtifactDownloadSucceeded,
        artifactDownloadFailed: onArtifactDownloadFailed
    }
});

function onTargetClientReady() {
    // make getOffers requests
    targetClient.getOffers({...})            
}

function onArtifactDownloadSucceeded(event) {
    console.log(`The artifact was successfully downloaded from '${event.artifactLocation}'`);
    // optionally do something with event.artifactPayload, like persist it
}

function onArtifactDownloadFailed(event) {
    console.log(`The artifact failed to download from '${event.artifactLocation}' with the following error message: ${event.error.message}`);
}
```
