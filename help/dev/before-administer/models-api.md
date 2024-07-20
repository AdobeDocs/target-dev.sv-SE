---
title: API-översikt för Adobe-modeller
description: Översikt över API:t för modeller, som användare kan använda för att blockera funktioner från att inkluderas i maskininlärningsmodeller.
exl-id: e34b9b03-670b-4f7c-a94e-0c3cb711d8e4
feature: APIs/SDKs, Recommendations, Administration & Configuration
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '1288'
ht-degree: 0%

---

# API-översikt för modeller

Med API:t för modeller, som även kallas API för Blockeringslista, kan användare visa och hantera listan med funktioner som används i maskininlärningsmodeller för [!UICONTROL Automated Personalization]- (AP) och [!DNL Auto-Target] (AT)-aktiviteter. Om en användare vill utesluta en funktion från att användas av modellerna för AP- eller AT-aktiviteter kan de använda API:t för modeller för att lägga till den funktionen i blockeringslista.

En **[!UICONTROL blocklist]** definierar den uppsättning funktioner som kommer att exkluderas av [!DNL Adobe Target] från dess maskininlärningsmodeller. Mer information om funktioner finns i [Data som används av  [!DNL Target] maskininlärningsalgoritmer](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/ap-data.html).

Blockeringslista kan definieras per aktivitet (aktivitetsnivå) eller för alla aktiviteter inom ett [!DNL Target]-konto (global nivå).

