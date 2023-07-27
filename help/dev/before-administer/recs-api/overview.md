---
title: Vad är Adobe Recommendations API?
description: I den här guiden får utvecklare hjälp med praktiska övningar med hjälp av Adobe Target Recommendations API:er för att konfigurera och hantera Recommendations-kataloger och anpassade villkor, samt med hjälp av API:t för leverans för att hämta rekommendationsinnehåll.
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 0d03c650-0b00-44b8-a794-10e5d738e42c
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 0%

---

# Adobe Recommendations API - översikt

API:er som är relevanta för Recommendations inkluderar [Administratörs-API:er](../../before-administer/target-api-overview.md) som gör att du kan:

* Hantera din katalog med rekommenderade produkter eller innehåll
* Hantera dina Recommendations-algoritmer och -aktiviteter

Använda målet [Leverans-API](../../implement/delivery-api/overview.md) Med Recommendations kan man också

* Hämta rekommendationer i JSON-, HTML- eller XML-objekt så att de kan visas i webben, mobiler, e-post, sakernas internet (IOT) och andra kanaler.

## Beskrivning

I den här handboken om Recommendations API:er får utvecklare hjälp med praktiska övningar där Recommendations API:er används för att konfigurera och hantera Recommendations-kataloger och anpassade villkor, samt hur man använder Delivery API för att hämta rekommendationer. Till slut kan du

* Konfigurera och hantera entiteter med Recommendations API
* Konfigurera och hantera anpassade villkor med Recommendations API
* Förstå hur du använder Recommendations med leverans-API:t för att använda rekommendationer i andra enheter än HTML

## Målgrupp

Den här handboken är avsedd för utvecklare som inte använder Target API:er eller Recommendations API:er.

## Förutsättningar {#prerequisites}

Måladministratörs-API:erna kräver [Inställningar för autentisering i Adobe](../configure-authentication.md). Kontrollera att detta är konfigurerat innan du använder Recommendations API.

## Resurs

Observera följande resurser som är nödvändiga för att du ska kunna förstå den här handboken och kunna följa den utan problem:

| Resurs | Information |
| --- | --- |
| Postman | Skaffa [Postman](https://www.postman.com/downloads/) för ditt operativsystem. Postman Basic är kostnadsfritt när man skapar konto. Postman är inte nödvändigt för att kunna använda Adobe Target API:er i allmänhet, men gör API-arbetsflöden enklare, och Adobe Target tillhandahåller flera Postman-samlingar som hjälper dig att köra API:erna och lära dig hur de fungerar. Resten av guiden förutsätter att man har kunskap om Postman. Om du behöver hjälp kan du läsa [Postman-dokumentation](https://learning.getpostman.com/). |
| Referenser | Du bör känna till följande resurser under resten av den här guiden:<UL><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Dokumentation för Target Admin och Profile API](../../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API-dokumentation](https://developers.adobetarget.com/api/recommendations/)</li></UL> |
