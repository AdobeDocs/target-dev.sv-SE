---
title: Adobe Target API - översikt
description: Översikt över olika Adobe Target-API:er, inklusive leverans-API, rapportering-API, admin-API, profil-API, rekommendationer-i och länkar till postmankollektioner.
exl-id: bf886103-36af-4061-b8be-2fe645f45ff3
feature: APIs/SDKs
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%

---

# API-översikt för mål

I den här artikeln beskrivs de olika mål-API:erna i allmänhet, innan den fokuserar på krav som är specifika för API:erna Admin och Profile. Om du vill administrera Target via användargränssnittet läser du avsnittet [Administration i *Adobe Target Business User Guide*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=sv-SE).

## API-typer

Adobe Target API:er är en samling API:er som kan användas med Adobe Target produkter, till exempel Adobe Recommendations. Dessa API:er gör det möjligt att skapa dataintensiva användargränssnitt som du kan använda för att hantera och integrera data.

Adobe Target API:er kan grupperas efter typ: Admin, Profil, Leverans och Rapportering.

>[!NOTE]
>
>Admin-API:erna och profil-API:erna kallas ofta tillsammans (&quot;Admin- och Profile-API:er&quot;), men kan också hänvisas till separat (&quot;Admin-API:er&quot; och&quot;Profil-API:er&quot;). Recommendations-API:t är en specifik implementering av ett Target Admin API.

| API-typ | Vad du kan göra | Hämta länk | Andra praktiska länkar |
| --- | --- | --- |--- |
| [Admin](../administer/admin-api/admin-api-overview-new.md) | Skapa, ändra och ta bort aktiviteter, målgrupper, erbjudanden och andra objekt (inklusive Recommendations-enheter, kriterier, designer osv.). Recommendations API:er är en typ av Admin API.) | <UL><li>[API för måladministratör för Postman Collection](https://developers.adobetarget.com/api/#admin-postman-collection)</li><li>[Recommendations API Postman Collection](https://developer.adobe.com/target/administer/recommendations-api/#section/Postman)</li></UL> | [Använd Recommendations API:er](../before-administer/recs-api/overview.md) |
| Profil | Hämta och ändra användarprofiler som lagras i Adobe Target. | [API för målprofil för Postman Collection](https://developers.adobetarget.com/api/#profiles) |  |
| [Leverans](../implement/delivery-api/overview.md) | Hämta optimerat och personaliserat innehåll från Target för leverans till en slutanvändare. | [Målleverans-API Postman Collection](/help/dev/before-implement/delivery-api-overview/getting-started.md#postman) |  |
| [Rapportering](../administer/admin-api/admin-api-overview-new.md) | Exportera aktivitetsresultat och andra rapportresultat. | Rapporterings-API:er ingår i [Target Admin API-samlingen för Postman](https://developers.adobetarget.com/api/#admin-postman-collection). |  |
| [Modeller](../administer/models-api/models-api-overview.md) | Hantera listan med funktioner som du vill att Target ska exkludera från sina maskininlärningsmodeller (&quot;blockeringslista&quot;). Models-API är en typ av Admin-API, men här visas det separat på grund av dess unika åtgärder mot objekt (blockeringslista) som inte är tillgängliga via användargränssnittet. |  |  |

## API-skillnader

Det finns viktiga skillnader mellan API:er för måladministratörer (inklusive API:er för Recommendations) och API:er för målleverans:

* Med Admin API:er kan du konfigurera olika aspekter av Target som du också kan konfigurera i målgränssnittet. Undantaget till detta är Models API, som gör att du kan konfigurera aspekter av Target som inte är tillgängliga i gränssnittet. Oavsett vilket krävs autentisering för **alla Admin API:er.**

* Med leverans-API:er kan du hämta innehåll. Leverans-API:er kräver inte autentisering.

Om du vill använda API:er för måladministratörer måste du konfigurera autentiseringen med [Adobe Developer Console](https://developer.adobe.com/console/home). Mer information finns i [Konfigurera autentisering](../before-administer/configure-authentication.md).
