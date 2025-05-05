---
title: Leverera personalisering med Adobe Target SDK
description: Lär dig leverera personalisering med [!UICONTROL on-device decisioning].
feature: APIs/SDKs
exl-id: bac64c78-0d3a-40d7-ae2b-afa0f1b8dc4f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# Leverera personalisering

## Sammanfattning av steg

1. Aktivera [!UICONTROL on-device decisioning] för din organisation
1. Skapa en [!UICONTROL Experience Targeting] (XT) aktivitet
1. Definiera personaliserade upplevelser per målgrupp
1. Verifiera personaliserade upplevelser per målgrupp
1. Ställ in rapportering
1. Lägg till mätvärden för att spåra KPI:er
1. Implementera personaliserade erbjudanden i din applikation
1. Implementera kod för att spåra konverteringshändelser
1. Aktivera din [!UICONTROL Experience Targeting] (XT)-personaliseringsaktivitet

Anta att du är ett turistföretag. Ni vill leverera ett personaliserat erbjudande om 25 % rabatt på vissa resepaket. För att erbjudandet ska få genklang hos era användare bestämmer du dig för att visa ett landmärke för målstaden. Ni vill också försäkra er om att leveransen av era personaliserade erbjudanden genomförs med nästan nolltidsfördröjning, så att den inte påverkar användarupplevelserna negativt och fördröjer resultatet.

## 1. Aktivera [!UICONTROL on-device decisioning] för din organisation

1. Aktivering av enhetsbeslut säkerställer att en A/B-aktivitet utförs vid nästan noll-fördröjning. Om du vill aktivera den här funktionen går du till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** i [!DNL Adobe Target] och aktiverar växlingsknappen **[!UICONTROL On-Device Decisioning]**.

   ![alt-bild](assets/asset-odd-toggle.png)

   >[!NOTE]
   >
   >Du måste ha administratörs- eller godkännarrollen [användare](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html?lang=sv-SE) för att aktivera eller inaktivera växlingsknappen [!UICONTROL On-Device Decisioning].

   När du har aktiverat växeln **[!UICONTROL On-Device Decisioning]** börjar [!DNL Adobe Target] generera *regelartefakter* för klienten.

## 2. Skapa en [!UICONTROL Experience Targeting] (XT) aktivitet

1. Gå till sidan **[!UICONTROL Activities]** i [!DNL Adobe Target] och välj sedan **[!UICONTROL Create Activity]** > **[!UICONTROL Experience Targeting]**.

   ![alt-bild](assets/asset-xt.png)

1. I modala **[!UICONTROL Create Experience Targeting Activity]** låter du standardalternativet **[!UICONTROL Web]** vara markerat (1), väljer **[!UICONTROL Form]** som din upplevelsedisposition (2), väljer en arbetsyta och egenskap (3) och klickar på **[!UICONTROL Next]** (4).

   ![alt-bild](assets/asset-xt-next.png)

## 3. Definiera en personaliserad upplevelse per målgrupp

1. Klicka på **[!UICONTROL Change Audience]** i steget **[!UICONTROL Experiences]** när du skapar aktiviteter för att skapa en målgrupp med de besökare som vill resa till San Francisco i Kalifornien.

   ![alt-bild](assets/asset-change-audience.png)

1. I **[!UICONTROL Create Audience]** modal definierar du en anpassad regel där `destinationCity = San Francisco`. Detta definierar den grupp användare som vill resa till San Francisco.

   ![alt-bild](assets/asset-audience-sf.png)

1. I steget **[!UICONTROL Experiences]** anger du namnet på platsen (1) i programmet där du vill visa ett specialerbjudande för Golden Gate Bridge, men bara för dem som är på väg till San Francisco. I exemplet som visas här är hemsidan den plats som valts för erbjudandet HTML (2), som definieras i området **[!UICONTROL Content]**.

   ![alt-bild](assets/asset-content-sf.png)

1. Lägg till ytterligare målgrupp genom att klicka på **[!UICONTROL Add Experience Targeting]**. Den här gången kan du rikta in dig på en målgrupp som vill resa till New York genom att definiera en målgruppsregel där `destinationCity = New York`. Definiera den plats i programmet där du vill visa ett specialerbjudande om Empire State Building. I exemplet som visas här är `homepage` den plats som valts för erbjudandet HTML (2), som definieras i området **[!UICONTROL Content]**.

   ![alt-bild](assets/asset-content-ny.png)

## 4. Verifiera personaliserade upplevelser per målgrupp

I steget **[!UICONTROL Targeting]** kontrollerar du att du har konfigurerat önskad personlig upplevelse per målgrupp.

![alt-bild](assets/asset-verify-sf-ny.png)

## 5. Ställ in rapportering

I steget **[!UICONTROL Goals & Settings]** väljer du **[!UICONTROL Adobe Target]** som **[!UICONTROL Reporting Source]** om du vill visa aktivitetsresultaten i användargränssnittet för [!DNL Adobe Target] eller **[!UICONTROL Adobe Analytics]** om du vill visa dem i användargränssnittet för Adobe Analytics.

![alt-bild](assets/asset-reporting-sf-ny.png)

## 6. Lägg till mätvärden för att spåra nyckeltal

Välj en **[!UICONTROL Goal Metric]** för att mäta om aktiviteten lyckades. I det här exemplet baseras en lyckad konvertering på om användaren klickar på det anpassade destinationserbjudandet.

## 7. Implementera dina personaliserade erbjudanden i din applikation

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {      
    execute: {
      pageLoad: {
        parameters: {
          destinationCity: "San Francisco"
        }
      }
    }       
  }
})
.then(console.log)
.catch(console.error);
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

RequestDetails pageLoad = new RequestDetails();
pageLoad.setParameters(
    new HashMap<String, String>() {
      {
        put("destinationCity", "San Francisco");
      }
    });

executeRequest.setPageLoad(pageLoad);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

## 8. Implementera kod för att spåra konverteringshändelser

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
            name : "destinationOffer"
          }
        }
      ]
    }
})
```

>[!TAB Java]

```java {line-numbers="true"
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

Context context = new Context().channel(ChannelType.WEB);

ExecuteRequest executeRequest = new ExecuteRequest();

RequestDetails pageLoad = new RequestDetails();
pageLoad.setParameters(
    new HashMap<String, String>() {
      {
        put("destinationCity", "San Francisco");
      }
    });

executeRequest.setPageLoad(pageLoad);
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
```

>[!ENDTABS]

## 9. Aktivera aktiviteten Experience Targeting (XT)

![alt-bild](assets/asset-xt-activate.png)
