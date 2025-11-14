---
title: Adobe Target API för gruppprofilsuppdatering
description: Lär dig hur du använder  [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] för att skicka profildata för flera besökare till  [!DNL Target] för användning i mål.
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 0f38d109-5273-4f73-9488-80eca115d44d
source-git-commit: 23cbece45828cf057594445b010a353d865d36ed
workflow-type: tm+mt
source-wordcount: '1056'
ht-degree: 0%

---

# [!DNL Adobe Target Bulk Profile Update API]

Med [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] kan du uppdatera användarprofiler för flera besökare till en webbplats i grupp med hjälp av en gruppfil.

Med hjälp av [!UICONTROL Bulk Profile Update API] kan du enkelt skicka detaljerade data om besökarprofiler i form av profilparametrar för många användare till [!DNL Target] från valfri extern källa. Externa källor kan vara CRM- eller POS-system (Point of Sale) som vanligtvis inte finns på en webbsida.

| Version | URL-exempel | Funktioner |
| --- | --- | --- |
| v1 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/profile/batchUpdate` | Stöd endast för bulkprofiluppdatering. |
| v2 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate` | <ul><li>Skapa profil om den inte hittas.</li><li>Statusuppdatering per rad.</li></ul> |

>[!NOTE]
>
>Version 2 (v2) av [!DNL Bulk Profile Update API] är den aktuella versionen. [!DNL Target] har dock fortfarande stöd för version 1 (v1).
>
>* Om din [!DNL Target]-implementering använder [!DNL Experience Cloud ID] (ECID) som en av profilidentifierarna för anonyma besökare ska du inte använda `pcId` som nyckel i en version 2 (v2)-batchfil. Användning av `pcId` med v2 av [!DNL Bulk Profile Update API] är endast avsedd för fristående [!DNL Target] implementeringar som inte är beroende av ECID.
>
>* Om implementeringen använder ECID för profilidentifiering och du vill använda `pcId` som nyckel i gruppfilen använder du version 1 (v1) av API:t.
>
>* Om implementeringen använder `thirdPartyId` för profilidentifiering använder du version 2 (v2) av API:t med `thirdPartyId` som nyckel.

## Fördelar med [!UICONTROL Bulk Profile Update API]

* Det finns ingen gräns för antalet profilattribut.
* Profilattribut som skickas via webbplatsen kan uppdateras via API och tvärtom.

## Caveats

* Batchfilens storlek måste vara mindre än 50 MB. Dessutom bör det totala antalet rader inte överstiga 500 000 rader per överföring.
* Uppdateringar sker vanligtvis på mindre än en timme, men kan ta så lång tid som 24 timmar att reflektera.
* Det finns ingen gräns för hur många rader du kan överföra under 24 timmar i efterföljande batchar. Intag kan dock begränsas under kontorstid för att säkerställa att andra processer körs effektivt.
* Flera anrop till efterföljande v2-batchuppdatering utan mbox-anrop däremellan för samma thirdPartyIds åsidosätter egenskaperna som uppdaterades i det första batchuppdateringsanropet.
* [!DNL Adobe] garanterar inte att 100 % av gruppprofildata kommer att sparas i Target och därmed vara tillgängliga för användning i målgrupper. I den aktuella designen finns det en möjlighet att en liten andel data (upp till 0,1 % av stora tillverkningssatser) inte tas med eller behålls.

## Gruppfil

Om du vill uppdatera flera profildata samtidigt skapar du en gruppfil. Gruppfilen är en textfil med värden som avgränsas med kommatecken som liknar följande exempelfil.

```
batch=pcId,param1,param2,param3,param4
123,value1
124,value1,,,value4
125,,value2
126,value1,value2,value3,value4
```

>[!NOTE]
>
>Parametern `batch=` är obligatorisk och måste anges i början av filen.

Du refererar till den här filen i POST-anropet till [!DNL Target] servrar för att bearbeta filen. Tänk på följande när du skapar gruppfilen:

* Den första raden i filen måste ange kolumnrubriker.
* Den första rubriken ska antingen vara `pcId` eller `thirdPartyId`. [!UICONTROL Marketing Cloud visitor ID] stöds inte. [!UICONTROL pcId] är ett [!DNL Target]-genererat besökar-ID. `thirdPartyId` är ett ID som anges av klientprogrammet, som skickas till [!DNL Target] via ett mbox-anrop som `mbox3rdPartyId`. Den måste här kallas `thirdPartyId`.
* Parametrar och värden som du anger i gruppfilen måste vara URL-kodade med UTF-8 av säkerhetsskäl. Parametrar och värden kan vidarebefordras till andra kantnoder för bearbetning via HTTP-begäranden.
* Parametrarna får endast ha formatet `paramName`. Parametrar visas i [!DNL Target] som `profile.paramName`.
* Om du använder [!UICONTROL Bulk Profile Update API] v2 behöver du inte ange alla parametervärden för varje `pcId`. Profiler skapas för alla `pcId` eller `mbox3rdPartyId` som inte hittas i [!DNL Target]. Om du använder v1 skapas inte profiler för pcIds som saknas eller mbox3rdPartyIds. Mer information finns i [Hantera tomma värden i  [!DNL Bulk Profile Update API]](#empty) nedan.
* Batchfilens storlek måste vara mindre än 50 MB. Dessutom bör det totala antalet rader inte överstiga 500 000. Den här gränsen gör att servrarna inte översvämmas av för många förfrågningar.
* Det finns ingen begränsning för hur många attribut du kan överföra. Den totala storleken på externa profildata, som innehåller kundattribut, profil-API, parametrar för in-Mbox-profiler och profilskript, får dock inte överstiga 64 kB.
* Parametrar och värden är versalkänsliga.

## HTTP POST-begäran

Gör en HTTP POST-begäran till [!DNL Target] edge-servrar för att bearbeta filen. Här följer ett exempel på en HTTP POST-begäran för filen batch.txt med kommandot curl:

```
curl -X POST --data-binary @BATCH.TXT http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate
```

Var:

BATCH.TXT är filnamnet. CLIENTCODE är klientkoden [!DNL Target].

Om du inte känner till din klientkod klickar du på [!DNL Target] > **[!UICONTROL Administration]** i användargränssnittet för **[!UICONTROL Implementation]**. Klientkoden visas i avsnittet [!UICONTROL Account Details].

### Granska svaret

Profiles API returnerar batchjobbets överföringsstatus för bearbetning tillsammans med en länk under&quot;batchStatus&quot; till en annan URL som visar det aktuella batchjobbets övergripande status.

### Exempel på API-svar

Följande kod är ett exempel på ett Profiles API-svar:

```
<response>
    <success>true</success>
    <batchStatus>http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383</batchStatus>
    <message>Batch submitted for processing</message>
</response>
```

Om ett fel uppstår innehåller svaret `success=false` och ett detaljerat meddelande för felet.

### Standardbatchstatussvar

Ett lyckat standardsvar när någon klickar på URL-länken ovan `batchStatus` ser ut så här:

```
<response><batchId>demo4-1701473848678-13029383</batchId><status>complete</status><batchSize>1</batchSize></response>
```

Förväntade värden för statusfälten är:

| Status | Information |
| --- | --- |
| [!UICONTROL complete] | Begäran om uppdatering av profilbatch har slutförts. |
| [!UICONTROL incomplete] | Begäran om uppdatering av profilbatch bearbetas fortfarande och slutförs inte. |
| [!UICONTROL stuck] | Begäran om uppdatering av profilgrupp har fastnat och kunde inte slutföras. |

### Detaljerat svar på batchstatus-URL

Ett mer detaljerat svar kan hämtas genom att en parameter `showDetails=true` skickas till `batchStatus` url ovan.

Exempel:

```
http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383&showDetails=true
```

#### Detaljerat svar

```
<response>
    <batchId>demo4-1701473848678-13029383</batchId>
    <status>complete</status>
    <batchSize>1</batchSize>
    <consumedCount>1</consumedCount>
    <successfulUpdates>1</successfulUpdates>
    <profilesNotFound>0</profilesNotFound>
    <failedUpdates>0</failedUpdates>
</response>
```

## Hantera tomma värden i [!DNL Bulk Profile Update API] {#empty}

När du använder [!DNL Target] [!DNL Bulk Profile Update API] (v1 eller v2) är det viktigt att förstå hur systemet hanterar tomma parameter- eller attributvärden.

### Förväntat beteende

Om du skickar tomma värden (&quot;&quot;, null eller saknade fält) för befintliga parametrar eller attribut återställs eller tas inte dessa värden bort i profilarkivet. Det här är designat.

* **Tomma värden ignoreras**: API:t filtrerar bort tomma värden under bearbetningen för att undvika onödiga eller meningslösa uppdateringar.

* **Ingen rensning av befintliga data**: Om en parameter redan har ett värde ändras inte om du skickar ett tomt värde.

* **Tomma batchar ignoreras**: Om en batch bara innehåller tomma värden eller null ignoreras den helt och inga uppdateringar tillämpas.

### Ytterligare information

Detta beteende gäller både v1 och v2 för [!DNL Bulk Profile Update API].

Försök att ta bort eller ta bort ett attribut genom att skicka ett tomt värde har ingen effekt.

Stöd för explicit attributborttagning planeras för en framtida version (v3) av API:t, men är för närvarande inte tillgängligt.