<!-- To get started with the Models API in order to create and manage your blocklist, download the Postman Collection [here](https://git.corp.adobe.com/target/ml-configuration-management-service/tree/nextRelease/rest_api_library). Note this is an Adobe internal link. Need to publish this publicly if want to share with customers. -->

## API-specifikation för modeller

Visa Models API-specifikationen [här](../administer/models-api/models-api-overview.md).

## Förutsättningar

Om du vill använda API:t för modeller måste du konfigurera autentisering med [Adobe Developer Console](https://developer.adobe.com/console/home), precis som med [API:t för måladministratörer](../administer/admin-api/admin-api-overview-new.md). Mer information finns i [Konfigurera autentisering](../before-administer/configure-authentication.md).

## Riktlinjer för användning av API-modeller

Hantera blockeringslista

[**Steg 1:**](#step1) Visa en lista över funktioner för en aktivitet

[**Steg 2:**](#step2) Kontrollera aktivitetens blockeringslista

[**Steg 3:**](#step3) Lägg till funktioner till blockeringslista i aktiviteten

[**Steg 4:**](#step4) (valfritt) Avblockera

[**Steg 5:**](#step5) (valfritt) Hantera den globala blockeringslista


## Steg 1: Visa lista med funktioner för en aktivitet {#step1}

Innan du blocklist en funktion ska du visa en lista över funktioner som för närvarande ingår i modellerna för den aktiviteten.

>[!BEGINTABS]

>[!TAB Begäran]

```json {line-numbers="true"}
GET https://mc.adobe.io/<tenant>/target/models/features/<campaignId>
```

>[!TAB Svar]

```json {line-numbers="true"}
{
    "features": [
        {
            "externalName": "Visitor Profile - Total Visits to Activity",
            "internalName": "SES_PREVIOUS_VISIT_COUNT",
            "type": "CONTINUOUS"
        },
        {
            "externalName": "Visitor Profile - Total Visits",
            "internalName": "SES_TOTAL_SESSIONS",
            "type": "CONTINUOUS"
        },
        {
            "externalName": "Visitor Profile - Pages Seen Before Activity",
            "internalName": "SES_PREVIOUS_VISIT_COUNT",
            "type": "CONTINUOUS"
        },
        {
            "externalName": "Visitor Profile - Activity Lifetime Time on Site",
            "internalName": "SES_TOTAL_TIME",
            "type": "CONTINUOUS"
        }
    ],
    "reportParameters": {
        "clientCode": <tenant>,
        "campaignId": <campaignId>
    }
}
```

>[!ENDTABS]

<!-- JUDY: Update codeblock above once you have the complete Response. -->

I exemplet som visas här kontrollerar användaren om det finns en lista över funktioner som används i modellen för aktiviteten vars aktivitets-ID är 260840.

![Steg 1](assets/models-api-step-1.png)

>[!NOTE]
>
>Navigera till aktivitetslistan i användargränssnittet för [!DNL Target] om du vill hitta aktivitetens aktivitets-ID. Klicka på aktiviteten. Aktivitets-ID visas i texten på sidan Översikt över aktiviteter, samt i slutet av URL:en för sidan.

**[!UICONTROL externalName]** är ett användarvänligt namn för en funktion. Den har skapats av [!DNL Target] och det är möjligt att det här värdet kan ändras över tiden. Användare kan visa de här användarvänliga namnen i [Personalization Insights-rapporten](https://experienceleague.adobe.com/docs/target/using/reports/insights/personalization-insights-reports.html).

**[!UICONTROL internalName]** är funktionens faktiska identifierare. Den har också skapats av [!DNL Target], men kan inte ändras. Detta är det värde som du måste referera till för att identifiera de funktioner som du vill blocklist.

Observera, att en aktivitet krävs för att funktionslistan ska innehålla värden (det vill säga för att den ska vara icke-null):

1. Måste ha status = Live eller ha aktiverats tidigare
1. Måste ha körts tillräckligt länge för att det ska finnas kampanjaktiviteter, så att modellen har haft data att köra mot.

## Steg 2: Kontrollera aktivitetens blockeringslista {#step2}

Visa sedan blockeringslista. Kontrollera med andra ord vilka funktioner, om några, som för närvarande blockeras från att inkluderas i modellerna för den här aktiviteten.

>[!ERROR]
>
>Observera att `/blockList/` är skiftlägeskänslig i begäran.

>[!BEGINTABS]

>[!TAB Begäran]

```json {line-numbers="true"}
GET https://mc.adobe.io/<tenant>/target/models/features/blockList/<campaignId>
```

>[!TAB Svar]

```json {line-numbers="true"}

```

>[!ENDTABS]

I exemplet som visas här kontrollerar användaren listan över blockerade funktioner för aktiviteten vars aktivitets-ID är 260840. Resultatet är tomt, vilket innebär att den här aktiviteten inte har några blocklist funktioner.

![Steg 2](assets/models-api-step-2.png)

>[!NOTE]
>
>Du kan se tomma resultat som detta, första gången du kontrollerar hela blockeringslista, innan du lägger till några funktioner i den. När du har lagt till (och sedan tagit bort) funktioner från en blockeringslista kan du dock se något annorlunda resultat, där en tom blocklist funktionsarray returneras. Fortsätt läsa för att se ett exempel på detta i [steg 4](#step4).

## Steg 3: Lägg till funktioner till blockeringslista i aktiviteten {#step3}

Om du vill lägga till funktioner i blockeringslista ändrar du begäran från GET till PUT och ändrar texten i begäran så att `blockedFeatureSources` eller `blockedFeatures` anges efter behov.

* Innehållet i begäran kräver antingen `blockedFeatures` eller `blockedFeatureSources`. Båda kan inkluderas.
* Fyll i `blockedFeatures` med värden som identifieras från `internalName`. Se [Steg 1](#step1).
* Fyll i `blockedFeatureSources` med värden från tabellen nedan.

Observera att `blockedFeatureSources` indikerar varifrån en funktion kom. När du blocklist fungerar de som grupper eller kategorier av funktioner, som gör att användare kan blockera hela uppsättningar med funktioner samtidigt. Värdena för `blockedFeatureSources` matchar de första tecknen i en funktions identifierare (`blockedFeatures` eller `internalName` värden). Därför kan de också betraktas som &quot;funktionsprefix&quot;.

### Tabell med `blockedFeatureSources` värden {#table}

| Prefix | Beskrivning |
| --- | --- |
| RUTA | Mbox-parameter |
| URL | Anpassad - URL-parameter |
| ENV | Miljö |
| SE | Besökarprofil |
| GEO | Geo-plats |
| PRO | Anpassad - profil |
| SEG | Anpassad - Rapporteringssegment |
| AAM | Anpassad - Experience Cloud-segment |
| MOB | Mobil |
| CRS | Anpassad - kundattribut |
| UPA | Anpassad - RT-CDP-profilattribut |
| IAC | Intresseområden för besök |  |

>[!BEGINTABS]

>[!TAB Begäran]

```json {line-numbers="true"}
PUT https://mc.adobe.io/<tenant>/target/models/features/blockList/<campaignId>

{
    "blockedFeatureSources": ["AAM"],
    "blockedFeatures": ["SES_PREVIOUS_VISIT_COUNT", "SES_TOTAL_SESSIONS"]
}
```

>[!TAB Svar]

```json {line-numbers="true"}
{
    "blockedFeatures": [
            "SES_PREVIOUS_VISIT_COUNT",
            "SES_TOTAL_SESSIONS"
        ],
    "blockedFeatureSources": [
            "AAM"
        ]
}
```

>[!ENDTABS]

I exemplet som visas här blockerar användaren två funktioner, `SES_PREVIOUS_VISIT_COUNT` och `SES_TOTAL_SESSIONS`, som de tidigare identifierade genom att fråga en fullständig lista över funktioner för aktiviteten vars aktivitets-ID är 260480, vilket beskrivs i [Steg 1](#step1). De blockerar också alla funktioner som kommer från Experience Cloud-segment, vilket uppnås genom att blockera funktioner med prefixet &quot;AAM&quot;, vilket beskrivs i [tabellen](#table) ovan.

![Steg 3](assets/models-api-step-3.png)

Observera att när du har blocklist en funktion bör du verifiera den uppdaterade blockeringslista genom att utföra [Steg 2](#step2) igen (GET blocklist). Kontrollera att resultatet visas som förväntat (kontrollera att resultatet innehåller de funktioner som lagts till från senaste PUT-begäran).

## Steg 4: (valfritt) Avblockera {#step4}

Om du vill ta bort blockeringen för alla blocklist funktioner tar du bort värdena från `blockedFeatureSources` eller `blockedFeatures`.

>[!BEGINTABS]

>[!TAB Begäran]

```json {line-numbers="true"}
PUT https://mc.adobe.io/<tenant>/target/models/features/blockList/<campaignId>

{
    "blockedFeatureSources": [],
    "blockedFeatures": []
}
```

>[!TAB Svar]

```json {line-numbers="true"}
{
    "blockedFeatures": [],
    "blockedFeatureSources": []
}
```

>[!ENDTABS]

I exemplet som visas här rensar användaren blockeringslista för aktiviteten vars aktivitets-ID är 260840. Observera att svaret bekräftar tomma arrayer för både blockerade funktioner och deras källor -`blockedFeatureSources` respektive `blockedFeatures`.

![Steg 4](assets/models-api-step-4.png)

När du har ändrat blockeringslista bör du som alltid utföra [Steg 2](#step2) igen (GET blockeringslista för att verifiera att listan innehåller funktioner som du förväntade dig). I exemplet som visas här bekräftar användaren att blockeringslista nu är tom.

![Steg 4b](assets/models-api-step-4b.png)

Fråga: Hur tar jag bort vissa, men inte alla, av ett blockeringslista?

Svar: Om du vill ta bort en separat delmängd av blocklist funktioner från en blockeringslista med flera funktioner kan användare helt enkelt skicka den uppdaterade listan över funktioner som de vill blockera i [blockeringslista-begäran](#step3), i stället för att rensa hela blockeringslista och lägga till de önskade funktionerna igen. Skicka med andra ord den uppdaterade funktionslistan (som visas i [steg 3](#step3)) och se till att de funktioner som du vill ta bort inte finns med på blockeringslista.

## Steg 5: (Valfritt) Hantera den globala blockeringslista {#step5}

Exemplen ovan var alla i samband med en enda aktivitet. Du kan också blockera funktioner för alla aktiviteter för en viss klient (klientorganisation) i stället för att behöva ange blockeringslista för varje aktivitet separat. Använd anropet `/blockList/global` i stället för `blockList/<campaignId>` om du vill utföra en global blockeringslista.

>[!BEGINTABS]

>[!TAB Begäran]

```json {line-numbers="true"}
PUT https://mc.adobe.io/<tenant>/target/models/features/blockList/global

{
    "blockedFeatureSources": ["AAM", "PRO", "ENV"],
    "blockedFeatures": ["AAM_FEATURE_1", "AAM_FEATURE_2"]
}
```

>[!TAB Svar]

```json {line-numbers="true"}
{
    "blockedFeatures": [
        "AAM_FEATURE_1",
        "AAM_FEATURE_2"
    ],
    "blockedFeatureSources": [
        "AAM",
        "PRO",
        "ENV"
    ]
}
```

>[!ENDTABS]

I exemplet med begäran ovan blockerar användaren två funktioner, &quot;AAM_FEATURE_1&quot; och &quot;AAM_FEATURE_2&quot;, för alla aktiviteter i sitt [!DNL Target]-konto. Det innebär, oavsett aktivitet, att &quot;AAM_FEATURE_1&quot; och &quot;AAM_FEATURE_2&quot; inte kommer att inkluderas i datorutbildningsmodellerna för det här kontot. Dessutom blockerar användaren globalt alla funktioner vars prefix är&quot;AAM&quot;,&quot;PRO&quot; eller&quot;ENV&quot;.

Fråga: Är inte kodexemplet ovan överflödigt?

Svar: Ja. Det är överflödigt att blockera funktioner med värden som börjar med&quot;AAM&quot;, samtidigt som alla funktioner vars källa är&quot;AAM&quot; blockeras. Nettoresultatet är att alla funktioner som kommer från AAM (Experience Cloud Segments) blockeras. Om målet är att blockera alla funktioner från Experience Cloud-segment är det därför onödigt att ange vissa funktioner som börjar med&quot;AAM&quot; separat, i exemplet ovan.

Slutligt steg: Oavsett om du arbetar på aktivitets- eller global nivå rekommenderar vi att du kontrollerar blockeringslista efter att du har ändrat den, så att den innehåller de värden du förväntar dig. Gör detta genom att ändra `PUT` till `GET`.

Det exempelsvar som visas nedan indikerar att [!DNL Target] blockerar två enskilda funktioner, plus alla funktioner som kommer från&quot;AAM&quot;,&quot;PRO&quot; och&quot;ENV&quot;.

![Steg 5](assets/models-api-step-5.png)
