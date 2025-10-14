---
title: Konfigurera datainsamling
description: Se till att alla nödvändiga åtgärder för datainsamling utförs i rätt sekvens.
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 66e0f18d-c78c-463b-8c47-132ef6332927
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# Konfigurera datainsamling

Följ stegen i diagrammet *Datainsamling* för att se till att alla nödvändiga åtgärder som krävs för datainsamling utförs i rätt sekvens.

>[!TIP]
>
>Klicka på bilderna i det här avsnittet för att utöka till helskärm.

Datalagret är klart vid sidinläsning eller när datalagret ändras efter sidinläsning.

Om du redan har mappat data under SDK-fasen [initiera &#x200B;](/help/dev/patterns/recs-atjs/initialize-sdk.md) måste du köra stegen i det här diagrammet om:

* Ditt datalager har utökats på något sätt på samma sida och du vill skicka dessa ytterligare data till [!DNL Target]
* Du vill skicka produktkatalogdata till [!DNL Target Recommendations]

## Samla in datagram {#diagram}

Stegnumren i följande bild motsvarar avsnitten nedan.

![Datainsamlingsdiagram](/help/dev/patterns/recs-atjs/assets/data-collection-diagram.png){width="600" zoomable="yes"}

Klicka på följande länkar för att navigera till önskade avsnitt:

* [2.1: Konfigurera datamappning](#configure)
* [2.2 Länka till entitetsattribut](#entity-attributes)
* [2.3 Starta Adobe Target Track API](#fire-api)

## 2.1: Konfigurera datamappning {#configure}

Det här steget hjälper till att se till att alla data som måste skickas till [!DNL Adobe Target] är angivna.

+++Se information

![Konfigurera datamappningsdiagram](/help/dev/patterns/recs-atjs/assets/configure-data-mapping-combined.png){width="400" zoomable="yes"}

**Förutsättningar**

* Datalagret ska vara klart med alla data som måste skickas till [!DNL Target].

**Läser**

[Funktionen targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**Åtgärder**

Använd funktionen `targetPageParams()` för att ange alla nödvändiga data som måste skickas till [!DNL Target].

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 2.2: Länk till entitetsattribut {#entity-attributes}

Länka till entitetsattribut för att uppdatera produktkatalogen för [!DNL Target Recommendations].

+++Se information

**Läser**

* [Entitetsattribut](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=sv-SE){target=_blank}

**Överväganden**

* Ett annat sätt att skicka entitetsattribut är att uppdatera produktkatalogen i [!DNL Target]-gränssnittet så att [Recommendations produktflöden](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html?lang=sv-SE){target=_blank} används.
* Överföringsentitetsattribut gäller bara för sidor där produktkatalogdata är tillgängliga i datalagret.
* Att skicka parametern `entity.event.detailsOnly=true` i ett anrop har högre prioritet.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 2.3 Starta Adobe Target Track API {#fire-api}

Det här steget hjälper till att se till att alla data som måste skickas till [!DNL Target] skickas.

+++Se information

![Branda Adobe Target Track API-diagram](/help/dev/patterns/recs-atjs/assets/fire-track-api-combined.png){width="400" zoomable="yes"}

**Förutsättningar**

* All datamappning måste ha utförts med funktionen [targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**Läser**

* [adobe.target.trackEvent(), metod](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)

**Åtgärder**

Använd metoden [adobe.target.trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) om du vill skicka alla data som måste skickas till [!DNL Target].

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

Fortsätt till steg 3: [Återge upplevelser](/help/dev/patterns/recs-atjs/render-experiences.md)
