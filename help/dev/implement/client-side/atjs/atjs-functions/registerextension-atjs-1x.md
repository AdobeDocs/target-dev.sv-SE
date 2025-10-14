---
keywords: registerExtension, registerextension, register extension, register extension, at.js, functions, function, clientCode, serverDomain, globalMboxName, globalMboxAutoCreate, timeout, registerExtension2
description: Använd funktionen [!UICONTROL registerExtension()] för JavaScript-biblioteket  [!DNL Adobe Target]  at.js för att registrera ett specifikt tillägg. (at.js 1.x)
title: Hur använder jag funktionen [!UICONTROL registerExtension()]?
feature: at.js
exl-id: 71decf00-84c5-4914-b0cd-bb061fa6265f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# [!UICONTROL registerExtension()] - at.js 1.x

Tillhandahåller ett standardsätt att registrera ett specifikt tillägg.

>[!NOTE]
>
>Den här funktionen är tillgänglig för version 1 av at.js.Endast *x*. Den här funktionen har ersatts med versionen av at.js 2.*x*. Den här funktionen returnerar standardinnehåll om den används med at.js 2.x.

Alternativparametern är obligatorisk och har följande struktur:

| Nyckel | Typ | Obligatoriskt | Beskrivning |
|--- |--- |--- |--- |
| name | Sträng | Ja | Tilläggsnamn. |
| moduler | Array[String] | Ja | En array med strängar som representerar begärda modulnamn. |
| register | Funktion | Ja | En funktion som används för att initiera och skapa tillägget. Den här funktionen tar emot argument baserade på modularrayen. |

Anteckningar:

* Om en av parametrarna inte anges genereras ett undantag.
* Om modularrayen är tom genereras ett undantag.

Mer information och exempel på hur du använder `[!UICONTROL registerExtension]` finns på sidan [&#x200B; Adobe Experience Cloud Target atjs Extensions &#x200B;](https://github.com/Adobe-Marketing-Cloud/target-atjs-extensions) på GitHub.

## Metoder för inställningsmodulen

| Nyckel | Typ | Beskrivning |
|--- |--- |--- |
| clientCode | Sträng | Klientkod |
| serverDomain | Sträng | Edge serverdomän |
| globalMboxName | Sträng | [!DNL Target] globalt mbox-namn |
| globalMboxAutoCreate | Boolean | Anger om autoskapa är aktiverat eller inte |
| timeout | Nummer | Timeout för begäran |

## Metoder för loggningsmodul

| Nyckel | Typ | Beskrivning |
|--- |--- |--- |
| logg | Funktion | Loggar variabellistan med argument till webbläsarkonsolen, om den finns. Den aktiveras bara när `mboxDebug=true` skickas till URL:en. |
| fel | Funktion | Loggar variabellistan med argument till webbläsarkonsolen. Den aktiveras endast när det finns allvarliga fel, som t.ex. nätverkstimeout, HTML-nod som inte hittas osv. |
