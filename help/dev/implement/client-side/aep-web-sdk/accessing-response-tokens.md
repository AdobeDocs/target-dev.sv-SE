---
title: Åtkomst till svarstoken med Adobe Experience Platform Web SDK
description: Lär dig hur du får åtkomst till svarstoken med  [!DNL Adobe Experience Platform Web SDK].
keywords: personalisering;mål;adobe target;renderDecision;sendEvent;DecisionScopes;result.Decision,response tokens;
feature: AEP Web SDK
source-git-commit: f010ca54aac3c2a644a77fb2f88aff1996f6ddfe
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# Åtkomst till svarstoken

Personalization-innehåll som returneras från [!DNL Adobe Target] innehåller [svarstoken](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html), som är information om aktivitet, erbjudande, upplevelse, användarprofil, geoinformation med mera. Dessa uppgifter kan delas med verktyg från tredje part eller användas för felsökning. Svarstoken kan konfigureras i användargränssnittet [!DNL Target].

Om du vill få åtkomst till innehåll för personalisering måste du tillhandahålla en callback-funktion när du skickar en händelse. Det här återanropet anropas efter att SDK har fått ett svar från servern. Återanropet tillhandahålls som ett `result`-objekt, som kan innehålla en `propositions`-egenskap som innehåller returnerat personaliseringsinnehåll. Nedan visas ett exempel på hur du kan tillhandahålla en callback-funktion.

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Manually render propositions
    }
  });
```

I det här exemplet är `result.propositions`, om det finns, en matris som innehåller personaliseringsförslag som är relaterade till händelsen. Mer information om innehållet i [ finns i ](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)Återge anpassat innehåll`result.propositions.`

Anta att du vill samla alla aktivitetsnamn från alla utkast som automatiskt återges av SDK på webben och överföra dem till en enda array. Du kan sedan skicka den enskilda arrayen till en tredje part. I detta fall:

1. Extrahera utdrag från objektet `result`.
1. Slinga igenom varje förslag.
1. Kontrollera om SDK har återgett förslaget.
1. I så fall gör du en slinga genom varje objekt i förslaget.
1. Hämta aktivitetsnamnet från egenskapen `meta`, som är ett objekt som innehåller svarstoken.
1. Placera aktivitetsnamnet i en array.
1. Skicka aktivitetsnamnen till en tredje part.

Koden ser ut så här:

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    var activityNames = [];
    propositions.forEach(function(proposition) {
      if (proposition.renderAttempted) {
        proposition.items.forEach(function(item) {
          if (item.meta) {
            // item.meta contains the response tokens.
            var activityName = item.meta["activity.name"];
            // Ignore duplicates
            if (activityNames.indexOf(activityName) === -1) {
              activityNames.push(activityName);
            }
          }
        });
      }
    });
    // Now that activity names are in an array,
    // you can send them to a third party or use
    // them in some other way.
  });
```
