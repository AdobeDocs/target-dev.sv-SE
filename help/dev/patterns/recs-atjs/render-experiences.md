---
title: Återge upplevelser
description: Se till att alla nödvändiga steg för återgivning utförs i rätt sekvens.
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 7cf0c70b-a4bc-46f4-9b33-099bdb7dd9a9
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '908'
ht-degree: 0%

---

# Återge upplevelser

Följ stegen i diagrammet *Återge upplevelser* för att se till att alla nödvändiga åtgärder som behövs för att återge upplevelser utförs i rätt sekvens.

>[!NOTE]
>
>Om du har aktiverat Automatisk sidinläsningsbegäran under steget [Konfigurera automatisk sidinläsningsbegäran](/help/dev/patterns/recs-atjs/initialize-sdk.md#automatic) i *Initiera SDKS* kan du hoppa över den här aktiviteten om du inte vill anropa Adobe Target SDK för att återge ytterligare upplevelser med hjälp av en begäran om regional plats.

>[!TIP]
>
>Klicka på bilderna i det här avsnittet för att utöka till helskärm.

## Rendera upplevelsediagram {#diagram}

Automatisk flimmerhantering som är tillgänglig med at.js är bara användbar när du har [!UICONTROL Automatic Page Load Request] aktiverat. Det här alternativet döljer hela HTML-kroppen när upplevelserna hämtas från [!DNL Target]. I det här fallet är det ditt ansvar att hantera flimmer. Sök efter implementeringsmönster som är tillgängliga för flimmerhantering för vägledning.

>[!NOTE]
>
>Stegnumren i följande bild motsvarar avsnitten nedan. Stegnumren är inte i någon speciell ordning och återspeglar inte ordningen på steg som vidtas i användargränssnittet för [!DNL Target] när aktiviteten skapas.

![Återge upplevelsediagram](/help/dev/patterns/recs-atjs/assets/diagram-render-experiences-new.png){width="600" zoomable="yes"}

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

Lägg till framhävda objekt och styr deras placering i din rekommendationsdesign genom att välja fram- eller bakåtriktade kampanjer i [!DNL Target]-gränssnittet när aktiviteten skapas.

+++Se information

**Tillgängliga alternativ**

* Befordra efter ID
* [Befordra efter samling](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html?lang=sv-SE){target=_blank}
* [Befordra efter attribut](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=sv-SE){target=_blank}

**Entitetsparametrar krävs**

* Objektattribut i kampanjer måste skickas när du använder alternativet &quot;befordra efter attribut&quot;.

**Läser**

* [Lägg till kampanjer](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/adding-promotions.html?lang=sv-SE){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.2: Cart-baserade kriterier {#cart}

Utför rekommendationer baserat på användarens kundvagnsinnehåll.

+++Se information

**Tillgängliga villkor**

* [!UICONTROL People Who Viewed These, Viewed Those]
* [!UICONTROL People Who Viewed These, Bought Those]
* [!UICONTROL People Who Bought These, Bought Those]

**Entitetsparametrar krävs**

* cartIds

**Läser**

* [Kundvagnsbaserad](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=sv-SE#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.3: Popularitetsbaserade kriterier {#popularity}

Utför rekommendationer baserat på hur populärt ett objekt på webbplatsen är eller utifrån hur populärt det är att ha objekt inom en besökares favoritkategori eller mest visade kategori, varumärke, genre osv.

+++Se information

**Tillgängliga villkor**

* [!UICONTROL Most Viewed Across the Site]
* [!UICONTROL Most Viewed by Category]
* [!UICONTROL Most Viewed by Item Attribute]
* [!UICONTROL Top Sellers Across the Site]
* [!UICONTROL Top Sellers by Category]
* [!UICONTROL Top Sellers by Item Attribute]
* [!UICONTROL Top by Analytics Metric]

**Entitetsparametrar krävs**

* `entity.categoryId` eller objektattributet för popularitet baserat på villkoret som baseras på det aktuella attributet eller objektattributet.
* Inget måste skickas för Mest visade/Säljda över hela webbplatsen.

**Läser**

* [Popularitetsbaserad](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=sv-SE#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

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

**Entitetsparametrar krävs**

* `entity.id`
* Om ett profilattribut används som nyckel

**Läser**

* [Objektbaserad](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=sv-SE#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.5: Användarbaserade villkor {#user}

Utför rekommendationer baserat på användarens beteende.

+++Se information

**Tillgängliga villkor**

* [!UICONTROL Recently Viewed Items]
* [!UICONTROL Recommended for You]

**Entitetsparametrar krävs**

* `entity.id`

**Läser**

* [Användarbaserad](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=sv-SE#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.6: Anpassade kriterier {#custom}

Utför rekommendationer baserat på en anpassad fil som du överför.

+++Se information

**Tillgängliga villkor**

* [!UICONTROL Custom algorithm]

**Entitetsparametrar krävs**

`entity.id` eller det attribut som används som nyckel för den anpassade algoritmen

**Läser**

* [Anpassade villkor](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=sv-SE#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.7: Ange attribut som används i inkluderingsregler {#inclusion}

+++Se information

**Läser**

* [Använd dynamiska och statiska inkluderingsregler](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html?lang=sv-SE){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.8: Ange ExcludeIds {#exclude}

Skicka enhets-ID:n för entiteter som du vill utesluta från dina rekommendationer. Du kan t.ex. utesluta artiklar som redan finns i kundvagnen.

+++Se information

**Läser**

* [Kan jag utesluta en entitet dynamiskt?](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=sv-SE#exclude){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.9: Ange entitetsattribut för att uppdatera produktkatalogen för [!DNL Recommendations] {#entity-attributes}

+++Se information

**Läser**

* [Entitetsattribut](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=sv-SE){target=_blank}

Du kan också slutföra det här steget genom att skapa [produktflöden](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html?lang=sv-SE){target=_blank} med användargränssnittet i [!DNL Target] för att uppdatera produktkatalogen för [!DNL Recommendations].

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.10: Ange profilattribut som används som nycklar för inkluderingsregler {#keys}

Ange profilattributen som används som nycklar för inkluderingsregler i de Recommendations-kriterier som nämns ovan.

+++ Se detaljer

**Läser**

* [Profilattribut](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html?lang=sv-SE){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.11: Begäran om sidinläsning vid brand {#fire}

Det här steget utlöser ett [!DNL Delivery API]-anrop med `execute` > `pageLoad` nyttolast i begäran. Metoden `getOffers()` hämtar upplevelsen och `applyOffers()` återger upplevelsen på sidan. `pageLoad`-begäran krävs för återgivning av upplevelser som har skapats i [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=sv-SE){target=_blank} (VEC).

+++Se information

![Starta sidladdningsbegärandediagram](/help/dev/patterns/recs-atjs/assets/fire-page-load-request-combined.png){width="400" zoomable="yes"}

**Förutsättningar**

* All datamappning måste utföras med funktionen `targetPageParams`.

**Läser**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**Åtgärder**

* Använd metoderna `getOffers` och `applyOffers` för att hämta upplevelsen med ett API-anrop för sidinläsningsbegäran.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 3.12: Eld regional location request (#location)

Det här steget utlöser ett [!DNL Delivery API]-anrop med `execute` > `mboxes` nyttolast i sin begäran. Metoden `getOffers` hämtar upplevelsen och `applyOffers` återger upplevelsen på sidan. Du kan skicka mer än en mbox under `execute` > `mboxes`-nyttolasten.

+++Se information

![Diagram över begäran om lokal branding](/help/dev/patterns/recs-atjs/assets/fire-regional-location-request-combined.png){width="400" zoomable="yes"}

**Förutsättningar**

* All datamappning måste utföras med funktionen `targetPageParams`.

**Läser**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**Åtgärder**

* Använd metoderna `getOffers` och `applyOffers` för att hämta upplevelsen med ett API-anrop för sidinläsningsbegäran.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

Fortsätt till steg 4: [Meddela mål](/help/dev/patterns/recs-atjs/notify-target.md).
