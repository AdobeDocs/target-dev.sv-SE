---
title: Adobe Target API för gruppprofilsuppdatering
description: Lär dig använda [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] för att skicka profildata till flera besökare [!DNL Target].
feature: APIs/SDKs
contributors: https://github.com/icaraps
source-git-commit: 6f7d9875e3b73352ead3a55e40a4b2f81f3d4400
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---

# [!DNL Adobe Target Bulk Profile Update API]

The [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] Med kan du uppdatera användarprofiler för flera besökare på en webbplats i grupp med hjälp av en gruppfil.

Använda [!UICONTROL Bulk Profile Update API]kan du enkelt skicka detaljerade besökarprofildata i form av profilparametrar för många användare till [!DNL Target] från valfri extern källa. Externa källor kan vara CRM- eller POS-system (Point of Sale), som vanligtvis inte finns på en webbsida.

| Version | URL-exempel | Funktioner |
| --- | --- | --- |
| v1 | `http://CLIENTCODE.tt.omtrdc.net/m2/ CLIENTCODE/profile/batchUpdate` | Stöd endast för bulkprofiluppdatering. |
| v2 | `http://CLIENTCODE.tt.omtrdc.net/m2/ CLIENTCODE/v2/profile/batchUpdate` | <ul><li>Skapa profil om den inte hittas.</li><li>Statusuppdatering per rad.</li></ul> |

>[!NOTE]
>
>Version 2 (v2) av [!UICONTROL Bulk Profile Update API] är den aktuella versionen. Men [!DNL Target] stöder fortfarande version 1 (v1).

## Caveats

* Batchfilens storlek måste vara mindre än 50 MB. Dessutom bör det totala antalet rader inte överstiga 500 000 rader per överföring.
* Det finns ingen gräns för hur många rader du kan överföra under 24 timmar i efterföljande batchar. Intag kan dock begränsas under kontorstid för att säkerställa att andra processer körs effektivt.
* Flera anrop till efterföljande v2-batchuppdatering utan mbox-anrop däremellan för samma thirdPartyIds åsidosätter egenskaperna som uppdaterades i det första batchuppdateringsanropet.

## Gruppfil

Om du vill uppdatera flera profildata samtidigt skapar du en gruppfil. Gruppfilen är en textfil med värden som avgränsas med kommatecken som liknar följande exempelfil.

``````
batch=pcId, param1, param2, param3, param4 123, value1 124, value1,,, value4 125,, value2 126, value1, value2, value3, value4
``````

Du refererar till den här filen i POSTENS anrop till [!DNL Target] servrar för att bearbeta filen. Tänk på följande när du skapar gruppfilen:

* Den första raden i filen måste ange kolumnrubriker.
* Den första rubriken ska antingen vara en `pcId` eller `thirdPartyId`. The [!UICONTROL Marketing Cloud visitor ID] stöds inte. [!UICONTROL pcId] är en [!DNL Target]-genererad visitorID. `thirdPartyId` är ett ID som anges av klientprogrammet, som skickas till [!DNL Target] via ett mbox-anrop som `mbox3rdPartyId`. Här anges den som `thirdPartyId`.
* Parametrar och värden som du anger i gruppfilen måste vara URL-kodade med UTF-8 av säkerhetsskäl. Parametrar och värden kan vidarebefordras till andra kantnoder för bearbetning via HTTP-begäranden.
* Parametrarna måste ha formatet `paramName` endast. Parametrar visas i [!DNL Target] as `profile.paramName`.
* Om du använder [!UICONTROL Bulk Profile Update API] v2, du behöver inte ange alla parametervärden för varje `pcId`. Profiler skapas för alla `pcId` eller `mbox3rdPartyId` som inte finns i [!DNL Target]. Om du använder v1 skapas inte profiler för pcIds som saknas eller mbox3rdPartyIds.
* Batchfilens storlek måste vara mindre än 50 MB. Dessutom bör det totala antalet rader inte överstiga 500 000. Den här gränsen gör att servrarna inte översvämmas av för många förfrågningar.
* Du kan skicka flera filer. Summan av raderna i alla filer som du skickar en dag får dock inte överstiga en miljon för varje kund.
* Det finns ingen gräns för hur många attribut du överför. Den totala storleken på en profil, inklusive systemdata, bör dock inte överstiga 2000 kB. [!DNL Adobe] rekommenderar att du använder mindre än 1 000 kB lagringsutrymme för profilattribut.
* Parametrar och värden är versalkänsliga.

## Begäran om HTTP-POST

Gör en HTTP-POST-förfrågan till [!DNL Target] edge-servrar för att bearbeta filen. Här följer ett exempel på en HTTP-POST-begäran för filen batch.txt med kommandot curl:

``````
curl -X POST --data-binary @BATCH.TXT http://CLIENTCODE.tt.omtrdc.net/m2/ CLIENTCODE/v2/profile/batchUpdate
``````

Var:

BATCH.TXT är filnamnet. CLIENTCODE är [!DNL Target] klientkod.

Om du inte känner till din klientkod går du till [!DNL Target] användargränssnitt klicka **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**. Klientkoden visas i [!UICONTROL Account Details] -avsnitt.

### Inspect svar

v2 returnerar en profil-för-profil-status och v1 returnerar bara den övergripande statusen. Svaret innehåller en länk till en annan URL som innehåller ett meddelande om att profilen lyckades.

### Exempel på svar

```
true http://mboxedge19.tt.omtrdc.net/m2/demo/v2/profile/batchStatus?batchId=demo-1845664501&m2Node=00 Batch submitted for processing
```

Om ett fel uppstår innehåller svaret `success=false` och ett detaljerat felmeddelande.

Ett lyckat svar ser ut så här:

``````
demo-1845664501 1436187396849-250353.03_03 success 2403081156529-351655.03_03 success 2403081156529-351656.03_03 success 1436187396849-250351.01_00 success 
``````

Förväntade värden för statusfälten är:

**framgång**: Profilen uppdaterades. Om profilen inte hittas skapades en med värdena från gruppen.
**fel**: Profilen uppdaterades eller skapades inte på grund av fel, undantag eller meddelandeförlust.
**väntande**: Profilen har inte uppdaterats eller skapats ännu.



