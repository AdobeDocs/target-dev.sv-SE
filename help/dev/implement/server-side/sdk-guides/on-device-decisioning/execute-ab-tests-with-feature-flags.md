---
title: Kör A/B-tester med funktionsflaggor och enhetsbeslut
description: Kör A/B-tester med funktionsflaggor genom att använda enhetsbeslut.
feature: APIs/SDKs
exl-id: abf66e00-742d-4d40-9b6e-9bd71638c31a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '712'
ht-degree: 0%

---

# Kör A/B-tester med funktionsflaggor

## Sammanfattning av steg

1. Aktivera [!UICONTROL on-device decisioning] för er organisation
1. Skapa en [!UICONTROL A/B Test] aktivitet
1. Definiera A och B
1. Lägg till en målgrupp
1. Ange trafikallokering
1. Ange trafikfördelning till variationer
1. Ställ in rapportering
1. Lägg till mätvärden för att spåra KPI:er
1. Implementera kod för att köra A/B-tester med funktionsflaggor
1. Aktivera A/B-testet med funktionsflaggor

>[!NOTE]
>
>Anta att du vill ta reda på om din hemsida kommer att bli bra tillgänglig för dina användare. Du bestämmer dig för att testa det genom att köra ett A/B-experiment i [!DNL Adobe Target]. Du bör också se till att experimentet levereras med höga prestanda så att en negativ eller långsam användarupplevelse inte förvränger resultatet.

## 1. Aktivera [!UICONTROL on-device decisioning] för er organisation

Aktivering av enhetsbeslut säkerställer att en A/B-aktivitet utförs vid nästan noll-fördröjning. Om du vill aktivera den här funktionen går du till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** in [!DNL Adobe Target]och aktivera **[!UICONTROL On-Device Decisioning]** växla.

&lt;!— Insert image-odd4.png —>
![alt-bild](assets/asset-odd-toggle.png)

>[!NOTE]
>
>Du måste ha administratören eller godkännaren [användarroll](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) om du vill aktivera eller inaktivera alternativet för beslut på enhet.

När du har aktiverat **[!UICONTROL On-Device Decisioning]** växla, [!DNL Adobe Target] börjar generera regelartefakter för klienten.

## 2. Skapa en [!UICONTROL A/B Test] aktivitet

I [!DNL Adobe Target], navigera till **[!UICONTROL Activities]** sida och sedan välja **[!UICONTROL Create Activity]** > **[!UICONTROL A/B test]**.

![alt-bild](assets/asset-ab.png)

I **[!UICONTROL Create A/B Test Activity]** modal, lämna standardvärdet **[!UICONTROL Web]** markerat alternativ (1), markera **[!UICONTROL Form]** som upplevelsedisposition (2) väljer du **[!UICONTROL Default Workspace]** med Nej **[!UICONTROL Property Restrictions]** (3) och klicka **[!UICONTROL Next]** (4)

![alt-bild](assets/asset-form.png)

## 3. Definiera A och B

1. I **[!UICONTROL Experiences]** skapa aktivitet, ange ett namn för aktiviteten (1) och lägg till ytterligare en upplevelse, Experience B, genom att klicka på **[!UICONTROL Add Experience]** (2). Ange namnet på den plats (3) i programmet där du vill köra A/B-testet. I exemplet nedan är hemsidan den plats som definieras för Experience A. (Det är också den plats som definieras för Experience B.)

   Experience A definierar kontrollen, som är den aktuella webbplatsdesignen.

   ![alt-bild](assets/asset-exp-a.png)

   Experience B definierar utmanaren, som kommer att representera en omdesignad hemsida. Klicka för att ändra standardinnehåll (1).

   ![alt-bild](assets/asset-exp-b.png)

1. I Experience B klickar du för att ändra innehållet från **[!UICONTROL Default Content]** till det omdesignade innehållet genom att välja **[!UICONTROL Create JSON Offer]** som visas nedan (1).

   ![alt-bild](assets/asset-offer.png)

1. Definiera JSON med attribut som kommer att användas som flaggor för att göra det möjligt för din affärslogik att återge den nyligen omdesignade hemsidan, i stället för den aktuella hemsidan i produktionen.


   >[!NOTE]
   >
   >När [!DNL Adobe Target] används för att användaren ska kunna se Experience B (den omdesignade hemsidan), returneras JSON med de attribut som definieras i exemplet. I koden måste du kontrollera attributvärdena för att avgöra om affärslogiken ska köras för att återge den omdesignade startsidan. Du kan definiera namn, värden och antal attribut i det här JSON-svaret.

   ![alt-bild](assets/asset-homepage.png)

## 4. Lägg till en målgrupp

Anta att du först vill testa designen av dina lojala kunder, som du kan identifiera baserat på om de är inloggade eller inte.

1. I **[!UICONTROL Targeting]** steg, klicka för att ersätta **[!UICONTROL All Visitors]** målgrupp, vilket visas.

   ![alt-bild](assets/asset-all-audiences.png)

1. I **[!UICONTROL Create Audience]** modal, definiera en anpassad regel där `logged-in = true`. Detta definierar gruppen med användare som är inloggade. Använd den här målgruppen i din aktivitet.

   ![alt-bild](assets/asset-audience.png)

## 5. Ange trafikallokering

Ange hur många procent av de inloggade användarna som du vill testa den nya omdesignen av hemsidan mot. Med andra ord, hur stor procentandel av användarna vill du använda det här testet? Om du i det här exemplet vill distribuera det här testet till alla inloggade användare ska du behålla trafikallokeringen på 100 %.

![alt-bild](assets/asset-allocation.png)

## 6. Ange trafikfördelning till variationer

Ange hur många procent av dina inloggade användare som ska se hemsidans aktuella design eller den helt nya designen. I det här exemplet behåller du trafikfördelningen som en 50/50-delning mellan upplevelserna A och B.

![alt-bild](assets/asset-traffic-distribution.png)

## 7. Ställ in rapportering

I **[!UICONTROL Goals & Settings]** steg, välja **[!UICONTROL Adobe Target]** som **[!UICONTROL Reporting Source]** för att visa aktivitetsresultat i [!DNL Adobe Target] Användargränssnitt, eller välj **[!UICONTROL Adobe Analytics]** för att visa dem i Adobe Analytics användargränssnitt.

![alt-bild](assets/asset-reporting.png)

## 8. Lägg till mätvärden för att spåra nyckeltal

Välj en **[!UICONTROL Goal Metric]** för att mäta A/B-testet. I det här exemplet baseras en lyckad konvertering på om användaren når sidans nederkant, vilket anger engagemang. Därför bör **[!UICONTROL Conversion]** bestäms utifrån om användaren har visat platsen längst ned på sidan.

## 9. Implementera kod för att köra A/B-tester med funktionsflaggor i programmet

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
  return targetClient.getAttributes(["homepage"]).then(function(attributes) {
    const flag = attributes.getValue("homepage", "feature-flag");
    // ...
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
MboxRequest mbox = new MboxRequest().name("homepage").index(0);
TargetDeliveryRequest request = TargetDeliveryRequest.builder()
    .context(new Context().channel(ChannelType.WEB))
    .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
    .build();
Attributes attributes = targetClient.getAttributes(request, "homepage");
String flag = attributes.getString("homepage", "feature-flag");
```

>[!ENDTABS]

## 10. Aktivera A/B-testet med funktionsflagga

![alt-bild](assets/asset-activate.png)
