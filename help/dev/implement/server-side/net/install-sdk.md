---
title: Installera .NET SDK
description: Lär dig hur du installerar [!DNL Adobe Target] .NET SDK.
feature: APIs/SDKs
exl-id: 3cc84775-4692-4d14-9e82-db2873140835
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '60'
ht-degree: 0%

---

# Installera .NET SDK

.NET SDK distribueras av [NuGet](https://www.nuget.org/packages/Adobe.Target.Client). Om du vill komma igång lägger du till det som ett beroende genom att installera via `Package Manage` eller `.NET CLI`:

## Pakethanteraren

>[!BEGINTABS]

>[!TAB Pakethanteraren]

```csharp {line-numbers="true"}
Install-Package Adobe.Target.Client
```

>[!TAB .NET CLI]

```csharp {line-numbers="true"}
dotnet add package Adobe.Target.Client
```

>[!ENDTABS]

Den öppna källkoden finns på [https://github.com/adobe/target-dotnet-sdk](https://github.com/adobe/target-dotnet-sdk).
