---
title: Programimplementering på en sida för [!DNL &#x200B; Adobe Experience Platform Web SDK]
description: Lär dig hur du skapar en SPA-implementering av  [!DNL Adobe Experience Platform Web SDK]med  [!DNL Target].
keywords: mål;adobe target;xdm views; views;single page applications;SPA;SPA lifecycle;client-side;AB testing;AB;Experience targeting;XT;VEC
feature: AEP Web SDK
source-git-commit: f4e0e1b202863eb9fddb40836cf53d2df7cdbebe
workflow-type: tm+mt
source-wordcount: '1680'
ht-degree: 0%

---


# Programimplementering på en sida

[!DNL Adobe Experience Platform Web SDK] innehåller omfattande funktioner som gör det möjligt för ditt företag att utföra personalisering på nästa generations klienttekniker, som SPA (Single-page applications).

Traditionella webbplatser använder navigeringsmodellerna &quot;Page-to-Page&quot;, som också kallas för Multi-Page Applications. På dessa webbplatser är designen nära länkad till URL:er, och om du förflyttar dig mellan sidorna krävs en full sidladdning.

Moderna webbprogram, till exempel ensidiga program, har i stället antagit en modell som möjliggör snabb användning av webbläsargränssnittsåtergivning, som ofta är oberoende av sidomladdning. De här upplevelserna triggas av kundinteraktioner som rullningar, klick och markörrörelser. I takt med att de moderna webbens paradigmer har utvecklats fungerar inte längre de traditionella generiska eventernas relevans, till exempel en sidladdning, för personalisering och experimenterande.

![Diagram som visar SPA-livscykeln jämfört med traditionell sidlivscykel.](/help/dev/implement/client-side/aep-web-sdk/assets/spa-vs-traditional-lifecycle.png)

## Fördelar med [!DNL Experience Platform Web SDK] för SPA

Här är några fördelar med att använda [!DNL Adobe Experience Platform Web SDK] för dina enkelsidiga program:

* Möjlighet att cachelagra alla erbjudanden vid sidinläsning för att minska antalet serveranrop till ett enda serveranrop.
* Förbättra användarupplevelsen på webbplatsen eftersom erbjudandena visas direkt via cachen utan den fördröjning som traditionella serversamtal ger.
* Med en enda kodrad och en engångskonfiguration för utvecklare kan marknadsförarna skapa och köra [!UICONTROL A/B Test]- och [!UICONTROL Experience Targeting] (XT)-aktiviteter via [!UICONTROL Visual Experience Composer] (VEC) på ert SPA.

## XDM-vyer och enkelsidiga program

VEC [!UICONTROL Adobe Target] för SPA utnyttjar konceptet [!UICONTROL Views]: en logisk grupp visuella element som tillsammans utgör en SPA-upplevelse. Ett enkelsidigt program kan därför betraktas som en övergång genom vyer i stället för URL-adresser som baseras på användarinteraktioner. En [!UICONTROL View] kan vanligtvis representera en hel plats eller grupperade visuella element inom en plats.

För att ytterligare förklara vad vyer är använder följande exempel en hypotetisk e-handelsplats online som implementerats i [!DNL React] för att utforska exempel [!UICONTROL Views].

När du har navigerat till hemsidan befordrar en hjältebild en påskaffär samt de senaste produkterna på webbplatsen. I det här fallet kan en [!UICONTROL View] definieras för hela hemskärmen. [!UICONTROL View] kan helt enkelt kallas &quot;home&quot;.

![Exempelbild av ett enkelsidigt program i ett webbläsarfönster.](/help/dev/implement/client-side/aep-web-sdk/assets/example-views.png)

När kunden blir mer intresserad av de produkter som företaget säljer bestämmer de sig för att klicka på länken **Produkter**. På samma sätt som hemsidan kan hela produktwebbplatsen definieras som [!UICONTROL View]. [!UICONTROL View] kan heta &quot;products-all&quot;.

![Exempelbild av ett enkelsidigt program i ett webbläsarfönster, med alla produkter synliga.](/help/dev/implement/client-side/aep-web-sdk/assets/example-products-all.png)

Eftersom [!UICONTROL View] kan definieras som en hel webbplats eller som en grupp visuella element på en webbplats. De fyra produkterna som visas på produktwebbplatsen kan grupperas och betraktas som en [!UICONTROL View]. Den här vyn kan heta &quot;products&quot;.

![Exempelbild av ett enkelsidigt program i ett webbläsarfönster med exempelprodukter.](/help/dev/implement/client-side/aep-web-sdk/assets/example-products.png)

När kunden bestämmer sig för att klicka på knappen **Läs in mer** för att utforska fler produkter på webbplatsen ändras inte webbplatsens URL i det här fallet. En [!UICONTROL View] kan dock skapas här för att representera endast den andra produktraden som visas. Namnet [!UICONTROL View] kan vara &quot;products-page-2&quot;.

![Exempelbild av ett enkelsidigt program i ett webbläsarfönster, där exempelprodukter visas på ytterligare en sida.](/help/dev/implement/client-side/aep-web-sdk/assets/example-load-more.png)

Kunden bestämmer sig för att köpa några produkter från sajten och går vidare till kassan. På kassan får kunden möjlighet att välja normal leverans eller expressleverans. En [!UICONTROL View] kan vara vilken grupp som helst av visuella element på en webbplats, så en [!UICONTROL View] kan skapas för leveransinställningar och kallas&quot;Leveransinställningar&quot;.

![Exempelbild av en enkelsidig programutcheckningssida i ett webbläsarfönster.](/help/dev/implement/client-side/aep-web-sdk/assets/example-check-out.png)

Konceptet för [!UICONTROL Views] kan utökas mycket mer än det här scenariot. Dessa scenarier är bara några exempel på [!UICONTROL Views] som kan definieras på en plats.

## Implementerar [!UICONTROL XDM Views]

[!UICONTROL XDM Views] kan utnyttjas i [!DNL Target] för att marknadsförarna ska kunna köra A/B- och XT-tester på SPA via [!UICONTROL Visual Experience Composer]. För att göra detta måste du utföra följande steg för att slutföra en engångskonfiguration av utvecklare:

1. Installera [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/sv/docs/experience-platform/web-sdk/install/overview).
2. Identifiera alla [!UICONTROL XDM Views] i ditt enkelsidiga program som du vill anpassa.
3. När du har definierat [!UICONTROL XDM Views] ska du implementera funktionen `sendEvent()` med `renderDecisions` inställd på `true` och motsvarande [!UICONTROL XDM View] i Single Page-programmet för att leverera A/B- eller XT VEC-aktiviteter. [!UICONTROL XDM View] måste skickas i `xdm.web.webPageDetails.viewName`. I det här steget kan marknadsförare utnyttja [!UICONTROL Visual Experience Composer] för att starta A/B- och XT-tester för dessa XDM.

   ```javascript
   alloy("sendEvent", { 
     "renderDecisions": true, 
     "xdm": { 
       "web": { 
         "webPageDetails": { 
         "viewName":"home" 
         }
       } 
     } 
   });
   ```

>[!NOTE]
>
>Vid det första `sendEvent()`-anropet hämtas och cachelagras alla [!UICONTROL XDM Views] som ska återges för slutanvändaren. Efterföljande `sendEvent()` anrop med [!UICONTROL XDM Views] som skickas läses från cachen och återges utan ett serveranrop.

## `sendEvent()` funktionsexempel

I det här avsnittet beskrivs tre exempel som visar hur du anropar funktionen `sendEvent()` i React för en hypotetisk SPA för e-handel.

### Exempel 1: A/B-teststartsida

Marknadsföringsteamet vill köra A/B-tester på hela hemsidan.

![Exempelbild av ett enkelsidigt program i ett webbläsarfönster.](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-1.png)

Om du vill köra A/B-tester på hela hemplatsen måste `sendEvent()` anropas med XDM `viewName` inställd på `home`:

```jsx
function onViewChange() { 
  
  var viewName = window.location.hash; // or use window.location.pathName if router works on path and not hash 

  viewName = viewName || 'home'; // view name cannot be empty 

  // Sanitize viewName to get rid of any trailing symbols derived from URL 

  if (viewName.startsWith('#') || viewName.startsWith('/')) { 
    viewName = viewName.substr(1); 
  }
   
  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName":"home" 
        } 
      } 
    }
  }); 
} 

// react router v4 

const history = syncHistoryWithStore(createBrowserHistory(), store); 

history.listen(onViewChange); 

// react router v3 

<Router history={hashHistory} onUpdate={onViewChange} > 
```

### Exempel 2: Personaliserade produkter

Marknadsföringsteamet vill anpassa den andra produktraden genom att ändra prisetikettens färg till rött efter att en användare har klickat på **Läs in mer**.

![Exempelbild av ett enkelsidigt program i ett webbläsarfönster, med personaliserade erbjudanden.](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-2.png)

```jsx
function onViewChange(viewName) { 

  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName": viewName
        }
      } 
    } 
  }); 
} 

class Products extends Component { 
  
  render() { 
    return ( 
      <button type="button" onClick={this.handleLoadMoreClicked}>Load more</button> 
    ); 
  } 

  handleLoadMoreClicked() { 
    var page = this.state.page + 1; // assuming page number is derived from component's state 
    this.setState({page: page}); 
    onViewChange('PRODUCTS-PAGE-' + page); 
  } 

} 
```

### Exempel 3: Inställningar för A/B-testleverans

Marknadsföringsteamet vill köra ett A/B-test för att se om färg på knappen ändras från blå till röd när **Express Delivery** väljs. Teamet tror att den här förändringen kan öka konverteringsgraden (till skillnad från att behålla knappfärgen blå för båda leveransalternativen).

![Exempelbild av ett enkelsidigt program i ett webbläsarfönster, med A/B-testning.](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-3.png)

Om du vill anpassa innehållet på webbplatsen beroende på vilken leveransinställning som har valts, kan du skapa en [!UICONTROL View] för varje leveransinställning. När **Normal leverans** har valts kan [!UICONTROL View] heta&quot;checkout-normal&quot;. Om du väljer **Express Delivery** kan [!UICONTROL View] heta&quot;checkout-express&quot;.

```jsx
function onViewChange(viewName) { 
  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName": viewName 
        }
      }
    }
  }); 
} 

class Checkout extends Component { 

  render() { 
    return ( 
      <div onChange={this.onDeliveryPreferenceChanged}> 
        <label> 
          <input type="radio" id="normal" name="deliveryPreference" value={"Normal Delivery"} defaultChecked={true}/> 
          <span> Normal Delivery (7-10 business days)</span> 
        </label> 
        <label> 
          <input type="radio" id="express" name="deliveryPreference" value={"Express Delivery"}/> 
          <span> Express Delivery* (2-3 business days)</span> 
        </label> 
      </div> 
    ); 
  } 

  onDeliveryPreferenceChanged(evt) { 
    var selectedPreferenceValue = evt.target.value; 
    onViewChange(selectedPreferenceValue); 
  } 

} 
```

## Använda [!UICONTROL Visual Experience Composer] för ett SPA

När du har definierat din [!UICONTROL XDM Views] och implementerat `sendEvent()` med de [!UICONTROL XDM Views] skickade, kan VEC identifiera dessa [!UICONTROL Views] och tillåta användare att skapa åtgärder och ändringar för A/B- eller XT-aktiviteter.

>[!NOTE]
>
>Om du vill använda VEC för ditt SPA måste du installera och aktivera [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/) eller [Chrome VEC Helper Extension](https://experienceleague.adobe.com/sv/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension).

### Panelen [!UICONTROL Modifications]

Panelen [!UICONTROL Modifications] innehåller de åtgärder som har skapats för en viss [!UICONTROL View]. Alla åtgärder för en [!UICONTROL View] grupperas under den [!UICONTROL View].

### Åtgärder

Om du klickar på en åtgärd markeras elementet på platsen där den här åtgärden används. Varje VEC-åtgärd som skapas under en [!UICONTROL View] har följande ikoner: **Information**, **Redigera**, **Klona**, **Flytta** och **Ta bort**. Dessa ikoner förklaras närmare i följande tabell.

| Ikon | Beskrivning |
|---|---|
| Information | Visar information om åtgärden. |
| Redigera | Gör att du kan redigera åtgärdens egenskaper direkt. |
| Klona | Klona åtgärden till en eller flera [!UICONTROL Views] som finns på panelen [!UICONTROL Modifications] eller till en eller flera [!UICONTROL Views] som du har bläddrat till och navigerat till i VEC. Åtgärden behöver inte nödvändigtvis finnas på panelen [!UICONTROL Modifications].<br/><br/>**Obs!** När en klonåtgärd har utförts måste du navigera till [!UICONTROL View] i VEC via [!UICONTROL Browse] för att se om den klonade åtgärden var en giltig åtgärd. Om åtgärden inte kan tillämpas på [!UICONTROL View] visas ett fel. |
| Flytta | Flyttar åtgärden till en [!UICONTROL Page Load Event] eller någon annan [!UICONTROL View] som redan finns på panelen [!UICONTROL Modifications].<br/><br/>**Sidinläsningshändelse:** Alla åtgärder som motsvarar sidinläsningshändelsen tillämpas på den första sidinläsningen i webbprogrammet. <br/><br/>**Obs!** När en flyttåtgärd har utförts måste du navigera till [!UICONTROL View] i VEC via [!UICONTROL Browse] för att se om flyttningen var en giltig åtgärd. Om åtgärden inte kan tillämpas på [!UICONTROL View], se ett fel. |
| Ta bort | Tar bort åtgärden. |

## Använda VEC för SPA-exempel

I det här avsnittet beskrivs tre exempel på hur du använder [!UICONTROL Visual Experience Composer] för att skapa åtgärder och ändringar för A/B- eller XT-aktiviteter.

### Exempel 1: Uppdatera hemvyn

Tidigare i den här artikeln definierades en [!UICONTROL View] med namnet&quot;home&quot; för hela hemsidan. Nu vill marknadsföringsteamet uppdatera vyn&quot;home&quot; på följande sätt:

* Ändra knapparna **Lägg till i kundvagn** och **Gilla** till en ljusare nyans av blått. Den här ändringen bör utföras under sidinläsningen eftersom det handlar om att ändra sidhuvudets komponenter.
* Ändra etiketten **Senaste produkter för 2026** till **Värdprodukter för 2026** och ändra textfärgen till lila.

Om du vill göra de här uppdateringarna i VEC-vyn väljer du **Disponera** och tillämpar ändringarna på hemvyn.

![Exempelsida för Visual Experience Composer.](/help/dev/implement/client-side/aep-web-sdk/assets/vec-home.png)

### Exempel 2: Ändra produktetiketter

För&quot;products-page-2&quot; [!UICONTROL View] vill marknadsföringsteamet ändra etiketten **Price** till **Sale Price** och ändra etikettfärgen till red.

Följande steg krävs för att göra uppdateringarna i VEC:

1. Välj **Bläddra** i VEC.
2. Välj **Produkter** i den översta navigeringen på webbplatsen.
3. Välj **Läs in mer** en gång för att visa den andra produktraden.
4. Välj **Disponera** i VEC.
5. Använd åtgärder för att ändra textetiketten till **Försäljningspris** och färgen till röd.

![Exempelsida för Visual Experience Composer med produktetiketter.](/help/dev/implement/client-side/aep-web-sdk/assets/vec-products-page-2.png)

### Exempel 3: Anpassa format för leveransinställningar

[!UICONTROL Views] kan definieras på en detaljnivå, till exempel ett läge eller ett alternativ från en alternativknapp. Tidigare i den här artikeln definierades [!UICONTROL Views] för leveransinställningar,&quot;checkout-normal&quot; och&quot;checkout-express&quot;. Marknadsföringsteamet vill ändra knappfärgen till röd för vyn&quot;checkout-express&quot;.

Följande steg krävs för att göra uppdateringarna i VEC:

1. Välj **Bläddra** i VEC.
2. Lägg produkter i kundvagnen på sajten.
3. Välj kundvagnsikonen i webbplatsens övre högra hörn.
4. Välj **Checka ut din beställning**.
5. Välj alternativknappen **Express Delivery** under **Leveransinställningar**.
6. Välj **Disponera** i VEC.
7. Ändra **Betala**-knappfärgen till röd.

>[!NOTE]
>
>Utcheckning-express [!UICONTROL View] visas inte på panelen [!UICONTROL Modifications] förrän alternativknappen **Express Delivery** har valts. Detta beror på att funktionen `sendEvent()` körs när alternativknappen **Express Delivery** är markerad, och därför är VEC inte medveten om &quot;checkout-express&quot; [!UICONTROL View] förrän alternativknappen är markerad.

![Visuell Experience Composer visar väljare för leveransinställningar.](/help/dev/implement/client-side/aep-web-sdk/assets/vec-delivery-preference.png)
