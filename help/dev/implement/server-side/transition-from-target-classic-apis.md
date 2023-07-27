---
keywords: api, adobe i/o, classic, adobe developer console
description: Lär dig hur du övergår från [!DNL Adobe Target Classic] API:er till [!DNL Target] API:er på [!DNL Adobe Developer Console].
title: Hur övergår jag från [!DNL Target Classic] API:er till [!DNL Target] API:er på [!DNL Adobe Developer Console]?
feature: APIs/SDKs
exl-id: b84e3767-89ad-4e2d-9bb4-7e31bffbc285
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# Övergång från [!DNL Target Classic] API:er till [!DNL Target] API:er på [!DNL Adobe Developer Console]

Information som kan underlätta övergången från [!DNL Target Classic] API:er till [!DNL Target] API:er på [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home).

Med avvecklingen av [!DNL Adobe Target Classic], de API:er som är anslutna till [!DNL Target Classic] kontot har också gjorts otillgängligt. Den här artikeln hjälper dig att byta [!DNL Target Classic] API-baserade integreringar av [!DNL Target] API:er som drivs av [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home).

Mer information om [!DNL Target] API:er, se [[!DNL Target] API:er](/help/dev/before-administer/target-api-overview.md). Mer information om [!DNL Target] SDK, se [[!DNL Target] Implementering på serversidan](/help/dev/implement/server-side/server-side-overview.md)

## Terminologi

| Villkor | Beskrivning |
|--- |--- |
| Klassiskt API | API:er som är länkade till [!DNL Target Classic] konto. Dessa API-anrop baseras på användarnamn och lösenordsbaserad autentisering och använder värdnamnet `testandtarget.omniture.com`. Om dina API-anrop innehåller ett användarnamn och lösenord i den begärda URL:en måste du gå över till [!DNL Adobe Developer Console] API. |
| [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home) | The [!DNL Adobe Developer Console] är gatewayen för [!DNL Target] API. Dessa API:er är anslutna till [!DNL Target Standard/Premium] konto. The [!DNL Target] API:er på [!DNL Adobe Developer Console] använd en [JWT-baserad autentisering](../../before-administer/configure-authentication.md), som är branschstandarden för säkra företags-API:er. |

## Tidslinje

Följande API:er togs bort när [!DNL Target Classic] kasserades:

| Datum | Information |
|--- |--- |
| 17 oktober 2017 | Alla API-metoder som utför en write-åtgärd (`saveCampaign`, `copyCampaign`, `saveHTMLOfferContent`och `setCampaignState`) kasserades.<P>Detta är samma datum när alla [!DNL Target Classic] användarkonton var skrivskyddade. |
| 14 november 2017 | De återstående API:erna har tagits bort. Detta är datumet då [!DNL Target Classic] användargränssnittet har avaktiverats |

[!DNL Recommendations Classic] API:er påverkades inte av den här tidslinjen.

## Likvärdiga metoder

I följande tabell visas motsvarigheten [!DNL Adobe Developer Console] API-metoder för klassiska API-metoder. The [!DNL Adobe Developer Console] API:er returnerar JSON, medan klassiska API:er returnerar XML.

The [!DNL Adobe Developer Console] API-metoder är länkade till motsvarande avsnitt på API-dokumentationswebbplatsen. Ett exempel ges för varje API-metod. Du kan också använda [!DNL Target] Admin API Postman Collection som innehåller exempel-API-anrop för alla Adobe API-metoder i [!DNL Adobe Developer Console].

| Gruppering | Klassisk API-metod | [!DNL Adobe Developer Console] API-metod | Anteckningar |
|--- |--- |--- |--- |
| Kampanj/aktivitet | Skapa kampanj | [Skapa AB-aktivitet](https://developers.adobetarget.com/api/#create-ab-activity)<P>[Skapa XT-aktivitet](https://developers.adobetarget.com/api/#create-xt-activity) | De nya API:erna har separata skapandemetoder för AB och XT |
|  | Kampanjuppdatering | [Uppdatera AB-aktivitet](https://developers.adobetarget.com/api/#update-ab-activity)<P>[Uppdatera XT-aktivitet](https://developers.adobetarget.com/api/#update-xt-activity) |  |
|  | Kopiera kampanj | Ej tillämpligt | Använda API:er för att skapa aktivitet |
|  | Kampanjlista | [Listaktiviteter](https://developers.adobetarget.com/api/#list-activities) |  |
|  | Kampanjtillstånd | [Uppdatera aktivitetsstatus](https://developers.adobetarget.com/api/#update-activity-state) |  |
|  | Kampanjvy | [Hämta AB-aktivitet efter ID](https://developers.adobetarget.com/api/#get-ab-activity-by-id)<P>[Hämta XT Activity by ID](https://developers.adobetarget.com/api/#get-xt-activity-by-id) |  |
|  | Kampanj-ID från tredje part | Ej tillämpligt | Om du använder ett tredjeparts-ID kan de relevanta aktivitetsmetoderna användas |
| Erbjudanden | Skapa erbjudande | [Skapa erbjudande](https://developers.adobetarget.com/api/#create-offer) |  |
|  | Erbjudande | [Erbjud via ID](https://developers.adobetarget.com/api/#get-offer-by-id) |  |
|  | Erbjudandelista | [Listerbjudanden](https://developers.adobetarget.com/api/#list-offers) |  |
|  | Mapplista | Ej tillämpligt | Mappar stöds inte i [!DNL Target Standard/Premium] |
| Rapportering | Kampanjresultatrapport | [Hämta prestandarapport för AB](https://developers.adobetarget.com/api/#get-ab-performance-report)<P>[Hämta XT-prestandarapport](https://developers.adobetarget.com/api/#get-xt-performance-report) |  |
|  | Granskningsrapport | [Hämta revideringsrapport](https://developers.adobetarget.com/api/#get-audit-report) |  |
|  | 1-1 Innehållsrapport | [Hämta rapport för AP-prestanda](https://developers.adobetarget.com/api/#get-ap-activity-performance-report) |  |
| Kontoinställningar | Hämta värdgrupper | [Listmiljöer](https://developers.adobetarget.com/api/#list-environments) |  |

## Undantag

Kontakta din Customer Success Manager om du behöver ett undantag.

## Hjälp

Kontakta [!DNL Adobe Target Client Care] (tt-support@adobe.com) om du har några frågor eller behöver hjälp med att gå över från de klassiska API:erna till [!DNL Target] API:er på [!DNL Adobe Developer Console].
