---
title: Återge upplevelser
description: Se till att alla nödvändiga steg för återgivning utförs i rätt sekvens.
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 5321ce43be26e8f0776da49e597ecb5f8dfb5984
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---

# Återge upplevelser

Följ stegen i *Återge upplevelser* diagram för att säkerställa att alla nödvändiga uppgifter som behövs för att återge upplevelser utförs i rätt sekvens.

>[!TIP]
>
>Klicka på bilderna i det här avsnittet för att utöka till helskärm.

## Rendera upplevelsediagram {#diagram}

Automatisk flimmerhantering som är tillgänglig med at.js är bara användbar om du har [!UICONTROL Automatic Page Load Request] aktiverat. Det här alternativet döljer hela HTML-kroppen samtidigt som upplevelserna hämtas från [!DNL Target]. I det här fallet är det ditt ansvar att hantera flimmer. Sök efter implementeringsmönster som är tillgängliga för flimmerhantering för vägledning.

Stegnumren i följande bild motsvarar avsnitten nedan.

![Rendera upplevelsediagram](/help/dev/patterns/assets/render-experiences-diagram.png){width="600" zoomable="yes"}

Klicka på följande länkar för att navigera till önskade avsnitt:

* [3.1: Kampanj](#promotion)
* [3.2: Cart-baserade kriterier](#cart)
* [3.3: Popularitetsbaserade kriterier](#popularity)
* [3.4: Objektbaserade kriterier](#item)
* [3.5: Användarbaserade villkor](#user)
* [3.6: Anpassade kriterier](#custom)
* [3.7: Ange attribut som används i inkluderingsregler](#inclusion)
* [3.8: Ange ExcludeIds](#exclude)
* [3.9: Ange entitetsattribut för att uppdatera produktkatalogen för Recommendations](#entity-attributes)
* [3.10: Ange profilattribut som används som nycklar för inkluderingsregler](#keys)
* [3.11: Begäran om sidinläsning vid brand](#fire)
* [3.12: Anmälan om brandställe](#location)

## 3.1: Kampanj {#promotion}

Lägg till framhävda objekt och styr placeringen av dem i din Target Recommendations [design](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}.

+++Se information

**Tillgängliga alternativ**

* Befordra efter ID
* [Befordra efter samling](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [Befordra efter attribut](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**Enhetsparametrar krävs**

* Objektattribut i kampanjer måste skickas när du använder attributet &quot;Promoby&quot;.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.2: Cart-baserade kriterier {#cart}

Utför rekommendationer baserat på användarens kundvagnsinnehåll.

+++Se information

**Tillgängliga villkor**

* [!UICONTROL People Who Viewed These, Viewed Those]
* [!UICONTROL People Who Viewed These, Bought Those]
* [!UICONTROL People Who Bought These, Bought Those]

**Enhetsparametrar krävs**

* cartIds

**Läser**

* [Cart-baserad](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.3: Popularitetsbaserade kriterier {#popularity}

Utför rekommendationer baserat på hur populärt ett objekt på webbplatsen är eller utifrån hur populärt det är att ha objekt inom en användares favoritkategori, varumärke, genre osv.

+++Se information

**Tillgängliga villkor**

* [!UICONTROL Most Viewed Across the Site]
* [!UICONTROL Most Viewed by Category]
* [!UICONTROL Most Viewed by Item Attribute]
* [!UICONTROL Top Sellers Across the Site]
* [!UICONTROL Top Sellers by Category]
* [!UICONTROL Top Sellers by Item Attribute]
* [!UICONTROL Top by Analytics Metric]

**Enhetsparametrar krävs**

* `entity.categoryId` eller objektattributet för popularitet baserat på om villkoret är baserat på det aktuella attributet eller objektattributet.
* Ingenting får skickas för Most Viewed/Top sålt på webbplatsen.

**Läser**

* [Popularitetsbaserad](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.4: Objektbaserade kriterier {#item}

Rekommendationer baserade på sökning efter liknande objekt för ett objekt som användaren visar eller nyligen har visat.

+++Se information

**Tillgängliga villkor**

* [!UICONTROL People Who Viewed This, Viewed That]
* [!UICONTROL People Who Viewed This, Bought That]
* [!UICONTROL People Who Bought This, Bought That]
* [!UICONTROL Items with Similar Attributes]

**Enhetsparametrar krävs**

* `entity.id`
* Om ett profilattribut används som nyckel

**Läser**

* [Objektbaserad](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.5: Användarbaserade villkor {#user}

Utför rekommendationer baserat på användarens beteende.

+++Se information

**Tillgängliga villkor**

* [!UICONTROL Recently Viewed Items]
* [!UICONTROL Recommended for You]

**Enhetsparametrar krävs**

* `entity.id`

**Läser**

* [Användarbaserad](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.6: Anpassade kriterier {#custom}

Utför rekommendationer baserat på en anpassad fil som du överför

+++Se information

**Tillgängliga villkor**

* [!UICONTROL Custom algorithm]

**Enhetsparametrar krävs**

`entity.id` eller attributet som används som nyckel för den anpassade algoritmen

**Läser**

* [Egna villkor](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.7: Ange attribut som används i inkluderingsregler {#inclusion}

+++Se information

**Läser**

* [Använd dynamiska och statiska inkluderingsregler](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.8: Ange ExcludeIds {#exclude}

Skicka enhets-ID:n för entiteter som du vill utesluta från dina rekommendationer. Du kan t.ex. utesluta artiklar som redan finns i kundvagnen.

+++Se information

**Läser**

* [Kan jag utesluta en entitet dynamiskt?](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.9: Ange entitetsattribut för att uppdatera produktkatalogen för [!DNL Recommendations] {#entity-attributes}

+++Se information

**Läser**

* [Entitetsattribut](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

Du kan också utföra det här steget genom att skapa produktflöden med [!DNL Target] Gränssnitt för att uppdatera produktkatalogen för [!DNL Recommendations].

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.10: Ange profilattribut som används som nycklar för inkluderingsregler {#keys}

Ange profilattributen som används som nycklar för inkluderingsregler i de Recommendations-kriterier som nämns ovan.

+++ Se detaljer

**Läser**

* [Profilattribut](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.11: Begäran om sidinläsning vid brand {#fire}

Det här steget utlöser en [!DNL Delivery API] ring med `execute` > `pageLoad` nyttolast i begäran. The `getOffers()` hämtar upplevelsen och `applyOffers()` återger upplevelsen på sidan. pageLoad-begäran behövs för återgivningsupplevelser som skapats i [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html){target=_blank} (VEC)

+++Se information

![Begärandiagram för sidinläsning vid brand](/help/dev/patterns/assets/fire-page-load-request.png){width="100" zoomable="yes"}

**Förutsättningar**

* All datamappning måste göras med `targetPageParams` funktion.

**Läser**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**Åtgärder**

* Använd `getOffers` och `applyOffers` metoder för att hämta upplevelsen med hjälp av ett API-anrop för sidinläsningsbegäran.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.12: Eld regional location request (#location)

Det här steget utlöser en [!DNL Delivery API] ring med `execute` > `mboxes` nyttolast i sin begäran. The `getOffers` hämtar upplevelsen och `applyOffers` återger upplevelsen på sidan. Du kan skicka mer än en mbox under `execute` > `mboxes` nyttolast.

+++Se information

![Diagram över begäran om lokal brandkrets](/help/dev/patterns/assets/fire-regional-location-request.png){width="100" zoomable="yes"}

**Förutsättningar**

* All datamappning måste göras med `targetPageParams` funktion.

**Läser**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**Åtgärder**

* Använd `getOffers` och `applyOffers` metoder för att hämta upplevelsen med hjälp av ett API-anrop för sidinläsningsbegäran.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)