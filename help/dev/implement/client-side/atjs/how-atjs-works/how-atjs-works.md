---
keywords: systemdiagram, flimmer, at.js, implementering, javascript-bibliotek, js, atjs, $8
description: Lär dig hur JavaScript-biblioteket  [!DNL Target] at.js fungerar, inklusive systemdiagram som hjälper dig förstå arbetsflödet när sidorna läses in.
title: Hur fungerar biblioteket at.js Javascript?
feature: at.js
exl-id: 9183797c-857b-4b7f-a573-6bb1d583f7b1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1127'
ht-degree: 2%

---

# Hur at.js fungerar

Om du vill implementera [!DNL Adobe Target] på klientsidan måste du använda JavaScript-biblioteket at.js.

I en implementering på klientsidan av [!DNL Adobe Target] levererar [!DNL Target] upplevelserna som är kopplade till en aktivitet direkt till klientwebbläsaren. Webbläsaren avgör vilken upplevelse som ska visas och visar den. Med en implementering på klientsidan kan du använda en WYSIWYG-redigerare, [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) (VEC) eller ett icke-visuellt gränssnitt, [formulärbaserad Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html), för att skapa test- och personaliseringsupplevelser.

## Vad är at.js?

At.js-biblioteket är implementeringsbiblioteket för implementering på klientsidan av [!DNL Adobe Target]. at.js-biblioteket ger bättre sidladdningstider för webbimplementeringar och ger bättre implementeringsalternativ för enkelsidiga program. at.js är det rekommenderade implementeringsbiblioteket och uppdateras ofta med nya funktioner. Vi rekommenderar att alla kunder implementerar eller migrerar till den [senaste versionen av at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md).

