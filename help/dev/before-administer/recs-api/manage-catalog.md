---
title: Hantera din Recommendations-katalog med API:er
description: Steg som krävs för att använda Adobe Target API:er för att skapa, uppdatera, spara, hämta och ta bort enheter i din Recommendations-katalog.
feature: APIs/SDKs, Recommendations, Administration & Configuration
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: aea82607-cde4-456a-8dfb-2967badce455
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 0%

---

# Hantera din Recommendations Catalog med API:er

Du försäkrar dig om att du uppfyller [kraven för att använda Recommendations API](/help/dev/before-administer/recs-api/overview.md#prerequisites), men du lärde dig att [generera en åtkomsttoken](/help/dev/before-administer/configure-authentication.md) med JWT-autentiseringsflödet för att använda [!DNL Adobe Target] Admin API:er på [Adobe Developer Console](https://developer.adobe.com/console/home) .

Du kan nu använda [Recommendations API:er](https://developer.adobe.com/target/administer/recommendations-api/) för att lägga till, uppdatera eller ta bort objekt i din rekommendationskatalog. Precis som med övriga Adobe Target Admin API:er kräver Recommendations API:er autentisering.

>[!NOTE]
>
>Skicka **[!UICONTROL IMS: JWT Generate + Auth via User Token]**-begäran när du behöver uppdatera din åtkomsttoken för autentisering, eftersom den upphör att gälla efter 24 timmar. Mer information finns i [Konfigurera Adobe API-autentisering](../configure-authentication.md).

![JWT3ff](assets/configure-io-target-jwt3ff.png)

Hämta [Recommendations Postman-samlingen](https://developer.adobe.com/target/administer/recommendations-api/#section/Postman) innan du fortsätter.

## Skapa och uppdatera objekt med API:t för att spara enheter

Om du vill fylla i din Recommendations-produktdatabas med API:t i stället för en CSV-produktfeed eller Target-begäran som aktiveras på produktsidor använder du [API:t för att spara entiteter](https://developer.adobe.com/target/administer/recommendations-api/#operation/saveEntities). Den här begäran lägger till eller uppdaterar ett objekt i en enda målmiljö. Syntaxen:

```
POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities
```

Du kan till exempel använda Spara enheter för att uppdatera artiklar när vissa tröskelvärden har uppnåtts, t.ex. tröskelvärden för lager eller pris, för att flagga dessa artiklar och förhindra att de rekommenderas.

1. Navigera till **[!UICONTROL Target]** > **[!UICONTROL Setup]** > **[!UICONTROL Hosts]** > **[!UICONTROL CONTROL Environments]** för att hämta det målmiljö-ID där du vill lägga till eller uppdatera ett objekt.

   ![SaveEntities1](assets/SaveEntities01.png)

1. Verifiera att `TENANT_ID` och `API_KEY` refererar till de Postman-miljövariabler som upprättats tidigare. Använd bilden nedan för att jämföra. Om det behövs kan du ändra rubrikerna och sökvägen i din API-begäran så att de matchar dem i bilden nedan.

   ![SaveEntities3](assets/SaveEntities03.png)

1. Ange din JSON som **raw**-kod i **Body**. Glöm inte att ange ditt miljö-ID med variabeln `environment`. (I exemplet nedan är miljö-ID 6781.)

   ![SaveEntities4.png](assets/SaveEntities04.png)

   Nedan visas exempel-JSON som lägger till entity.id kit2001 med associerade enhetsvärden för en Toaster Oven-produkt i miljö 6781.

   ```
       {
       "entities": [{
               "name": "Toaster Oven",
               "id": "kit2001",
               "environment": 6781,
               "categories": [
                   "housewares:appliances"
               ],
               "attributes": {
                   "inventory": 77,
                   "margin": 23,
                   "message": "crashing helicopter",
                   "pageUrl": "www.foobar.foo.com/helicopter.html",
                   "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                  "value": 19.2
               }
           }]
       }
   ```

1. Klicka på **[!UICONTROL Send]**. Du bör få följande svar.

   ![SaveEntities5.png](assets/SaveEntities05.png)

   JSON-objektet kan skalas för att skicka flera produkter. Denna JSON anger till exempel två enheter.

   ```
       {
           "entities": [{
                   "name": "Toaster Oven",
                   "id": "kit2001",
                   "environment": 6781,
                   "categories": [
                       "housewares:appliances"
                   ],
                   "attributes": {
                       "inventory": 89,
                       "margin": 11,
                       "message": "Toaster Oven",
                       "pageUrl": "www.foobar.foo.com/helicopter.html",
                       "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                       "value": 102.5
                   }
               },
               {
                   "name": "Blender",
                   "id": "kit2002",
                   "environment": 6781,
                   "categories": [
                       "housewares:appliances"
                   ],
                   "attributes": {
                       "inventory": 36,
                       "margin": 5,
                       "message": "Blender",
                       "pageUrl": "www.foobar.foo.com/helicopter.html",
                       "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                       "value": 54.5
                   }
               }
           ]
       }
   ```

1. Nu är det din tur! Använd API:t **[!UICONTROL Save Entities]** för att lägga till följande objekt i katalogen. Använd JSON-exempelkoden ovan som utgångspunkt. (Du måste utöka JSON för att inkludera ytterligare entiteter.)

   ![SaveEntities6.png](assets/SaveEntities06.png)

De sista två objekten hör inte hemma. Låt oss inspektera dem med API:t **[!UICONTROL Get Entity]** och vid behov ta bort dem med API:t **[!UICONTROL Delete Entities]**.

## Hämta objektinformation med Get Entity API

Om du vill hämta information om ett befintligt objekt använder du [Get Entity API](https://developer.adobe.com/target/administer/recommendations-api/#operation/getEntity). Syntaxen:

```
GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities/[entity.id]
```

Det går bara att hämta entitetsinformation för en enskild entitet åt gången. Du kan använda Hämta entitet för att bekräfta att uppdateringar har gjorts i katalogen som förväntat, eller för att på annat sätt granska innehållet i katalogen.

1. Ange enhets-ID i API-begäran med variabeln `entityId`. Följande exempel returnerar information för den entitet vars entityId=kit2004.

   ![GetEntity1](assets/GetEntity1.png)

1. Verifiera att `TENANT_ID` och `API_KEY` refererar till de Postman-miljövariabler som upprättats tidigare. Använd bilden nedan för att jämföra. Om det behövs kan du ändra rubrikerna och sökvägen i din API-begäran så att de matchar dem i bilden nedan.

   ![GetEntity2](assets/GetEntity2.png)

1. Skicka begäran.

   ![GetEntity3](assets/GetEntity3.png)
Om du får ett felmeddelande om att enheten inte hittades, som visas i exemplet ovan, kontrollerar du att du skickar begäran till rätt målmiljö.



   >[!NOTE]
   >
   >Om ingen miljö uttryckligen anges försöker Get Entity att hämta entiteten enbart från din [standardmiljö](https://experienceleague.adobe.com/docs/target/using/administer/environments.html). Om du vill hämta från någon annan miljö än standardmiljön måste du ange miljö-ID:t.

1. Om det behövs lägger du till parametern `environmentId` och skickar begäran igen.

   ![GetEntity4](assets/GetEntity4.png)

1. Skicka ytterligare en **[!UICONTROL Get Entity]**-begäran, den här gången för att inspektera entiteten vars entityId=kit2005.

   ![GetEntity5](assets/GetEntity5.png)

Anta att du måste ta bort de här entiteterna från katalogen. Låt oss använda **[!UICONTROL Delete Entities]**-API:t.

## Ta bort objekt med API:t Ta bort entiteter

Om du vill ta bort objekt från katalogen använder du [API:t Ta bort entiteter](https://developer.adobe.com/target/administer/recommendations-api/#operation/deleteEntities). Syntaxen:

```
DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities?ids=[comma-delimited-entity-ids]&environment=[environmentId]
```

>[!WARNING]
>
>API:t Ta bort entiteter tar bort entiteter som refereras av de ID som du anger. Om inga enhets-ID anges tas alla enheter i den angivna miljön bort. Om inget miljö-ID anges tas enheter bort från alla miljöer. Använd det här med försiktighet!

1. Navigera till **[!UICONTROL Target]** > **[!UICONTROL Setup]** > **[!UICONTROL Hosts]** > **[!UICONTROL Environments]** för att hämta det målmiljö-ID som du vill ta bort objekt från.

   ![DeleteEntities1](assets/SaveEntities01.png)

1. I API-begäran anger du enhets-ID för de entiteter som du vill ta bort med syntaxen `&ids=[comma-delimited-entity-ids]` (en frågeparameter). Om du tar bort mer än en enhet avgränsar du ID:n med kommatecken.

   ![DeleteEntities2](assets/DeleteEntities2.png)

1. Ange miljö-ID med syntaxen `&environment=[environmentId]`, annars tas entiteter bort i alla miljöer.

   ![DeleteEntities3](assets/DeleteEntities3.png)

1. Verifiera att `TENANT_ID` och `API_KEY` refererar till de Postman-miljövariabler som upprättats tidigare. Använd bilden nedan för att jämföra. Om det behövs kan du ändra rubrikerna och sökvägen i din API-begäran så att de matchar dem i bilden nedan.

   ![DeleteEntities4](assets/DeleteEntities4.png)

1. Skicka begäran.

   ![DeleteEntities5](assets/DeleteEntities5.png)

1. Verifiera dina resultat med **[!UICONTROL Get Entity]**, som nu bör ange att det inte går att hitta de borttagna entiteterna.

   ![DeleteEntities6](assets/DeleteEntities6.png)

   ![DeleteEntities6](assets/DeleteEntities7.png)

Grattis! Nu kan du använda Recommendations API:er för att skapa, uppdatera, ta bort och få information om enheterna i din katalog. I nästa avsnitt får du lära dig hur du hanterar anpassade villkor.

&lt;!— [Nästa &quot;Hantera anpassade villkor&quot; >](manage-custom-criteria.md) —>
