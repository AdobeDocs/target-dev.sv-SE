---
keywords: implementera, implementera, vitlista, vit lista, tillåtelselista, tillåtelselista, kant, kanter, $9
description: Visa en lista med värdar som hjälper dig att tillåtslista  [!DNL Adobe Target] kanter (geografiskt fördelade noder som ger optimala svarstider för slutanvändarna).
title: Hur Tillåtslista jag Edge  [!DNL Target] noder?
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 662d415bc3c216bcd038f07dcaa0fd83f6518690
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Tillåtelselista [!DNL Target]-kantnoder

Information och en uppdaterad lista över värdar som kan hjälpa dig att tillåtslista [!DNL Adobe Target]-kanter.

En kant är en geografiskt fördelad serverarkitektur som ger optimala svarstider för slutanvändare som behöver innehåll, oavsett var de befinner sig. Varje edge-nod har all information som krävs för att svara på användarens innehållsförfrågan och för att spåra analysdata på den begäran. Användarförfrågningar dirigeras till närmaste kantnod. Mer information finns i [Kantnätverket](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#concept_0AE2ED8E9DE64288A8B30FCBF1040934).

Du kan tillåtslista [!DNL Target]-kantnoder om du vill.

>[!IMPORTANT]
>
>Förutom att tillåtslista IP-adresserna för NAT (Network Address Translation) för [!DNL Target] kanter och IP-adresser för [!DNL Target]-kanter som beskrivs i artikeln, bör du även tillåtslista alla IP-adressblock för [!DNL Adobe Analytics].
>
>Mer information finns i [Alla Adobe Analytics IP-adressblock](https://experienceleague.adobe.com/docs/analytics/technotes/ip-addresses.html?lang=en#all-adobe-analytics-ip-address-blocks){target=_blank} i dokumentationen för *Adobe Analytics Tech Notes*.
>
>Infrastrukturen för [!DNL Adobe Target] uppdateras och kunder som vill tillåtslista adresser måste använda båda uppsättningarna med IP-adresser. Om du inte gör det kommer det att påverka kunder som använder implementeringar på serversidan eller hybridimplementeringar där Target API-anrop för att hämta upplevelser kommer från ett nätverk bakom en brandvägg som är konfigurerad att använda en tillåtelselista.

För att säkerställa oavbruten åtkomst till [!DNL Target] via [!DNL Experience Edge Connector] kan kunderna uppdatera sina nätverkskonfigurationer för att tillåta trafik till proxytjänsten.

## Översikt över proxytjänsten

* **Tjänstslutpunkt**: `https://tnt-web-proxy.adobe.io`.
* **Infrastruktur**: Finns på EOS-plattformen [!DNL Adobe].
* **Obs!**: Den här tjänsten använder latensbaserad DNS-routning och är inte beroende av statiska IP-adresser.

## CNAME-mål

Proxytjänsten dirigerar trafik dynamiskt över flera regioner med CNAME-poster. Detta är de aktuella målen:

| Edge | IP-adresser för Egress |
| --- | --- |
| Län | CNAME-mål |
| Europa (eu-west-1) | `ethos.pub.ethos11-prod-nld2.ethos.adobe.net` |
| US East (us-east-2) | `ethos.pub.ethos11-prod-va7.ethos.adobe.net` |
| US East (us-east-1) | `ethos.pub.ethos11-prod-aus5.ethos.adobe.net` |

## Rekommenderade poster i tillåtelselista

Tillåtslista följande värdnamn om du vill ha en säker anslutning:

* `ethos.pub.ethos11-prod-nld2.ethos.adobe.net`
* `ethos.pub.ethos11-prod-va7.ethos.adobe.net`
* `ethos.pub.ethos11-prod-aus5.ethos.adobe.net`

## Valfritt: IP-identifiering

Om dina nätverksprinciper kräver IP-baserad tillåtelselistning kan du visa de offentliga IP-adresser som är associerade med proxytjänsten med det här verktyget:

* `DNSChecker – A Record Lookup for tnt-web-proxy.adobe.io`