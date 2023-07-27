---
keywords: mobilapp, vanliga frågor, frågor och svar, målmobilapp
description: Visa vanliga frågor och svar om [!DNL Adobe Target] för mobilappar.
title: Vanliga frågor och svar [!DNL About Target] för mobilappar?
feature: Implement Mobile
exl-id: 06cae3de-83a4-4018-a832-66fb292a1d0f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# [!DNL Target] Vanliga frågor om mobilappar

Lista med vanliga frågor om [!DNL Target] för mobilappar.

## Bör jag använda taggar i [!DNL Adobe Experience Platform] för att distribuera SDK, eller kan jag distribuera SDK utan att använda Launch?

SDK är tillgängligt på [Adobe Marketing Cloud Git](https://github.com/Adobe-Marketing-Cloud/acp-sdks/){target=_blank}. If you don't use [tags in Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html){target=_blank}måste du hantera din egen inställningsfil och hantera den i din app.

## Vilka SDK:er finns tillgängliga idag?

Adobe Experience Platform Mobile SDK:er har för närvarande stöd för iOS, Android och React. Mer information finns i [Adobe Experience Cloud Platform Mobile SDKs guide](https://experienceleague.adobe.com/docs/mobile.html){target=_blank}.

## Vilken frekvens har den platsbaserade funktionen för verifiering av latitud och longitud?

Se [Dokumentation för Adobe Platse](https://experienceleague.adobe.com/docs/places/using/home.html){target=_blank} för mer information.

## Behöver jag at.js för att Adobe Experience Platform Mobile SDK ska fungera?

Nej, du behöver inte at.js för att använda SDK:n för mobilen. at.js är [!DNL Target] JavaScript-bibliotek för webbplatser. Adobe Experience Platform SDK för mobiler är till för mobilappar.

## Är [!DNL Target] Mobilt en funktion hos [!DNL Adobe Target] Premium Product SKU only?

Nej. För [!DNL Adobe Target Standard] -kunder kan du använda våra SDK:er för mobiler för [!UICONTROL A/B Test] och [!UICONTROL Experience Targeting] (XT) endast med [!DNL Target Standard] Mobilappstillägg. Om du vill använda [!UICONTROL Recommendations] eller AI-baserade funktioner i mobilappen behöver du en [Adobe Target Premium](https://experienceleague.adobe.com/docs/target/using/introduction/intro.html#premium) licens.

## Finns det någon mobilappsintegrering mellan [!DNL Adobe Experience Manager] AEM och [!DNL Target] mobilaktiviteter?

För närvarande kan du dela JSON [Upplevelsefragment](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html){target=_blank} från AEM till [!DNL Target] och det kan finnas möjligheter att sedan använda dem i en mobilappsaktivitet.