Mer information finns i [Målbibliotek för JavaScript](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#libraries).

I implementeringen av [!DNL Target]som visas nedan implementeras följande Adobe Experience Cloud-lösningar: [!DNL Analytics], Target och [!DNL Audience Manager]. Dessutom implementeras följande [!DNL Experience Cloud]-bastjänster: [!DNL Adobe Experience Platform], [!UICONTROL Audiences] och [!UICONTROL Visitor ID Service].

## Vad är skillnaden mellan at.js 1?*x* och at.js 2.x arbetsflödesdiagram?

Mer information om skillnaderna som introducerades i 2.O från 1 finns i [Uppgradera från at.js 1.x till at.js 2.x](/help/dev/implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md).*x*.

Från en högnivåvy finns det några skillnader mellan de två versionerna:

* at.js 2.x har inte något globalt koncept för mbox-begäran utan snarare en sidinläsningsbegäran. En sidladdningsbegäran kan visas som en begäran om hämtning av innehåll som ska användas vid den första sidladdningen på webbplatsen.
* at.js 2.x hanterar koncepten [!UICONTROL Views], som används för Single Page-program (SPA). at.js 1.*x* känner inte till det här konceptet.

## at.js 2.x-diagram

Följande diagram hjälper dig att förstå arbetsflödet för at.js 2.x med [!UICONTROL Views] och hur detta förbättrar SPA integrering. Mer information om begreppen som används i at.js 2.x finns i [Implementering av enkelsidiga program](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

(Klicka på bilden för att expandera till full bredd.)

![Målflöde med at.js 2.x](/help/dev/implement/client-side/assets/system-diagram-atjs-20.png "Målflöde med at.js 2.x"){zoomable="yes"}

| Steg | Information |
| --- | --- |
| 1 | Samtalet returnerar [!UICONTROL Experience Cloud ID] om användaren är autentiserad. Ett annat anrop synkroniserar kund-ID:t. |
| 2 | At.js-biblioteket läses in synkront och döljer dokumentets brödtext.<br /> at.js kan också läsas in asynkront med ett valfritt predhide-fragment implementerat på sidan. |
| 3 | En sidinläsningsbegäran görs med alla konfigurerade parametrar (MCID, SDID och kund-ID). |
| 4 | Profilskript körs och matas sedan in i [!UICONTROL Profile Store]. Sto begär kvalificerade målgrupper från [!UICONTROL Audience Library] (till exempel målgrupper som delas från [!DNL Adobe Analytics], [!DNL Audience Manager] osv.).<br />Kundattribut skickas till [!UICONTROL Profile Store] i en gruppbearbetning. |
| 5 | Baserat på parametrar för URL-begäran och profildata avgör [!DNL Target] vilka aktiviteter och upplevelser som ska returneras till besökaren för den aktuella sidan och framtida vyer. |
| 6 | Målinriktat innehåll skickas tillbaka till sidan, eventuellt med profilvärden för ytterligare personalisering.<br />Målinnehåll på den aktuella sidan visas så snabbt som möjligt utan att standardinnehållet flimrar.<br />Målanpassat innehåll för vyer som visas som ett resultat av användaråtgärder i en SPA cachelagras i webbläsaren så att det kan tillämpas direkt utan ett extra serveranrop när vyerna aktiveras via `triggerView()`. |
| 7 | Analysdata skickas till [!UICONTROL Data Collection] servrar. |
| 8 | Måldata matchas mot analysdata via SDID och bearbetas till rapportlagringen [!DNL Analytics].<br />[!DNL Analytics] data kan sedan visas i både [!DNL Analytics]- och [!DNL Target] via (A4T)-rapporter. |

Nu hämtas [!UICONTROL Views] och åtgärder från cachen och visas för användaren utan ett serveranrop, oavsett var `triggerView()` implementeras på din SPA. `triggerView()` skickar också en aviseringsbegäran till [!DNL Target]-serverdelen för att öka antalet och registrera antalet visningar. Mer information om at.js för SPA med vyer finns i [Implementering av Single Page-program](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

(Klicka på bilden för att expandera till full bredd.)

![Målflöde at.js 2.x triggerView](/help/dev/implement/client-side/assets/atjs-20-triggerview.png "Målflöde at.js 2.x triggerView"){zoomable="yes"}

| Steg | Information |
| --- | --- |
| 1 | `triggerView()` anropas i SPA för att återge [!UICONTROL View] och tillämpa åtgärder för att ändra visuella element. |
| 2 | Målinnehåll för vyn läses från cachen. |
| 3 | Målinriktat innehåll visas så snabbt som möjligt utan att man behöver flimra standardinnehållet. |
| 4 | Meddelandebegäran skickas till [!DNL Target] [!UICONTROL Profile Store] för att räkna besökaren i aktiviteten och öka måtten. |
| 5 | [!DNL Analytics] data har skickats till [!UICONTROL Data Collection Servers]. |
| 6 | [!DNL Target]-data matchas mot [!DNL Analytics]-data via SDID och bearbetas till rapportlagringen [!DNL Analytics]. [!DNL Analytics] data kan sedan visas i både [!DNL Analytics] och [!DNL Target] via A4T-rapporter. |

### Video - at.js 2.x - arkitekturdiagram

at.js 2.x förbättrar Adobe Target stöd för SPA och kan integreras med andra Experience Cloud-lösningar. Den här videon förklarar hur allt hänger ihop.

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

Mer information finns i [Förstå hur at.js 2.x fungerar](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html).

## at.js 1.x-diagram

Följande diagram hjälper dig att förstå arbetsflödet i at.js 1.x.

(Klicka på bilden för att expandera till full bredd.)

![Målflöde at.js 1.x](/help/dev/implement/client-side/assets/target-flow.png "Målflöde at.js 1.x"){zoomable="yes"}

| Steg | Beskrivning | Utlysning | Beskrivning |
|--- |--- |--- |--- |
| 1 | Samtalet returnerar Experience Cloud-ID (MCID) om användaren är autentiserad. Ett annat anrop synkroniserar kund-ID:t. | 2 | At.js-biblioteket läses in synkront och döljer dokumentets brödtext. |
| 3 | En global mbox-begäran görs med alla konfigurerade parametrar, MCID, SDID och kund-ID (valfritt). | 4 | Profilskript körs och matas sedan in i profilarkivet. Store begär kvalificerade målgrupper från Audience Library (till exempel målgrupper som delas från Adobe Analytics, Audience Manager).<br />Kundattribut skickas till profilarkivet i en gruppbearbetning. |
| 5 | Baserat på URL:er, mbox-parametrar och profildata avgör [!DNL Target] vilka aktiviteter och upplevelser som ska returneras till besökaren. | 6 | Målinriktat innehåll skickas tillbaka till sidan, och eventuellt även profilvärden för ytterligare personalisering.<br />Upplevelsen visas så snabbt som möjligt utan att något standardinnehåll flimrar. |
| 7 | Analysdata skickas till datainsamlingsservrar. | 8 | Måldata matchas mot Analytics-data via SDID och bearbetas till lagringsplatsen för analysrapporter.<br />Analysdata kan sedan visas både i Analytics- och [!DNL Target] via [!UICONTROL Analytics for Target]-rapporter (A4T). |

### Video - kontorstid: tips och översikt (26 juni 2019)

Den här videon är en inspelning av&quot;Office Hours&quot;, ett projekt som leds av [!UICONTROL Adobe Customer Care]-teamet.

* Fördelar med att använda at.js
* at.js-inställningar
* Hantering av flimmer
* Felsöka på.js
* Kända fel
* Vanliga frågor

>[!VIDEO](https://video.tv.adobe.com/v/27959/?quality=12)

## How at.js renders offers with HTML content

Vid återgivning av erbjudanden med HTML-innehåll använder at.js följande algoritm:

1. Bilder är förinlästa (om det finns några `<img>`-taggar i HTML-innehåll).

1. HTML-innehåll är kopplat till DOM-noden.

1. Textbundna skript körs (koden omges av `<script>` taggar).

1. Fjärrskript läses in asynkront och körs (`<script>` taggar med `src` attribut).

Viktigt:

* at.js ger inga garantier i ordningen för fjärrexekvering av skript eftersom dessa läses in asynkront.
* Textbundna skript får inte ha några beroenden till fjärrskript eftersom de läses in och körs senare.
