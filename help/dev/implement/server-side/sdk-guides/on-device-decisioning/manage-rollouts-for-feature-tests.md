---
title: Hantera utrullningar för funktionstester
description: Lär dig hantera rollouts för funktionstester med [!UICONTROL on-device decisioning].
feature: APIs/SDKs
exl-id: caa91728-6ac0-4583-a594-0c8fe616342d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 0%

---

# Hantera utrullningar för funktionstester

## Sammanfattning av steg

1. Aktivera [!UICONTROL on-device decisioning] för er organisation
1. Skapa en [!UICONTROL A/B Test] aktivitet
1. Definiera funktioner och rollout-inställningar
1. Implementera och återge funktionen i ditt program
1. Implementera spårning för händelser i ditt program
1. Aktivera din A/B-aktivitet
1. Justera utrullning och trafikallokering efter behov

## 1. Aktivera [!UICONTROL on-device decisioning] för er organisation

Aktivering av enhetsbeslut säkerställer att en A/B-aktivitet utförs vid nästan noll-fördröjning. Om du vill aktivera den här funktionen går du till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** in [!DNL Adobe Target]och aktivera **[!UICONTROL On-Device Decisioning]** växla.

![alt-bild](assets/asset-odd-toggle.png)

>[!NOTE]
>
>Du måste ha administratören eller godkännaren [användarroll](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) för att aktivera eller inaktivera [!UICONTROL On-Device Decisioning] växla.

När du har aktiverat [!UICONTROL On-Device Decisioning] växla, [!DNL Adobe Target] börjar generera *regelartefakter* för kunden.

## 2. Skapa en [!UICONTROL A/B Test] aktivitet

1. I [!DNL Adobe Target], navigera till **[!UICONTROL Activities]** sida och sedan välja **[!UICONTROL Create Activity]** > **[!UICONTROL A/B test]**.

   ![alt-bild](assets/asset-ab.png)

1. I **[!UICONTROL Create A/B Test Activity]** modal, lämna standardvärdet **[!UICONTROL Web]** markerat alternativ (1), markera **[!UICONTROL Form]** som upplevelsedisposition (2) väljer du **[!UICONTROL Default Workspace]** med **[!UICONTROL No Property Restrictions]** (3) och klicka **[!UICONTROL Next]** (4)

   ![alt-bild](assets/asset-form.png)

## 3. Definiera funktioner och rollout-inställningar

I **[!UICONTROL Experiences]** Ange ett namn för aktiviteten (1) när du skapar aktiviteten. Ange namnet på den plats (2) i programmet där du vill hantera rollouter för funktionen. Till exempel:  `ondevice-rollout` eller `homepage-addtocart-rollout` är platsnamn som anger mål för hantering av rollout för funktioner. I exemplet nedan `ondevice-rollout` är den plats som definieras för Experience A. Du kan också lägga till förbättringar av målgruppen (4) för att begränsa behörigheten för aktiviteten.

![alt-bild](assets/asset-location-rollout.png)

1. I **[!UICONTROL Content]** på samma sida väljer **[!UICONTROL Create JSON Offer]** i listrutan (1) enligt bilden.

   ![alt-bild](assets/asset-offer.png)

1. I **[!UICONTROL JSON Data]** som visas anger du variabeln för funktionsflagga för den funktion som du vill använda för den här aktiviteten i Experience A (1) med ett giltigt JSON-objekt (2).

   ![alt-bild](assets/asset-json-a-rollout.png)

1. Klicka **[!UICONTROL Next]** (1) att gå vidare till **[!UICONTROL Targeting]** steg för att skapa aktiviteter.

   ![alt-bild](assets/asset-next-2-t-rollout.png)

1. I **[!UICONTROL Targeting]** steg, behåll **[!UICONTROL All Visitors]** målgrupp (1), för enkelhetens skull. Men justera trafiktilldelningen (2) till 10 %. Detta begränsar funktionen till endast 10 % av webbplatsens besökare. Klicka på Nästa (3) för att gå vidare till **[!UICONTROL Goals & Settings]** steg.

   ![alt-bild](assets/asset-next-2-g-rollout.png)

1. I **[!UICONTROL Goals & Settings]** steg, välja **[!UICONTROL Adobe Target]** (1) **[!UICONTROL Reporting Source]** för att se dina aktivitetsresultat i [!DNL Adobe Target] Gränssnitt.

1. Välj en **[!UICONTROL Goal Metric]** för att mäta aktiviteten. I det här exemplet baseras en lyckad konvertering på om användaren köper ett objekt, vilket anges av om användaren har nått platsen orderConfirm (2).

1. Klicka **[!UICONTROL Save & Close]** (3) för att spara aktiviteten.

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

Ökning av trafikallokeringen från 10 % till 50 % på grund av den första utrullningen.

![alt-bild](assets/asset-adjust-rollout.png)
