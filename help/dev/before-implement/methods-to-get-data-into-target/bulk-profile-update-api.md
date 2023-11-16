---
keywords: implementera, implementera, konfigurera, konfigurera, bulkprofiluppdatering
description: Hämta data till [!DNL Target] med uppdaterings-API:t för gruppprofiler.
title: Hur får jag in data på [!DNL Target] Använda API:t för uppdatering av gruppprofil?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 773f8a2f22cfbf740a5ce68809b38b33b621f3b5
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# API för gruppprofilsuppdatering

Skicka en CSV-fil till via API [!DNL Adobe Target] med uppdaterade besökarprofiler för många besökare. Varje besökarprofil kan uppdateras med flera profilattribut på sidan i ett anrop.

Det här alternativet liknar kundattribut med några skillnader:

* Kundattribut använder en FTP-överföring medan [!UICONTROL Target Bulk Profile Update API] använder ett API för HTTP-POST.
* Data för kundattribut kan delas med [!DNL Analytics]. [!UICONTROL Bulk Profile Update] är bara användbart i Target.
* Kundattribut stöder skapandet av en profil för en användare [!DNL Target] har inte sett än. API:t för uppdatering av gruppprofil uppdaterar befintligt [!DNL Target] endast profiler.
* Kundattribut kräver att du använder Experience Cloud ID (ECID) och ett käll-ID, som CRM ID eller Lojalty-ID.
* API:t för uppdatering av gruppprofil kräver antingen TNT-ID eller `mbox3rdPartyId`.
* Du kan inte skicka följande tecken i `mbox3rdPartyID`: plustecken (+) och snedstreck (/).

## Format

CSV-filen måste hänvisa till varje besökare via hans eller hennes [!DNL Target] PCID eller `mbox3rdPartyId`. Experience Cloud ID (ECID) stöds inte. Alla profilattribut/värden skapas och uppdateras via API:t. Formatinformation finns i API-dokumentationen.

## Exempel på användningsområden

CRM-systemet eller andra interna system lagrar värdefulla data om besökarna som du vill uppdatera konsekvent i Target, utan att visa profildata i din sidimplementering.

## Fördelar med metoden

Det finns ingen gräns för antalet profilattribut.

Profilattribut som skickas via webbplatsen kan uppdateras via API och vice versa.

## Caveats

Batchfilens storlek måste vara mindre än 50 MB. Dessutom bör det totala antalet rader inte överstiga 500 000 rader per överföring.

Uppdateringar sker vanligtvis på mindre än en timme, men kan ta upp till 24 timmar att reflektera.

Det finns ingen gräns för hur många rader du kan överföra under 24 timmar i efterföljande batchar. Intag kan dock begränsas under kontorstid för att säkerställa att andra processer körs effektivt.

Flera sekvenser [Uppdateringsanrop för V2-batch](https://developers.adobetarget.com/api/#updating-profiles) utan mbox-anrop däremellan för samma thirdPartyIds åsidosätter egenskaperna som uppdaterades i det första batchuppdateringsanropet.

## Exempel på koder

Se [Uppdaterar profiler](https://developers.adobetarget.com/api/#updating-profiles).

### Länkar till relevant information

[Uppdaterar profiler](https://developers.adobetarget.com/api/#updating-profiles)
