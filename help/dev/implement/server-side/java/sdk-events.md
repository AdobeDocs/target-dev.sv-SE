---
title: Prenumerera på händelser i [!DNL Adobe Target] Java SDK
description: Lär dig hur du prenumererar på olika händelser i Java SDK med [!UICONTROL OnDeviceDecisioningHandler] -objekt.
feature: APIs/SDKs
exl-id: f2d56762-6bf7-4c6b-9c14-fb20e5cfd60d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# SDK-händelser (Java)

## Beskrivning

När [initierar SDK](initialize-sdk.md), ett valfritt `OnDeviceDecisioningHandler` kan anges på `ClientConfig` -objekt. Det kan användas för att prenumerera på olika händelser som inträffar i SDK. Till exempel `onDeviceDecisioningReady` -händelsen kan användas med en callback-funktion som anropas när SDK är redo för metodanrop.

## Händelser

The `OnDeviceDecisioningHandler` -objektet innehåller följande återanrop som anropas för vissa händelser:

| Namn | Argument | Beskrivning |
| --- | --- | --- |
| onDeviceDecisioningReady | Ingen | Anropas endast första gången klienten är klar för [!UICONTROL on-device decisioning] |
| artifactDownloadSucceeded | byte[] innehåll i artefaktfil | Anropas varje gång [!UICONTROL on-device decisioning] defekten har hämtats |
| artifactDownloadFailed | Undantag | Anropas varje gång det inte går att hämta en [!UICONTROL on-device decisioning] artefakt |

## Exempel

### SDK-händelser

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .defaultDecisioningMethod(DecisioningMethod.ON_DEVICE)
        .onDeviceDecisioningHandler(new OnDeviceDecisioningHandler() {
            @Override
            public void onDeviceDecisioningReady() {
                // make getOffers requests
                makeTargetRequests();
            }

            @Override
            public void artifactDownloadSucceeded(byte[] artifactData) {
                System.out.println("The artifact was successfully downloaded.");
            }

            @Override
            public void artifactDownloadFailed(TargetClientException e) {
                System.out.println("The artifact failed to download.");
            }
        }).build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);


void makeTargetRequests() {
    List<MboxRequest> mboxRequests = new ArrayList<>();
    mboxRequests.add((MboxRequest) new MboxRequest().name("a1-serverside-ab").index(1));

    TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
            .context(new Context().channel(ChannelType.WEB))
            .execute(new ExecuteRequest().setMboxes(mboxRequests))
            .build();

    TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
}
```
