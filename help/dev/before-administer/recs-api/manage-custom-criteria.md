---
title: Hantera anpassade villkor
description: Steg som krävs för att använda Adobe Target API:er för att hantera, skapa, lista, redigera, hämta och ta bort villkor för Adobe Target Recommendations.
feature: APIs/SDKs, Recommendations, Administration & Configuration
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 51a67a49-a92d-4377-9a9f-27116e011ab1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '915'
ht-degree: 1%

---

# Hantera anpassade villkor

Ibland kan de algoritmer som tillhandahålls av Recommendations inte visa vissa element som du vill lyfta fram. I sådana fall kan du med anpassade kriterier leverera en specifik uppsättning rekommenderade objekt för ett visst nyckelobjekt eller en viss kategori.

Om du vill skapa anpassade villkor definierar du och importerar den önskade mappningen mellan nyckelobjektet eller kategorin och de rekommenderade objekten. Den här processen beskrivs i [dokumentation om anpassade kriterier](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/recommendations-csv.html). Som du kan se i den dokumentationen kan du skapa, redigera och ta bort anpassade villkor via användargränssnittet i Target. Target innehåller dock även en uppsättning API:er för anpassade kriterier som ger en mer detaljerad hantering av anpassade villkor.

>[!WARNING]
>
>För anpassade villkor utför du antingen alla åtgärder (skapa, redigera, ta bort) för ett givet anpassat villkor med API:erna, eller också utför du alla åtgärder (skapa, redigera, ta bort) med gränssnittet. Om du hanterar anpassade villkor med en kombination av användargränssnittet och API:t kan det leda till att information eller oväntade resultat hamnar i konflikt. Om du till exempel skapar ett anpassat villkor i användargränssnittet, men sedan redigerar det via API, återspeglas inte dina uppdateringar i användargränssnittet, även om det uppdateras i serverdelen, vilket visas via API:t.

## Skapa anpassade villkor

