---
title: Installera Java SDK
description: Lär dig hur du installerar  [!DNL Adobe Target] Java SDK.
feature: APIs/SDKs
exl-id: 5828d5b3-c487-49bf-9458-7ef94374e32d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '46'
ht-degree: 0%

---

# Installera Java SDK

Java SDK distribueras av [Maven Central](https://search.maven.org/artifact/com.adobe.target/target-java-sdk). Om du vill komma igång lägger du till det som ett beroende genom att installera i `gradle` eller `maven`:

>[!BEGINTABS]

>[!TAB Gradle]

```javascript {line-numbers="true"}
compile 'com.adobe.target:java-sdk:1.0'
```

>[!TAB Maven]

```javascript {line-numbers="true"}
<dependency>
    <groupId>com.adobe.target</groupId>
    <artifactId>java-sdk</artifactId>
    <version>2.0</version>
</dependency>
```

>[!ENDTABS]

Den öppna källkoden finns på <https://github.com/adobe/target-java-sdk>.
