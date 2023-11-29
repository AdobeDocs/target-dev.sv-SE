---
keywords: implementera, implementera, konfigurera, konfigurera, uppdatera en profil
description: Hämta data till [!DNL Target] med API:t för uppdatering av en profil.
title: Hur får jag in data på [!DNL Target] Använda API:t för uppdatering av en profil?
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
source-git-commit: 734bda64915a08f2edba37cbbb66b2de581c2237
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 0%

---

# API för enkel profiluppdatering

Nästan identiskt med [!UICONTROL Bulk Profile Update API] in [!DNL Adobe Target], men en besökarprofil uppdateras åt gången, i enlighet med API-anropet i stället för med en CSV-fil.

## Format

Besökaren måste identifieras via [!DNL Target] `mboxPC` värde eller `mbox3rdPartyId` värde. The [!UICONTROL Experience Cloud ID] (ECID) stöds inte.

## Exempel på användningsområden

Du vill uppdatera profilen för en enskild besökare som utför en offlineåtgärd. Åtgärderna kan omfatta att nå en kundtjänst, ett lån finansieras, använda ett förmånskort i butik, få tillgång till en kioskdator och så vidare.

## Fördelar med metoden

* Det finns ingen gräns för antalet profilattribut.*
* Profilattribut som skickas via webbplatsen kan uppdateras via API och tvärtom.

## Caveats

* Gräns på 1 000 000 API-anrop (1 miljon) per 24-timmarsperiod.
* Uppdaterar endast profiler. Det går inte att skapa en profil för en potentiell användare [!DNL Target] har ännu inte sett.
* Uppdateringar sker vanligtvis på mindre än en timme, men kan ta så lång tid som 24 timmar att reflektera.

## Exempel på koder

GET och POST stöds.

```
https://CLIENT.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr1=0&profile.attr2=1...
```

## Resurs

* [Uppdaterar profiler](https://developers.adobetarget.com/api/#updating-profiles)
