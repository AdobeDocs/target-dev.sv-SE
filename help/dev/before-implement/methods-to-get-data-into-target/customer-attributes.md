---
keywords: implementera, implementera, konfigurera, konfigurera, kundattribut
description: Hämta data till [!DNL Target] med kundattribut.
title: Hur hämtar jag data till  [!DNL Target] med hjälp av kundattribut?
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Kundattribut

Med kundattribut kan du överföra besökarprofildata via FTP till [!DNL Adobe Experience Cloud]. Använd data i [!DNL Adobe Analytics] och [!DNL Adobe Target] när de har överförts.

Target Standard-kunder kan använda fem attribut, [!DNL Target Premium]-kunder kan använda 200 attribut.

## Format

En CSV-fil med [!DNL Experience Cloud] ID:n (ECID:n) och attributnamn/värde-par överförs via FTP eller manuellt i användargränssnittet i Experience Cloud.

## Exempel på användningsområden

CRM eller något annat internt system lagrar värdefull information som du vill dela med [!DNL Adobe Experience Cloud], inklusive [!DNL Target] och [!DNL Analytics].

## Fördelar med metoden

När du överför kunddata skapas en profilpost för besökaren i Target, även om [!DNL Target] ännu inte har sett besökaren.

Samma data är automatiskt tillgängliga i [!DNL Target] och [!DNL Analytics].

FTP kan vara en enklare implementeringsmetod än API.

## Caveats

Target Standard-kunder kan använda fem attribut, [!DNL Target Premium]-kunder kan använda 200 attribut

Kan bara uppdatera värden via kundattribut, inte på sidan.

Kräver implementering av Experience Cloud ID (ECID).

## Exempel på koder

Information finns i [Skapa en kundattributkälla och överför datafilen](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=sv-SE).

### Länkar till relevant information

[Skapa en kundattributkälla och överför datafilen](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=sv-SE).
