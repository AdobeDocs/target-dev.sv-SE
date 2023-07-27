---
title: Adobe Target Admin API - översikt
description: Översikt över [!DNL Adobe Target Admin API]
exl-id: 1168d376-c95b-4c5a-b7a2-c7815799a787
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1365'
ht-degree: 0%

---

# Översikt över API för måladministratör

I den här artikeln finns en översikt över bakgrundsinformation som behövs för att förstå och använda [!DNL Adobe Target Admin API]har slutförts. Följande innehåll förutsätter att du förstår hur [konfigurera autentisering](../configure-authentication.md) for [!DNL Adobe Target Admin API]s.

>[!NOTE]
>
>Om du vill administrera [!DNL Target] via användargränssnittet, se [administreringsavdelning *Adobe Target Business Practitioner Guide*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=en).
>
>Admin-API:erna och profil-API:erna kallas ofta tillsammans (&quot;Admin- och Profile-API:er&quot;), men kan också hänvisas till separat (&quot;Admin-API:er&quot; och&quot;Profil-API:er&quot;). Recommendations API är en specifik implementering av en [!DNL Target] Admin-API.

## Innan du börjar

I alla kodexempel för [Administratörs-API:er](../../administer/admin-api/admin-api-overview-new.md), ersätt {tenant} med ert hyresvärde, `your-bearer-token` med den åtkomsttoken som du genererar med din JWT och `your-api-key` med din API-nyckel från [Adobe Developer Console](https://developer.adobe.com/console/home). Mer information om hyresgäster och JWT finns i artikeln om hur du [konfigurera autentisering](../configure-authentication.md) för Adobe [!DNL Target] Admin-API:er.

## Versioner

Alla API:er har en associerad version. Det är viktigt att du tillhandahåller rätt version av det API som du vill använda.

Om begäran innehåller en nyttolast (POST eller PUT), `Content-Type` begärans huvud används för att ange versionen.

Om begäran inte innehåller någon nyttolast (GET, DELETE eller OPTIONS), `Accept` används för att ange versionen.

Om ingen version anges används V1 (application/vnd.adobe.target.v1+json) som standard.

>[!NOTE]
>
>Om rätt version inte anges, till exempel om du använder en V2-nyttolast men inte anger Content-Type-huvudet, kommer API:t att svara med ett fel som inte stöds om API:t inte är bakåtkompatibelt.

Felmeddelande för funktioner som inte stöds

```
{
    "httpStatus": 406,
    "requestId": "8752b736-cf71-4d81-86c3-94be2b5ae648",
    "requestTime": "2018-02-02T21:39:06.405Z",
    "errors": [
        {
            "errorCode": "Unsupported.Feature",
            "message": "Unsupported features detected"
        }
    ]
}
```

Admin Postman Collection

Postman är ett program som gör det enkelt att utlösa API-anrop. Detta [API för måladministratör för Postman Collection](https://developers.adobetarget.com/api/#admin-postman-collection) innehåller alla Target Admin API-anrop som kräver autentisering med hjälp av aktiviteter, målgrupper, erbjudanden, rapporter, kartor och miljöer

## Svarskoder

Här är de vanliga svarskoderna för Target Admin API:erna.

| Status | Betydelse | Beskrivning |
| --- | --- | --- |
| 200 | [OK](https://www.rfc-editor.org/rfc/rfc7231#section-6.3.1) | OK |  |
| 400 | [Felaktig begäran](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.1) | Felaktig begäran. Troligen är de data som angavs i begäran ogiltiga. |  |
| 401 | [Obehörig](https://www.rfc-editor.org/rfc/rfc7235#section-3.1) | Användaren har inte behörighet att utföra den här åtgärden. |  |
| 403 | [Förbjuden](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.3) | Åtkomst till den här resursen är förbjuden. |  |
| 404 | [Hittades inte](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.4) | Den refererade resursen hittades inte. |  |

## Verksamhet

Med en aktivitet kan du testa och anpassa innehåll för dina användare. Verksamheter kan vara någon av följande typer:

* [A/B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html)
* [Experience Targeting (XT)](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html)
* [Recommendations](https://experienceleague.adobe.com/docs/target/using/activities/recommendations-activity.html)
* [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [Multivariata tester (MVT)](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html)

## Batchuppdateringar

Flera administratörs-API:er kan köras som en enda gruppbegäran.

### Kör gruppanrop

`POST /{tenant}/target/batch`

Stapla ihop flera API-anrop och kör dem i en enda batch.

Med gruppering kan du skicka instruktioner för flera åtgärder i en enda HTTP-begäran. Du kan också ange beroenden mellan relaterade åtgärder (beskrivs i ett avsnitt nedan). TNT bearbetar alla dina oberoende operationer (eventuellt parallellt) och bearbetar de beroende operationerna sekventiellt. När alla åtgärder har slutförts skickas ett konsoliderat svar tillbaka och HTTP-anslutningen stängs.

Batch-API:t innehåller en matris med logiska HTTP-begäranden som representeras av JSON-matriser - varje begäran har en metod (som motsvarar HTTP-metoden GET/PUT/POST/DELETE osv.), en relativeUrl (den del av URL:en som kommer efter admin/rest/), en valfri rubrikmatris (som motsvarar HTTP-huvuden) och ett valfritt brödtext (för POST- och PUT). Batch-API:t returnerar en array med logiska HTTP-svar representerade som JSON-arrayer - varje svar har en statuskod, en valfri rubrikarray och en valfri brödtext (som är en JSON-kodad sträng). Om du vill göra grupperade begäranden skapar du ett JSON-objekt som beskriver varje enskild åtgärd som ska utföras. Antalet tillåtna operationer är 256 (från 0 till 255).

Att ange beroenden mellan åtgärder i begäran Som standard är de åtgärder som anges i batch-API-begäran fristående. De kan köras i godtycklig ordning på servern och ett fel i en åtgärd påverkar inte körningen av andra åtgärder.

Ofta är åtgärderna i begäran beroende - utdata från en åtgärd kan till exempel användas i indata från nästa åtgärd. Erbjudandet som skapats i operationId=0 måste till exempel användas i kampanjskapandeoperationId=1.

För att länka ihop två batchåtgärder anger du ID:t för den nödvändiga åtgärden i den beroende åtgärden, till exempel: &quot;beroendeOnOperationId&quot; : 5. ID:n för skapade resurser via POST-begäranden från gruppåtgärder kan också användas i beroende åtgärder både i &quot;relativeUrl&quot; och &quot;body&quot;.

#### Behörigheter och begränsning

För att kunna köra batch-API-åtgärder måste den underliggande användaren ha minst&quot;redigeringsbehörighet&quot; (för varje enskild åtgärd om ytterligare behörighet krävs än användaren har kommer den enskilda åtgärden att misslyckas). Vanliga begränsningsstrategier tillämpas på API-åtgärder för batch som om varje åtgärd har utförts individuellt.

Gruppbearbetningen avslutas när alla åtgärder har slutförts, en åtgärd kan antingen slutföras (2xx statusCode), misslyckas (4xx, 5xx statuskod) eller hoppas över eftersom en beroendeåtgärd har misslyckats eller har hoppats över.

#### Begär objektparametrar

| Attribut | Beskrivning | Gränser | Standard |
| --- | --- | --- | --- |
| brödtext | brödtext för HTTP-batchåtgärd. ignoreras för alla åtgärder utom POST och PUT. kan referera till ID:n från tidigare batchåtgärder, till exempel: &quot;offerId&quot;: &quot;{operationIdResponse:0}&quot;, &quot;segmentId&quot;: &quot;{operationIdResponse:1}&quot; | ska vara en giltig JSON; om en operationIdResponse refereras ska det refererade operationId-svaret vara ett giltigt ID och metoden för den åtgärden ska vara POST | tomt objekt {} |  |
| LegityOnOperationIds | lista över begränsnings-ID:n som säkerställer att den aktuella åtgärden endast körs om de angivna åtgärderna har slutförts. Kan användas för att uppnå kedjning av operationer. | maximalt 255 åtgärder tillåts; unika värden tillåts bara; ska peka på ett giltigt operationId i arrayen; cykliska beroenden tillåts inte |  |  |
| rubriker | matris med nyckelvärdesrubriker som ska skickas med en viss åtgärd. Om autentisering för batch-API har utförts via auktoriseringshuvud kopieras den också för enskilda åtgärder. | maximalt antal rubriker i matrisen är 50 | Content-Type: application/json |  |
| headers->name | rubriknamn | ska vara unika bland andra rubriknamn. rubriker är skiftlägeskänsliga av rfc, annars åsidosätter värdena varandra. |  |  |
| headers->value | rubrikvärde | Ej tillämpligt | tom sträng |  |
| method | HTTP-metod som ska användas. Tillgängliga alternativ: GET, POST, PUT, PATCH, DELETE | Endast metoderna GET, POST, PUT, PATCH och DELETE är tillåtna |  |  |
| operationId | åtgärds-ID som används för att identifiera en åtgärd bland andra åtgärder för svar och referensresultat. | unika bland andra operationer; värden mellan 0 och 255 |  |  |
| operationer | lista över åtgärder som ska utföras i en batch. ordern inte är relevant. | högst 256 åtgärder tillåts |  |  |
| relativeUrl | relativ URL för admin rest API, delen efter /admin/rest/. Kan innehålla frågesträngsparametrar som: &quot;/v2/campaign?limit=10&amp;offset=10&quot;. kan referera till URL:er med ID:n från tidigare gruppåtgärder, till exempel: &quot;/v1/offers/{operationIdResponse:0}&quot;. Om frågeparametrar skickas måste de vara URL-kodade. | ska börja med / (be relative); endast nya giltiga JSON API:er stöds; om relativeURL är ogiltigt returneras ett 404-svar för en viss åtgärd. Om en operationIdResponse refereras ska operationId-svaret som refereras vara ett giltigt ID och metoden för den åtgärden ska vara POST |  |  |

#### Exempelbegärandeobjekt

```
{
  "operations": [
    {
      "operationId": 1,
      "dependsOnOperationIds~": [0],
      "method": "POST",
      "relativeUrl": "/v1/offers",
      "headers~": [
        {
          "name": "Content-Type",
          "value": "application/json"
        }
      ],
      "body~": {
        "key": "value"
      }
    }
  ]
}
```

#### Parametrar för svarsobjekt

| Parameter | Beskrivning |
| --- | --- |
| operationId | Åtgärds-ID som används för att identifiera en åtgärd bland andra åtgärder, samma ID som det har skickats i en begäran om POST. |  |
| överhoppad | boolen-flagga för att markera om åtgärden har körts eller hoppats över. Kommer att vara true om den aktuella åtgärden är beroende av en åtgärd som har misslyckats (returnerade ett annat statusCode-värde än 2xx). |  |
| statusCode | returneras kommer alla beroende åtgärder att hoppas över (inte utföras). |  |
| rubriker | matris med nyckelvärdesrubriker som ska skickas som svar på en viss åtgärd. |  |
| headers->name | rubriknamn |  |
| headers->value | rubrikvärde |  |
| brödtext | body for HTTP batch response operation |  |

#### Exempelsvarsobjekt

```
{
  "results": [
    {
      "operationId": 1,
      "skipped~": false,
      "statusCode~": 200,
      "headers~": [
        {
          "name": "Content-Type",
          "value": "application/json; charset=UTF-8"
        }
      ],
      "body~": {
        "id": 5
      }
    }
  ]
}
```
