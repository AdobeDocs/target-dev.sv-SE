---
title: Konfigurera datainsamling
description: Se till att alla nödvändiga åtgärder för datainsamling utförs i rätt sekvens.
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 83bb88b48cd1485fdffbc67b38da09c6209b823a
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 0%

---

# Konfigurera datainsamling

Följ stegen i *Datainsamling* diagram för att säkerställa att alla nödvändiga uppgifter som behövs för datainsamling utförs i rätt sekvens.

>[!TIP]
>
>Klicka på bilderna i det här avsnittet för att utöka till helskärm.

Datalagret är klart vid sidinläsning eller när datalagret ändras efter sidinläsning.

Om du redan har mappat data under [initiera SDK-fas](/help/dev/patterns/initialize-sdk.md)måste du utföra stegen i det här diagrammet om:

* Ditt datalager har förstärkts på alla sätt på samma sida och du vill skicka dessa ytterligare data till [!DNL Target]
* Du vill skicka produktkatalogdata till [!DNL Target Recommendations]

## Samla in datagram {#diagram}

Stegnumren i följande bild motsvarar avsnitten nedan.

![Datainsamlingsdiagram](/help/dev/patterns/assets/data-collection-diagram.png){width="600" zoomable="yes"}

Klicka på följande länkar för att navigera till önskade avsnitt:

* [2.1: Konfigurera datamappning](#configure)
* [2.2 Länka till entitetsattribut](#entity-attributes)
* [2.3 Starta Adobe Target Track API](#fire-api)

## 2.1: Konfigurera datamappning {#configure}

Detta steg hjälper till att säkerställa att alla data som måste skickas till [!DNL Adobe Target] är inställt.

+++Se information

![Konfigurera datamappningsdiagram](/help/dev/patterns/assets/cofigure-data-mapping.png){width="100" zoomable="yes"}

**Förutsättningar**

* Datalagret ska vara klart med alla data som måste skickas till [!DNL Target].

**Läser**

[Funktionen targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**Åtgärder**

Använd `targetPageParams()` för att ange alla nödvändiga data som måste skickas till [!DNL Target].

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 2.2: Länk till entitetsattribut {#entity-attributes}

Länka till entitetsattribut för att uppdatera produktkatalogen för [!DNL Target Recommendations].

+++Se information

**Läser**

* [Entitetsattribut](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**Överväganden**

* Ett annat sätt att skicka enhetsattribut är att uppdatera produktkatalogen i [!DNL Target] Användargränssnitt som ska användas [Recommendations produktflöden](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank}.
* Överföringsentitetsattribut gäller bara för sidor där produktkatalogdata är tillgängliga i datalagret.
* Skicka `entity.event.detailsOnly=true` -parametern i alla anrop får prioritet.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 2.3 Starta Adobe Target Track API {#fire-api}

Detta steg hjälper till att säkerställa att alla data som måste skickas till [!DNL Target] skickas.

+++Se information

![Fire Adobe Target Track API-diagram](/help/dev/patterns/assets/fire-track-api.png){width="100" zoomable="yes"}

**Förutsättningar**

* All datamappning måste ha gjorts med [Funktionen targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**Läser**

* [adobe.target.trackEvent(), metod](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)

**Åtgärder**

Använd [adobe.target.trackEvent(), metod](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) för att skicka alla data som måste skickas till [!DNL Target].

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

