---
keywords: implementera, implementera, konfigurera, konfigurera, bulkprofiluppdatering API
description: Hämta data till [!DNL Target] med [!UICONTROL Bulk Profile Update API].
title: Hur får jag in data på [!DNL Target] Använda [!UICONTROL Bulk Profile Update API]?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 946e9431e6bde30f564b4ba1a4cf0a78d8c5c6bf
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# API för gruppprofiluppdatering

The [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] Med kan du uppdatera användarprofiler för flera besökare på en webbplats i grupp med hjälp av en gruppfil.

Använda [!UICONTROL Bulk Profile Update API]kan du enkelt skicka detaljerade besökarprofildata i form av profilparametrar för många användare till [!DNL Target] från valfri extern källa. Externa källor kan vara CRM- eller POS-system (Point of Sale) som vanligtvis inte finns på en webbsida.

Kontrast för [!UICONTROL Bulk Profile Update API] med [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md).

## [!UICONTROL Customer attributes] kontra [!UICONTROL Bulk Profile Update API]

Det här alternativet liknar [[!UICONTROL customer attributes]](/help/dev/before-implement/methods-to-get-data-into-target/customer-attributes.md) med några skillnader:

* [!UICONTROL Customer attributes] använda en FTP-överföring. The [!UICONTROL Target Bulk Profile Update API] använder ett API för HTTP-POST.
* [!UICONTROL Customer attribute] data kan delas med [!DNL Analytics]. The [!UICONTROL Bulk Profile Update] är bara användbart i [!DNL Target].
* [!UICONTROL Customer attributes] stöd för att skapa en profil för en användare [!DNL Target] har inte sett än.
   * [!UICONTROL Bulk Profile Update API] v2: Du behöver inte ange alla parametervärden för varje `pcId`. Profiler skapas för alla `pcId` eller `mbox3rdPartyId` som inte finns i [!DNL Target].
   * [!UICONTROL Bulk Profile Update API] v1: [!UICONTROL Bulk Profile Update API] uppdaterar befintlig [!DNL Target] endast profiler. Om du använder v1 skapas inga profiler för att saknas `pcIds` eller `mbox3rdPartyIds`.
* [!UICONTROL Customer attributes] kräver att [!UICONTROL Experience Cloud ID] (ECID) och användningen av ett käll-ID, t.ex. CRM-ID eller Lojalty-ID.
* The [!UICONTROL Bulk Profile Update API] kräver antingen TNT ID eller `mbox3rdPartyId`.
* Du kan inte skicka följande tecken i `mbox3rdPartyID`: plustecken (+) och snedstreck (/).

## Resurs

Mer information finns i:

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)