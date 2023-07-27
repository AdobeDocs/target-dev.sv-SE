---
keywords: implementera, implementera, konfigurera, konfigurera, kundattribut
description: Hämta data till [!DNL Target] med kundattribut.
title: Hur får jag in data på [!DNL Target] Använda kundattribut?
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Kundattribut

Med kundattribut kan du överföra besökarprofildata via FTP till [!DNL Adobe Experience Cloud]. Använd data i [!DNL Adobe Analytics] och [!DNL Adobe Target].

Target Standard-kunder kan använda fem attribut, [!DNL Target Premium] kunder kan använda 200 attribut.

## Format

En CSV-fil med [!DNL Experience Cloud] ID (ECID) och attributnamn/värde-par överförs via FTP eller manuellt i användargränssnittet för Experience Cloud.

## Exempel på användningsområden

CRM-systemet eller andra interna system lagrar värdefull information som du vill dela med [!DNL Adobe Experience Cloud], inklusive [!DNL Target] och [!DNL Analytics].

## Fördelar med metoden

När kunddata överförs skapas en profilpost för besökaren i Target, även om [!DNL Target] har ännu inte sett besökaren.

Samma data är automatiskt tillgängliga i [!DNL Target] och [!DNL Analytics].

FTP kan vara en enklare implementeringsmetod än API.

## Caveats

Target Standard-kunder kan använda fem attribut, [!DNL Target Premium] kunder kan använda 200 attribut

Kan bara uppdatera värden via kundattribut, inte på sidan.

Kräver implementering av Experience Cloud ID (ECID).

## Exempel på koder

Information finns i [Skapa en kundattributkälla och överför datafilen](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html).

### Länkar till relevant information

[Skapa en kundattributkälla och överför datafilen](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html).
