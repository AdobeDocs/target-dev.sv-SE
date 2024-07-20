---
keywords: implementera, implementera, vitlista, vit lista, tillåtelselista, tillåtelselista, kant, kanter, $9
description: Visa en lista med värdar som hjälper dig att tillåtslista  [!DNL Adobe Target] kanter (geografiskt fördelade noder som ger optimala svarstider för slutanvändarna).
title: Hur Tillåtslista jag Edge  [!DNL Target] noder?
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 49b6572c0d414ab304712691c97794bb0b1e3781
workflow-type: tm+mt
source-wordcount: '476'
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
>
>Alla Edge4 *x*-adresser som anges i båda tabellerna nedan är schemalagda att uppdateras den 9 augusti 2023.

## NAT-IP-adresser (Network Address Translation) för [!DNL Target] kanter

Lista över IP-adresser för utgångar för [!DNL Target] kanter. Tillåtslista de här IP-adresserna om du vill att [!DNL Target] ska nå ut till dina tjänster.

| Edge | IP-adresser för Egress |
| --- | --- |
| Edge41 (Mumbai) | 3.6.2.221<br />13.235.112.4 <br />52.66.66.192 |
| Edge42 (Tokyo) | 52.69.55.232<br />43.206.61.43 <br />13.113.73.214 |
| Edge44 (East Coast US) | 54.164.192.223<br />52.86.86.203 <br /> 54.88.167.98 |
| Edge45 (West Coast US) | 52.40.124.129<br />54.148.219.69 <br />54.189.208.212 |
| Edge46 (Sydney) | 54.253.144.4<br />54.66.198.142 <br />13.211.218.51 |
| Edge47 (Irland) | 52.208.136.136<br />54.170.28.19 <br />99.80.111.82 |
| Edge48 (Singapore) | 3.1.141.36<br />18.143.112.116 <br />52.76.61.44 |

## IP-adresser för kant [!DNL Target]

Lista över IP-adresser för [!DNL Target] kanter. Tillåtslista de här IP-adresserna om du vill göra API-anrop till [!DNL Target]-kanterna.

Den här listan ändras ofta när belastningsutjämnarna skalas upp och ned baserat på trafikprofiler.

| Edge | Domän | IP-adresser |
| --- | --- | --- |
|  | `CLIENTCODE.tt.omtrdc.net`<br />(där CLIENTCODE är ditt [!DNL Target] klient-ID) |  |
| Edge41 (Mumbai) | `mboxedge41.tt.omtrdc.net` | 15.206.104.6<br />3.109.14.178 <br />13.234.139.131 |
| Edge42 (Tokyo) | `mboxedge42.tt.omtrdc.net` | 52.194.84.34<br />3.115.158.39 <br />18.180.123.21 |
| Edge44 (East Coast US) | `mboxedge44.tt.omtrdc.net` | 54.205.210.54<br />23.20.189.8 <br />35.169.173.155 |
| Edge45 (West Coast US) | `mboxedge45.tt.omtrdc.net` | 35.161.163.45<br />44.230.114.101 <br />35.161.120.22 |
| Edge46 (Sydney) | `mboxedge46.tt.omtrdc.net` | 3.104.142.61<br />52.62.4.152 <br />54.253.105.140 |
| Edge47 (Irland) | `mboxedge47.tt.omtrdc.net` | 18.203.168.186<br />54.228.83.91 <br />54.217.181.83 |
| Edge48 (Singapore) | `mboxedge48.tt.omtrdc.net` | 54.179.6.70<br />13.215.150.94 <br />18.136.47.70 |

När belastningsutjämnarna upptäcker förändringar i trafikprofilen kommer den att skalas upp eller ned. Den tid som krävs för att skala den elastiska belastningsfördelningen kan variera mellan 1 och 7 minuter, beroende på vilka ändringar som har identifierats. När belastningsutjämnarna skalas uppdaterar de DNS-posten med den nya listan över IP-adresser. För att du ska kunna utnyttja den ökade kapaciteten använder Elastic Load Balancing en TTL-inställning på DNS-posten på 60 sekunder.
