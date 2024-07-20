---
title: Så här använder du asynkrona begäranden i  [!DNL Adobe Target] Java SDK
description: Lär dig hur  [!DNL Target] Java SDK stöder asynkrona begäranden, vilket kan minska den effektiva måltiden till noll.
feature: APIs/SDKs
exl-id: e11f8d16-76f6-4d39-822a-34a1cf7f623f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Asynkrona begäranden (Java)

## Beskrivning

En fördel med integration på serversidan är att du kan utnyttja den enorma bandbredd och de datorresurser som finns tillgängliga på serversidan genom att använda parallellitet. [!DNL Target] Java SDK har stöd för asynkrona begäranden, vilket kan minska den effektiva måltiden till noll.

## Metoder som stöds

### Metoder

```javascript {line-numbers="true"}
CompletableFuture<TargetDeliveryResponse> getOffersAsync(TargetDeliveryRequest request);
CompletableFuture<ResponseStatus> sendNotificationsAsync(TargetDeliveryRequest request);
CompletableFuture<Attributes> getAttributesAsync(TargetDeliveryRequest targetRequest, String ...mboxes);
```

## Exempel

Ett exempel på `Spring`-programstyrenhet kan se ut så här:

### Exempelstyrenhet

```javascript {line-numbers="true"}
@RestController
public class TargetRestController {

    @Autowired
    private TargetClient targetJavaClient;

    @GetMapping("/mboxTargetOnlyAsync")
        public TargetDeliveryResponse mboxTargetOnlyAsync(
                @RequestParam(name = "mbox", defaultValue = "server-side-mbox") String mbox,
                HttpServletRequest request, HttpServletResponse response) {
            ExecuteRequest executeRequest = new ExecuteRequest()
                    .mboxes(getMboxRequests(mbox));

            TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
                    .context(getContext(request))
                    .execute(executeRequest)
                    .cookies(getTargetCookies(request.getCookies()))
                    .build();
            CompletableFuture<TargetDeliveryResponse> targetResponseAsync =
                    targetJavaClient.getOffersAsync(targetDeliveryRequest);
            targetResponseAsync.thenAccept(tr -> setCookies(tr.getCookies(), response));
            simulateIO();
            TargetDeliveryResponse targetResponse = targetResponseAsync.join();
            return targetResponse;
        }

    /**
     * Function for simulating network calls like other microservices and database calls
     */
    private void simulateIO() {
        try {
            Thread.sleep(200L);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}
```

I det här exemplet antas att du har [initierat SDK](initialize-sdk.md) som en vårböna och att du har [verktygsmetoder](utility-methods.md) tillgängliga.

[!DNL Target]-begäran utlöses före `simulateIO` och när den körs bör målresultatet också vara klart. Även om så inte är fallet har ni stora besparingar i de flesta fall.
