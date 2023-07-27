---
title: Komma igång med mål-SDK:er
description: Hur använder jag Adobe Target SDK:er?
feature: APIs/SDKs
exl-id: a5ae9826-7bb5-41de-8796-76edc4f5b281
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Komma igång med [!DNL Target] SDK

För att komma igång rekommenderar vi att du skapar din första [beslut på enheten](../on-device-decisioning/overview.md) aktivitetsflaggan på det språk du väljer:

* Node.js
* Java
* .NET
* Python

## Sammanfattning av steg

1. Aktivera beslut på enheten för din organisation
1. Installera SDK
1. Initiera SDK
1. Konfigurera funktionsflaggor i en [!DNL Adobe Target] [!UICONTROL A/B Test] aktivitet
1. Implementera och återge funktionen i ditt program
1. Implementera spårning för händelser i ditt program
1. Aktivera [!UICONTROL A/B Test] aktivitet

## 1. Aktivera beslut på enheten för din organisation

Genom att aktivera beslut på enheten säkerställs att [!UICONTROL A/B Test] -aktiviteten utförs vid latens nära noll. Om du vill aktivera den här funktionen går du till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** och aktivera **[!UICONTROL On-Device Decisioning]** växla.

![alt-bild](assets/asset-odd-toggle.png)

>[!NOTE]
>
>Du måste ha **[!UICONTROL Admin]** eller **[!UICONTROL Approver]** [användarroll](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) för att aktivera eller inaktivera **[!UICONTROL On-Device Decisioning]** växla.

När du har aktiverat **[!UICONTROL On-Device Decisioning]** växla, [!DNL Adobe Target] börjar generera [regelartefakter](../on-device-decisioning/rule-artifact-overview.md) för kunden.

## 2. Installera SDK

För Node.js, Java och Python kör du följande kommando i din projektkatalog i terminalen. För .NET lägger du till det som ett beroende av [installera från NuGet](https://www.nuget.org/packages/Adobe.Target.Client).

>[!BEGINTABS]

>[!TAB Node.js (NPM)]

```js {line-numbers="true"}
npm i @adobe/target-nodejs-sdk -P
```

>[!TAB Java (Maven)]

```javascript {line-numbers="true"}
<dependency>
   <groupId>com.adobe.target</groupId>
   <artifactId>java-sdk</artifactId>
   <version>2.0</version>
</dependency>
```

>[!TAB .NET (Bash)]

```bash {line-numbers="true"}
dotnet add package Adobe.Target.Client
```

>[!TAB Python (pip)]

```python {line-numbers="true"}
pip install target-python-sdk
```

>[!ENDTABS]

## 3. Initiera SDK

Regelartefakten hämtas under SDK-initieringssteget. Du kan anpassa initieringssteget för att avgöra hur artefakten hämtas och används.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
   client: "<your target client code>",
   organizationId: "your EC org id",
   decisioningMethod: "on-device",
   events: {
      clientReady: targetClientReady
      }
};

const tClient = TargetClient.create(CONFIG);

