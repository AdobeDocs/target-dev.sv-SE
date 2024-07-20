---
title: Adobe Target API för uppdatering av enstaka profil
description: Lär dig hur du använder  [!DNL Adobe Target] [!UICONTROL Single Profile Update API] för att skicka en enskild besökares profildata till  [!DNL Target].
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 4e022db3-215f-461b-9222-38ce2f2dbc28
source-git-commit: e2462d12cf58ab5a588c13a96df5e6abafb9d675
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---

# [!DNL Adobe Target Single Profile Update API]

Med [!DNL Adobe Target] [!UICONTROL Single Profile Update API] kan du skicka en profiluppdatering för en enskild användare. [!UICONTROL Single Profile Update API] är nästan identisk med [!UICONTROL Bulk Profile Update API], men en besökarprofil uppdateras åt gången, i linje med API-anropet i stället för med en .cvs-fil.

[!UICONTROL Single Profile Update API] och används vanligtvis när en uppdatering måste ske i relation till en transaktion som inträffar i en kanal som inte har implementerat [!DNL Target]. Du vill till exempel uppdatera profilen för en enskild besökare som utför en offlineåtgärd. Åtgärderna kan omfatta att nå en kundtjänst, ett lån finansieras, använda ett förmånskort i butik, få tillgång till en kioskdator och så vidare.

Fördelarna med [!UICONTROL Single Profile Update API] är:

* Det finns ingen gräns för antalet profilattribut.
* Profilattribut som skickas via webbplatsen kan uppdateras via API och tvärtom.

## Caveats

* [!UICONTROL Single Profile Update API] är begränsad till att utföra 1 miljon uppdateringar under en 24-timmarsperiod.
* Uppdateringar sker vanligtvis på mindre än en timme, men kan ta så lång tid som 24 timmar att reflektera.

  Om du måste skicka fler uppdateringar eller begära att uppdateringar bearbetas i kortare tidsramar kan det vara bra att skicka profiluppdateringar via klientuppdatering (rekommenderas) eller via [!DNL Adobe Target] server-side [Delivery API](/help/dev/implement/delivery-api/overview.md).

* [!UICONTROL Single Profile Update API] är ett server-till-server-API och är inte utformat för att fungera inom en webbsida. Om du vill uppdatera en besökarprofil inifrån webbsidan kan du använda funktionen [trackEvent()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) eller [leverans-API](/help/dev/implement/delivery-api/overview.md).

## Format

Ange profilparametrarna i formatet `profile.paramName=value`.

Om du vill uppdatera profilen för en `pcId` använder du:

``````
https://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr=0&profile.attr2=1...
``````

Om du vill uppdatera profilen för en `mbox3rdPartyId` använder du:

``````
shell http://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mbox3rdPartyId=123456&profile.attr=0&profile.attr2=1...
``````

[!UICONTROL Single Profile Update API] är endast till för uppdateringar. Om inget hittas skapas ingen profil.

## Anteckningar

* Parametrar och värden måste vara URL-kodade med UTF-8.
* Parameterformatet är `profile.paramName`.
* Alla parametervärden måste inte finnas för alla pcIds och mbox3rdPartyIds.
* Parametrar och värden är versalkänsliga.
* Både GET och POST stöds.
* Begränsningen för GET är för närvarande 8 kB och 60 kB för POST.

## Svar

Ett exempelsvar för ovanstående begäranden ser ut så här:

`trueRequest successfully submitted`

Detta svar anger att svaret har skickats och kommer att behandlas snart.
