---
keywords: Recommendations, inställningar, inställningar, branschvertikal, filterinkompatibla villkor, standardvärdgrupp, tumbas-URL, rekommendationer-API-token, $9
description: Lär dig hur du implementerar [!UICONTROL Recommendations] aktiviteter i  [!DNL Adobe Target].
title: Hur implementerar jag [!UICONTROL Recommendations] aktiviteter?
feature: Recommendations
exl-id: af1e8b60-6dbb-451b-aa4f-e167d1800d1c
source-git-commit: 777c8b9aeb866e1b20ec24e85532a99f57d1fe72
workflow-type: tm+mt
source-wordcount: '1365'
ht-degree: 0%

---

# Planera och implementera [!UICONTROL Recommendations]

Information som kan hjälpa dig att planera och implementera [!DNL Adobe Target Recommendations].

>[!NOTE]
>
>Utöver den här artikeln innehåller [Adobe Target Business Practitioner](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=sv-SE){target=_blank} detaljerad information om [Target Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=sv-SE){target=_blank}.

Innan du konfigurerar din första [!UICONTROL Recommendations]-aktivitet i [!DNL Adobe Target] utför du följande steg:

1. [Implementera [!UICONTROL Target]](#implement-target) på de webbsidor och mobilappsytor som du vill använda för att hämta användarbeteenden och leverera rekommendationer.
1. [Konfigurera din [!UICONTROL Recommendations] katalog ](#set-up-your-recommendations-catalog) med produkter eller innehåll som du vill rekommendera dina användare.
1. [Skicka beteendeinformation och kontext](#pass-behavioral-information-and-context) till [!DNL Target Recommendations] så att den kan leverera personaliserade rekommendationer.
1. [Konfigurera globala undantag](#configure-global-exclusions).
1. [Konfigurera [!UICONTROL Recommendations] inställningar](#configure-recommendations-settings).
1. (Valfritt) [Administrera [!UICONTROL Recommendations] med Admin-API:er](#administer-recommendations-using-admin-apis).

## 1. Implementera [!UICONTROL Target]

[!DNL Target Recommendations] kräver att du implementerar Adobe Experience Platform Web SDK eller at.js 0.9.2 (eller senare). Mer information finns i implementeringsguiderna för [[!UICONTROL Target] på klientsidan ](../client-side/overview.md).

## 2. Konfigurera [!UICONTROL Recommendations]-katalogen

[!UICONTROL Target] måste känna till vilka produkter eller vilket innehåll du vill rekommendera för att kunna leverera rekommendationer av hög kvalitet. Kataloger innehåller vanligtvis tre typer av information om rekommenderade objekt. Anta att du rekommenderar filmer. Inkludera följande:

1. Data som du vill visa för användaren som tar emot rekommendationen. Du kan till exempel visa namnet på filmen och en URL för en miniatyrbild av filmminiatyren.
1. Data som är användbara när det gäller marknadsföring och marknadsföring. Du kan till exempel visa filmens klassificering så att du inte rekommenderar NC-17-filmer.
1. Data som är användbara när du vill fastställa likheter mellan objekt och andra objekt. Du kan till exempel visa filmens genre och filmens regissör.

[!UICONTROL Target] erbjuder flera integreringsalternativ för att fylla i katalogen. Dessa alternativ kan användas tillsammans för att uppdatera olika objekt i katalogen eller för att uppdatera olika objektattribut för olika frekvenser.

| Metod | Vad det är | När ska den användas | Ytterligare information |
| --- | --- | --- | --- |
| Katalogfeed | Schemalägg en feed (CSV, Google Product XML eller Analytics Product Classifications) som ska överföras och importeras dagligen. | Om du vill skicka information om flera objekt samtidigt. För att skicka information som ändras sällan. | Se [Feeds](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html?lang=sv-SE). |
| Entiteter-API | Anropa ett API för att skicka uppdateringar som är aktuella för ett enskilt objekt. | För att skicka uppdateringar när de inträffar, ungefär ett objekt i taget. För att skicka information som ändras ofta (till exempel pris, lager/lagernivå). | Se [dokumentation för utvecklare av entiteter-API](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities). |
| Skicka uppdateringar på sidan | Skicka uppdateringarna till de allra senaste för ett enstaka objekt med JavaScript på sidan eller med hjälp av leverans-API:t. | För att skicka uppdateringar när de inträffar, ungefär ett objekt i taget. För att skicka information som ändras ofta (till exempel pris, lager/lagernivå). | Se [Artikelvyer/produktsidor](#item-views-or-product-pages) nedan. |

De flesta kunder bör implementera minst en feed. Du kan sedan välja att komplettera din feed med uppdateringar för ofta ändrade attribut eller objekt med hjälp av antingen Entity API eller on-the-page method.

## 3. Överföra beteendeinformation och sammanhang

Vilken beteendeinformation och vilket sammanhang du ska skicka till [!UICONTROL Target] beror på vilken åtgärd besökaren vidtar, vilket ofta är kopplat till vilken typ av sida besökaren interagerar med.

### Artikelvyer eller produktsidor

På sidor där en besökare visar ett enstaka objekt, t.ex. en produktinformationssida, bör du skicka identiteten för det objekt som besökaren visar. Du bör även skicka den mest detaljerade kategorin av objektet som besökaren visar, så att filterrekommendationer tillåts till den aktuella kategorin.

Du kan också skicka vissa attribut som snabbt ändras på själva produktsidan. Du kan till exempel skicka priset (`value`) och lager-/lagernivån.

#### Lösenpris och lager

```js {line-numbers="true"}
<script type="text/javascript">
function targetPageParams() { 
   return { 
      "entity": { 
         "id": "32323", 
         "categoryId": "running-shoes", 
         "value": 119.99, 
         "inventory": 329 
      } 
   } 
}
</script>
```

### Kategorivyer/kategorisidor

På en kategorisida vill du troligtvis begränsa dina rekommendationer till produkter eller innehåll i den kategorin. Om du vill göra det måste du skicka identiteten för den kategori som visas.

#### Godkänn aktuell kategori

```js {line-numbers="true"}
function targetPageParams() { 
   return { 
      "entity": { 
         "categoryId": "running-shoes" 
      } 
   } 
}
```

### Vyer/utcheckningssidor för kundvagn

På en kundvagnssida kan du rekommendera artiklar baserat på innehållet i besökarens aktuella kundvagn. Om du vill göra det skickar du ID:n för alla objekt i besökarens aktuella kundvagn med den särskilda parametern `cartIds`.

#### Godkänn artiklar som finns i varukorgen

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "cartIds": "352,223,23432,432,553"
      }
}
```

Mer information om Cart-baserade rekommendationer finns i [Cart-Based](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=sv-SE#cart-based) i *[!DNL Adobe Target]Business Practitioner Guide*.

### Uteslut artiklar som redan finns i besökarens kundvagn

På sidor på hela webbplatsen kan du utesluta vissa objekt från rekommendationerna. Du kanske inte vill rekommendera artiklar som redan finns i besökarens kundvagn. Om du vill göra det skickar du ID:n för alla objekt som du vill utesluta med den särskilda parametern `excludedIds`.

#### Skicka objekt som ska exkluderas

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "excludedIds": "352,223,23432,432,553"
      }
}
```

### Bekräftelsesidor för inköp/beställning

När en köphändelse inträffar skickar du identiteten för den eller de köpta artiklarna. Se [Spåra konverteringar](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions) i artikeln [Distribuera på.js > Implementera [!UICONTROL Target] utan tagghanterare](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md) .

## 4. Konfigurera globala undantag

Uteslut alla objekt på global nivå som du aldrig vill rekommendera en besökare. Se [Undantag](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/exclusions.html?lang=sv-SE) i *[!DNL Adobe Target]Business Practitioner-handboken*.

## 5. Konfigurera inställningarna för [!UICONTROL Recommendations]

Använd inställningar för att hantera implementeringen av [!UICONTROL Recommendations].

Om du vill komma åt alternativen för **[!UICONTROL Recommendations Settings]** öppnar du Mål i [!DNL Adobe Experience Cloud] och klickar sedan på **[!UICONTROL Recommendations]** > **[!UICONTROL Settings]**.

![Recommendations Settings page](/help/dev/implement/recommendations/assets/recs_settings.png)

Följande alternativ är tillgängliga:

| Inställning | Beskrivning |
|--- |--- |
| Anpassad global mbox | (Valfritt) Ange den anpassade globala mbox som används för att hantera [!UICONTROL Target]-aktiviteter. Som standard används den globala mbox som används av [!UICONTROL Target] för [!UICONTROL Recommendations].<P>Obs! Det här alternativet är inställt på sidan [!UICONTROL Target] **[!UICONTROL Administration]**. Öppna [!UICONTROL Target] och klicka sedan på **[!UICONTROL Administration]** > **[!UICONTROL Visual Experience Composer]**. |
| Branschvertikal | Branschvertikalen används för att kategorisera era era rekommendationer. Den här informationen hjälper teammedlemmarna att hitta kriterier som passar en viss sida, till exempel kriterier som passar bäst för kundvagnssidan eller för en mediesida. |
| Filtrera inkompatibla villkor | Aktivera det här alternativet om du bara vill visa de villkor där den valda sidan skickar de data som krävs. Alla villkor fungerar inte korrekt på alla sidor. Sidan eller mbox måste skickas `entity.id` eller `entity.categoryId` för att aktuella rekommendationer för objekt/aktuell kategori ska vara kompatibla. I allmänhet är det bäst att bara visa kompatibla villkor. Om du vill att inkompatibla villkor ska vara tillgängliga för aktiviteten avmarkerar du det här alternativet.<P>Du bör inaktivera det här alternativet om du använder en tagghanteringslösning.<P>Mer information om det här alternativet finns i [[!UICONTROL Recommendations] Vanliga frågor ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=sv-SE) i *[!DNL Adobe Target]Business Practitioner-handboken*. |
| Standardvärdgrupp | Välj din standardvärdgrupp.<P>Värdgruppen kan användas för att skilja de tillgängliga objekten i katalogen åt för olika användningsområden. Du kan till exempel använda värdgrupper för utvecklings- och produktionsmiljöer, olika varumärken eller olika geografiska platser. Som standard baseras förhandsgranskningsresultaten i Katalogsökning, Samlingar och Undantag på standardvärdgruppen. (Du kan också välja en annan värdgrupp om du vill förhandsgranska resultaten med hjälp av miljöfiltret.) Som standard är nyligen tillagda objekt tillgängliga i alla värdgrupper om inte ett miljö-ID anges när objektet skapas eller uppdateras. Levererade rekommendationer beror på värdgruppen som anges i begäran.<P>Om du inte ser dina produkter bör du kontrollera att du använder rätt värdgrupp. Om du t.ex. har konfigurerat din rekommendation att använda en mellanlagringsmiljö och du har angett mellanlagringsgruppen som värdgrupp kan du behöva återskapa dina samlingar i mellanlagringsmiljön för att produkterna ska kunna visas. Om du vill se vilka produkter som är tillgängliga i respektive miljö använder du Katalogsökning för varje miljö. Du kan också förhandsgranska innehållet i [!UICONTROL Recommendations] samlingar och undantag för en vald miljö (värdgrupp).<P>**Obs!** När du har ändrat den valda miljön måste du klicka på Sök för att uppdatera de returnerade resultaten.<P> Filtret **[!UICONTROL The Environment]** är tillgängligt från följande platser i målgränssnittet:<ul><li>Katalogsökning (**[!UICONTROL Recommendations]** > **[!UICONTROL Catalog Search]**)</li><li>Dialogrutan Skapa samling (**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Create New]**)</li><li>Dialogrutan Uppdatera samling (**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Edit]**)</li><li>Dialogrutan Skapa undantag (**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Create New]**)</li><li>Dialogrutan Uppdatera undantag (**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Edit]**)</li></ul>Mer information finns i [Värdar](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=sv-SE) i *[!DNL Adobe Target]Business Practitioner-handboken*. |
| Bas-URL för miniatyrbild | Genom att ange en bas-URL för din produktkatalog kan du använda relativa URL-adresser när du anger miniatyrbilder för dina produkter när du skickar en miniatyrbilds-URL.<P>Exempel:<P>`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`<P>anger en URL som är relativ till miniatyrbildens bas-URL. |
| API-token för [!UICONTROL Recommendations] | Använd den här token i [!UICONTROL Recommendations] API-anrop, till exempel Hämtnings-API. |

## 6. (Valfritt) Administrera [!UICONTROL Recommendations] med Admin API:er

Läs handboken för [Använd [!UICONTROL Recommendations] API:er](../../before-administer/recs-api/overview.md) om du vill veta mer om hur du konfigurerar och använder [!UICONTROL Target] admin- och leverans-API:er för [!UICONTROL Recommendations].
