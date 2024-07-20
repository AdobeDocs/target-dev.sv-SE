---
title: Prenumerera på händelser i  [!DNL Adobe Target] .NET SDK
description: Lär dig hur du prenumererar på olika händelser i .NET SDK med objektet [!UICONTROL OnDeviceDecisioningHandler].
feature: APIs/SDKs
exl-id: 7578033f-3de5-4d13-9739-46ad1269ec5f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---

# SDK-händelser (.NET)

## Beskrivning

När [SDK](initialize-sdk.md) initieras kan ett valfritt `OnDeviceDecisioningReady`-ombud anges för objektet `TargetClientConfig`, som anropas när SDK är redo för metodanrop på enheten. Det finns också ett par andra delegater tillgängliga som kan hantera hämtning av artefakter från [!UICONTROL on-device decisioning].

## Händelser

Följande delegater kan konfigureras för vissa händelser:

| Namn | Argument | Beskrivning |
| --- | --- | --- |
| OnDeviceDecisioningReady | Ingen | Anropas endast en gång första gången klienten är klar för [!UICONTROL on-device decisioning] |
| ArtifactDownloadSucceeded | stränginnehåll i artefaktfilen | Anropas varje gång en [!UICONTROL on-device decisioning]-artefakt hämtas |
| ArtifactDownloadFailed | Undantag | Anropas varje gång det inte går att hämta en [!UICONTROL on-device decisioning]-artefakt |

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
