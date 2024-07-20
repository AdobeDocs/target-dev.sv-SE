---
keywords: Webbläsare, Förutsättningar, Krav, Internet Explorer, Chrome, Firefox, Safari, Android, Surface, Webbläsare0
description: Lär dig vilka webbläsare [!DNL Adobe Target] har stöd för för sitt gränssnitt och för innehållsleverans.
title: Vilka webbläsare stöder  [!DNL Target] ?
feature: Implementation
exl-id: 1d778e14-26b0-477b-ac28-d304db70a133
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# Webbläsare som stöds

Programmet [!DNL Adobe Target] och innehållsleveransen har testats i ett stort antal webbläsare och enheter.

Mer information om TLS finns i [TLS (Transport Layer Security) Krypteringsändringar](tls-transport-layer-security-encryption.md).

## [!DNL Target] Standard-/Premium-gränssnitt

Gränssnittet [!DNL Target] har stöd för följande webbläsare och enheter:

| Enhetstyp | Webbläsarversion |
|--- |--- |
| Windows | <ul><li>Microsoft Edge</li><li>Google Chrome (senaste, senaste minus 1)</li><li>Mozilla Firefox (senaste, senaste minus 1)</li></ul> |
| Mac | <ul><li>Firefox (senaste, senaste minus 1)</li><li>Chrome (senaste, senaste minus 1)</li></ul> |

## Innehållsleverans

Materialet har testats i följande webbläsare och enheter:

| Enhetstyp | Webbläsarversion |
|--- |--- |
| Windows | <ul><li>Microsoft Internet Explorer 9 och 10. Testad i emuleringsläge. **Obs!**: Innehållsleverans på IE 9 stöds inte längre med at.js 1.3.0 (och senare). Innehållsleverans på IE 10, 11 och alla äldre versioner stöds inte längre med at.js 2.5.0 (och senare).</li><li>Internet Explorer 11. **Obs!**: Innehållsleverans på IE 10, 11 och alla äldre versioner stöds inte längre med at.js 2.5.0 (och senare).</li><li>Microsoft Edge</li><li>Chrome (senaste, senaste minus 1)</li><li>Firefox (senaste, senaste minus 1)</li></ul> |
| Mac | <ul><li>Apple Safari (senaste). **Obs!** Mer information om hur Safari hanterar cookies från första och tredje part finns i [Målcookies](../implement/client-side/atjs/atjs-cookies.md).</li><li>Firefox (senaste, senaste minus 1)</li><li>Chrome (senaste, senaste minus 1)</li></ul> |
| Mobil/surfplatta | <ul><li>Apple iOS (senaste)</li><li>Android-enheter och surfplattor (Android 4 och senare)</li><li>Microsoft Surface (Windows 8.1)</li></ul> |

Observera följande:

* För at.js-implementeringar visar [!DNL Target] standardinnehåll i tidigare versioner av Internet Explorer och eventuellt i tidigare versioner av webbläsarna ovan.
* I Internet Explorer behandlas alla okända element (t.ex. anpassade element) som samma elementtyp. Leveransen fungerar därför inte med anpassade element.
* [!DNL Target] visar standardinnehåll i webbläsare som inte listas ovan och i webbläsare som använder läget [quirks](https://en.wikipedia.org/wiki/Quirks_mode). at.js kräver en doctype som återges i standardläge, till exempel: `<!DOCTYPE html>` .
* [!DNL Adobe] Leveransinfrastruktur skyddas för att INTE stödja TLS 1.0-enheter och -webbläsare efter den 12 september 2018. Se [TLS (Transport Layer Security) Krypteringsändringar](../before-implement/tls-transport-layer-security-encryption.md) för att förstå den övergripande effekten av den här ändringen.
