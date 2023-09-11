---
title: Initiera SDK:er
description: Se till att alla nödvändiga steg för att läsa in [!DNL Adobe Target] at.js JavaScript-biblioteket körs i rätt sekvens.
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 948ebe0c5011fb0327a7b5d45f3e7ac857fcb8ad
workflow-type: tm+mt
source-wordcount: '1690'
ht-degree: 0%

---

# Initiera SDK:er

Följ stegen i *Initiera SDK* diagram för att säkerställa att alla nödvändiga uppgifter som behövs för att läsa in [!DNL Adobe Target] at.js JavaScript-biblioteket körs i rätt sekvens.

>[!TIP]
>
>Klicka på bilderna i det här avsnittet för att utöka till helskärm.

## Initiera SDK-diagram {#diagram}

För flersidiga program sker det här flödet varje gång sidan läses in igen, eller när besökaren navigerar till en ny sida på webbplatsen.

Stegnumren i följande bild motsvarar avsnitten nedan.

![Initiera SDK-diagram](/help/dev/patterns/recs-atjs/assets/diagram-initiaze-sdk.png){width="600" zoomable="yes"}

Klicka på följande länkar för att navigera till önskade avsnitt:

* [1.1: Läs in besökar-API SDK](#load)
* [1.2: Ange kund-ID](#set)
* [1.3: Konfigurera automatisk sidinläsningsbegäran](#automatic)
* [1.4: Konfigurera flimmerhantering](#flicker)
* [1.5: Konfigurera datamappning](#data-mapping)
* [1.6: Kampanj](#promotion)
* [1.7: Cart-baserade kriterier](#cart)
* [1.8: Popularitetsbaserade kriterier](#popularity)
* [1.9: Objektbaserade kriterier](#item)
* [1.10: Användarbaserade villkor](#user)
* [1.11: Anpassade villkor](#custom)
* [1.12: Ange attribut som används i inkluderingsregler](#inclusion)
* [1.13: Ange ExcludeIds](#exclude)
* [1.14: Skicka parametern entity.event.detailsOnly=true](#true)
* [1.15: Konfigurera fjärrdatamappning](#remote)
* [1.16: Load at.js](#web)

## 1.1: Läs in besökar-API SDK {#load}

Detta steg hjälper till att säkerställa att `VisitorAPI.js` biblioteket har lästs in, konfigurerats och initierats korrekt.

+++Se information

![Läs in SDK-diagram för besökar-API](/help/dev/patterns/recs-atjs/assets/load-visitor-api-sdk.png){width="300" zoomable="yes"}

**Förutsättningar**

* Om du vill använda Visitor ID/API-tjänsten måste ditt företag vara aktiverat för [!DNL Adobe Experience Cloud] och har en [!UICONTROL Organization ID]. Mer information finns i [Krav för Experience Cloud: Organisations-ID](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html?){target=_blank} i *Hjälp för identitetstjänst* guide.
* Du behöver `VisitorAPI.js` -fil. Kontakta ert digitala marknadsföringsteam för att få fram den här filen.

**Konfigurera och referera till VisitorAPI.js**

Mer information finns i [Implementera tjänsten Experience Cloud för Target](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-target.html){target=_blank}.

**Läser**

* [Översikt över identitetstjänsten i Experience Cloud](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html){target=_blank}
* [Om ID-tjänsten](https://experienceleague.adobe.com/docs/id-service/using/intro/about-id-service.html){target=_blank}
* [Cookies och Experience Cloud Identity Service](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html){target=_blank}
* [Så här begär och anger identitetstjänsten i Experience Cloud](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html){target=_blank}
* [Förstå ID-synkronisering och matchningsfrekvenser](https://experienceleague.adobe.com/docs/id-service/using/intro/match-rates.html){target=_blank}

**Åtgärder**

* Bädda in `VisitorAPI.js` på dina webbsidor.
* Läs om [tillgängliga konfigurationer för besökar-ID/API-tjänsten](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html){target=_blank}.
* Efter `VisitorAPI.js` filen har lästs in, använd `Visitor.getInstance` metod för att initiera med de nödvändiga konfigurationer du behöver.
* Bekanta dig med [tillgängliga metoder](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/get-set.html){target=_blank}.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.2: Ange kund-ID {#set}

I det här steget kan du se till att dina besökares kända ID:n (CRM-ID, användar-ID o.s.v.) är kopplade till det anonyma ID:t för [!DNL Adobe] för personalisering på olika enheter.

+++Se information

![Ange kund-ID](/help/dev/patterns/recs-atjs/assets/set-customer-id.png){width="300" zoomable="yes"}

**Förutsättningar**

* Besökarens kända ID bör vara tillgängligt i datalagret.

**Ange kund-ID**
Mer information finns i [setCustomerIDs](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html){target=_blank}.

**Läser**

* [Profilsynkronisering i realtid för mbox3rdPartyId](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html){target=_blank}

**Åtgärder**

* Använd `visitor.setCustomerIDs` för att ange besökarens kända ID.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.3: Konfigurera automatisk sidinläsningsbegäran {#automatic}

I det här steget kan at.js hämta alla upplevelser som måste återges på sidan när JavaScript-biblioteksfilen at.js läses in.

+++Se information

![Konfigurera automatisk sidladdningsbegäran](/help/dev/patterns/recs-atjs/assets/configure-automatic-page-request.png){width="300" zoomable="yes"}

**Förutsättningar**

* Alla data i datalagret måste inte skickas till [!DNL Target]. Kontakta ert affärsteam (digital marknadsföring) för att ta reda på vilka data som är värdefulla för experiment, optimering och personalisering. Endast dessa data ska skickas till [!DNL Target].
* Se till att du inte skickar data för personligt identifierbar information till [!DNL Target].

**Konfigurera automatisk sidladdningsbegäran**

Mer information finns i [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**Läser**

Läs mer om `pageLoadEnabled` ställa in [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**Åtgärder**

* Ändra `window.targetGlobalSettings` objekt för att aktivera automatisk sidinläsning.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.4: Konfigurera flimmerhantering {#flicker}

Detta steg hjälper till att säkerställa att det inte finns någon sidflimmer när du levererar upplevelser.

+++Se information

![Konfigurera flimmerhanteringsdiagram](/help/dev/patterns/recs-atjs/assets/flicker-handling.png){width="300" zoomable="yes"}

**Förutsättningar**

* Diskutera med det team som ansvarar för webbsidans prestanda vad gäller fördelar och nackdelar med att styra flimmer med den standardmetod som används av at.js. Du kan söka efter designmönster där du kan använda en anpassad flimmerhanteringslösning, till exempel inläsaranimering. Om du inte hittar något mönster kan du begära ett nytt mönster.

**Konfigurera flimmerhantering**

Mer information finns i [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

Inställning `bodyHidingEnabled` till `true` Döljer hela sidbrödtexten medan sidinläsningsbegäran pågår. Om du inte har aktiverat den automatiska sidinläsningsbegäran av någon anledning (data är inte klara senare, till exempel, är det bäst att ange den här inställningen till `false`.

Om du har inaktiverat `bodyHidingEnabled` eftersom du inte vill starta APLR och vill starta sidbegäran senare, eller om du inte behöver flimmerhantering, måste du implementera en egen flimmerhantering. Du kan hantera flimmer på två sätt: dölja avsnitten under testet eller genom att visa ett reglage på de avsnitt som testas.

**Läser**

* [Hur at.js hanterar flimmer](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
* Läs mer om bodyHiddenStyle och bodyHidingEnabled-objekt i [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**Åtgärder**

* Ändra `window.targetGlobalSettings` objekt som ska anges `bodyHiddenStyle` och `bodyHidingEnabled`.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.5: Konfigurera datamappning {#data-mapping}

Detta steg hjälper till att säkerställa att alla data som måste skickas till [!DNL Target] är inställt.

+++Se information

![Datamappningsdiagram](/help/dev/patterns/recs-atjs/assets/data-mapping.png){width="300" zoomable="yes"}

**Förutsättningar**

* Datalagret ska vara klart med alla data som måste skickas till [!DNL Target].
* Recommendations: berika profilen.
   * Godkänd `entity.id` för att samla in data för nyligen visade villkor och objekt baserat på kriterier som baseras på den senast visade produkten.
   * Godkänd `entity.id` om du vill hämta in data för popularitetskriterier baserat på favoritkategori.
   * Skicka profilattributet om anpassade villkor är baserade på det eller används i inkluderingsregelfiltrering i alla villkor.
* Recommendations: importera produktdata.
   * Andra enhetsparametrar (reserverade och anpassade) kan skickas till import eller uppdatering av produktkatalogen i [!DNL Recommendations].
   * Produktkatalogen kan också uppdateras med hjälp av entitetsfeeds med [!DNL Target] Gränssnitt eller API.

**Mappa data till[!DNL Target]**

Mer information finns i [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**Läser**

* [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)
* [Planera och implementera Recommendations](/help/dev/implement/recommendations/recommendations.md)
* [Konfigurera Recommendations-katalogen](/help/dev/implement/recommendations/recommendations.md)

**Åtgärder**

* Använd `targetPageParams()` för att ange alla nödvändiga data som måste skickas till [!DNL Target].

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.6: Kampanj {#promotion}

Lägg till framhävda objekt och styr deras placering i [!DNL Target Recommendations] [design](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}.

+++Se information

**Tillgängliga alternativ**

* Befordra efter ID
* [Befordra efter samling](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [Befordra efter attribut](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**Enhetsparametrar krävs**

* Objektattributet i en befordran måste skickas när du använder attributet &quot;Promoby&quot;.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.7: Cart-baserade kriterier {#cart}

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

## 1.8: Popularitetsbaserade kriterier {#popularity}

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

* `entity.categoryId` eller objektattributet för popularitet baserat på om villkoret är baserat på det aktuella objektet eller objektattributet.
* Ingenting får skickas för Most Viewed/Top sålt på webbplatsen.

**Läser**

* [Popularitetsbaserad](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.9: Objektbaserade kriterier {#item}

Rekommendationer baserade på sökning efter liknande objekt för ett objekt som användaren visar eller nyligen har visat.

+++Se information

**Tillgängliga villkor**

* [!UICONTROL People Who Viewed This, Viewed That]
* [!UICONTROL People Who Viewed This, Bought That]
* [!UICONTROL People Who Bought This, Bought That]
* [!UICONTROL Items with Similar Attributes]

**Enhetsparametrar krävs**

* `entity.id` eller ett profilattribut som används som nyckel

**Läser**

* [Objektbaserad](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.10: Användarbaserade villkor {#user}

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

## 1.11: Anpassade villkor {#custom}

Utför rekommendationer baserat på en anpassad fil som du överför.

+++Se information

**Tillgängliga villkor**

* [!UICONTROL Custom algorithm]

**Enhetsparametrar krävs**

`entity.id` eller attributet som används som nyckel för den anpassade algoritmen

**Läser**

* [Egna villkor](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.12: Ange attribut som används i inkluderingsregler {#inclusion}

+++Se information

**Läser**

* [Använd dynamiska och statiska inkluderingsregler](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.13: Ange ExcludeIds {#exclude}

Skicka enhets-ID:n för entiteter som du vill utesluta från dina rekommendationer. Du kan t.ex. utesluta artiklar som redan finns i kundvagnen.

+++Se information

**Läser**

* [Kan jag utesluta en entitet dynamiskt?](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.14: Skicka `entity.event.detailsOnly=true` parameter {#true}

Använd entitetsattribut för att skicka produkt- eller innehållsinformation till [!DNL Target Recommendations].

+++Se information

**Läser**

* [Entitetsattribut](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=en){target=_blank}

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.15: Konfigurera fjärrdatamappning

Detta steg säkerställer att alla data som måste skickas till [!DNL Target] är inställt.

+++Se information

![Diagram över fjärrdatamappning](/help/dev/patterns/recs-atjs/assets/remote-data-mapping.png){width="300" zoomable="yes"}

**Förutsättningar**

* Datalagret ska vara klart med alla data som måste skickas till [!DNL Target].

**Ställ in dataleverantörer**

Mer information finns i [Dataleverantörer](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers).

**Läser**

[Funktionen targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**Åtgärder**

Använd `targetPageParams()` för att ange alla nödvändiga data som måste skickas till [!DNL Target].

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)

## 1.16: Load at.js {#web}

Detta steg säkerställer att JavaScript-biblioteket at.js läses in och initieras.

+++Se information

![Läs in Adobe Target Web SDK-diagram](/help/dev/patterns/recs-atjs/assets/load-web-sdk.png){width="300" zoomable="yes"}

**Förutsättningar**

* Ladda ned eller fråga ditt digitala marknadsföringsteam om Web SDK-biblioteksfilen: `at.js 2.*x*`

*Läser*

* [Så här fungerar Target](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html){target=_blank}
* [Hur at.js fungerar](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
* [Implementera mål utan tagghanterare](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

**Åtgärder**

Bädda in filen at.js på alla dina webbsidor där experimenterande, optimering, personalisering och datainsamling måste ske.

+++

[Återgå till diagrammet längst upp på den här sidan.](#diagram)





























