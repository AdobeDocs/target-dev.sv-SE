---
keywords: implementera, implementera, konfigurera, konfigurera, bulkprofiluppdatering API
description: Hämta data till  [!DNL Target] med [!UICONTROL Bulk Profile Update API].
title: Hur hämtar jag data till  [!DNL Target] med [!UICONTROL Bulk Profile Update API]?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 946e9431e6bde30f564b4ba1a4cf0a78d8c5c6bf
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# API för gruppprofiluppdatering

Med [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] kan du uppdatera användarprofiler för flera besökare till en webbplats i grupp med hjälp av en gruppfil.

Med hjälp av [!UICONTROL Bulk Profile Update API] kan du enkelt skicka detaljerade data om besökarprofiler i form av profilparametrar för många användare till [!DNL Target] från valfri extern källa. Externa källor kan vara CRM- eller POS-system (Point of Sale) som vanligtvis inte finns på en webbsida.

Kontrastera [!UICONTROL Bulk Profile Update API] med [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md).

## [!UICONTROL Customer attributes] jämfört med [!UICONTROL Bulk Profile Update API]

Det här alternativet liknar [[!UICONTROL customer attributes]](/help/dev/before-implement/methods-to-get-data-into-target/customer-attributes.md) med några skillnader:

* [!UICONTROL Customer attributes] använder en FTP-överföring. [!UICONTROL Target Bulk Profile Update API] använder ett API för HTTP-POST.
* [!UICONTROL Customer attribute] data kan delas med [!DNL Analytics]. [!UICONTROL Bulk Profile Update] kan bara användas i [!DNL Target].
* [!UICONTROL Customer attributes] har ännu inte stöd för att skapa en profil för en användare [!DNL Target].
   * [!UICONTROL Bulk Profile Update API] v2: Du behöver inte ange alla parametervärden för varje `pcId`. Profiler skapas för alla `pcId` eller `mbox3rdPartyId` som inte hittas i [!DNL Target].
   * [!UICONTROL Bulk Profile Update API] v1: [!UICONTROL Bulk Profile Update API] uppdaterar endast befintliga [!DNL Target] profiler. Om du använder v1 skapas inga profiler för `pcIds` eller `mbox3rdPartyIds` som saknas.
* [!UICONTROL Customer attributes] kräver att [!UICONTROL Experience Cloud ID] (ECID) och ett käll-ID används, till exempel CRM-ID eller Lojalty-ID.
* [!UICONTROL Bulk Profile Update API] kräver antingen TNT-ID eller `mbox3rdPartyId`.
* Du kan inte skicka följande tecken i `mbox3rdPartyID`: plustecken (+) och snedstreck (/).

## Resurs

Mer information finns i:

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)