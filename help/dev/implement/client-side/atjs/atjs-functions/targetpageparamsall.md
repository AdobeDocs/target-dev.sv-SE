---
keywords: targetPageParamsAll, targetingPageparamsall, PageParamsAll, pageparamsall, page params, page parameters, at.js, functions, function, targetPageParamsAll0
description: Använd funktionen [!UICONTROL targetPageParamsAll()] för JavaScript-biblioteket  [!DNL Adobe Target]  at.js för att koppla parametrar till alla mbox-filer utanför begärandekoden.
title: Hur använder jag funktionen [!UICONTROL targetPageParamsAll()]?
feature: at.js
exl-id: 32045e60-6904-42a1-bf71-fd7e167a829f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# [!UICONTROL targetPageParamsAll()]

Med den här metoden kan du bifoga parametrar till alla rutor utanför begärandekoden.

Detta är mycket användbart om du vill inkludera samma uppsättning parametrar för flera mbox-anrop. Funktionen måste definieras av kunden. Den ska returnera en array med parametrar som skickas till alla mbox-begäranden på sidan. Den här funktionen kan definieras innan at.js har lästs in eller i **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit]** > **[!UICONTROL Code Settings]** > **[!UICONTROL Library Header]**.

Du kan skicka in parametrar till target-global-mbox med funktionen [!UICONTROL targetPageParamsAll()] på något av följande sätt:

* En avgränsad lista
* En array
* Ett JSON-objekt

## Exempel

Et-avgränsad lista (värdena måste vara URL-kodade):

```javascript {line-numbers="true"}
function targetPageParamsAll() { 
    return "param1=value1&param2=value2&p3=hello%20world"; 
}
```

Array (värdena behöver inte vara URL-kodade):

```javascript {line-numbers="true"}
targetPageParamsAll = function() { 
     return ["a=1", "b=2", "c=hello world"]; 
};
```

JSON (värdena behöver inte vara URL-kodade):

```javascript {line-numbers="true"}
targetPageParamsAll = function() { 
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
