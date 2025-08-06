---
title: Profilsynkronisering i realtid för mbox3rdPartyId
description: Lär dig hur du använder mbox3rdPartyId med  [!DNL Adobe Experience Platform Web SDK].
keywords: personalisering;mål;adobe target;renderDecision;sendEvent;mbox3rdPartyId;
feature: AEP Web SDK
source-git-commit: b694698b0957db499172af34ff3a61c10d22b0d1
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# Använd mbox3rdPartyId

`mbox3rdPartyId` i [!DNL Adobe Target] är ditt företags besökar-ID, till exempel medlemskaps-ID:t för ditt företags lojalitetsprogram.

När en besökare loggar in på ett företags webbplats skapar företaget vanligtvis ett ID som är knutet till besökarens konto, förmånskort, medlemsnummer eller andra tillämpliga identifierare för det företaget. [Läs mer](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=sv-SE)

## Använda `mbox3rdPartyId` med [!DNL Platform Web SDK]

### Steg 1: Konfigurera `Target Third Party ID Namespace`

Konfigurera `Target Third Party ID Namespace` i [Datastream](https://experienceleague.adobe.com/sv/docs/experience-platform/datastreams/overview) med det ID-namnområde som du vill använda som ett ID för en tredje part i en mbox. [Läs mer om ID-namnutrymmen](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=sv-SE)

![Experience Platform-gränssnitt som visar namnområdesfältet för mål-ID för tredje part.](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

### Steg 2: Skicka `mbox3rdpartyId` till [!DNL Target]

Skicka `mbox3rdpartyId` till [!DNL Target] i kommandot `sendEvent` med ID-namnområdet som du konfigurerade i steg 1.
[Läs mer om att skicka ID:n](/help/dev/implement/client-side/aep-web-sdk/using-mbox-3rdpartyid.md)

```javascript
alloy("sendEvent", {
  xdm: {
    "identityMap": {
      "ID_NAMESPACE": [ // Replace `ID_NAMESPACE` with the namespace you have configured in Step 1.
        {
          "id": "1234",
          "authenticatedState": "authenticated"
        }
      ]
    }
  }
});
```
