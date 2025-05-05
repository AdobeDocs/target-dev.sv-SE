---
keywords: sekretess, ip-adress, geosegmentering, avanmälan, avanmälan, avanmälan, datasekretess, myndighetsbestämmelser, gdpr, ccpa, sekretess, personligt identifierbar information, PII
description: Lär dig hur [!DNL Adobe Target] följer gällande datasekretesslagstiftning, inklusive insamling och hantering av IP-adresser, PII och avanmälningsanvisningar.
title: Hur hanterar Target sekretessfrågor, inklusive PII?
feature: Privacy & Security
exl-id: 4330e034-2483-4a25-9c87-48dbef6fc9de
source-git-commit: 88bde40aa6dfb96e1d53e4db6ba5547d38dbbb99
workflow-type: tm+mt
source-wordcount: '771'
ht-degree: 0%

---

# Integritet

[!DNL Adobe Target] har aktiverat processer och inställningar som gör att du kan använda [!DNL Target] i enlighet med gällande datasekretesslagstiftning.

## Insamling av IP-adresser och personligt identifierbar information (PII)

IP-adressen till en besökare på din webbplats överförs till ett Adobe Data Processing Center (DPC). Beroende på besökarens nätverkskonfiguration representerar IP-adressen inte nödvändigtvis IP-adressen för besökarens dator. IP-adressen kan till exempel vara den externa IP-adressen för en NAT-brandvägg, HTTP-proxy eller Internet-gateway.

>[!IMPORTANT]
>
>[!DNL Target] lagrar inte användarens IP-adresser eller någon PII (Personally Identiitable Information). IP-adresser används endast av [!DNL Target] under sessionen (i minnet, bevaras aldrig).

## Ersättning av den senaste oktetten med IP-adresser

Adobe har utvecklat en inställning för sekretess efter design som användare kan aktivera för Adobe [!DNL Target]. När det här alternativet är aktiverat döljer Adobe [!DNL Target] omedelbart den sista oktetten (den sista delen) i IP-adressen när IP-adressen samlas in. Denna anonymisering utförs innan någon bearbetning av IP-adressen utförs, inklusive före en valfri geosökning av IP-adressen.

När den här funktionen är aktiverad görs IP-adressen tillräckligt anonym så att den inte längre kan identifieras som personlig information. Därför kan [!DNL Target] användas i enlighet med dataintegritetslagstiftningen i länder som inte tillåter insamling av personuppgifter. Att få information på stadsnivå kommer sannolikt att påverkas avsevärt av att IP-adressen döljs. Att få information på region- och landsnivå bör endast få en liten inverkan.

Följande inställningar är tillgängliga i [!DNL Target]-gränssnittet genom att gå till **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**:

* [!UICONTROL Last octet obfuscation]: [!DNL Target] döljer IP-adressens sista oktett.
* [!UICONTROL Entire IP obfuscation]: [!DNL Target] döljer hela IP-adressen.
* [!UICONTROL None]: [!DNL Target] döljer inte någon del av IP-adressen.

  ![obfuscate-ip-options](assets/obfuscate-ip.png)

[!DNL Target] tar emot den fullständiga IP-adressen och döljer den (om den är inställd på Last octet eller Hela IP) enligt specifikationen. [!DNL Target] innehåller sedan den dolda IP-adressen i minnet endast under den aktuella sessionen.

### IP-förfalskning på datastandardsnivå vid användning av [!DNL Adobe Experience Platform Web SDK] {#aep}

När du använder [!DNL Platform Web SDK] (version 23.4 eller senare) har IP-begränsningsinställningen på datastream-nivå företräde framför IP-begränsningsalternativ som har angetts i [!DNL Target]. Om till exempel alternativet IP-obfuktion på datastream-nivå är inställt på [!UICONTROL Full] och alternativet [!DNL Target] för IP-obfuktion är inställt på [!UICONTROL Last octet obfuscation] får [!DNL Target] en helt okomplicerad IP.

Mer information finns i [!UICONTROL IP Obfuscation] i [Konfigurera ett datastream](https://experienceleague.adobe.com/docs/experience-platform/datastreams/configure.html?lang=sv-SE){target=_blank} i *[!DNL Adobe Experience Platfrom]Datastreams Guide*.

## GeoSegmentation

Om du aktiverar ersättning av den sista oktetten i IP-adressen kan de återstående värdena för IP-adressen analyseras med hjälp av rapporter i [!DNL Target]. Om den sista oktetten i IP-adressen inte har dolts kan den fullständiga IP-adressen analyseras i [!DNL Target]. Du kan använda funktionen GeoSegmentation för att mappa ut besökarnas plats efter geografiskt område. GeoSegmentation-data är endast granulerade till stadsnivån eller postnummernivån, och inte till den enskilda nivån.

Om IP-adresserna är helt dolda är inte GeoSegmentation och geoanpassning tillgängliga.

## Länk för avanmälan

Du kan lägga till en länk för att avanmäla dig till webbplatserna så att besökarna kan avanmäla sig från allt innehåll och allt som ingår i räkningen.

1. Lägg till följande länk till din webbplats:

   `<a href="https://clientcode.tt.omtrdc.net/optout"> Your Opt Out Language Here</a>`

1. (Villkorligt) Om du använder CNAME bör länken innehålla parametern &quot;client=`clientcode`, till exempel:
   `https://my.cname.domain/optout?client=clientcode`.

1. Ersätt `clientcode` med din klientkod och lägg till texten eller bilden som ska länkas till avanmälnings-URL:en.

Besökare som klickar på den här länken tas inte med i några mbox-förfrågningar som anropas från webbläsarsessionerna förrän de tar bort sina cookies, eller under två år, beroende på vilket som inträffar först. Detta fungerar genom att ange en cookie för besökaren `disableClient` i domänen `clientcode.tt.omtrdc.net`.

Även om du använder en cookie-implementering från en annan tillverkare anges den angivna avanmälningen via en cookie från en annan tillverkare. Om klienten bara använder en cookie från en annan tillverkare kontrollerar [!DNL Target] om en cookie för avanmälan har angetts.

## Sekretess- och dataskyddsbestämmelser

Mer information om EU:s allmänna dataskyddsförordning (GDPR), Kaliforniens konsumentsekretesslag (CCPA) och andra internationella sekretesskrav finns i [Sekretess- och dataskyddsbestämmelser](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) och hur dessa bestämmelser påverkar din organisation och [!DNL Target].

## Samling av funktionsanvändningsdata

Enskilda data om funktionsanvändning samlas in för Adobe för att identifiera om [!DNL Target]-funktioner fungerar som avsett eller för att identifiera funktioner som används för lite. Olika mätningar av fördröjning samlas in för att bidra till att åtgärda prestandaproblem. Personuppgifter samlas inte in.

Du kan välja bort att rapportera användningsdata i våra SDK:er genom att ange `telemetryEnabled` som false i klientinitieringsalternativen. Mer information finns i [telemetryEnabled i targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled).
