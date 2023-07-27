---
keywords: adobe.target.applyOffer, applyOffer, applyoffer, apply offer, at.js, functions, function, $8
description: Använd [!UICONTROL adobe.target.applyOffer()] funktionen för [!DNL Adobe Target] at.js JavaScript-bibliotek för att tillämpa svarsinnehållet.
title: Hur jag använder [!UICONTROL adobe.target.applyOffer()] Funktion?
feature: at.js
exl-id: 957bbe92-8012-4bd5-95d6-1ae38b72bb16
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---

# [!UICONTROL adobe.target.applyOffer(options)]

Den här funktionen används för att tillämpa svarsinnehållet med [!DNL Adobe Target].

>[!NOTE]
>
>`applyOffer` kräver `mbox` parameter. Om inget mbox-namn anges inträffar ett fel.

Alternativparametern är obligatorisk och har följande struktur:

| Nyckel | Typ | Obligatoriskt | Beskrivning |
|--- |--- |--- |--- |
| mbox | Sträng | Ja | Namn på mbox<br />Med at.js 1.3.0 (och senare) Target tvingas mbox-nyckeln att användas. Den här nyckeln har krävts tidigare, men Target använder den nu för att säkerställa att Target har korrekt validering och att kunderna använder funktionen korrekt. |
| väljare | Sträng- eller DOM-element | Nej | HTML-element eller CSS-väljare som används för att identifiera det HTML-element där Target ska placera erbjudandeinnehållet. Om ingen väljare anges förutsätter Target att elementet HTML ska använda HTML HEAD. |
| Erbjudande | Array | Ja | En array-åtgärd som ska tillämpas på elementet. |

## Exempel

I följande exempel visas hur du använder `[!UICONTROL getOffer]` och `[!UICONTROL applyOffer]` tillsammans:

```javascript {line-numbers="true"}
adobe.target.getOffer({   
  "mbox": "mbox",   
  "success": function(offers) {           
        adobe.target.applyOffer( {  
           "mbox": "mbox", 
           "offer": offers  
        } ); 
  },   
  "error": function(status, error) {           
      if (console && console.log) { 
        console.log(status); 
        console.log(error); 
      } 
  }, 
 "timeout": 5000 
}); 
```
