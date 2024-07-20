---
title: Integrering med Experience Cloud AAM segment
description: Integrering med Experience Cloud, Audience Manager
keywords: delivery api, server-side, server-side, integration, målgruppshanterare, aam
exl-id: c21e0200-23ba-4a0b-adf4-38e03c087f00
feature: Implement Server-side
source-git-commit: e3f14e97fa48ffb1f07b29aca5711d16e75faa80
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 0%

---

# AAM segment

[!DNL Adobe Audience Manager] segment kan utnyttjas via [!DNL Adobe Target] SDK:er. För att kunna utnyttja AAM segment måste följande fält anges:

>[!NOTE]
>
>AAM segment stöds inte för enhetsspecifika beslutsaktiviteter.

| Fältnamn | Obligatoriskt | Beskrivning |
| --- | --- | --- |
| `locationHint` | Ja | DCS-platstips används för att avgöra vilken AAM DCS-slutpunkt som ska tryckas för att hämta profilen. Måste vara >= 1. |
| `marketingCloudVisitorId` | Ja | Marketing Cloud Visitor-ID |
| `blob` | Ja | AAM används för att skicka ytterligare data till AAM. Får inte vara tom och storleken &lt;= 1024. |

SDK fyller automatiskt i dessa fält när du gör ett `getOffers`-metodanrop, men du måste se till att en giltig besöks-cookie anges. För att få denna cookie måste du implementera VisitorAPI.js i webbläsaren.

## Användarhandbok om implementering

### Användning av cookies

Cookies används för att korrelera [!DNL Adobe Audience Manager]-begäranden med [!DNL Adobe Target]-begäranden. Detta är de cookies som används i den här implementeringen.

| Cookie | Namn | Beskrivning |
| --- | --- | --- |
| besöks-cookie | `AMCVS_XXXXXXXXXXXXXXXXXXXXXXXX%40AdobeOrg` | Den här cookien anges av `VisitorAPI.js` när den initieras med `visitorState` från målsvaret `getOffers`. |
| mål-cookie | `mbox` | Webbservern måste ange den här cookien med namnet och värdet för `targetCookie` från `getOffers`-målsvaret. |

### Översikt över steg

Anta att en användare anger en URL i en webbläsare som skickar en begäran till webbservern. När den begäran uppfylls:

1. Servern läser besökar- och målcookies från begäran.
1. Servern anropar metoden `getOffers` för SDK:n [!DNL Target] och anger besökar- och målcookies om sådana finns.
1. När anropet `getOffers` har slutförts används värden för `targetCookie` och `visitorState` från svaret.
   1. En cookie har angetts för svaret med värden från `targetCookie`. Detta görs med svarshuvudet `Set-Cookie`, som anger för webbläsaren att behålla målcookien.
   1. Ett HTML-svar förbereds som initierar `VisitorAPI.js` och skickar in `visitorState` från målsvaret.
1. HTML-svaret läses in i webbläsaren..
   1. `VisitorAPI.js` ingår i dokumenthuvudet.
   1. VisitorAPI har initierats med `visitorState` från SDK-svaret `getOffers`. Detta gör att besökarens cookie ställs in i webbläsaren så att den skickas till servern vid efterföljande begäranden.

### Exempelkod

Följande kodexempel implementerar vart och ett av stegen ovan. Varje steg visas i koden som en intern kommentar bredvid implementeringen.

#### Node.js

