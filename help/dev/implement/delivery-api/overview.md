---
title: Adobe Target Delivery API Overview
description: Adobe Target Delivery API Overview
keywords: leverans-API
exl-id: e760bddc-b1ae-4b7b-bff2-aba81c6b6d34
feature: APIs/SDKs
source-git-commit: ccc27e66207e58dcd33865e5d28a51644e8e1931
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 0%

---

# Översikt över leverans-API

[!DNL Adobe Target Delivery API] baseras på REST. I den här dokumentationen beskrivs de resurser som utgör [!DNL Adobe Target] [!DNL Delivery API]. HTTP-metoder används för att köra åtgärder på dessa resurser.

Med [!UICONTROL Adobe Target's Delivery API] kan du:

* Leverera upplevelser på webben, inklusive SPA, och mobila kanaler liksom icke-webbläsarbaserade IoT-enheter som en ansluten TV, kioskskärm eller digital butiksskärm.
* Leverera upplevelser från alla plattformar eller applikationer på serversidan som kan ringa HTTP/s.
* Leverera enhetliga och personaliserade upplevelser till en användare oavsett vilken kanal eller vilka enheter användaren har interagerat med företaget.
* Cachelagra upplevelser för en användare i en session på servern så att flera API-anrop kan undvikas, vilket ger bättre prestanda.
* Integrera sömlöst med [!DNL Adobe Experience Cloud] produkter som [!DNL Adobe Analytics], [!DNL Adobe Audience Manager] och [!DNL Experience Cloud ID Service] från serversidan.

>[!IMPORTANT]
>
>Var försiktig när du uppdaterar [!DNL Recommendations] [!UICONTROL Catalog] via [!DNL Delivery API]. [!DNL Delivery API] är offentlig, så undvik att använda den för att fylla i klickbara objekt i din rekommendationskatalog. Om du gör det kan innehållet bli ogiltigt och katalogen förorenas.
>
>God praxis:
>
>Använd bara [!DNL Delivery API] för att uppdatera katalogattribut som:
>* Ändra ofta (till exempel pris, aktienivå).
>* Använd ett fördefinierat format som enkelt kan valideras på webbplatsen.
>* Använd den inte för att lägga till eller ändra klickbara objekt eller annat overifierat innehåll.
>
>Vid behov kan du begära kundsupport för att inaktivera kataloguppdateringar via leverans-API:t.

Mer information finns i dokumentationen för [[!UICONTROL Adobe Target Delivery API]](https://developer.adobe.com/target/implement/delivery-api/){target=_blank}.

>[!NOTE]
>
>Du kan fortfarande komma åt [äldre API-dokumentation för /v1/mbox och /v2/batchmbox](https://developers.adobetarget.com/api/legacy-api/index.html). Funktioner kommer dock att utvecklas i leverans-API (som beskrivs här) som inte kommer att backporteras till de äldre API:erna.
