---
keywords: implementera, implementera, konfigurera, konfigurera, page-parameter
description: Hämta data till [!DNL Target] med hjälp av profilattribut på sidan.
title: Hur får jag in data på [!DNL Target] Använda profilattribut på sidan?
feature: Implementation
exl-id: c19fd746-21a2-4eb5-8c2a-c24806e09324
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Profilattribut på sidan

Profilattribut på sidan i [!DNL Adobe Target] (kallas även&quot;in-mbox profile attributes&quot;) är namn/värde-par som skickas direkt via sidkod och som lagras i besökarens profil för framtida bruk.

Med profilattribut på sidan kan användarspecifika data lagras i Target-profilen för senare målinriktning och segmentering.

## Format

Profilattribut på sidan skickas till [!DNL Target] via ett serveranrop som ett strängnamn/värde-par med prefixet &quot;profile&quot;. före attributnamnet.

Attributnamn och värden är anpassningsbara (även om det finns &quot;reserverade namn&quot; för vissa användningsområden).

Här är några exempel på profilattribut på sidan:

* `profile.membershipLevel=silver`
* `profile.visitCount=3`

## Exempel på användningsområden

* **Inloggningsinformation**: Dela icke-PII-data (personligt identifierbar information) till [!DNL Target] baserat på användarens inloggning. Dessa data kan vara medlemskapsstatus, orderhistorik eller mer.
* **Butiksinformation**: Spåra vilket arkiv som är användarens föredragna plats.
* **Tidigare interaktioner**: Spåra vad användaren har gjort på webbplatsen tidigare för att informera om framtida personalisering.

## Fördelar med metoden

Data skickas till [!DNL Target] i realtid och kan användas på samma serveranrop som data kommer in i.

## Caveats

Kräver uppdatering av sidkod (direkt eller via ett tagghanteringssystem).

Attribut och värden visas vid serveranrop, så att besökaren kan se värdena. Om du delar information som kreditintervall eller annan potentiellt privat information kanske den här metoden inte är den bästa metoden.

## Exempel på koder

targetPageParamsAll (bifogar attributen till alla mbox-anrop på sidan):

`function targetPageParamsAll() { return "profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

targetPageParams (appends the attributes to the global mbox on the page):

`function targetPageParams() { return profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

Attribut i mboxSkapa kod:

`<div class="mboxDefault"> default content to replace by offer </div> <script> mboxCreate('mboxName','profile.param1=value1','profile.param2=value2'); </script>`

## Länkar till relevant information

[Profilattribut](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html)

[Besökarprofil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html)