Det här exemplet bygger på [express, ett Node.js-webbramverk](https://expressjs.com/).

>[!BEGINTABS]

>[!TAB server.js]

```js {line-numbers="true"}
const fs = require("fs");
const express = require("express");
const cookieParser = require("cookie-parser");
const Handlebars = require("handlebars");
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  timeout: 10000,
  logger: console,
};
const targetClient = TargetClient.create(CONFIG);
const TEMPLATE = fs.readFileSync(`${__dirname}/index.handlebars`).toString();
const handlebarsTemplate = Handlebars.compile(TEMPLATE);

Handlebars.registerHelper("toJSON", function (object) {
  return new Handlebars.SafeString(JSON.stringify(object, null, 4));
});

const app = express();
app.use(cookieParser());
app.use(express.static(__dirname + "/public"));

app.get("/", async (req, res) => {
  // The server reads the visitor and target cookies from the request.
  const visitorCookie =
    req.cookies[
      encodeURIComponent(
        TargetClient.getVisitorCookieName(CONFIG.organizationId)
      )
    ];
  const targetCookie = req.cookies[TargetClient.TargetCookieName];
  const address = { url: req.headers.host + req.originalUrl };

  const targetRequest = {
    execute: {
      mboxes: [
        { name: "homepage", index: 1, address },
        { name: "SummerShoesOffer", index: 2, address },
        { name: "SummerDressOffer", index: 3, address }
      ],
    },
  };

  res.set({
    "Content-Type": "text/html",
    Expires: new Date().toUTCString(),
  });

  try {
    // The server makes a call to the `getOffers` method of the Target SDK specifying the visitor and target cookies if available.
    const targetResponse = await targetClient.getOffers({
      request: targetRequest,
      visitorCookie,
      targetCookie,
    });

    // When the `getOffers` call is fulfilled, values for `targetCookie` and `visitorState` from the response are used.
    // A cookie is set on the response with values taken from `targetCookie`.  This is done using the `Set-Cookie` response header which tells the browser to persist the target cookie.
    res.cookie(
      targetResponse.targetCookie.name,
      targetResponse.targetCookie.value,
      { maxAge: targetResponse.targetCookie.maxAge * 1000 }
    );

    // An HTML response is prepared that initializes VisitorAPI.js and passes in `visitorState` from the target response.
    const html = handlebarsTemplate({
      organizationId: CONFIG.organizationId,
      targetResponse,
    });

    res.status(200).send(html);
  } catch (error) {
    console.error("Target:", error);
    res.status(500).send(error);
  }
});

app.listen(3000, function () {
  console.log("Listening on port 3000 and watching!");
});
```

>[!TAB index.handlebars]

```html {line-numbers="true"}
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ECID (Visitor API) Integration Sample</title>

  <!-- VisitorAPI.js is included in the document header. -->
  <script src="VisitorAPI.js"></script>
  <script>
    // VisitorAPI is initialized with visitorState from the `getOffers` SDK response. This will cause the visitor cookie to be set in the browser so it will be sent to the server on subsequent requests.
    Visitor.getInstance("{{organizationId}}", {serverState: {{toJSON targetResponse.visitorState}} });
  </script>
</head>
<body>
  <h1>response</h1>
  <pre>{{toJSON targetResponse}}</pre>
</body>
</html>
```

>[!ENDTABS]

#### Java

I det här exemplet används [spring, ett Java-webbramverk](https://spring.io/).

>[!BEGINTABS]

>[!TAB ClientSampleApplication.java]

```java {line-numbers="true"}
@SpringBootApplication
public class ClientSampleApplication {

    public static void main(String[] args) {
        System.setProperty(SimpleLogger.DEFAULT_LOG_LEVEL_KEY, "DEBUG");
        SpringApplication.run(ClientSampleApplication.class, args);
    }

    @Bean
    TargetClient marketingCloudClient() {
        ClientConfig clientConfig = ClientConfig.builder()
                .client("acmeclient")
                .organizationId("1234567890@AdobeOrg")
                .defaultDecisioningMethod(DecisioningMethod.SERVER_SIDE)
                .build();

        return TargetClient.create(clientConfig);
    }
}
```

>[!TAB TargetController.java]

```java {line-numbers="true"}
@Controller
@RequestMapping("/")
public class TargetController {

    @Autowired
    private TargetClientService targetClientService;

    @GetMapping
    public String index(Model model, HttpServletRequest request, HttpServletResponse response) {
        // The server reads the visitor and target cookies from the request.
        List<TargetCookie> targetCookies = getTargetCookies(request.getCookies());

        Address address = getAddress(request);

        List<MboxRequest> mboxRequests = new ArrayList<>();
        mboxRequests.add((MboxRequest) new MboxRequest().name("homepage").index(1).address(address));
        mboxRequests.add((MboxRequest) new MboxRequest().name("SummerShoesOffer").index(2).address(address));
        mboxRequests.add((MboxRequest) new MboxRequest().name("SummerDressOffer").index(3).address(address));

        TargetDeliveryResponse targetDeliveryResponse = targetClientService.getOffers(mboxRequests, targetCookies, request,
                response);

        // An HTML response is prepared that initializes VisitorAPI.js and passes in `visitorState` from the target response.
        model.addAttribute("visitorState", targetDeliveryResponse.getVisitorState());
        model.addAttribute("targetResponse", targetDeliveryResponse);
        model.addAttribute("organizationId", "1234567890@AdobeOrg");

        return "index";
    }
}
```

>[!TAB TargetClientService.java]

```java {line-numbers="true"}
@Service
public class TargetClientService {

    private final TargetClient targetJavaClient;

    public TargetClientService(TargetClient targetJavaClient) {
        this.targetJavaClient = targetJavaClient;
    }

    public TargetDeliveryResponse getOffers(List<MboxRequest> executeMboxes, List<TargetCookie> cookies, HttpServletRequest request, HttpServletResponse response) {

        Context context = getContext(request);
        ExecuteRequest executeRequest = new ExecuteRequest();
        executeRequest.setMboxes(executeMboxes);

        TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
                .context(context)
                .execute(executeRequest)
                .cookies(cookies)
                .build();

        // The server makes a call to the `getOffers` method of the Target SDK specifying the visitor and target cookies if available.
        TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);

        // When the `getOffers` call is fulfilled, values for `targetCookie` and `visitorState` from the response are used.
        // A cookie is set on the response with values taken from `targetCookie`.  This is done using the `Set-Cookie` response header which tells the browser to persist the target cookie.
        setCookies(targetResponse.getCookies(), response);
        return targetResponse;
    }
}
```

>[!TAB TargetRequestUtils.java]

```java {line-numbers="true"}
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Target Only : GetOffer</title>

    <!-- VisitorAPI.js is included in the document header. -->
    <script src="../../js/VisitorAPI.js"></script>
    <script th:inline="javascript">
        // VisitorAPI is initialized with visitorState from the `getOffers` SDK response. This will cause the visitor cookie to be set in the browser so it will be sent to the server on subsequent requests.
        Visitor.getInstance(/*[[${organizationId}]]*/ "", {serverState: /*[[${visitorState}]]*/ {}});
    </script>
</head>
<body>
    <h1>response</h1>
    <pre>[[${targetResponse}]]</pre>
</body>
</html>
```

>[!ENDTABS]

Mer information om `TargetRequestUtils.java` finns i [Verktygsmetoder (Java)](https://experienceleague.adobe.com/docs/target-dev/developer/server-side/java/utility-methods.html){target=_blank}
