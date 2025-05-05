---
keywords: tls, tls 1.0, transport layer security, encryption, tls 1.1, tls 1.2
description: Läs om hur  [!DNL Target] använder TLS-protokollet (Transport Layer Security) för att upprätthålla högsta säkerhetsstandarder och främja säkerheten för dina kunddata.
title: Hur använder  [!DNL Target] TLS för att tillhandahålla säkerhet?
feature: Privacy & Security
exl-id: f5ea2272-27ab-49c9-b096-b15dd277d4e5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1197'
ht-degree: 0%

---

# TLS-krypteringsändringar (Transport Layer Security)

Information om förändringar i hur [!DNL Adobe] och [!DNL Adobe Target] använder TLS (Transport Layer Security) för att upprätthålla högsta säkerhetsstandard och främja kunddatasäkerheten.

TLS (Transport Layer Security) är det vanligaste säkerhetsprotokoll som används idag för webbläsare och andra program som kräver att data utbyts säkert över ett nätverk. Adobe har standarder för att uppfylla säkerhetskraven som kräver att äldre protokoll upphör att gälla och kräver att TLS 1.2 används för att få den senaste och säkraste versionen att använda.

>[!WARNING]
>
>Från och med den 1 mars 2020 har [!DNL Target] inte längre stöd för TLS 1.1-kryptering för Visual Experience Composer (VEC), Enhanced Experience Composer (EEC), aktivitetsleverans, API:er osv. Uppgradera till TLS 1.2 för att undvika problem.

Vi förväntar oss inte att detta ska ha någon större inverkan på kunddata eller rapportering.

## Visual Experience Composer (VEC) med Enhanced Experience Composer (EEC) aktiverat

TLS 1.2 är standard från och med 1 mars 2020 och TLS 1.1 stöds inte längre.

Adobe kommer att flytta kunder i faser till TLS 1.2. För dem vars domäner redan är 1.2-kompatibla kommer vi att flytta dem till TLS 1.2 utan att du behöver göra några ändringar. De flesta kunddomäner har redan stöd för TLS 1.2, men om din domän inte har stöd för TLS 1.2 behåller vi dessa domäner på TLS 1.1 som i dag (fram till mars 2020).

