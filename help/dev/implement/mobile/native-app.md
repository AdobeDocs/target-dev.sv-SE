---
keywords: mobilapp,aep sdk,inbyggd app,webbvy,inbyggd;swift,adobe experience platform mobile sdk,mobile sdk,intern kod
description: Lär dig hur du implementerar [!DNL Adobe Target] med  [!DNL AEP Mobile SDK]  i ett inbyggt program med webbvyer.
title: Implementera [!DNL Adobe Target] i en mobilapp som använder inbyggd kod med webbvyer
feature: Implement Mobile
role: Developer
exl-id: 3dd2e1d7-c744-4ba8-aaa4-6c2fe64d01fa
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---

# Implementera [!DNL Target] med [!DNL AEP Mobile SDK] i en intern app med webbvyer

I den här artikeln beskrivs de bästa sätten att implementera [!DNL Adobe Target] i en mobilapp som använder inbyggd kod med webbvyer med [!DNL Adobe Experience Platform Mobile SDK].

I den här artikeln används ett exempel på en iOS-app som använder [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/getting-started/){target=_blank} och en [!DNL Target]-integrering som skrivits i [Swift från GitHub-databasen](https://github.com/adobe/aep-sdk-app/){target=_blank}.

I verkligheten använder företagsappen troligen webbvyer i din mobilapp. En webbvy är en behållare som läser in en webbsida med en URL-adress. Behållaren liknar ett webbläsarfönster utan kontroller. I iOS fungerar webbvybehållaren som en Safari-webbläsare när webbsidor bearbetas.

## Förutsättningar

För att komma igång med [!DNL Adobe Experience Platform Mobile SDK] måste du utföra några nödvändiga uppgifter.

Mer information finns i [Adobe Target](https://developer.adobe.com/client-sdks/documentation/adobe-target/){target=_blank} i [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/){target=_blank} -dokumentationen.

## Synkronisera inbyggd kod med webbvyer

Utmaningen när [!DNL Target] implementeras i ett inbyggt program med webbvyer är att [!DNL Adobe Experience Platform Mobile SDK] redan har genererat alla nödvändiga identifierare som krävs för att [!DNL Adobe]-lösningar ska fungera sömlöst. Identifierarna är dock inte synliga för webbvyerna än eftersom dessa identifierare inte finns i den ursprungliga plattformsmiljön. Därför måste du skapa en brygga för att skicka vissa SDK-identifierare till webbvyerna så att besöksidentiteten kvarstår i webbmiljön. Om du inte gör detta på rätt sätt kommer dubblettbesök att utföras, vilket påverkar din rapportering.

Lyckligtvis erbjuder [!DNL Adobe Experience Platform Mobile SDK] en praktisk metod för att generera [!DNL Adobe] parametrar som krävs för att webbvyer ska kunna användas och sparas för samma besökare, vilket visas i följande exempelkod:

```swift
Identity.appendTo(url: URL(string: url), completion: {appendedURL, error in
  print("appendedURL \(String(describing: appendedURL))")
  // load the url with ECID on the main thread
  DispatchQueue.main.async {
    let request = NSMutableURLRequest(url: appendedURL!)
    self.webView.load(request as URLRequest)
  }
});
```

Mer information om metoden `Identity.appendTo` och ett exempel på hur du använder metoden finns i [Swift > Exempel](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/tabs/api-reference/){target=_blank} i *Mobile SDK-dokumentationen*.

Använder `Identity.appendTo`, den här URL:en:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2
```

omformar till:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg
```

Som du ser har parametern `adobe_mc` lagts till i URL:en. Den här parametern innehåller kodade värden för:

* TS=1660667205: Den aktuella tidsstämpeln. Den här tidsstämpeln ser till att webbvyn inte får förfallna värden.
* MCMID=69624092487065093697422606480535692677: [!UICONTROL Experience Cloud ID] (ECID). Kallas även MID eller [!UICONTROL Marketing Cloud ID] som krävs för [!DNL Adobe]-identifiering av korslösningsbesökare.
* MCORGID=EB9CAE8B56E003697F000101@AdobeOrg: The [!UICONTROL Adobe Organization ID].

`Identity.getUrlVariables` är en alternativ [!DNL Adobe Experience Platform Mobile SDK]-metod som returnerar en korrekt formaterad sträng som innehåller URL-variablerna [!DNL Experience Cloud Identity Service]. Mer information finns i [getUrlVariables](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/api-reference/#geturlvariables){target=_blank} i *Identity API reference*.

## Skicka [!DNL Target] sessions-ID för samma sessionsupplevelse

Ett extra steg krävs för att få [!DNL Target]-användarresan att fungera smidigt i de inbyggda vyerna och webbvyerna. I det här steget extraheras och skickas [!DNL Target] sessions-ID från [!DNL Adobe Experience Platform Mobile SDK] till webbvyerna för mobilappen.

`Target.getSessionId` extraherar det sessions-ID som kan skickas till webbvyns URL som en `mboxSession`-parameter:

```swift
Target.getSessionId { (id, err) in
    // read Target sessionId
}
```

## Testa i webbvyerna

Webbförhandsgranskningslänkar genereras på sidan [!UICONTROL Activity detail] genom att klicka på länken [[!UICONTROL Adobe QA] &#x200B;](/help/dev/implement/mobile/target-mobile-preview.md) för att visa ett popup-fönster där varje förhandsgranskningslänk för upplevelsen kopieras, ungefär så här:

```
?at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```

Webbförhandsgranskningslänkar innehåller ytterligare `at_preview_index`- och `at_preview_listed_activities_only`-parametrar. Kopiera de här parametrarna för att skapa mobilvänliga förhandsgranskningslänkar med webblänksparametrar.

Exempel:

```
com.adobe.targetmobile://?at_preview_token=mhFIzJSF7JWb-RsnakpBqhBwj-TiIlZsRTx_1QQuiXLIJFdpSLeEZwKGPUyy57O_&at_preview_index=1_1&at_preview_listed_activities_only=true
```

När du har öppnat länken i en iOS Safari-webbläsare hämtar din app URL:en i din `AppDelegate`-klass, som i följande exempel:

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
  print("url= \(String(describing: url.absoluteString))")
  //...
```

Nu när du har hämtat alla nödvändiga parametrar i programmet kan du skicka dem till webben när det behövs:

```swift
Identity.appendTo(url: URL(string: url), completion: {appendedURL, error in
  let urlWithWebPreviewLink = appendedURL + "&" + myPreviewLinkFromAppDelegate
```

Det slutliga resultatet för webbvylänken kan se ut så här:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg&at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```
