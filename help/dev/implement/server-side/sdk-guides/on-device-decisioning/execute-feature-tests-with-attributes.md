---
title: Utför funktionstester med attribut
description: Utför funktionstester med attribut
feature: APIs/SDKs
exl-id: c89d337c-20a9-454c-930c-79d9217e23b6
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '877'
ht-degree: 0%

---

# Utför funktionstester med attribut

## Sammanfattning av steg

1. Aktivera [!UICONTROL on-device decisioning] för din organisation
1. Skapa en [!UICONTROL A/B Test]-aktivitet
1. Definiera A och B
1. Lägg till en målgrupp
1. Ange trafikallokering
1. Ange trafikfördelning till variationer
1. Ställ in rapportering
1. Lägg till mätvärden för att spåra KPI:er
1. Implementera kod för att köra funktionstester med attribut
1. Implementera kod för att spåra konverteringshändelser
1. Aktivera funktionstester med attribut

>[!NOTE]
>
>Anta att du är ett e-handelsföretag inom detaljhandeln. Du vill öka konverteringsgraden när kunderna bläddrar och sorterar i produktkatalogen. Du har en hypotes om att vissa sorteringsalgoritmer och pagineringsstrategier ger bättre resultat än andra. Om du vill testa den här teorin bestämmer du dig för att köra ett funktionstest som innebär att sorteringswidgeten designas om med olika sorteringsalternativ för slutanvändarna. Du vill vara säker på att det här funktionstestet utförs med nästan noll fördröjning så att det inte påverkar användarupplevelserna negativt och skevar resultatet.

## 1. Aktivera [!UICONTROL on-device decisioning] för din organisation

Aktivering av enhetsbeslut säkerställer att en A/B-aktivitet utförs vid nästan noll-fördröjning. Om du vill aktivera den här funktionen går du till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** i [!DNL Adobe Target] och aktiverar växlingsknappen **[!UICONTROL On-Device Decisioning]**.

![alt-bild](assets/asset-odd-toggle.png)

>[!NOTE]
>
>Du måste ha administratörs- eller godkännarrollen [användare](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) för att aktivera eller inaktivera växlingsknappen **[!UICONTROL On-Device Decisioning]**.

När du har aktiverat växeln **[!UICONTROL On-Device Decisioning]** börjar [!DNL Adobe Target] generera *regelartefakter* för klienten.

## 2. Skapa en [!UICONTROL A/B Test]-aktivitet

1. Gå till sidan **[!UICONTROL Activities]** i [!DNL Adobe Target] och välj sedan **[!UICONTROL Create Activity]** > **[!UICONTROL A/B test]**.

   ![alt-bild](assets/asset-ab.png)

1. I **[!UICONTROL Create A/B Test Activity]** modal låter du standardalternativet **[!UICONTROL Web]** vara markerat (1), väljer **[!UICONTROL Form]** som din upplevelsedisposition (2), väljer **[!UICONTROL Default Workspace]** med **[!UICONTROL No Property Restrictions]** (3) och klickar på **[!UICONTROL Next]** (4).

   ![alt-bild](assets/asset-form.png)

## 3. Definiera A och B

1. I steget **[!UICONTROL Experiences]** när du skapar en aktivitet anger du ett namn för aktiviteten (1) och lägger till en andra upplevelse, Experience B, genom att klicka på knappen **[!UICONTROL Add Experience]** (2). Ange namnet på den plats (3) i programmet där du vill köra funktionstestet med attribut. I exemplet nedan är `product-results-page` den plats som definieras för Experience A. (Det är också den plats som definieras för Experience B.)

   ![alt-bild](assets/asset-location.png)

   **[!UICONTROL Experience A]** innehåller JSON som signalerar till din affärslogik att göra följande:

   * Initiera sorteringsalgoritmfunktionen via funktionsflaggan `test_sorting`
   * Kör den rekommenderade sorteringsalgoritmen som definierats i `sorting_algorithm _**_attribute`
   * Returnera 50 produkter per sida enligt den sidnumreringsstrategi som definieras i `pagination_limit`