Du bör inte ställas inför något problem under den här migreringsfasen. Om VEC har slutat läsa in en plats som tidigare fungerade [öppnar du en kundtjänstbiljett](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?lang=sv-SE&#reference_ACA3391A00EF467B87930A450050077C) och anger den här migreringen som en möjlig orsak.

Om du däremot är en av de kunder som har TSL 1.1 utan stöd för TLS 1.2 bör du planera för att flytta dina domäner/din infrastruktur till TLS 1.2. Vi kommer att fortsätta att stödja TLS 1.1-protokollet fram till 1 mars 2020. Från och med 1 mars 2020 stöder inte [!DNL Target] TLS 1.1-protokollet som ska användas för VEC via funktionen Enhanced Experience Composer.

Även om vi rekommenderar alla att vara med på TLS 1.2 även om du är en ny kund men *INTE* har stöd för TLS 1.2 ber vi dig kontakta kundtjänst för att få veta att du behöver använda TLS 1.1 för Enhanced Experience Composer. Men planera för att gå över till TLS 1.2 eftersom du inte heller kommer att få stöd efter 1 mars 2020.

## Aktivitetsleverans

Från och med 1 mars 2020 har [!DNL Target]-servrar inte längre stöd för TLS 1.1. Den här ändringen innebär att [!DNL Target] -servrar inte längre accepterar förfrågningar från besökare med äldre enheter eller webbläsare som inte stöder TLS 1.2 (eller senare). Därför kommer äldre enheter och webbläsare som bara stöder TLS 1.1 (eller som standard stöder TLS 1.1) inte att få aktivitetsinnehåll från Adobe Target. Webbplatsens standardinnehåll återges.

Några av de äldre enheterna och webbläsarna som påverkas är:

* Google Chrome (Chrome för Android) version 29 och tidigare
* Opera Browser (Opera Mobile) version 12.17 och tidigare
* Mozilla Firefox (Firefox for Mobile) version 26 och tidigare
* Android 4.3 och tidigare
* Internet Explorer 8-10 i Windows 7 och tidigare
* Internet Explorer 10 på Windows Phone 8.0
* Safari 6.0.4/OS X10.8.4 och tidigare versioner

När du planerar för den här ändringen bör du tänka på följande (observera att 1 mars 2020-datumet påverkar alla dessa punkter):

* Du måste se till att din standardwebbplats är klar på ett sätt som kan användas för kompatibla enheter och webbläsare.
* Observera att antalet besökare i dina [!DNL Target]-rapporter kan se en obetydlig minskning av antalet besökare.
* Du kan behöva ändra målgrupper som skapats specifikt för äldre enheter eller webbläsare som inte stöder TLS 1.2. Leverans till dessa enheter och webbläsare kommer inte längre att fungera.

Mer information om vilka webbläsare som stöds och vilka versioner som stöds finns i [Webbläsare som stöds](supported-browsers.md).

## [!DNL Adobe Target] API:er

Från och med 1 mars 2020 har [!DNL Target] API:er inte längre stöd för TLS 1.1-kryptering. Kunder som har åtkomst till API bör verifiera att de inte kommer att påverkas.

* API-klienter som använder Java 7 med standardinställningar behöver ändras för att stödja TLS 1.2. Mer information finns i [Ändra standardversion för TLS-protokoll för klientslutpunkter: TLS 1.0 till TLS 1.2](https://www.java.com/en/configure_crypto.html) på Java-webbplatsen.
* API-klienter som använder Java 8 bör inte påverkas eftersom standardinställningen är TLS 1.2.
* API-klienter som använder andra ramverk måste kontakta sina leverantörer för att få information om TLS 1.2-stödet.

## Tillgång till Experience Cloud Solutions-gränssnitt

Eftersom gränssnittet [!DNL Target] Standard/Premium redan kräver en [modern webbläsare](supported-browsers.md), kan vi inte förutse några problem. Om du inte kan ansluta till Target bör du uppgradera webbläsaren till den senaste versionen.

## Kontrollera vilken TLS-version webbläsaren använder

Så här kontrollerar du TLS-versionen på din webbplats med Google Chrome:

1. Öppna den berörda webbplatsen i Chrome.
1. På Chrome-menyn (de tre lodräta ellipserna) klickar du på Fler verktyg > Utvecklingsverktyg.

   ![Chrome Developer Tools](assets/chrome-developer-tools.png)

1. Öppna fliken Säkerhet och granska sedan TLS-versionsinformationen under Connection:

   ![TLS-versionsinformation](assets/chrome-tls-version.png)

>[!NOTE]
>
>Dessa instruktioner är aktuella från och med publiceringen och kan komma att ändras. En snabb Internetsökning bör hjälpa dig om instruktionerna ändras. Andra webbläsare har liknande steg.

## Förväntat beteende med webbläsare som stöder TLS-versioner under 1.2

I det här avsnittet beskrivs vad du kan förvänta dig av webbläsare som stöder TLS-versioner under 1.2 endast när du använder en at.js-implementering. I det här avsnittet beskrivs också vad som ska förväntas med webbläsare som stöder TLS 1.2.

### Centrala slutpunkter

| [!DNL Target] JavaScript-implementering | Information |
|--- |--- |
| at.js | Med TLS 1.0 eller TLS 1.1 aktiverat:<ul><li>Med hjälp av webbläsarens utvecklingsverktyg visas&quot;200 OK&quot; på fliken Nätverk. Detta innebär att begäran har slutförts.</li><li>Användaren ser meddelandet&quot;Kan inte ansluta säkert till den här sidan&quot;. Meddelandet förklarar att detta kan bero på att webbplatsen använder föråldrade eller osäkra TLS-säkerhetsinställningar.</li><li>Inga konsolfel visas.</li></ul>Med TLS 1.2 aktiverat:<ul><li>filen at.js hämtas.</li></ul> |

### Edge slutpunkter

| [!DNL Target] JavaScript-implementering | Information |
|--- |--- |
| Adobe Experience Platform Web SDK | Med TLS 1.0 eller TLS 1.1 aktiverat:<ul><li>Med hjälp av webbläsarens utvecklingsverktyg visas&quot;200 OK&quot; på fliken Nätverk. Detta innebär att begäran har slutförts.</li><li>Användaren ser meddelandet&quot;Kan inte ansluta säkert till den här sidan&quot;. Meddelandet förklarar att detta kan bero på att webbplatsen använder föråldrade eller osäkra TLS-säkerhetsinställningar.</li><li>Inga konsolfel visas.</li><li>Standardinnehåll hanteras.</li></ul>Med TLS 1.2 aktiverat:<ul><li>Erbjudandet gäller.</li></ul> |
| at.js | Med TLS 1.0 eller TLS 1.1 aktiverat:<ul><li>Med hjälp av webbläsarens utvecklingsverktyg visas&quot;200 OK&quot; på fliken Nätverk. Detta innebär att begäran har slutförts.</li><li>Användaren ser meddelandet&quot;Kan inte ansluta säkert till den här sidan&quot;. Meddelandet förklarar att detta kan bero på att webbplatsen använder föråldrade eller osäkra TLS-säkerhetsinställningar.</li><li>Inga konsolfel visas.</li><li>Standardinnehåll hanteras.</li></ul>Med TLS 1.2 aktiverat:<ul><li>Erbjudandet gäller.</li></ul> |

### Aktivitet avsedd för användare som har webbläsarversion (Internet Explorer, version 6, 7 eller 8)

Målgrupperna slutar att fungera.

| [!DNL Target] JavaScript-implementering | Information |
|--- |--- |
| Adobe Experience Platform Web SDK | Plattforms-SDK stöds inte i tidigare versioner än version 10 av Internet Explorer. |
| at.js | at.js stöds inte i tidigare versioner än version 10 av Internet Explorer. |
