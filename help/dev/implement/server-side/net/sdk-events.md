---
title: Prenumerera på händelser i [!DNL Adobe Target] .NET SDK
description: Lär dig hur du prenumererar på olika händelser i .NET SDK med [!UICONTROL OnDeviceDecisioningHandler] -objekt.
feature: APIs/SDKs
exl-id: 7578033f-3de5-4d13-9739-46ad1269ec5f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---

# SDK-händelser (.NET)

## Beskrivning

När [initierar SDK](initialize-sdk.md), ett valfritt `OnDeviceDecisioningReady` delegat kan anges på `TargetClientConfig` -objekt, som anropas när SDK är redo för metodanrop på enheten. Det finns även ett par andra delegater som kan hantera [!UICONTROL on-device decisioning] artefaktnedladdning.

## Händelser

Följande delegater kan konfigureras för vissa händelser:

| Namn | Argument | Beskrivning |
| --- | --- | --- |
| OnDeviceDecisioningReady | Ingen | Anropas endast första gången klienten är klar för [!UICONTROL on-device decisioning] |
| ArtifactDownloadSucceeded | stränginnehåll i artefaktfilen | Anropas varje gång [!UICONTROL on-device decisioning] defekten har hämtats |
| ArtifactDownloadFailed | Undantag | Anropas varje gång det inte går att hämta en [!UICONTROL on-device decisioning] artefakt |

## Exempel

### \.NET

```dotnet {line-numbers="true"}
var clientConfig = new TargetClientConfig.Builder("acmeclient", "1234567890@AdobeOrg")
    .SetDecisioningMethod(DecisioningMethod.OnDevice)
    .SetOnDeviceDecisioningReady(DecisioningReady)
    .SetArtifactDownloadSucceeded(artifact => Console.WriteLine("The artifact was successfully downloaded. Contents: " + artifact))
    .SetArtifactDownloadFailed(exception => Console.WriteLine("The artifact failed to download. Exception: " + exception.Message))
    .Build();

var targetClient = TargetClient.Create(clientConfig);

// ...

static void DecisioningReady()
{
    var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

    var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
        .SetExecute(new ExecuteRequest(mboxes: mboxRequests))
        .Build();

    var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
}
```
