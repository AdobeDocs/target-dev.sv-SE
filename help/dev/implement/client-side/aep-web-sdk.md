---
keywords: Adobe Experience Platform Web SDK, aep web sdk, web sdk, sdk, adobe experience cloud, platform edge network, adobe experience platform platform network, edge network, aep edge network, Adobe Experience Platform Web SDK0
description: Lär dig hur du använder [!UICONTROL Adobe Experience Platform Web SDK] för att interagera med de olika tjänsterna i [!UICONTROL Adobe Experience Cloud] via [!UICONTROL AEP Edge Network].
title: Hur implementerar jag [!UICONTROL Experience Platform Web SDK]?
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] (AEP Web SDK) är ett JavaScript-bibliotek på klientsidan som tillåter användare av [!UICONTROL Adobe Experience Cloud] att interagera med de olika tjänsterna i [!DNL Adobe Experience Cloud] (inklusive [!DNL Target]) via [!UICONTROL Adobe Experience Platform Edge Network]. Förutom JavaScript-biblioteket finns det ett [!UICONTROL Adobe Experience Platform]-tillägg som kan hjälpa dig med dina Web SDK-konfigurationer.

Mer information finns i följande länkar i hjälpen för *[!UICONTROL Adobe Experience Platform Web SDK]*:

* Mer information: [Vad är [!UICONTROL Adobe Experience Platform Web SDK]](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=sv-SE)
* För information som är specifik för [!DNL Target]: [[!DNL Target] Översikt](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html?lang=sv-SE)

## Självstudiekurser

Följande självstudiekurser hjälper dig med implementeringen:

### Implementera [!DNL Adobe Experience Cloud] med [!DNL Platform Web SDK]

Lär dig hur du implementerar [!DNL Experience Cloud]-program med [!DNL Adobe Experience Platform Web SDK] med [den här självstudiekursen](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=sv-SE). Mer information om [!DNL Target] finns i självstudieavsnittet [Konfigurera [!DNL Target] med SDK för plattformen](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html?lang=sv-SE).

### Migrera [!DNL Target] från at.js 2.*x* till [!DNL Platform Web SDK]

Lär dig hur du migrerar din [!DNL Target]-implementering från at.js 2.*x* till [!DNL Adobe Experience Platform Web SDK] med [den här självstudien](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=sv-SE).

## Rekommenderad dokumentation

Förutom den [!UICONTROL Platform Web SDK]-dokumentation som nämns ovan innehåller avsnitten i den här handboken även information som är specifik för [!UICONTROL Platform Web SDK] vad gäller [!DNL Target]-funktioner.

| Funktion | Beskrivning/länk |
| --- | --- |
| [Aktivitets-QA](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=sv-SE) | Använd QA-URL:er i [!DNL Target] för att utföra enkel QA för hela aktiviteten med förhandsgranskningslänkar som aldrig ändras, målgruppsanpassning som tillval och QA-rapportering som förblir segmenterad från live-aktivitetsdata. Med Activity QA kan du testa dina [!DNL Target]-aktiviteter fullständigt innan du startar dem live.<p>Se [Kompatibilitet med mållager-QA-läge för JavaScript-bibliotek](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=sv-SE#compatibility) och [Förhandsgranska URL:er](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=sv-SE#preview). |
| [[!UICONTROL Analytics for Target] (A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=sv-SE) | [!UICONTROL Adobe Analytics for Target] (A4T) är en integrering med flera lösningar som gör att du kan skapa aktiviteter baserade på [!DNL Analytics] konverteringsvärden och målgruppssegment. Med A4T-integreringen kan ni använda Analytics-rapporter för att undersöka era resultat.<p>Se [Aktivitetstyper som stöds](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=sv-SE#section_F487896214BF4803AF78C552EF1669AA) och [Implementeringssteg för en Adobe Experience Platform Web SDK-implementering](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html?lang=sv-SE#platform). |
| [Publiker](https://experienceleague.adobe.com/docs/target/using/audiences/target.html?lang=sv-SE) | Målgrupper i [!DNL Target] avgör vem som ser innehåll och upplevelser i en målinriktad aktivitet.<p>Se [Använd publiklistan](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=sv-SE#use-list) och [Kombinera flera målgrupper](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html?lang=sv-SE). |
| [Skapa målgrupper](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=sv-SE) | Om målgrupper skapas i [!DNL Adobe Experience Platform] får du mer omfattande kunddata som leder till mer slagkraftig personalisering.<p>Se [Använda målgrupper från Adobe Experience Platform](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=sv-SE#aep). |
| [Erbjudandebeslut](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html?lang=sv-SE) | Lägg till offertbeslut som har skapats i [!DNL Adobe Journey Optimizer] till [!DNL Target]-aktiviteter (manuellt A/B-test eller Experience Targeting) för att fastställa och leverera nästa bästa erbjudande för dina besökare på webben och i mobilen. |
| [Omdirigeringserbjudanden - Vanliga frågor om A4T](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html?lang=sv-SE) | Omdirigeringserbjudanden gör att besökarnas webbläsare dirigerar om till en ny sida.<p>Se [Stöder [!UICONTROL Adobe Experience Platform Web SDK] omdirigeringserbjudanden för A4T?](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html?lang=sv-SE#platform) |
| [Svarstoken](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=sv-SE) | Med svarstoken kan du skicka [!DNL Target]-data till Google Analytics och andra tredjepartsintegreringar.<p>Se [Skicka data till Google Analytics via Platform Web SDK](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=sv-SE#sending-data-to-google-analytics-via-platform-web-sdk) om du vill se ett kodexempel på hur du kan utföra den här åtgärden. |
| [Programimplementering på en sida](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html?lang=sv-SE) i *[!UICONTROL Platform Web SDK]översiktshandboken*. | [!UICONTROL Adobe Experience Platform Web SDK] innehåller omfattande funktioner som gör det möjligt för ditt företag att utföra personalisering på nästa generations klienttekniker, som enkelsidiga program (SPA). |
| [TLS-krypteringen (Transport Layer Security) ändras](../../before-implement/tls-transport-layer-security-encryption.md) | TLS (Transport Layer Security) hjälper er att upprätthålla högsta säkerhetsstandard och främja säkerheten för kunddata. |