1. I upplevelse A klickar du för att ändra innehållet från **[!UICONTROL Default Content]** till JSON genom att välja **[!UICONTROL Create JSON Offer]** enligt nedan (1).

   ![alt-bild](assets/asset-offer.png)

1. Definiera JSON med flaggorna `test_sorting`, `sorting_algorithm` och `pagination_limit` och attribut som ska användas för att initiera den rekommenderade sorteringsalgoritmen med en pagineringsgräns på 50 produkter.

   >[!NOTE]
   >
   >När [!DNL Adobe Target] blockerar en användare för att se upplevelse A, returneras JSON med de definierade attributen i exemplet. I koden måste du kontrollera värdet för funktionsflaggan `test_sorting` för att se om sorteringsfunktionen ska vara aktiverad. I så fall använder du det rekommenderade värdet för attributet `sorting_algorithm` för att visa rekommenderade produkter i produktlistvyn. Gränsen för produkter som ska visas för ditt program är 50, eftersom det är värdet för attributet `pagination_limit`.

   ![alt-bild](assets/asset-sorting.png)

   **[!UICONTROL Experience B]** definierar den JSON som signalerar till din affärslogik att göra följande:

   * Initiera sorteringsalgoritmfunktionen via flaggan test_sorting feature
   * Kör sorteringsalgoritmen `best_sellers` som definierats i `sorting_algorithm _**_attribute`
   * Returnera 50 produkter per sida enligt den sidnumreringsstrategi som definieras i `pagination_limit`

   >[!NOTE]
   >
   >När [!DNL Adobe Target] blockerar en användare för att se Experience B, returneras JSON med de definierade attributen i exemplet. I koden måste du kontrollera värdet för funktionsflaggan `test_sorting` för att se om sorteringsfunktionen ska vara aktiverad. I så fall använder du värdet `best_sellers` för attributet `sorting_algorithm` för att visa de bästsäljande produkterna i produktlistvyn. Gränsen för produkter som ska visas för ditt program är 50, eftersom det är värdet för attributet `pagination_limit`.

   ![alt-bild](assets/asset-sorting-b.png)

## 4. Lägg till en målgrupp

Behåll målgruppen **[!UICONTROL All Visitors]** i steget **[!UICONTROL Targeting]**. På så sätt kan du förstå vilken effekt sorteringsfunktionen har, liksom vilken algoritm och vilket antal objekt som bäst påverkar resultatet.

![alt-bild](assets/asset-audience-b.png)

## 5. Ange trafikallokering

Definiera den procentandel av besökarna som du vill testa sorteringsalgoritmerna och sidnumreringsstrategin mot. Med andra ord, hur stor procentandel av användarna vill du använda det här testet? Om du i det här exemplet vill distribuera det här testet till alla inloggade användare ska du behålla trafikallokeringen på 100 %.

![alt-bild](assets/asset-allocation-100.png)

## 6. Ange trafikfördelning till variationer

Definiera den procentandel av besökarna som ska se den rekommenderade jämfört med den bästa sorteringsalgoritmen för säljare, med en begränsning på 50 produkter per sida. I det här exemplet behåller du trafikfördelningen som en 50/50-delning mellan upplevelserna A och B.

![alt-bild](assets/asset-variations-50.png)

## 7. Ställ in rapportering

I steget **[!UICONTROL Goals & Settings]** väljer du **[!UICONTROL Adobe Target]** som **[!UICONTROL Reporting Source]** för att visa dina A/B-testresultat i användargränssnittet för [!DNL Adobe Target] eller **[!UICONTROL Adobe Analytics]** för att visa dem i användargränssnittet för Adobe Analytics.

![alt-bild](assets/asset-reporting-b.png)

## 8. Lägg till mätvärden för att spåra nyckeltal

