---
title: Hantera utrullningar för funktionstester
description: Lär dig hur du hanterar rollouts för funktionstester med [!UICONTROL on-device decisioning].
feature: APIs/SDKs
exl-id: caa91728-6ac0-4583-a594-0c8fe616342d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# Hantera utrullningar för funktionstester

## Sammanfattning av steg

1. Aktivera [!UICONTROL on-device decisioning] för din organisation
1. Skapa en [!UICONTROL A/B Test]-aktivitet
1. Definiera funktioner och rollout-inställningar
1. Implementera och återge funktionen i ditt program
1. Implementera spårning för händelser i ditt program
1. Aktivera din A/B-aktivitet
1. Justera utrullning och trafikallokering efter behov

## 1. Aktivera [!UICONTROL on-device decisioning] för din organisation

Aktivering av enhetsbeslut säkerställer att en A/B-aktivitet utförs vid nästan noll-fördröjning. Om du vill aktivera den här funktionen går du till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** i [!DNL Adobe Target] och aktiverar växlingsknappen **[!UICONTROL On-Device Decisioning]**.

![alt-bild](assets/asset-odd-toggle.png)

>[!NOTE]
>
>Du måste ha administratörs- eller godkännarrollen [användare](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html?lang=sv-SE) för att aktivera eller inaktivera växlingsknappen [!UICONTROL On-Device Decisioning].

När du har aktiverat växeln [!UICONTROL On-Device Decisioning] börjar [!DNL Adobe Target] generera *regelartefakter* för klienten.

## 2. Skapa en [!UICONTROL A/B Test]-aktivitet

1. Gå till sidan **[!UICONTROL Activities]** i [!DNL Adobe Target] och välj sedan **[!UICONTROL Create Activity]** > **[!UICONTROL A/B test]**.

   ![alt-bild](assets/asset-ab.png)

1. I **[!UICONTROL Create A/B Test Activity]** modal låter du standardalternativet **[!UICONTROL Web]** vara markerat (1), väljer **[!UICONTROL Form]** som din upplevelsedisposition (2), väljer **[!UICONTROL Default Workspace]** med **[!UICONTROL No Property Restrictions]** (3) och klickar på **[!UICONTROL Next]** (4).

   ![alt-bild](assets/asset-form.png)

## 3. Definiera funktioner och rollout-inställningar

Ange ett namn för aktiviteten (1) i steget **[!UICONTROL Experiences]** när aktiviteten skapas. Ange namnet på den plats (2) i programmet där du vill hantera rollouter för funktionen. `ondevice-rollout` eller `homepage-addtocart-rollout` är till exempel platsnamn som anger mål för hantering av rollout för funktioner. I exemplet nedan är `ondevice-rollout` den plats som definieras för Experience A. Du kan också lägga till förbättringar av målgruppen (4) för att begränsa behörigheten för aktiviteten.

![alt-bild](assets/asset-location-rollout.png)

1. I avsnittet **[!UICONTROL Content]** på samma sida väljer du **[!UICONTROL Create JSON Offer]** i listrutan (1) så som visas.

   ![alt-bild](assets/asset-offer.png)

1. I textrutan **[!UICONTROL JSON Data]** som visas anger du variabeln för funktionsflaggan för den funktion som du vill använda för den här aktiviteten i Experience A (1) med ett giltigt JSON-objekt (2).

   ![alt-bild](assets/asset-json-a-rollout.png)

1. Klicka på **[!UICONTROL Next]** (1) för att gå vidare till steget **[!UICONTROL Targeting]** när du skapar aktiviteter.

   ![alt-bild](assets/asset-next-2-t-rollout.png)

1. I steget **[!UICONTROL Targeting]** ska du behålla målgruppen **[!UICONTROL All Visitors]** (1) för enkelhetens skull. Men justera trafiktilldelningen (2) till 10 %. Detta begränsar funktionen till endast 10 % av webbplatsens besökare. Klicka på Nästa (3) för att gå vidare till steget **[!UICONTROL Goals & Settings]**.

   ![alt-bild](assets/asset-next-2-g-rollout.png)

1. I steget **[!UICONTROL Goals & Settings]** väljer du **[!UICONTROL Adobe Target]** (1) som **[!UICONTROL Reporting Source]** om du vill visa aktivitetsresultaten i användargränssnittet för [!DNL Adobe Target].

1. Välj en **[!UICONTROL Goal Metric]** för att mäta aktiviteten. I det här exemplet baseras en lyckad konvertering på om användaren köper ett objekt, vilket anges av om användaren har nått platsen orderConfirm (2).

1. Klicka på **[!UICONTROL Save & Close]** (3) för att spara aktiviteten.

   ![alt-bild](assets/asset-conv-rollout.png)

## 4. Implementera och återge funktionen i ditt program

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
targetClient.getAttributes(["ondevice-rollout"]).then(function(attributes) {
      const featureFlags = attributes.asObject("ondevice-rollout");

      // Your flag variables are now available in the featureFlags object variable.
      //If you failed to qualify for the Activity, you will have an empty object.
      console.log(featureFlags);
    });
```

>[!TAB Java]

```java {line-numbers="true"}
    Attributes attrs = targetJavaClient.getAttributes(targetDeliveryRequest, "ondevice-rollout");
    Map<String, Object> featureFlags = attrs.toMboxMap("ondevice-rollout");
​
    // Your flag variables are now available in the featureFlags object variable.
    //If you failed to qualify for the Activity, you will have an empty object.
    System.out.println(featureFlags);
```

>[!ENDTABS]

## 5. Implementera spårning för händelser i programmet

När du har gjort variabeln för funktionsflagga tillgänglig i programmet kan du använda den för att aktivera alla funktioner som redan är en del av programmet. Om en besökare inte är berättigad till aktiviteten innebär det att de inte ingick i den 10-procentiga bucket som definierats som målgrupp.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity

if(featureFlags.enable == "yes") { //Fell within 10% traffic
    console.log("Render Feature");
}
else {
    console.log("Disable Feature");
}

// alternatively, the getValue method could be used on the Attributes object.

if(attributes.getValue("ondevice-rollout", "enable") === "yes") { //Fell within 10% traffic
    console.log("Render Feature");
}
else {
    console.log("Disable Feature");
}
```

>[!TAB Java]

```java {line-numbers="true"}
//... Code removed for brevity
​
if("yes".equals(String.valueOf(featureFlags.get("enable")))) { //Fell within 10% traffic
    System.out.println("Render Feature");
}
else {
    System.out.println("Disable Feature");
}
​
// alternatively, the getString method could be used on the Attributes object.
​
if("yes".equals(attrs.getString("ondevice-rollout", "enable"))) { //Fell within 10% traffic
    System.out.println("Render Feature");
}
else {
    System.out.println("Disable Feature");
}
```

>[!ENDTABS]

## 6. Aktivera din utrullningsaktivitet

![alt-bild](assets/asset-activate-rollout.png)

## 7. Justera utrullning och trafikallokering efter behov

När du har aktiverat aktiviteten kan du redigera den när som helst för att öka eller minska trafiktilldelningen efter behov.

Ökning av trafiktilldelningen från 10 % till 50 % på grund av den första utrullningen.

![alt-bild](assets/asset-adjust-rollout.png)
