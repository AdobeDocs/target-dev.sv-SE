---
keywords: mobilapp,aep sdk,inbyggd app,webbvy,inbyggd;swift,adobe experience platform mobile sdk,mobile sdk,intern kod
description: Lär dig implementera [!DNL Adobe Target] med [!DNL AEP Mobile SDK] i ett program med webbvyer.
title: Implementera [!DNL Adobe Target] i en mobilapp som använder inbyggd kod med webbvyer
feature: Implement Mobile
role: Developer
source-git-commit: c0fda36cb5472d71438c47b8b484716003da4214
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---


# Implementera [!DNL Target] med [!DNL AEP Mobile SDK] i ett program med webbvyer

I den här artikeln beskrivs de bästa sätten att implementera [!DNL Adobe Target] i en mobilapp som använder inbyggd kod med webbvyer med [!DNL Adobe Experience Platform Mobile SDK].

I den här artikeln används ett exempel på en iOS-app med [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/getting-started/){target=_blank} and a [!DNL Target] integration written in [Swift from the GitHub repository](https://github.com/adobe/aep-sdk-app/){target=_blank}.

I verkligheten använder företagsappen troligen webbvyer i din mobilapp. En webbvy är en behållare som läser in en webbsida med en URL-adress. Behållaren liknar ett webbläsarfönster utan kontroller. I iOS fungerar webbvybehållaren som en Safari-webbläsare när webbsidor bearbetas.

## Förutsättningar

Så här kommer du igång med [!DNL Adobe Experience Platform Mobile SDK]måste du utföra några nödvändiga uppgifter.

Mer information finns i [Adobe Target](https://developer.adobe.com/client-sdks/documentation/adobe-target/){target=_blank} in the [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/){target=_blank} dokumentation.

## Synkronisera inbyggd kod med webbvyer

Utmaningen vid implementering [!DNL Target] i ett program med webbvyer är [!DNL Adobe Experience Platform Mobile SDK] har redan genererat alla nödvändiga identifierare som krävs för [!DNL Adobe] lösningar som fungerar smidigt. Identifierarna är dock inte synliga för webbvyerna än eftersom dessa identifierare inte finns i den ursprungliga plattformsmiljön. Därför måste du skapa en brygga för att skicka vissa SDK-identifierare till webbvyerna så att besöksidentiteten kvarstår i webbmiljön. Om du inte gör detta på rätt sätt kommer dubblettbesök att utföras, vilket påverkar din rapportering.

Som tur är [!DNL Adobe Experience Platform Mobile SDK] är ett praktiskt sätt att generera [!DNL Adobe] parametrar som krävs för att webbvyer ska kunna användas och finnas kvar för samma besökare, vilket visas i följande exempelkod:

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

Mer information om `Identity.appendTo` och om du vill se ett exempel på hur du använder metoden läser du [Swift > Exempel](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/tabs/api-reference/){target=_blank} i *Mobil SDK-dokumentation*.

Använda `Identity.appendTo`, den här URL:en:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2
```

omformar till:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg
```

Som du ser finns det `adobe_mc` parametern har lagts till i URL:en. Den här parametern innehåller kodade värden för:

* TS=1660667205: Den aktuella tidsstämpeln. Den här tidsstämpeln ser till att webbvyn inte får förfallna värden.
* MCMID=69624092487065093697422606480535692677: [!UICONTROL Experience Cloud ID] (ECID) Kallas även MID eller [!UICONTROL Marketing Cloud ID] krävs för [!DNL Adobe] identifiering av besökare i flera lösningar.
* MCORGID=EB9CAE8B56E003697F000101@AdobeOrg [!UICONTROL Adobe Organization ID].

The `Identity.getUrlVariables` är ett alternativ [!DNL Adobe Experience Platform Mobile SDK] metod som returnerar en sträng med rätt format som innehåller [!DNL Experience Cloud Identity Service] URL-variabler. Mer information finns i [getUrlVariables](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/api-reference/#geturlvariables){target=_blank} i *API-referens för identitet*.

## Skicka [!DNL Target] Sessions-ID för samma sessionsupplevelse

Ett steg till krävs för att [!DNL Target] användarresan fungerar smidigt i de inbyggda vyerna och webbvyerna. Det här steget inkluderar extrahering och godkännande av [!DNL Target] Sessions-ID från [!DNL Adobe Experience Platform Mobile SDK] till webbvyerna för mobilappen.

The `Target.getSessionId` extraherar det sessions-ID som kan skickas till webbvyns URL som en `mboxSession` parameter:

```swift
Target.getSessionId { (id, err) in
    // read Target sessionId
}
```

## Testa i webbvyerna

Länkar för webbförhandsvisning genereras på [!UICONTROL Activity detail] genom att klicka på [[!UICONTROL Adobe QA] link](/help/dev/implement/mobile/target-mobile-preview.md) om du vill visa ett popup-fönster där du kan kopiera varje länk för förhandsvisning av upplevelsen, som följande:

```
?at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```

Webbförhandsgranskningslänkar innehåller ytterligare `at_preview_index` och `at_preview_listed_activities_only` parametrar. Kopiera de här parametrarna för att skapa mobilvänliga förhandsgranskningslänkar med webblänksparametrar.

Exempel:

```
com.adobe.targetmobile://?at_preview_token=mhFIzJSF7JWb-RsnakpBqhBwj-TiIlZsRTx_1QQuiXLIJFdpSLeEZwKGPUyy57O_&at_preview_index=1_1&at_preview_listed_activities_only=true
```

När du har öppnat länken i en iOS Safari-webbläsare hämtar din app URL:en i din `AppDelegate` klass som liknar följande exempel:

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
