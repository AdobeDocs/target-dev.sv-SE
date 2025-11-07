---
title: Prenumerera på händelser i  [!DNL Adobe Target] Node.js SDK
description: Lär dig hur du prenumererar på olika händelser som inträffar i Node.js SDK med hjälp av objektet [!UICONTROL OnDeviceDecisioningHandler].
feature: APIs/SDKs
exl-id: 40c53840-a560-4819-ae04-f527c36b22fe
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 0%

---

# SDK Events (Node.js)

## Beskrivning

När [SDK](initialize-sdk.md) initieras är `options.events`-objektet ett valfritt objekt med händelsenamnstangenter och callback-funktionsvärden. Det kan användas för att prenumerera på olika händelser inom SDK. Händelsen `clientReady` kan till exempel användas med en callback-funktion som anropas när SDK är redo för metodanrop.

När återanropsfunktionen anropas skickas ett händelseobjekt. Varje händelse har en `type` som motsvarar händelsenamnet. Vissa händelser innehåller ytterligare egenskaper med relevant information.

## Händelser

| Händelsenamn (typ) | Beskrivning | Ytterligare händelseegenskaper |
| --- | --- | --- |
| clientReady | Skickas när artefakten har hämtats och SDK är redo för `getOffers` samtal. Rekommenderas när du använder enhetsspecifik beslutsmetod. |  |
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