Skapa anpassade villkor med [Skapa API för anpassade kriterier](https://developers.adobetarget.com/api/recommendations/#operation/createCriteriaCustom)är syntaxen:

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

>[!WARNING]
>
>Anpassade villkor som skapats med API:t Skapa anpassade kriterier, som beskrivs i den här övningen, visas i användargränssnittet där de finns kvar. Du kan inte redigera eller ta bort dem från användargränssnittet. Du kan redigera eller ta bort dem **via API** men i båda fallen visas de fortfarande i målgränssnittet. Om du vill behålla möjligheten att redigera eller ta bort från användargränssnittet skapar du anpassade villkor med användargränssnittet per [dokumentationen](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/recommendations-csv.html)i stället för att använda Create Custom Criteria API.

Fortsätt bara med följande steg när du har läst varningen ovan och är säker på att du kan skapa nya anpassade villkor som inte kan tas bort från användargränssnittet.

1. Verifiera `TENANT_ID` och `API_KEY` for **[!UICONTROL Create custom criteria]** referera till de Postman-miljövariabler som fastställts tidigare. Använd bilden nedan för att jämföra.

   ![CreateCustomCriteria1](assets/CreateCustomCriteria1.png)

1. Lägg till **Brödtext** as **råformat** JSON som definierar platsen för CSV-filen med anpassade villkor. Använd exemplet i [Skapa API för anpassade kriterier](https://developers.adobetarget.com/api/recommendations/#operation/getAllCriteriaCustom) som en mall, ange `environmentId` och andra värden efter behov. I det här exemplet använder vi LAST_PURCHASED som nyckel.

   ![CreateCustomCriteria2](assets/CreateCustomCriteria2.png)

1. Skicka förfrågan och observera svaret, som innehåller information om de anpassade villkor du just skapade.

   ![CreateCustomCriteria3](assets/CreateCustomCriteria3.png)

1. Navigera i Adobe Target till **[!UICONTROL Recommendations > Criteria]** och söka efter dina kriterier efter namn eller använda **[!UICONTROL List Custom Criteria API]** i nästa steg.

   ![CreateCustomCriteria4](assets/CreateCustomCriteria4.png)

I det här fallet har vi ett fel. Låt oss undersöka felet närmare genom att undersöka anpassade kriterier med hjälp av **[!UICONTROL List Custom Criteria API]**.

## Lista anpassade villkor

Använd [Lista anpassat villkor-API](https://developers.adobetarget.com/api/recommendations/#operation/getAllCriteriaCustom). Syntaxen:

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

1. Verifiera `TENANT_ID` och `API_KEY` som tidigare och skicka begäran. Observera det anpassade villkors-ID:t i svaret, liksom information om felmeddelandet som nämndes tidigare.
   ![ListCustomCriteria](assets/ListCustomCriteria.png)

I det här fallet inträffade felet eftersom serverinformationen är felaktig, vilket innebär att Target inte kan komma åt CSV-filen som innehåller den anpassade villkorsdefinitionen. Låt oss redigera de anpassade villkoren för att korrigera detta.

## Redigera anpassade villkor

Om du vill ändra detaljerna för en anpassad villkorsdefinition använder du [Redigera API för anpassade villkor](https://developers.adobetarget.com/api/recommendations/#operation/updateCriteriaCustom). Syntaxen:

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. Verifiera `TENANT_ID` och `API_KEY`, som tidigare.
   ![EditCustomCriteria1](assets/EditCustomCriteria1.png)

1. Ange villkor-ID för det (enkla) anpassade villkor som du vill redigera.
   ![EditCustomCriteria2](assets/EditCustomCriteria2.png)

1. I Body anger du uppdaterad JSON med rätt serverinformation. (I det här steget anger du FTP-åtkomst till en server som du har åtkomst till.)
   ![EditCustomCriteria3](assets/EditCustomCriteria3.png)

1. Skicka förfrågan och notera svaret.
   ![EditCustomCriteria4](assets/EditCustomCriteria4.png)

Låt oss kontrollera om de uppdaterade anpassade villkoren har lyckats med **[!UICONTROL Get Custom Criteria API]**.

## Hämta anpassade villkor

Om du vill visa information om anpassade villkor för ett specifikt anpassat villkor använder du [Hämta API för anpassade villkor](https://developers.adobetarget.com/api/recommendations/#operation/getCriteriaCustom). Syntaxen:

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. Ange villkor-ID för de anpassade villkor vars information du vill få. Skicka förfrågan och granska svaret.
   ![GetCustomCriteria.png](assets/GetCustomCriteria.png)
1. Bekräfta att åtgärden lyckades. (Kontrollera i vårt fall att det inte finns några ytterligare FTP-fel.)
   ![GetCustomCriteria1.png](assets/GetCustomCriteria1.png)
1. (Valfritt) Kontrollera att uppdateringen visas korrekt i användargränssnittet.
   ![GetCustomCriteria2.png](assets/GetCustomCriteria2.png)

## Ta bort anpassade villkor

Ta bort dina anpassade villkor med hjälp av ID:t som nämndes tidigare med [Ta bort API för anpassade villkor](https://developers.adobetarget.com/api/recommendations/#operation/deleteCriteriaCustom). Syntaxen:

`DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. Ange kriterier-ID:t för det (enkla) anpassade villkor som du vill ta bort. Klicka på **[!UICONTROL Send]**.
   ![DeleteCustomCriteria1](assets/DeleteCustomCriteria1.png)

1. Kontrollera att villkoren har tagits bort med Hämta anpassade villkor.
   ![DeleteCustomCriteria2](assets/DeleteCustomCriteria2.png)
I det här fallet anger det förväntade 404-felet att det inte går att hitta de borttagna villkoren.

>[!NOTE]
>
>Som en påminnelse tas villkoren inte bort från målgränssnittet även om det har tagits bort, eftersom det skapades med API:t Skapa anpassade kriterier.

Grattis! Du kan nu skapa, lista, redigera, ta bort och få information om anpassade villkor med hjälp av Recommendations API. I nästa avsnitt använder du Target Delivery API för att hämta rekommendationer.

&lt;!— [Nästa&quot;Hämta Recommendations med leverans-API:t på serversidan&quot; >](fetch-recs-server-side-delivery-api.md) —>
