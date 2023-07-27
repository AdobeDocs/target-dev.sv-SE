---
keywords: Adobe Experience Platform Web SDK, aep web sdk, web sdk, sdk, adobe experience cloud, platform edge network, adobe experience platform platform network, edge network, aep edge network, Adobe Experience Platform Web SDK0
description: Lär dig använda [!UICONTROL Adobe Experience Platform Web SDK] interagera med de olika tjänsterna i [!UICONTROL Adobe Experience Cloud] via [!UICONTROL AEP Edge Network].
title: Hur implementerar jag [!UICONTROL Experience Platform Web SDK]?
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '677'
ht-degree: 0%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] (AEP Web SDK) är ett JavaScript-bibliotek på klientsidan som tillåter kunder som [!UICONTROL Adobe Experience Cloud] interagera med de olika tjänsterna i [!DNL Adobe Experience Cloud] (inklusive [!DNL Target]) via [!UICONTROL Adobe Experience Platform Edge Network]. Förutom JavaScript-biblioteket finns det en [!UICONTROL Adobe Experience Platform] för att hjälpa dig med dina Web SDK-konfigurationer.

Mer information finns i följande länkar i *[!UICONTROL Adobe Experience Platform Web SDK]* hjälp:

* För utförlig information: [Vad är [!UICONTROL Adobe Experience Platform Web SDK]](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html)
* Specifik information för [!DNL Target]: [[!DNL Target] Ökning](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html)

## Självstudiekurser

Följande självstudiekurser hjälper dig med implementeringen:

### Implementera [!DNL Adobe Experience Cloud] med [!DNL Platform Web SDK]

Lär dig implementera [!DNL Experience Cloud] program använda [!DNL Adobe Experience Platform Web SDK] med [denna självstudiekurs](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html). Specifik information för [!DNL Target], se självstudieavsnittet med titeln [Konfigurera [!DNL Target] med Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html).

### Migrera [!DNL Target] från at.js 2.*x* till [!DNL Platform Web SDK]

Lär dig hur du migrerar [!DNL Target] implementering från at.js 2.*x* till [!DNL Adobe Experience Platform Web SDK] med [denna självstudiekurs](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html).

## Rekommenderad dokumentation

Förutom [!UICONTROL Platform Web SDK] dokumentationen ovan innehåller även avsnitt i den här handboken specifik information om [!UICONTROL Platform Web SDK] som det relaterar till [!DNL Target] funktioner.

| Funktion | Beskrivning/länk |
| --- | --- |
| [Aktivitets-QA](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) | Använd QA-URL:er i [!DNL Target] att utföra enkel QA för hela verksamheten med förhandsgranskningslänkar som aldrig ändras, målgruppsanpassning som tillval och QA-rapportering som förblir segmenterad från liveaktivitetsdata. Med Activity QA kan du testa din [!DNL Target] aktiviteter innan de lanseras live.<p>Se [Kompatibilitet med mållayoutbibliotek i JavaScript - QA-läge](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#compatibility) och [Förhandsgranska URL:er](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#preview). |
| [[!UICONTROL Analytics for Target] (A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) | [!UICONTROL Adobe Analytics for Target] (A4T) är en integrering med flera lösningar som gör att du kan skapa aktiviteter baserade på [!DNL Analytics] konverteringsstatistik och målgruppssegment. Med A4T-integreringen kan ni använda Analytics-rapporter för att undersöka era resultat.<p>Se [Aktivitetstyper som stöds](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html#section_F487896214BF4803AF78C552EF1669AA) och [Implementeringssteg för en Adobe Experience Platform Web SDK-implementering](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html#platform). |
| [Målgrupper](https://experienceleague.adobe.com/docs/target/using/audiences/target.html) | Målgrupper i [!DNL Target] avgöra vilka som ser innehåll och upplevelser i en målinriktad aktivitet.<p>Se [Använda publiklistan](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#use-list) och [Kombinera flera målgrupper](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html). |
| [Skapa målgrupper](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html) | Använda målgrupper skapade i [!DNL Adobe Experience Platform] tillhandahålla mer omfattande kunddata som leder till mer slagkraftig personalisering.<p>Se [Använda målgrupper från Adobe Experience Platform](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#aep). |
| [Erbjudandebeslut](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html) | Lägg till offertbeslut som skapats i [!DNL Adobe Journey Optimizer] till [!DNL Target] aktiviteter (manuellt A/B-test eller Experience Targeting) för att fastställa och leverera nästa bästa erbjudande för era besökare på webben och i mobilen. |
| [Omdirigeringserbjudanden - A4T FAQ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html) | Omdirigeringserbjudanden gör att besökarnas webbläsare dirigerar om till en ny sida.<p>Se [Gör [!UICONTROL Adobe Experience Platform Web SDK] stöder omdirigeringserbjudanden för A4T?](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html#platform) |
| [Svarstoken](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) | Med svarstoken kan du skicka [!DNL Target] data till Google Analytics och andra integreringar från tredje part.<p>Se [Skicka data till Google Analytics via Platform Web SDK](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html#sending-data-to-google-analytics-via-platform-web-sdk) om du vill se ett kodexempel på hur du utför den här uppgiften. |
| [Programimplementering på en sida](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html) i *[!UICONTROL Platform Web SDK]översikt* guide. | [!UICONTROL Adobe Experience Platform Web SDK] innehåller omfattande funktioner som gör det möjligt för ditt företag att utföra personalisering på nästa generations klienttekniker, som single page-applikationer (SPA). |
| [TLS-krypteringsändringar (Transport Layer Security)](../../before-implement/tls-transport-layer-security-encryption.md) | TLS (Transport Layer Security) hjälper er att upprätthålla högsta säkerhetsstandard och främja säkerheten för kunddata. |
