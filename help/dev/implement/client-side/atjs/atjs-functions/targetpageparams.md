---
keywords: targetPageParams, targetPageparams, pageParams, pageparams, page params, page parameters, at.js, functions, function, targetPageParams0
description: Använd [!UICONTROL targetPageParams()] funktionen för [!DNL Adobe Target] at.js JavaScript-bibliotek för att koppla parametrar till den globala mbox utanför begärandekoden.
title: Hur jag använder [!UICONTROL targetPageParams()] Funktion?
feature: at.js
exl-id: 274e4d1f-843a-443b-ad98-7139dc4a13f8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# [!UICONTROL targetPageParams()]

Med den här metoden kan du bifoga parametrar till den globala mbox utanför begärandekoden.

Den här funktionen är mycket användbar om du vill ta med samma uppsättning parametrar för flera mbox-anrop. Funktionen måste definieras av kunden. Den ska returnera en array med parametrar som bara skickas till den globala mbox-begäran. Funktionen kan definieras innan at.js läses in eller **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit]** > **[!UICONTROL Library Header]**.

Du kan skicka in parametrar till target-global-mbox med `[!UICONTROL targetPageParams()]` på något av följande sätt:

* En avgränsad lista
* En array
* Ett JSON-objekt

## Exempel

Et-avgränsad lista (värdena måste vara URL-kodade):

```javascript {line-numbers="true"}
function targetPageParams() { 
    return "param1=value1&param2=value2&p3=hello%20world"; 
}
```

Array (värdena behöver inte vara URL-kodade):

```javascript {line-numbers="true"}
targetPageParams = function() { 
     return ["a=1", "b=2", "c=hello world"]; 
};
```

JSON (värdena behöver inte vara URL-kodade):

```javascript {line-numbers="true"}
targetPageParams = function() { 
  return { 
    "a": 1, 
    "b": 2, 
    "profile": { 
        "age": 26, 
        "country": { 
          "city": "San Francisco" 
        } 
      } 
  }; 
};
```
