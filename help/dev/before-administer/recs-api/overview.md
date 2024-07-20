---
title: Vad är Adobe Recommendations API?
description: I den här guiden får utvecklare hjälp med praktiska övningar med hjälp av Adobe Target Recommendations API:er för att konfigurera och hantera Recommendations-kataloger och anpassade villkor, samt med hjälp av API:t för leverans för att hämta rekommendationsinnehåll.
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 0d03c650-0b00-44b8-a794-10e5d738e42c
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Adobe Recommendations API - översikt

API:er som är relevanta för Recommendations inkluderar [Admin API:er](../../before-administer/target-api-overview.md) som gör att du kan:

* Hantera din katalog med rekommenderade produkter eller innehåll
* Hantera dina Recommendations-algoritmer och -aktiviteter

Om du använder [målets leverans-API](../../implement/delivery-api/overview.md) med Recommendations kan du även:

* Hämta rekommendationer i JSON-, HTML- eller XML-objekt så att de kan visas i webben, mobiler, e-post, sakernas internet (IOT) och andra kanaler.

## Beskrivning

I den här handboken om Recommendations API:er får utvecklare hjälp med praktiska övningar med hjälp av Recommendations API:er för att konfigurera och hantera Recommendations-kataloger och anpassade villkor, samt med hjälp av API:t för leverans för att hämta rekommendationer. Till slut kan du

* Konfigurera och hantera entiteter med Recommendations API
* Konfigurera och hantera anpassade villkor med Recommendations API
* Förstå hur du använder Recommendations med leverans-API:t för att använda rekommendationer i andra enheter än HTML

## Målgrupp

Den här handboken är avsedd för utvecklare som inte använder Target API:er eller Recommendations API:er.

## Förutsättningar {#prerequisites}

API:erna för måladministratörer kräver [Adobe-autentiseringsinställning](../configure-authentication.md). Kontrollera att detta är konfigurerat innan du använder Recommendations API.

## Resurs

Observera följande resurser som är nödvändiga för att du ska kunna förstå den här handboken och kunna följa den utan problem:

| Resurs | Information |
| --- | --- |
| Postman | Hämta [Postman-appen](https://www.postman.com/downloads/) för ditt operativsystem. Postman Basic är kostnadsfritt när man skapar konto. Postman är inte nödvändigt för att kunna använda Adobe Target API:er i allmänhet, men gör API-arbetsflöden enklare, och Adobe Target tillhandahåller flera Postman-samlingar som hjälper dig att köra API:erna och lära dig hur de fungerar. Resten av guiden förutsätter att man har kunskap om Postman. Mer information finns i [Postman-dokumentationen](https://learning.getpostman.com/). |
| Referenser | Du bör känna till följande resurser under resten av den här guiden:<UL><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Dokumentation för måladministratör och profil-API](../../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API-dokumentation](https://developer.adobe.com/target/administer/recommendations-api/)</li></UL> |
