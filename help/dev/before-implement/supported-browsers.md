---
keywords: Webbläsare, Förutsättningar, Krav, Internet Explorer, Chrome, Firefox, Safari, Android, Surface, Webbläsare0
description: Lär dig vilka webbläsare [!DNL Adobe Target] har stöd för för sitt gränssnitt och för innehållsleverans.
title: Vilka webbläsare stöder  [!DNL Target] ?
feature: Implementation
exl-id: 1d778e14-26b0-477b-ac28-d304db70a133
source-git-commit: 1b6dcb24d677b758ed1daf85dc0a7e9e5b42680d
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# Webbläsare som stöds

Programmet [!DNL Adobe Target] och innehållsleveransen har testats i ett stort antal webbläsare och enheter.

Mer information om TLS finns i [TLS (Transport Layer Security) Krypteringsändringar](tls-transport-layer-security-encryption.md).

## [!DNL Target] Standard-/Premium-gränssnitt

Gränssnittet [!DNL Target] har stöd för följande webbläsare och enheter:

>[!NOTE]
>
>Target har stöd för den senaste versionen av alla webbläsare i listan och den senaste versionen minus 1.


| Enhetstyp | Webbläsarversion |
|--- |--- |
| [!DNL Windows] | <ul><li>[!DNL Microsoft Edge]</li><li>[!DNL Google Chrome]</li><li>[!DNL Mozilla Firefox]</li></ul> |
| [!DNL Mac] | <ul><li>[!DNL Microsoft Edge]</li><li>[!DNL Google Chrome]</li><li>[!DNL Mozilla Firefox]</li></ul> |

## Krav för visuell redigering

Om du vill kunna öppna, redigera och förhandsgranska dina webbsidor på ett tillförlitligt sätt i [!UICONTROL Visual Experience Composer] (VEC) måste du ha webbläsartillägget [Adobe Experience Cloud Visual Editing Helper ](https://experienceleague.adobe.com/sv/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank} installerat i webbläsaren eller använda [!UICONTROL Enhanced Experience Composer (EEC)].

>[!NOTE]
>
>[!DNL Google Chrome] och [!DNL Microsoft Edge] är för närvarande de enda webbläsare som stöder visuell redigering av webbsidor i [!DNL Adobe Target].


## Innehållsleverans

Materialet har testats i följande webbläsare och enheter:

| Enhetstyp | Webbläsarversion |
|--- |--- |
| Windows | <ul><li>Microsoft Internet Explorer 9 och 10. Testad i emuleringsläge. **Obs!**: Innehållsleverans på IE 9 stöds inte längre med at.js 1.3.0 (och senare). Innehållsleverans på IE 10, 11 och alla äldre versioner stöds inte längre med at.js 2.5.0 (och senare).</li><li>Internet Explorer 11. **Obs!**: Innehållsleverans på IE 10, 11 och alla äldre versioner stöds inte längre med at.js 2.5.0 (och senare).</li><li>Microsoft Edge</li><li>Chrome (senaste, senaste minus 1)</li><li>Firefox (senaste, senaste minus 1)</li></ul> |
| Mac | <ul><li>Apple Safari (senaste). **Obs!** Mer information om hur Safari hanterar cookies från första och tredje part finns i [Målcookies](../implement/client-side/atjs/atjs-cookies.md).</li><li>Firefox (senaste, senaste minus 1)</li><li>Chrome (senaste, senaste minus 1)</li></ul> |
| Mobil/surfplatta | <ul><li>Apple iOS (senaste)</li><li>Android-enheter och surfplattor (Android 4 och senare)</li><li>Microsoft Surface (Windows 8.1)</li></ul> |

Observera följande:

* [!DNL Adobe Experience Platform Web SDK] är utformad för att fungera optimalt i de senaste versionerna av [!DNL Google Chrome], [!DNL Safari], [!DNL Firefox] och [!DNL Microsoft Edge Chromium]. Du kan ha problem med att använda vissa funktioner i äldre versioner av dessa webbläsare eller i inaktuella webbläsare, till exempel [!DNL Internet Explorer].
* För at.js-implementeringar visar [!DNL Target] standardinnehåll i tidigare versioner av Internet Explorer och eventuellt i tidigare versioner av webbläsarna ovan.
* I Internet Explorer behandlas alla okända element (t.ex. anpassade element) som samma elementtyp. Leveransen fungerar därför inte med anpassade element.
* [!DNL Target] visar standardinnehåll i webbläsare som inte listas ovan och i webbläsare som använder läget [quirks](https://en.wikipedia.org/wiki/Quirks_mode). at.js kräver en doctype som återges i standardläge, till exempel: `<!DOCTYPE html>` .
* [!DNL Adobe] Leveransinfrastruktur skyddas för att INTE stödja TLS 1.0-enheter och -webbläsare efter den 12 september 2018. Se [TLS (Transport Layer Security) Krypteringsändringar](../before-implement/tls-transport-layer-security-encryption.md) för att förstå den övergripande effekten av den här ändringen.
