---
keywords: implementera, implementera, konfigurera, konfigurera, skriptprofilattribut
description: Hämta data till  [!DNL Target] med skriptprofilattribut.
title: Hur hämtar jag data till  [!DNL Target] Använda skriptprofilattribut?
feature: Implementation
exl-id: ba11f1de-e68b-4505-8e3e-cd4d46ef59a2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# Skriptprofilattribut

Skriptprofilattribut är namn/värde-par som definieras i lösningen [!DNL Adobe Target]. Värdet avgörs av om ett JavaScript-fragment körs på målservern per serveranrop.

Användare skriver små kodfragment som körs per mbox-anrop och innan en besökare utvärderas för målgrupps- och aktivitetsmedlemskap.

## Format

Skriptprofilattribut skapas i målgruppsavsnittet. Alla attributnamn är giltiga och värdet är resultatet av en JavaScript-funktion som skrivits av användaren [!DNL Target]. Attributnamnet prefixeras automatiskt av användaren . &quot; in [!DNL Target] för att skilja dem från profilattribut på sidan.

Kodfragmentet är skrivet i JS-språket Rhino och kan referera till tokens och andra värden.

## Exempel på användningsfall

* **Cart Abandonment**: När besökaren kommer till kundvagnen anger du profilskriptet till 1. Återställ besökaren till 0 när besökaren konverterar. Om värdet = 1 har besökaren en artikel i kundvagnen.
* **Besök Antal**: Vid varje nytt besök ökar du antalet med 1 för att hålla reda på hur ofta en besökare återvänder till webbplatsen.

## Fördelar med metoden

Inga sidkodsuppdateringar krävs.

Körs före målgrupps- och aktivitetsmedlemskapsbeslut, så dessa profilskriptattribut kan påverka medlemskapet för ett enda serversamtal.

Kan vara mycket robust. Upp till 2 000 instruktioner kan köras per skript.

## Caveats

Kräver JavaScript kunskap.

Körningsordningen för profilskript kan inte garanteras, så de kan inte vara beroende av varandra.

Kan vara svårt att felsöka.

## Exempel på koder

Profilskript är relativt flexibla:

```
user.purchase_recency: var dayInMillis = 3600 * 24 * 1000; if (mbox.name == 'orderThankyouPage') {  user.setLocal('lastPurchaseTime', new Date().getTime()); } var lastPurchaseTime = user.getLocal('lastPurchaseTime'); if (lastPurchaseTime) {  return ((new Date()).getTime()-lastPurchaseTime)/dayInMillis; }
```

### Länkar till relevant information

[Profilskriptattribut](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html#concept_8C07AEAB0A144FECA8B4FEB091AED4D2)
