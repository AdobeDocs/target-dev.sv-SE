---
title: Initiera  [!DNL Adobe Target] Java SDK för att logga begäranden
description: Lär dig hur du loggar begäranden i  [!DNL Adobe Target] Java SDK.
feature: APIs/SDKs
exl-id: 85d1a6ef-0b08-4948-8133-740b7d6141dd
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 0%

---

# Logger (Java)

## Beskrivning

När [SDK](initialize-sdk.md) initieras finns det flera alternativ för objektet `ClientConfig` som kan ställas in på loggbegäranden.

| Alternativ | Beskrivning |
| --- | --- |
| `logRequests` | Loggar hela begärandetexten och svarsbrödtexten. |
| `logRequestStatus` | Loggar begärans URL, status samt svarstid. |

[!DNL Target] Java SDK använder `slf4j`-loggning. Du måste ange din implementering av en logger som `java.util.logging`, `logback` och `log4j`. Mer information finns på [http://www.slf4j.org/manual.html](http://www.slf4j.org/manual.html). Alla loggar skrivs ut i `debug`.

## Exempel

Lägg till beroendet `slf4j`.

>[!BEGINTABS]

>[!TAB Gradle]

### Gråta

```javascript {line-numbers="true"}
compile 'org.slf4j:slf4j-simple:2.0.0-alpha0'
```

>[!TAB Maven]

```javascript {line-numbers="true"}
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-simple</artifactId>
    <version>2.0.0-alpha0</version>
</dependency>
```

>[!ENDTABS]

Aktivera `DEBUG`-loggarna baserat på din implementering och markera loggningsflaggorna för begäran.

### Felsök

```javascript {line-numbers="true"}
System.setProperty(SimpleLogger.DEFAULT_LOG_LEVEL_KEY, "DEBUG");
ClientConfig config = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .logRequests(true)
        .logRequestStatus(true)
        .build();

TargetClient targetClient = TargetClient.create(config);
```

Du bör se förfrågningar, svar och svarstider skrivas ut i konsolen.
