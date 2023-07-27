---
keywords: Webbläsare, Förutsättningar, Krav, Internet Explorer, Chrome, Firefox, Safari, Android, Surface, Webbläsare0
description: Lär dig vilka webbläsare som används [!DNL Adobe Target] har stöd för sitt gränssnitt och för innehållsleverans.
title: Vad webbläsare gör [!DNL Target] Support?
feature: Implementation
exl-id: 1d778e14-26b0-477b-ac28-d304db70a133
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 0%

---

# Webbläsare som stöds

The [!DNL Adobe Target] applikationer och innehåll har testats i en rad olika webbläsare och enheter.

Mer information om TLS finns i [Krypteringsändringar för TLS (Transport Layer Security)](tls-transport-layer-security-encryption.md).

## [!DNL Target] Standard-/Premium-gränssnitt

The [!DNL Target] -gränssnittet har stöd för följande webbläsare och enheter:

| Enhetstyp | Webbläsarversion |
|--- |--- |
| Windows | <ul><li>Microsoft Edge</li><li>Google Chrome (senaste, senaste minus 1)</li><li>Mozilla Firefox (senaste, senaste minus 1)</li></ul> |
| Mac | <ul><li>Firefox (senaste, senaste minus 1)</li><li>Krom (senaste, senaste minus 1)</li></ul> |

## Innehållsleverans

Materialet har testats i följande webbläsare och enheter:

| Enhetstyp | Webbläsarversion |
|--- |--- |
| Windows | <ul><li>Microsoft Internet Explorer 9 och 10. Testad i emuleringsläge. **Anteckning**: Innehållsleverans på IE 9 stöds inte längre med at.js 1.3.0 (och senare). Innehållsleverans på IE 10, 11 och alla äldre versioner stöds inte längre med at.js 2.5.0 (och senare).</li><li>Internet Explorer 11. **Anteckning**: Innehållsleverans på IE 10, 11 och alla äldre versioner stöds inte längre med at.js 2.5.0 (och senare).</li><li>Microsoft Edge</li><li>Krom (senaste, senaste minus 1)</li><li>Firefox (senaste, senaste minus 1)</li></ul> |
| Mac | <ul><li>Apple Safari (senaste). **Anteckning**: Mer information om hur Safari hanterar cookies från första och tredje part finns i [Målcookie](../implement/client-side/atjs/atjs-cookies.md).</li><li>Firefox (senaste, senaste minus 1)</li><li>Krom (senaste, senaste minus 1)</li></ul> |
| Mobil/surfplatta | <ul><li>Apple iOS (senaste)</li><li>Android-enheter och -surfplattor (Android 4 och senare)</li><li>Microsoft Surface (Windows 8.1)</li></ul> |

Observera följande:

* För at.js-implementeringar, [!DNL Target] I visas standardinnehåll i tidigare versioner av Internet Explorer och eventuellt i tidigare versioner av webbläsarna ovan.
* I Internet Explorer behandlas alla okända element (t.ex. anpassade element) som samma elementtyp. Leveransen fungerar därför inte med anpassade element.
* [!DNL Target] visar standardinnehåll i webbläsare som inte listas ovan och i webbläsare som använder [quirks, läge](https://en.wikipedia.org/wiki/Quirks_mode). at.js kräver en doctype som återges i standardläge, till exempel: `<!DOCTYPE html>` .
* [!DNL Adobe] Leveransinfrastrukturen skyddas för att INTE stödja TLS 1.0-enheter och -webbläsare efter den 12 september 2018. Se [Krypteringsändringar för TLS (Transport Layer Security)](../before-implement/tls-transport-layer-security-encryption.md) för att förstå den övergripande effekten av denna förändring.