function targetClientReady() {
   //Adobe Target SDK has now downloaded the JSON artifact locally, which contains the activity details.
   //We will see how to use the artifact here very soon.
}
```

>[!TAB Java (Maven)]

```javascript {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
   .client("testClient")
   .organizationId("ABCDEF012345677890ABCDEF0@AdobeOrg")
   .build();
TargetClient targetClient = TargetClient.create(config);
```

>[!TAB .NET (C#)]

```csharp {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("testClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
   .Build();
this.targetClient.Initialize(targetClientConfig);
```

>[!TAB Python]

```python {line-numbers="true"}
from target_python_sdk import TargetClient

def target_client_ready():
   # Adobe Target SDK has now downloaded the JSON artifact locally, which contains the activity details.
   # We will see how to use the artifact here very soon.

CONFIG = {
   "client": "<your target client code>",
   "organization_id": "your EC org id",
   "decisioning_method": "on-device",
   "events": {
      "client_ready": target_client_ready
   }
}

target_client = TargetClient.create(CONFIG)
```

>[!ENDTABS]

## 4. Konfigurera funktionsflaggorna i en [!DNL Adobe Target] [!UICONTROL A/B Test] aktivitet

1. I [!DNL Target], navigera till **[!UICONTROL Activities]** sida och sedan välja **[!UICONTROL Create Activity]** > **[!UICONTROL A/B test]**.

   ![alt-bild](assets/asset-ab.png)

1. I **[!UICONTROL Create A/B Test Activity]** modal, lämna standardalternativet för webben markerat (1), välj **[!UICONTROL Form]** som upplevelsedisposition (2) väljer du **[!UICONTROL Default Workspace]** med **[!UICONTROL No Property Restrictions]**(3) och sedan klicka på **[!UICONTROL Next]** (4)

   ![alt-bild](assets/asset-form.png)

1. I **[!UICONTROL Experiences]** skapa aktivitet, ange ett namn för aktiviteten (1) och lägg till ytterligare en upplevelse, Experience B, genom att klicka på **[!UICONTROL Add Experience]** (2) Ange platsnamnet (3). Till exempel: `ondevice-featureflag` eller `homepage-addtocart-featureflag` är platsnamn som anger mål för funktionsflaggstestning.  I exemplet nedan `ondevice-featureflag` är den plats som definieras för Experience B. Om du vill kan du lägga till Audience Refinements (4) för att begränsa behörigheten för aktiviteten.

   ![alt-bild](assets/asset-location.png)

1. I **[!UICONTROL CONTENT]** på samma sida väljer **[!UICONTROL Create JSON Offer]** i listrutan (1) enligt bilden.

   ![alt-bild](assets/asset-offer.png)

1. I **[!UICONTROL JSON Data]** textruta som visas anger du variabler för funktionsflaggan för varje upplevelse (1) med ett giltigt JSON-objekt (2).

   Ange funktionens flaggvariabler för Experience A.

   ![alt-bild](assets/asset-json_a.png)

   **(Exempel på JSON för upplevelse A ovan)**

   ```json {line-numbers="true"}
   {
      "enabled" : true,
      "flag" : "expA"
   }
   ```

   Ange funktionens flaggvariabler för Experience B.

   ![alt-bild](assets/asset-json_b.png)

   **(Exempel-JSON för upplevelse B ovan)**

   ```json {line-numbers="true"}
   {
      "enabled" : true,
      "flag" : "expB"
   }
   ```

1. Klicka **[!UICONTROL Next]** (1) att gå vidare till **[!UICONTROL Targeting]** steg för att skapa aktiviteter.

   ![alt-bild](assets/asset-next_2_t.png)

1. I **[!UICONTROL Targeting]** som visas nedan är målgruppsanpassning (2) fortfarande standardinställningen för alla besökare, vilket förenklar arbetet. Det innebär att aktiviteten inte är målinriktad. Observera dock att Adobe alltid rekommenderar att ni riktar in er på era målgrupper för produktionsaktiviteter. Klicka **[!UICONTROL Next]** (3) att gå vidare till **[!UICONTROL Goals & Settings]** steg för att skapa aktiviteter.

   ![alt-bild](assets/asset-next_2_g.png)

1. I **[!UICONTROL Goals & Settings]** steg, ange **[!UICONTROL Reporting Source]** till **[!UICONTROL Adobe Target]** (1) Definiera **[!UICONTROL Goal Metric]** as **[!UICONTROL Conversion]**, och specificera detaljerna baserat på webbplatsens konverteringsmått (2). Klicka **[!UICONTROL Save & Close]** (3) för att spara aktiviteten.

   ![alt-bild](assets/asset-conv.png)

## 5. Implementera och återge funktionen i ditt program

När du har konfigurerat variablerna för funktionsflagga i [!DNL Target], ändra programkoden så att den används. När du har hämtat funktionsflaggan i programmet kan du till exempel använda den för att aktivera funktioner och återge den upplevelse som besökaren är kvalificerad för.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity
​
let featureFlags = {};
​
function targetClientReady() {
   tClient.getAttributes(["ondevice-featureflag"]).then(function(response) {
      const featureFlags = response.asObject("ondevice-featureflag");
      if(featureFlags.enabled && featureFlags.flag !== "expA") { //Assuming "expA" is control
         console.log("Render alternate experience" + featureFlags.flag);
      }
      else {
         console.log("Render default experience");
      }
   });
}
```

>[!TAB Java (Maven)]

```javascript {line-numbers="true"}
MboxRequest mbox = new MboxRequest().name("ondevice-featureflag").index(0);
TargetDeliveryRequest request = TargetDeliveryRequest.builder()
   .context(new Context().channel(ChannelType.WEB))
   .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
   .build();
Attributes attributes = targetClient.getAttributes(request, "ondevice-featureflag");
String flag = attributes.getString("ondevice-featureflag", "flag");
```

>[!TAB .NET (C#)]

```csharp {line-numbers="true"}
var mbox = new MboxRequest(index: 0, name: "ondevice-featureflag");
var deliveryRequest = new TargetDeliveryRequest.Builder()
   .SetContext(new Context(ChannelType.Web))
   .SetExecute(new ExecuteRequest(mboxes: new List<MboxRequest> { mbox }))
   .Build();
var attributes = targetClient.GetAttributes(request, "ondevice-featureflag");
var flag = attributes.GetString("ondevice-featureflag", "flag");
```

>[!TAB Python]

```python {line-numbers="true"}
# ... Code removed for brevity

feature_flags = {}

def target_client_ready():
   attribute_provider = target_client.get_attributes(["ondevice-featureflag"])
   feature_flags = attribute_provider.as_object(mbox_name="ondevice-featureflag")
   if feature_flags.get("enabled") and feature_flags.get("flag") != "expA": # Assuming "expA" is control
      print("Render alternate experience {}".format(feature_flags.get("flag")))
   else:
      print("Render default experience")
```

>[!ENDTABS]

## 6. Implementera ytterligare spårning för händelser i programmet

Du kan också skicka ytterligare händelser för att spåra konverteringar med funktionen sendNotification().

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity
​
//When a conversion happens
TargetClient.sendNotifications({
   targetCookie,
   "request" : {
      "notifications" : [
      {
         type: "display",
         timestamp : Date.now(),
         id: "conversion",
         mbox : {
            name : "orderConfirm"
         },
         order : {
            id: "BR9389",
            total : 98.93,
            purchasedProductIds : ["J9393", "3DJJ3"]
         }
      }
      ]
   }
})
```

>[!TAB Java (Maven)]

```javascript {line-numbers="true"}
Notification notification = new Notification();
notification.setId("conversion");
notification.setImpressionId(UUID.randomUUID().toString());
notification.setType(MetricType.DISPLAY);
notification.setTimestamp(System.currentTimeMillis());
Order order = new Order("BR9389");
order.total(98.93);
order.purchasedProductIds(["J9393", "3DJJ3"]);
notification.setOrder(order);

TargetDeliveryRequest notificationRequest =
   TargetDeliveryRequest.builder()
      .context(new Context().channel(ChannelType.WEB))
      .notifications(Collections.singletonList(notification))
      .build();

NotificationDeliveryService notificationDeliveryService = new NotificationDeliveryService();
notificationDeliveryService.sendNotification(notificationRequest);
```

>[!TAB .NET (C#)]

```csharp {line-numbers="true"}
var order = new Order
{
   Id = "BR9389",
   Total = 98.93M,
   PurchasedProductIds = new List<string> { "J9393", "3DJJ3" },
};
​
var notification = new Notification
{
   Id = "conversion",
   ImpressionId = Guid.NewGuid().ToString(),
   Type = MetricType.Display,
   Timestamp = DateTimeOffset.UtcNow.ToUnixTimeMilliseconds(),
   Order = order,
};
​
var notificationRequest = new TargetDeliveryRequest.Builder()
   .SetContext(new Context(ChannelType.Web))
   .SetNotifications(new List<Notification> {notification})
   .Build();
​
targetClient.SendNotifications(notificationRequest);
```

>[!TAB Python]

```python {line-numbers="true"}
# ... Code removed for brevity

# When a conversion happens
notification_mbox = NotificationMbox(name="orderConfirm")
order = Order(id="BR9389, total=98.93, purchased_product_ids=["J9393", "3DJJ3"])
notification = Notification(
   id="conversion",
   type=MetricType.DISPLAY,
   timestamp=1621530726000,  # Epoch time in milliseconds
   mbox=notification_mbox,
   order=order
)
notification_request = DeliveryRequest(notifications=[notification])


target_client.send_notifications({
   "target_cookie": target_cookie,
   "request" : notification_request
})
```

>[!ENDTABS]

## 7. Aktivera [!UICONTROL A/B Test] aktivitet

1. Klicka **[!UICONTROL Activate]** (1) aktivera [!UICONTROL A/B Test] aktivitet.

   >[!NOTE]
   >
   >Du måste ha **[!UICONTROL Approver]** eller **[!UICONTROL Publisher]** [användarroll](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) för att utföra det här steget.

   ![alt-bild](assets/asset-activate.png)