Välj en **[!UICONTROL Goal Metric]** för att mäta funktionstestet med attribut. I det här exemplet baseras framgången på om användaren köper en produkt, beroende på den sorteringsalgoritm och pagineringsstrategi som de visades.

## 9. Implementera funktionstester med attribut i programmet

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const options = {
  client: "testClient",
  organizationId: "ABCDEF012345677890ABCDEF0@AdobeOrg",
  decisioningMethod: "on-device",
  events: {
    clientReady: targetClientReady
  }
};
const targetClient = TargetClient.create(options);

function targetClientReady() {
  return targetClient.getAttributes(["product-results-page"]).then(function(attributes) {
    const test_sorting = attributes.getValue("product-results-page", "test-sorting");
    const sorting_algorithm = attributes.getValue("product-results-page", "sorting_algorithm");
    const pagination_limit = attributes.getValue("product-results-page", "pagination_limit");
  });
}
```

>[!TAB Java]

```java {line-numbers="true"}
import com.adobe.target.edge.client.ClientConfig;
import com.adobe.target.edge.client.TargetClient;
import com.adobe.target.delivery.v1.model.ChannelType;
import com.adobe.target.delivery.v1.model.Context;
import com.adobe.target.delivery.v1.model.ExecuteRequest;
import com.adobe.target.delivery.v1.model.MboxRequest;
import com.adobe.target.edge.client.entities.TargetDeliveryRequest;
import com.adobe.target.edge.client.model.TargetDeliveryResponse;

ClientConfig config = ClientConfig.builder()
    .client("testClient")
    .organizationId("ABCDEF012345677890ABCDEF0@AdobeOrg")
    .build();
TargetClient targetClient = TargetClient.create(config);
MboxRequest mbox = new MboxRequest().name("product-results-page").index(0);
TargetDeliveryRequest request = TargetDeliveryRequest.builder()
    .context(new Context().channel(ChannelType.WEB))
    .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
    .build();
Attributes attributes = targetClient.getAttributes(request, "product-results-page");
String testSorting = attributes.getString("product-results-page", "test-sorting");
String sortingAlgorithm = attributes.getString("product-results-page", "sorting_algorithm");
String paginationLimit = attributes.getString("product-results-page", "pagination_limit");
```

>[!ENDTABS]

## 10. Implementera kod för att spåra konverteringshändelser

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity

//When a conversion happens
TargetClient.sendNotifications({
    targetCookie,
    "request" : {
      "notifications" : [
        {
          type: "click",
          timestamp : Date.now(),
          id: "conversion",
          mbox : {
            name : "product-results-page"
          }
        }
      ]
    }
})
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

Context context = new Context().channel(ChannelType.WEB);

ExecuteRequest executeRequest = new ExecuteRequest();

NotificationDeliveryService notificationDeliveryService = new NotificationDeliveryService();

Notification notification = new Notification();
notification.setId("conversion");
notification.setImpressionId(UUID.randomUUID().toString());
notification.setType(MetricType.CLICK);
notification.setTimestamp(System.currentTimeMillis());
notification.setTokens(
    Collections.singletonList(
        "IbG2Jz2xmHaqX7Ml/YRxRGqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="));

TargetDeliveryRequest targetDeliveryRequest =
    TargetDeliveryRequest.builder()
        .context(context)
        .execute(executeRequest)
        .notifications(Collections.singletonList(notification))
        .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
notificationDeliveryService.sendNotification(request);

Attributes attributes = targetClient.getAttributes(request, "product-results-page");
String testSorting = attributes.getString("product-results-page", "test-sorting");
String sortingAlgorithm = attributes.getString("product-results-page", "sorting_algorithm");
String paginationLimit = attributes.getString("product-results-page", "pagination_limit");
```

>[!ENDTABS]

## 11. Aktivera funktionstester med attribut

![alt-bild](assets/asset-activate.png)
