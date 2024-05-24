---
keywords: Recommendations, inställningar, inställningar, branschvertikal, filterinkompatibla villkor, standardvärdgrupp, tumbas-URL, rekommendationer-API-token,
description: Lär dig implementera [!UICONTROL Recommendations] verksamhet i [!DNL Adobe Target].
title: Hur implementerar jag [!UICONTROL Recommendations] Verksamheter?
feature: Recommendations
hidefromtoc: true
hide: true
source-git-commit: 8d04fe249aafb5ee064dc614ae72f68b8f8459f2
workflow-type: tm+mt
source-wordcount: '1550'
ht-degree: 0%

---

# Planera och implementera [!UICONTROL Recommendations]

Information som kan hjälpa er att planera och implementera [!DNL Adobe Target Recommendations].

>[!NOTE]
>
>Förutom den här artikeln finns följande [Adobe Target Business Practitioner Guide](https://experienceleague.adobe.com/en/docs/target/using/target-home){target=_blank} innehåller detaljerad information om [Recommendations](https://experienceleague.adobe.com/en/docs/target/using/recommendations/recommendations){target=_blank}.

Innan du konfigurerar [!UICONTROL Recommendations] aktivitet i [!DNL Adobe Target]utför du följande steg:

1. [Implementera [!UICONTROL Target]](#implement-target) på de webbsidor och mobilappar som du vill använda för att fånga upp användarbeteenden och leverera rekommendationer.
1. [Konfigurera [!UICONTROL Recommendations] katalog](#set-up-your-recommendations-catalog) av produkter eller innehåll som du vill rekommendera dina användare.
1. [Överför beteendeinformation och sammanhang](#pass-behavioral-information-and-context) till [!DNL Target Recommendations] för att kunna leverera personaliserade rekommendationer.
1. [Konfigurera globala undantag](#configure-global-exclusions).
1. [Konfigurera [!UICONTROL Recommendations] inställningar](#configure-recommendations-settings).
1. (Valfritt) [Administrera [!UICONTROL Recommendations] med admin-API:er](#administer-recommendations-using-admin-apis).

## 1. Implementera [!UICONTROL Target]

[!DNL Target Recommendations] kräver att du implementerar [!DNL Adobe Experience Platform Web SDK] eller at.js 0.9.2 (eller senare). Se [[!UICONTROL Target] implementeringsguider på klientsidan](../client-side/overview.md) för mer information.

## 2. Konfigurera [!UICONTROL Recommendations] katalog

För att kunna leverera högkvalitativa rekommendationer [!UICONTROL Target] måste känna till vilka produkter eller vilket innehåll du vill rekommendera. Kataloger innehåller vanligtvis tre typer av information om rekommenderade objekt. Anta att du rekommenderar filmer. Inkludera följande:

1. Data som du vill visa för användaren som tar emot rekommendationen. Du kan till exempel visa namnet på filmen och en URL för en miniatyrbild av filmminiatyren.
1. Data som är användbara när det gäller marknadsföring och marknadsföring. Du kan till exempel visa filmens klassificering så att du inte rekommenderar NC-17-filmer.
1. Data som är användbara när du vill fastställa likheter mellan objekt och andra objekt. Du kan till exempel visa filmens genre och filmens regissör.

[!UICONTROL Target] har flera integreringsalternativ för att fylla i katalogen. Dessa alternativ kan användas tillsammans för att uppdatera olika objekt i katalogen eller för att uppdatera olika objektattribut för olika frekvenser.

| Metod | Vad det är | När ska den användas | Ytterligare information |
| --- | --- | --- | --- |
| Katalogfeed | Schemalägg en feed (CSV, [!DNL Google] Produkt-XML, eller [!UICONTROL Analytics Product Classifications]) som ska överföras och förtäras dagligen. | Om du vill skicka information om flera objekt samtidigt. För att skicka information som ändras sällan. | Se [Feeds](https://experienceleague.adobe.com/en/docs/target/using/recommendations/entities/feeds). |
| Entiteter-API | Anropa ett API för att skicka uppdateringar som är aktuella för ett enskilt objekt. | För att skicka uppdateringar när de inträffar, ungefär ett objekt i taget. För att skicka information som ändras ofta (till exempel pris, lager/lagernivå). | Se [Dokumentation för utvecklare av entitets-API](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities). |
| Skicka uppdateringar på sidan | Skicka uppdateringarna till de allra senaste för ett enskilt objekt med JavaScript på sidan eller med hjälp av leverans-API:t. | För att skicka uppdateringar när de inträffar, ungefär ett objekt i taget. För att skicka information som ändras ofta (till exempel pris, lager/lagernivå). | Se [Artikelvyer/produktsidor](#item-views-or-product-pages) nedan. |

De flesta kunder bör implementera minst en feed. Du kan sedan välja att komplettera din feed med uppdateringar för ofta ändrade attribut eller objekt med hjälp av antingen Entity API eller on-the-page method.

## 3. Överföra beteendeinformation och sammanhang

Beteendeinformation och sammanhang som du bör skicka till [!UICONTROL Target] beror på vad besökaren gör, vilket ofta är kopplat till vilken typ av sida besökaren interagerar med.

### Artikelvyer eller produktsidor

På sidor där en besökare visar ett enstaka objekt, t.ex. en produktinformationssida, bör du skicka identiteten för det objekt som besökaren visar. Skicka även den mest detaljerade kategorin av objektet som besökaren visar för att tillåta filtreringsrekommendationer till den aktuella kategorin.

Du kan också skicka vissa attribut som snabbt ändras på själva produktsidan. Du kan till exempel skicka priset (`value`) och lager/lagernivå.

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

På en kundvagnssida kan du rekommendera artiklar baserat på innehållet i besökarens aktuella kundvagn. Om du vill göra det skickar du ID:n för alla objekt i besökarens kundvagn med hjälp av den särskilda parametern `cartIds`.

#### Godkänn artiklar som finns i varukorgen

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "cartIds": "352,223,23432,432,553"
      }
}
```

Mer information om Cart-baserade rekommendationer finns i [Cart-baserad](https://experienceleague.adobe.com/en/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key#cart-based) i *[!DNL Adobe Target]Handbok för affärspersonalen*.

### Uteslut artiklar som redan finns i besökarens kundvagn

På sidor på hela webbplatsen kan du utesluta vissa objekt från rekommendationerna. Du kanske inte vill rekommendera artiklar som redan finns i besökarens kundvagn. Om du vill göra det skickar du ID:n för alla objekt som du vill utesluta med den speciella parametern `excludedIds`.

#### Skicka objekt som ska exkluderas

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "excludedIds": "352,223,23432,432,553"
      }
}
```

### Bekräftelsesidor för inköp/beställning

När en köphändelse inträffar skickar du identiteten för den eller de köpta artiklarna. Se [Spåra konverteringar](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions) i [Distribuera på.js > Implementera [!UICONTROL Target] utan tagghanterare](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md) artikel.

## 4. Konfigurera globala undantag

Uteslut alla objekt på global nivå som du aldrig vill rekommendera en besökare. Se [Undantag](https://experienceleague.adobe.com/en/docs/target/using/recommendations/entities/exclusions) i *[!DNL Adobe Target]Handbok för affärspersonalen*.

## 5. Konfigurera [!UICONTROL Recommendations] inställningar

Använd inställningarna för att hantera [!UICONTROL Recommendations] implementering.

Så här öppnar du **[!UICONTROL Recommendations Settings]** alternativ, öppna [!DNL Target] i [!DNL Adobe Experience Cloud]och sedan klicka **[!UICONTROL Administration]** > **[!UICONTROL Recommendations]**.

![Recommendations Settings page](/help/dev/implement/recommendations/assets/recs-settings-new.png)

Konfigurera följande alternativ:

### [!UICONTROL Recommendations API Token]

Följande alternativ är tillgängliga i [!UICONTROL Recommendations API Token] avsnitt:

#### [!UICONTROL Client code]

The [!DNL Target] [!UICONTROL client code].

Om du inte känner till dina [!UICONTROL client code], i [!DNL Target] användargränssnitt, klicka **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**. The [!UICONTROL client code] visas i [!UICONTROL Account Details] -avsnitt.

#### Autentiseringstoken

The [!DNL Adobe Target] Admin-API:er, inklusive [!DNL Recommendations Admin] API:er, skyddas av autentisering för att säkerställa att endast behöriga användare använder dem för åtkomst [!DNL Adobe Target]. Använd [Adobe Developer Console](https://developer.adobe.com/console/home) för att hantera denna autentisering för alla [!DNL Adobe Experience Cloud solutions], inklusive [!DNL Adobe Target].

Mer information finns i [Konfigurera autentisering för Adobe Target API:er](/help/dev/before-administer/configure-authentication.md).

>[!WARNING]
>
>Var försiktig när du genererar en ny autentiseringstoken. När en ny token genereras misslyckas API-anrop som använder den aktuella token. Uppdatera skript eller program med den nya genererade autentiseringstoken.

### Kriterier

Genom att känna till platsens vertikala struktur kan Target välja kriterier för era rekommendationer.

Kriterier i [!DNL Recommendations] är regler som bestämmer vilka produkter eller innehåll som ska rekommenderas utifrån en fördefinierad uppsättning besökarbeteenden. Kriterierna kan baseras på populära trender, en besökares aktuella och tidigare beteenden eller liknande produkter och innehåll. Du kan testa flera rekommendationstyper mot varandra genom att lägga till flera villkor.

Mer information finns i [Kriterier](https://experienceleague.adobe.com/en/docs/target/using/recommendations/criteria/algorithms){target=_blank} i *Adobe Target Business Practitioner Guide.*

Följande inställningar är tillgängliga i [!UICONTROL Criteria] avsnitt:

#### [!UICONTROL Industry/Vertical]

Branschvertikalen används för att kategorisera era era rekommendationer. Den här informationen hjälper teammedlemmarna att hitta kriterier som passar en viss sida, till exempel kriterier som passar bäst för kundvagnssidan eller för en mediesida.

Följande kategorier är tillgängliga i listrutan:

* Ingen personalisering
* Detaljhandel/e-handel
* Leadgenerering/B2B/Finansiella tjänster
* Media/publicering

#### [!UICONTROL Filter Incompatible Criteria]

Aktivera det här alternativet om du bara vill visa de villkor där den valda sidan skickar de data som krävs. Alla villkor fungerar inte korrekt på alla sidor. Sidan eller mbox måste skickas `entity.id` eller `entity.categoryId` för aktuell artikel/aktuella kategorirekommendationer att vara kompatibla.

I allmänhet är det bäst att bara visa kompatibla villkor. Om du vill att inkompatibla villkor ska vara tillgängliga för aktiviteten ska du inte aktivera det här alternativet.

Adobe rekommenderar att du inaktiverar det här alternativet om du använder en tagghanteringslösning.

Mer information om det här alternativet finns i [[!UICONTROL Recommendations] Vanliga frågor](https://experienceleague.adobe.com/en/docs/target/using/recommendations/recommendations-faq/recommendations-faq){target=_blank} i *[!DNL Adobe Target]Handbok för affärspersonalen*.

### [!UICONTROL Product Catalog]

Följande alternativ är tillgängliga i [!UICONTROL Product Catalog] avsnitt:

#### [!UICONTROL Default Host Group]

Välj din standardvärdgrupp.

Värdgruppen kan användas för att skilja de tillgängliga objekten i katalogen åt för olika användningsområden. Du kan till exempel använda värdgrupper för utvecklings- och produktionsmiljöer, olika varumärken eller olika geografiska platser. Som standard baseras förhandsgranskningsresultaten i Katalogsökning, Samlingar och Undantag på standardvärdgruppen. (Du kan också välja en annan värdgrupp om du vill förhandsgranska resultaten med hjälp av miljöfiltret.) Som standard är nyligen tillagda objekt tillgängliga i alla värdgrupper om inte ett miljö-ID anges när objektet skapas eller uppdateras. Levererade rekommendationer beror på värdgruppen som anges i begäran.

Om du inte ser dina produkter bör du kontrollera att du använder rätt värdgrupp. Om du t.ex. har konfigurerat din rekommendation att använda en mellanlagringsmiljö och du har angett mellanlagringsgruppen som värdgrupp kan du behöva återskapa dina samlingar i mellanlagringsmiljön för att produkterna ska kunna visas. Om du vill se vilka produkter som är tillgängliga i respektive miljö använder du Katalogsökning för varje miljö. Du kan även förhandsgranska innehållet i [!UICONTROL Recommendations] samlingar och undantag för en vald miljö (värdgrupp).

>[!NOTE]
>
>När du har ändrat den valda miljön måste du klicka **[!UICONTROL Search]** för att uppdatera de returnerade resultaten.

The **[!UICONTROL Environment]** -filtret är tillgängligt från följande platser i målgränssnittet:

* Katalogsökning (**[!UICONTROL Recommendations]** > **[!UICONTROL Catalog Search]**)
* Samlingar (**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]**)
* Dialogrutan Skapa samling (**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Create collection]**)
* Dialogrutan Uppdatera samling (**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Edit]**)
* Dialogrutan Skapa undantag (**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Create exclusion]**)
* Dialogrutan Uppdatera undantag (**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Edit]**)

Mer information finns i [Värdar](https://experienceleague.adobe.com/en/docs/target/using/administer/hosts){target=_blank} i *[!DNL Adobe Target]Handbok för affärspersonalen*.

#### [!UICONTROL Thumbnail Base]

Genom att ange en bas-URL för din produktkatalog kan du använda relativa URL-adresser när du anger miniatyrbilder för dina produkter när du skickar en miniatyrbilds-URL.

Exempel:

`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`

anger en URL som är relativ till miniatyrbildens bas-URL.

### [!UICONTROL Custom Attribute Key Configuration]

Basera dina rekommendationer på ett objekt som lagras i besökarens profil. Till exempel &quot;det senast tillagda objektet i kundvagnen&quot; eller &quot;den senaste videon visade 90 % eller mer&quot;.

Klicka **[!UICONTROL Add]** Om du vill skapa en ny konfiguration anger du ett namn för konfigurationen, markerar önskeprofilattributet och klickar sedan på **[!UICONTROL Save]**.

## 6. (Valfritt) Administrera [!UICONTROL Recommendations] med admin-API:er

Se [Använd [!UICONTROL Recommendations] API:er](../../before-administer/recs-api/overview.md) praktisk guide som lär dig konfigurera och använda [!UICONTROL Target] API:er för administration och leverans för [!UICONTROL Recommendations].