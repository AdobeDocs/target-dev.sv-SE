---
keywords: mobilapp, mobilappsplats, målmobilapp, mobilmålplatser, mått för mobilappens framgång
description: Se exempelkoden för att lära dig hur du skapar platser och framgångsmått i iOS-appar så att du kan använda [!DNL Adobe Target] för att personalisera och optimera appen.
title: Hur skapar jag? [!DNL Target] Platser och Success Metrics i en iOS-app?
feature: Implement Mobile
exl-id: 755c8b26-5c60-48fc-9e7e-5e97a25edb78
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# iOS - skapa en [!DNL Target] mätvärden för position och framgång

Används [!DNL Target] i mobilappen, skapa en plats och ett framgångsmått.

>[!IMPORTANT]
>
>Stöd för [!DNL Adobe Mobile] version 4.*x* SDK har upphört den 31 augusti 2021 och rekommenderas inte längre för [!DNL Adobe Target] mobilanvändare.
>
>The [Adobe Experience Platform SDK för mobilappar](https://developer.adobe.com/client-sdks/documentation/){target=_blank} är den rekommenderade lösningen för strömförsörjning [!DNL Adobe Experience Cloud] lösningar och tjänster i era mobilappar.

Det här avsnittet innehåller exempelkod som kan användas som mall för ditt program. Exemplen i det här avsnittet innehåller kod för iOS. Samma mönster gäller för Android. Android-specifik syntax finns i [Android SDK 4.x för Experience Cloud Solutions](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/target-main.html) guide.

>[!NOTE]
>
>Se [Mobildokumentation](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-target-methods.html) för en lista över alla tillgängliga [!DNL Target] metoder.

Skapa en [!DNL Target] plats i appen och göra en förfrågan finns det två primära metoder:

* `targetCreateRequestWithName`
* `targetLoadRequest`

1. Skapa en [!DNL Target] plats.

   Här följer ett exempel på ett anrop för att skapa en begäran:

   ```
   // make your request 
   ADBTargetLocationRequest *myRequest = [ADBMobile targetCreateRequestWithName:@"heroBanner" 
                                                    defaultContent:@"default.png" 
                                                    parameters:nil];
   ```

   | Parameter | Beskrivning |
   |---|---|
   | `ADBTargetLocationRequest *myRequest` | Ersätt `myRequest` med ditt namn `targetLocation` i appen. |
   | `targetCreateRequestWithName:@"heroBanner"` | Ersätt `heroBanner` med ditt namn `targetLocation` i Target. Detta är samma som mbox-namnet. Den här hjältebanderollen visas i Target-gränssnittet. |
   | `defaultContent:@"default.png"` | Ersätt `default.png` med det värde som appen använder om Target inte svarar. |
   | `parameters:nil` | Ange profil- eller mbox-parametrar. Mer information finns i avsnittet&quot;Skicka anpassade data&quot;. |

   Här följer ett exempel på ett anrop för att läsa in begäran:

   ```
   // load your request 
   [ADBMobile targetLoadRequest:myRequest callback:^(NSString *content) { 
                                        // do something with content 
                                        heroImage.image = [UIImage imageNamed:content]; 
   }];
   ```

   | Parameter | Beskrivning |
   |---|---|
   | `targetLoadRequest:myRequest` | Ersätt `myRequest` med ditt namn `targetLocation` i appen. |
   | `NSString *content` | Ersätt innehåll med det faktiska innehåll som kommer tillbaka från Adobe. Strängen kan vara XML, JSON eller en vanlig sträng. Använd det här avsnittet av koden för att definiera variabler, ange bildsökvägar, visa styrenhetsflöden, transaktionspunkter eller annat som du vill göra. Målet returnerar innehållet som anges i användargränssnittet i exakt samma format. |
   | `heroImage.image = [UIImage imageNamed:content];` | Till exempel: Ta innehåll och ange sökvägen till en hjältebild. |

1. Skapa ett framgångsmått.

   Metoden `targetCreateOrderConfirmRequestWithName` kan användas för att spåra en konverterings-/framgångsmätning i din app.

   ```
   ADBTargetLocationRequest *req = [ADBMobile targetCreateOrderConfirmRequestWithName: "orderConfirm" 
                                              orderId: orderId 
                                              orderTotal: @"39.95" 
                                              productPurchasedId: _galleryItem.title 
                                              parameters: nil]; 
   [ADBMobile targetLoadRequest: req callback: nil];
   ```

   | Parameter | Beskrivning |
   |---|---|
   | `orderId` | Ersätt med en dynamisk variabel som representerar ett unikt order-ID. |
   | `@"39.95"` | Ersätt med en dynamisk variabel som representerar en unik ordersumma. |
   | `_galleryItem.title` | Ersätt med en dynamisk variabel som representerar en kommaavgränsad lista över köpta produkter. |
   | `parameters: nil` | Valfri ordlista med ytterligare parametrar. |

1. Skapa appen.

   Stegresultat Skapa ett A/B-test när du har skapat en målplats och taggat ett framgångsmått. Aktiviteten kan skapas med den formulärbaserade upplevelsedispositionen.
