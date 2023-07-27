---
keywords: adobe.target.getOffer, getOffer, getoffer, get offer, at.js, functions, function, $8
description: Använd [!UICONTROL adobe.target.getOffer()] och dess alternativ för [!DNL Adobe Target] at.js-biblioteket som ska köra förfrågningar om att få [!DNL Target] erbjudande.
title: Hur jag använder [!UICONTROL adobe.target.getOffer()] Funktion?
feature: at.js
exl-id: 7b917d42-06e8-4838-a09d-0c4872c9beaa
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 0%

---

# [!DNL adobe.target.getOffer(options)]

Den här funktionen utlöser en begäran om att få en [!DNL Target] erbjudande.

Använd med `[!UICONTROL adobe.target.applyOffer()]` för att bearbeta svaret eller använda din egen hantering av lyckade resultat. Alternativparametern är obligatorisk och har följande struktur:

| Nyckel | Typ | Obligatoriskt | Beskrivning |
|--- |--- |--- |--- |
| mbox | Sträng | Ja | Namn på mbox |
| parametrar | Objekt | Nej | Mbox-parametrar. Ett objekt med nyckelvärdepar som har följande struktur:<P>`{ "param1": "value1", "param2": "value2"}` |
| framgång |  -funktion | Ja | Återanrop som ska köras när vi får ett svar från servern. Callback-funktionen för lyckade försök får en enda parameter som representerar en array med objekt för erbjudanden. Här är ett exempel på lyckad återanrop:<P>`function handleSuccess(response){......}`<P>Mer information finns i Svaren nedan. |
| fel |  -funktion | Ja | Återanrop som ska utföras när ett fel inträffar. Det finns några fall som anses felaktiga:<ul><li>HTTP-statuskoden skiljer sig från 200 OK</li><li>Svaret kan inte tolkas. Vi har till exempel dåligt konstruerat JSON eller HTML i stället för JSON.</li><li>Svaret innehåller felnyckeln. Ett undantag utlöstes till exempel på kanten som en begäran inte kunde behandlas korrekt. Ett fel kan uppstå när en mbox är blockerad och vi inte kan hämta något innehåll för den, osv. Återanropsfunktionen för fel får två parametrar: status och fel. Här är ett exempel på ett fel vid återanrop: `function handleError(status, error){......}`</li></ul>Mer information finns i Felsvar nedan. |
| timeout | Nummer | Nej | Timeout i millisekunder. Om inget anges används standardtidsgränsen i at.js.<P>Standardtidsgränsen kan anges från [!DNL Target] Användargränssnitt under [!UICONTROL Administration] > [!UICONTROL Implementation]. |

## Exempel

Lägga till parametrar med [!UICONTROL getOffer()] och använda [!UICONTROL applyOffer()] för lyckad hantering:

```javascript {line-numbers="true"}
adobe.target.getOffer({   
  "mbox": "target-global-mbox", 
  "params": { 
     "a": 1, 
     "b": 2 
  }, 
  "success": function(offer) {           
        adobe.target.applyOffer( {  
           "mbox": "target-global-mbox", 
           "offer": offer  
        } ); 
  },   
  "error": function(status, error) {           
      console.log('Error', status, error); 
  } 
});
```

Lägga till parametrar och profilparametrar med [!UICONTROL getOffer()] och använda [!UICONTROL applyOffer()] för lyckad hantering:

```javascript {line-numbers="true"}
adobe.target.getOffer({   
  "mbox": "target-global-mbox", 
  "params": { 
     "a": 1, 
     "b": 2, 
     "profile.age": 27, 
     "profile.gender": "male" 
  }, 
  "success": function(offer) {           
        adobe.target.applyOffer( {  
           "mbox": "target-global-mbox", 
           "offer": offer  
        } ); 
  },   
  "error": function(status, error) {           
      console.log('Error', status, error); 
  } 
});
```

Använda anpassad tidsgräns och anpassad hantering av lyckade resultat med [!UICONTROL getOffer()]:

&quot;YOUR_OWN_CUSTOM_HANDLING_FUNCTION&quot; är en platshållare för en funktion som kunden skulle definiera.

```javascript {line-numbers="true"}
adobe.target.getOffer({     
  "mbox": "target-global-mbox",   
  "success": function(offer) { 
    YOUR_OWN_CUSTOM_HANDLING_FUNCTION(offer);   
  }, 
  "error": function(status, error) {                 
    console.log('Error', status, error);   
  },   
  "timeout": 2000 
});
```

## Svar

Svarsparametern som skickas till motringningen om att åtgärden lyckades är en array med åtgärder. En åtgärd är ett objekt som vanligtvis har följande format:

| Namn | Typ | Beskrivning |
|--- |--- |--- |
| åtgärd | Sträng | Typ av åtgärd som ska tillämpas på det identifierade elementet. |
| väljare | Sting | Representerar en enkelväljare. |
| cssSelector | Sträng | Inbyggd DOM-väljare, används för att dölja element. |
| innehåll | Sträng | Det innehåll som ska användas på det identifierade elementet. |

## Exempel

```javascript {line-numbers="true"}
{ 
    "sessionId": "1444512212156-384616", 
    "tntId": "1444512212156-384616.17_35", 
    "offers": [{ 
        "plugins": ["<script type=\"text/javascript\">\r\n/*mboxHighlight+ (1of2) v1 ==> Response Plugin*/\r\nwindow.ttMETA=(typeof(window.ttMETA)!='undefined')?window.ttMETA:[];window.ttMETA.push({'mbox':'target-global-mbox','campaign':'at: redirect ootb','experience':'Experience B','offer':'/at_redirect_ootb/experiences/1/pages/0/1442082890250'});window.ttMBX=function(x){var mbxList=[];for(i=0;i<ttMETA.length;i++){if(ttMETA[i].mbox==x.getName()){mbxList.push(ttMETA[i])}}return mbxList[x.getId()]}\r\n</script>"], 
        "actions": { 
            "content": [{ 
                "passMboxSession": false, 
                "selector": "body", 
                "action": "redirect", 
                "url": "https://example.com/04.html", 
                "includeAllUrlParameters": true 
            }] 
        } 
    }] 
}
```

## Felsvar

Parametrarna &quot;status&quot; och &quot;error&quot; som skickas till återanropet har följande format:

| Namn | Typ | Beskrivning |
|--- |--- |--- |
| status | Sträng | Representerar felstatusen. Den här parametern kan ha följande värden:<ul><li>timeout: Anger att tidsgränsen för begäran uppnåddes.</li><li>parseerror: Anger att svaret inte kan tolkas, till exempel om vi får HTML eller oformaterad text i stället för JSON.</li><li>fel: Anger ett allmänt fel som om vi fick en annan HTTP-status än 200 OK</li></ul> |
| fel | Sträng | Innehåller ytterligare data, t.ex. ett undantagsmeddelande eller något annat som kan vara användbart vid felsökning. |
