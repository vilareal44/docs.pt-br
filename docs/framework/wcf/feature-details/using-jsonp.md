---
title: Usando JSONP
ms.date: 03/30/2017
ms.assetid: f386718c-b4ba-4931-a610-40c27a46672a
ms.openlocfilehash: 82290319b5d8b58708f0b2ebf40522ee76127b84
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594953"
---
# <a name="using-jsonp"></a>Usando JSONP

O preenchimento JSON (JSONP) é um mecanismo que permite o suporte a scripts entre sites em navegadores da Web. O JSONP foi projetado em relação à capacidade dos navegadores da Web de carregar scripts de um site diferente daquele no qual o documento carregado atual foi recuperado. O mecanismo funciona preenchendo a carga JSON com um nome de função de retorno de chamada definido pelo usuário, conforme mostrado no exemplo a seguir.

```javascript
callback({"a" = \\"b\\"});
```

No exemplo anterior, o conteúdo JSON, `{"a" = \\"b\\"}` , é encapsulado em uma chamada de função, `callback` . A função de retorno de chamada já deve estar definida na página da Web atual. O tipo de conteúdo de uma resposta JSONP é `application/javascript` .

JSONP não é habilitado automaticamente. Para habilitá-lo, defina o `javascriptCallbackEnabled` atributo como `true` em um dos pontos de extremidade padrão http ( <xref:System.ServiceModel.Description.WebHttpEndpoint> ou <xref:System.ServiceModel.Description.WebScriptEndpoint> ), conforme mostrado no exemplo a seguir.

```xml
<system.serviceModel>
  <standardEndpoints>
    <webHttpEndpoint>
      <standardEndpoint name="" javascriptCallbackEnabled="true"/>
    </webHttpEndpoint>
  </standardEndpoints>
</system.serviceModel>
```

O nome da função de retorno de chamada pode ser especificado em uma variável de consulta chamada callback, conforme mostrado na URL a seguir.

`http://baseaddress/Service/RestService?callback=functionName`

Quando invocado, o serviço envia uma resposta como a seguinte.

```javascript
functionName({"root":"Something"});
```  

Você também pode especificar o nome da função de retorno de chamada aplicando o <xref:System.ServiceModel.Web.JavascriptCallbackBehaviorAttribute> à classe de serviço, conforme mostrado no exemplo a seguir.

```csharp
[ServiceContract]
[JavascriptCallbackBehavior(ParameterName = "$callback")]
public class Service1
{
    [OperationContract]
    [WebGet(ResponseFormat=WebMessageFormat.Json)]
    public string GetData()
    {
    }
}
```

Para o serviço mostrado anteriormente, uma solicitação é semelhante ao seguinte.

`http://baseaddress/Service/RestService?$callback=anotherFunction`

Quando invocado, o serviço responde com o seguinte.

```javascript
anotherFunction ({"root":"Something"});
```

## <a name="http-status-codes"></a>Códigos de status HTTP

As respostas JSONP com códigos de status HTTP diferentes de 200 incluem um segundo parâmetro com a representação numérica do código de status HTTP, conforme mostrado no exemplo a seguir.

```javascript
anotherFunction ({"root":"Something"}, 201);
```

## <a name="validations"></a>Validações

As validações a seguir são executadas quando o JSONP está habilitado:

- A infraestrutura do WCF gera uma exceção se `javascriptCallback` estiver habilitada, um parâmetro de cadeia de caracteres de consulta de retorno de chamada está presente na solicitação e o formato de resposta é definido como JSON.

- Se a solicitação contiver o parâmetro de cadeia de caracteres de consulta de retorno de chamada, mas a operação não for um HTTP GET, o parâmetro de retorno de chamada será ignorado.

- Se o nome do retorno de chamada for `null` ou uma cadeia de caracteres vazia, a resposta não será formatada como JSONP.

## <a name="see-also"></a>Confira também

- [Visão geral do modelo de programação HTTP Web do WCF](wcf-web-http-programming-model-overview.md)
