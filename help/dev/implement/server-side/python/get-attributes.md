---
title: Så här använder du asynkrona begäranden i  [!DNL Adobe Target] Python SDK
description: Lär dig hur  [!DNL Target] Python SDK stöder asynkrona begäranden, vilket kan minska den effektiva måltiden till noll.
feature: APIs/SDKs
exl-id: fafb9e28-5ac5-41c1-8e7f-f40550b6749f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '123'
ht-degree: 0%

---

# Hämta attribut (Python)

## Beskrivning

`get_attributes()` används för att hämta experimentella och personaliserade upplevelser från [!DNL Target] och extrahera attributvärden.


## Metod

### getAttributes

```python {line-numbers="true"}
target_client_instance.get_attributes(mbox_names, options)
```

## Parametrar

| Namn | Typ | Obligatoriskt | Standard | Beskrivning |
| --- | --- | --- | --- | --- |
| mbox_names | list[str] | Ja | Ingen | En lista med mbox-namn |
| alternativ | dict | Nej | Ingen | Samma alternativ som används för [Get Offers](get-offers.md) |

## AttributesProvider

`AttributesProvider` som returneras av `target_client.get_attributes()` har följande metoder:

| Metod | Returtyp | Beskrivning |
| --- | --- | --- |
| get_value(mbox_name, key) | alla | Returnerar värdet för ett angivet mbox-namn och attributnyckel |
| as_object(mbox_name) | dict | Returnerar ett enkelt json-objekt med nyckelvärdepar |
| get_response() | [TargetDeliveryResponse](https://github.com/adobe/target-python-sdk/blob/main/target_python_sdk/types/target_delivery_response.py) | Returnerar det svarsobjekt som normalt returneras av `get_offers` |

## Exempel

### Python

```python {line-numbers="true"}
def client_ready_callback():
    context = Context(channel=ChannelType.WEB)
    mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_attributes_options = {
      "request": delivery_request
    }

    attributes_provider = target_client.get_attributes(["demo-engineering-flags"], get_attributes_options)
    # returns just the value of searchProviderId from the demo-engineering-flags mbox offer
    search_provider_id = attributes_provider.get_value("demo-engineering-flags", "searchProviderId")

    # returns a simple dict object representing the demo-engineering-flags mbox offer
    engineering_flags = attributes_provider.as_object("demo-engineering-flags")
    """  the value of engineeringFlags looks like this
        {
            "cdnHostname": "cdn.cloud.corp.net",
            "searchProviderId": 143,
            "hasLegacyAccess": false
        }
    """

    asset_url = "http://{}/path/to/asset".format(engineering_flags.get("cdnHostname"))


client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
