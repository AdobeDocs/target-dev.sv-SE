---
keywords: implementera, implementera, konfigurera, konfigurera, bulkprofiluppdatering API
description: Hämta data till [!DNL Target] med [!UICONTROL Bulk Profile Update API].
title: Hur får jag in data på [!DNL Target] Använda [!UICONTROL Bulk Profile Update API]?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 734bda64915a08f2edba37cbbb66b2de581c2237
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 0%

---

# API för gruppprofiluppdatering

Skicka en CSV-fil till via API [!DNL Adobe Target] med uppdaterade besökarprofiler för många besökare. Varje besökarprofil kan uppdateras med flera profilattribut på sidan i ett anrop.

Det här alternativet liknar [!UICONTROL customer attributes] med några skillnader:

* [!UICONTROL Customer attributes] använda en FTP-överföring. The [!UICONTROL Target Bulk Profile Update API] använder ett API för HTTP-POST.
* [!UICONTROL Customer attribute] data kan delas med [!DNL Analytics]. [!UICONTROL Bulk Profile Update] är bara användbart i [!DNL Target].
* [!UICONTROL Customer attributes] stöd för att skapa en profil för en användare [!DNL Target] har inte sett än.
   * [!UICONTROL Bulk Profile Update API] v2: Du behöver inte ange alla parametervärden för varje `pcId`. Profiler skapas för alla `pcId` eller `mbox3rdPartyId` som inte finns i [!DNL Target].
   * [!UICONTROL Bulk Profile Update API] v1: [!UICONTROL Bulk Profile Update API] uppdaterar befintlig [!DNL Target] endast profiler. Om du använder v1 skapas inga profiler för att saknas `pcIds` eller `mbox3rdPartyIds`.
* [!UICONTROL Customer attributes] kräver att [!UICONTROL Experience Cloud ID] (ECID) och användningen av ett käll-ID, t.ex. CRM-ID eller Lojalty-ID.
* The [!UICONTROL Bulk Profile Update API] kräver antingen TNT ID eller `mbox3rdPartyId`.
* Du kan inte skicka följande tecken i `mbox3rdPartyID`: plustecken (+) och snedstreck (/).

## Format

CSV-filen måste referera till varje besökare via deras [!DNL Target] PCID eller `mbox3rdPartyId`. The [!UICONTROL Experience Cloud ID] (ECID) stöds inte. Alla profilattribut/värden skapas och uppdateras via API:t. Formatinformation finns i API-dokumentationen.

## Exempel på användningsområden

I CRM-systemet eller andra interna system lagras värdefulla data om besökarna som du konsekvent vill uppdatera till [!DNL Target], utan att visa profildata i din sidimplementering.

## Fördelar med metoden

* Det finns ingen gräns för antalet profilattribut.
* Profilattribut som skickas via webbplatsen kan uppdateras via API och tvärtom.

## Caveats

* Batchfilens storlek måste vara mindre än 50 MB. Dessutom bör det totala antalet rader inte överstiga 500 000 rader per överföring.
* Uppdateringar sker vanligtvis på mindre än en timme, men kan ta så lång tid som 24 timmar att reflektera.
* Det finns ingen gräns för hur många rader du kan överföra under 24 timmar i efterföljande batchar. Intag kan dock begränsas under kontorstid för att säkerställa att andra processer körs effektivt.
* Flera sekvenser [Uppdateringsanrop för V2-batch](https://developers.adobetarget.com/api/#updating-profiles) utan mbox-anrop däremellan för samma `thirdPartyIds` åsidosätt egenskaperna som uppdaterades i det första batchuppdateringsanropet.

## Exempel på koder

Se [Uppdaterar profiler](https://developers.adobetarget.com/api/#updating-profiles).

### Länkar till relevant information

[Uppdaterar profiler](https://developers.adobetarget.com/api/#updating-profiles)
