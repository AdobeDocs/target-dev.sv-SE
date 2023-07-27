---
keywords: molninstanser, offentlig suffixlista, offentligt suffix, cookie, cookie, cookie från första part, cookie från första part, azurewebsites.net, cloudapp.net, amazonaws.com, cloudfront.net, herokuapp.com, firebaseapp.com, targetGlobalSettings, cookieDomain, cloud instances5, cloud instances6, cloud instances7, cloud instances8, cloud instances9, public suffix list0, public suffix list1, public suffix list2, public suffix list3, public suffix list4, public suffix list5
description: Utforska problem (med lösningar) som kunderna ställs inför när de använder molnbaserade instanser för att testa [!DNL Adobe Target] eller för konceptbevisändamål.
title: Kan jag använda [!DNL Target] med molnbaserade instanser?
feature: at.js
exl-id: 4b24fdc0-6c74-4b29-bbf9-7a761d4564a2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# Använd molnbaserade instanser med [!DNL Target]

Information om problem som kunderna ställs inför när de använder molnbaserade instanser för att testa [!DNL Adobe Target].

[!DNL Target] kunder använder ibland molnbaserade instanser med [!DNL Target] för testning eller enkla konceptbevis. De här instanserna kan omfatta följande domäner:

`azurewebsites.net`, `cloudapp.net`, `amazonaws.com`, `cloudfront.net`, `herokuapp.com`, eller `firebaseapp.com`

Dessa domäner, och många andra, ingår i [Lista över offentliga suffix](https://publicsuffix.org/list/public_suffix_list.dat).

**Problem:** Moderna webbläsare sparar inte cookies om du använder dessa domäner.

JavaScript-biblioteket at.js använder cookies för att spåra användare och säkerställa att [!DNL [!DNL Target]] alltid ger en enhetlig upplevelse. Om [!DNL Target] JavaScript-biblioteket kan inte spara cookies, Target-begäranden är inaktiverade.

**Lösning:** Om du tänker använda molnbaserade instanser med domäner som finns med i listan över offentliga suffix bör du se till att anpassa `cookieDomain` inställning. Mer information finns i [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).
