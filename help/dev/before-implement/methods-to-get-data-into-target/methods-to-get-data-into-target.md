---
keywords: implementera, implementera, konfigurera, konfigurera, page parameter, tomcat, url-kodad, in-page profile attribute, mbox parameter, in-page profile attributes, script profile attribute, bulk profile update API, single file update API, customer attributes, implementera5, implementera6, implementera7, implementera9, implementera9, implementera1, implementera2, implement3, implementera4, implementera5, data providers, data provider
description: Hämta data till [!DNL Target] (sidparametrar, profilattribut, skriptprofilattribut, dataleverantörer, uppdaterings-API:er för en och flera profiler, kundattribut).
title: Hur får jag in data i Target?
feature: Implementation
exl-id: a54e306a-ea8e-4d3f-bc5d-b5895b6b9a84
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# Översikt över metoder

Information om de olika metoder du kan använda för att hämta data till Adobe Target.

Tillgängliga metoder:

| Metod | Information |
| --- | --- |
| [Sidparametrar](page-parameters.md)<br />(Kallas även&quot;mbox parameters&quot;) | Sidparametrar är namn/värde-par som skickas direkt via sidkod och som inte lagras i besökarens profil för framtida bruk.<br />Sidparametrar är användbara när du vill skicka siddata till [!DNL Target] som inte behöver lagras med besökarens profil för framtida målinriktning. Dessa värden används i stället för att beskriva sidan eller den åtgärd som användaren utförde på den specifika sidan. |
| [Profilattribut på sidan](in-page-profile-attributes.md)<br />(kallas även&quot;in-mbox profile attributes&quot;) | Profilattribut på sidan är namn/värde-par som skickas direkt via sidkoden och som lagras i besökarens profil för framtida bruk.<br />Med profilattribut på sidan kan användarspecifika data lagras i Target-profilen för senare målinriktning och segmentering. |
| [Skriptprofilattribut](script-profile-attributes.md) | Skriptprofilattributen är namn/värde-par som definieras i [!DNL Target] lösning. Värdet avgörs av om ett JavaScript-fragment körs på målservern per serveranrop.<br />Användare skriver små kodfragment som körs per mbox-anrop och innan en besökare utvärderas för målgrupps- och aktivitetsmedlemskap. |
| [Dataleverantörer](data-providers.md) | Med dataleverantörer kan ni enkelt skicka data från tredje part till Target. |
| [API för gruppprofilsuppdatering](bulk-profile-update-api.md) | Skicka en CSV-fil till via API [!DNL Target] med uppdaterade besökarprofiler för många besökare. Varje besökarprofil kan uppdateras med flera profilattribut på sidan i ett anrop. |
| [API för enkel profiluppdatering](single-profile-update-api.md) | Nästan identiskt med API:t för uppdatering av gruppprofil, men en besökarprofil uppdateras åt gången, i enlighet med API-anropet i stället för med en CSV-fil. |
| [Kundattribut](customer-attributes.md) | Med kundattribut kan du överföra besökarprofildata via FTP till Experience Cloud. Använd data i Adobe Analytics och Adobe Target när de har överförts. |
