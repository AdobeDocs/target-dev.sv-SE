---
title: Så här använder du asynkrona begäranden i  [!DNL Adobe Target] Python SDK
description: Lär dig hur  [!DNL Target] Python SDK stöder asynkrona begäranden, vilket kan minska den effektiva måltiden till noll.
feature: APIs/SDKs
exl-id: 44ab74e5-3c1a-49cf-9fff-fe523b0c2592
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 0%

---

# Asynkrona begäranden (Python)

## Beskrivning

En fördel med integration på serversidan är att du kan utnyttja den enorma bandbredd och de datorresurser som finns tillgängliga på serversidan genom att använda parallellitet. [!DNL Target] Python SDK har stöd för asynkrona begäranden, vilket kan minska den effektiva måltiden till noll.

## Metoder som stöds

### Python

```python {line-numbers="true"}
get_offers(options)
send_notifications(options)
get_attributes(mbox_names, options)
```

## Exempel

Ett exempelprogram som använder `asyncio`-modulens asynkrona/förväntade i Python 3.9+ kan se ut så här:

### Python

```python {line-numbers="true"}
async def execute_mboxes(self, mboxes):
    context = Context(channel=ChannelType.WEB)
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_offers_options = {
      "request": delivery_request
    }
    return await asyncio.to_thread(target_client.get_offers, get_offers_options)

async def get_target_delivery_response(mboxes):
    target_delivery_response = await execute_mboxes(mboxes)
    response = Response(target_delivery_response.get("response").to_str(), status=200, mimetype='application/json')
    return response

mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
return asyncio.run(get_target_delivery_response(mboxes)
```

I det här exemplet antas du använda Python 3.9+. Om du använder en äldre version av Python kan du fortfarande skicka asynkrona begäranden genom att skicka in `options.callback` till `get_offers`. Titta i exempelappen Flash om du vill ha mer information om asynkron körning med antingen återanrop eller asynkron/await, [här](https://github.com/adobe/target-python-sdk/blob/main/samples/app.py).
