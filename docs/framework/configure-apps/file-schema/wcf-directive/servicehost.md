---
title: '@ServiceHost'
ms.date: 03/30/2017
ms.assetid: 96ba6967-00f2-422f-9aa7-15de4d33ebf3
ms.openlocfilehash: cb425d9f4dadd97e93946a2b4cd9d059ea8504ce
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051357"
---
# <a name="servicehost"></a>\@ServiceHost

Associa a fábrica usada para produzir o host de serviço com o serviço a ser hospedado e outros aspectos de programação necessários para acessar ou compilar o código de hospedagem fornecido no arquivo. svc.

## <a name="syntax"></a>Syntax

```aspx-csharp
<% @ServiceHost
Service = "Service, ServiceNamespace"
Factory = "Factory, FactoryNamespace"
Debug = "Debug"
Language = "Language"
CodeBehind = "CodeBehind"
%>
```

## <a name="attributes"></a>Atributos

### <a name="service"></a>Serviço

O nome do tipo CLR do serviço hospedado. Deve ser um nome qualificado de um tipo que implementa um ou mais dos contatos de serviço.

### <a name="factory"></a>Factory

O nome do tipo CLR da fábrica do host de serviço usada para instanciar o host de serviço. Esse atributo é opcional. Se não for especificado, o padrão <xref:System.ServiceModel.Activation.ServiceHostFactory> será usado, que retorna uma instância de <xref:System.ServiceModel.ServiceHost> .

### <a name="debug"></a>Depurar

Indica se o serviço de Windows Communication Foundation (WCF) deve ser compilado com símbolos de depuração. `true`Se o serviço WCF deve ser compilado com símbolos de depuração; caso contrário, `false` .

### <a name="language"></a>Linguagem

Especifica o idioma usado ao compilar todo o código embutido no arquivo (. svc). Os valores podem representar qualquer. Linguagem com suporte .net, incluindo `C#` , `VB` e `JS` , que se refere a C#, Visual Basic e JScript .net, respectivamente. Esse atributo é opcional.

### <a name="codebehind"></a>CodeBehind

Especifica o arquivo de origem que implementa o serviço Web XML, quando a classe que implementa o serviço Web XML não reside no mesmo arquivo e não foi compilada em um assembly e colocada no diretório *\Bin* .

## <a name="remarks"></a>Comentários

O <xref:System.ServiceModel.ServiceHost> usado para hospedar o serviço é um ponto de extensibilidade dentro do modelo de programação de Windows Communication Foundation (WCF). Um padrão de fábrica é usado para instanciar o <xref:System.ServiceModel.ServiceHost> porque é, potencialmente, um tipo polimórfico que o ambiente de hospedagem não deve instanciar diretamente.

A implementação padrão usa o <xref:System.ServiceModel.Activation.ServiceHostFactory> para criar uma instância do <xref:System.ServiceModel.ServiceHost> . Mas você pode fornecer sua própria fábrica (uma que retorna o host derivado) especificando o nome do tipo CLR da sua implementação de fábrica na `@ServiceHost` diretiva.

Para usar sua própria fábrica de hosts de serviço personalizada em vez da fábrica padrão, basta fornecer o nome do tipo na `@ServiceHost` diretiva da seguinte maneira.

```xml
<% @ServiceHost Factory="DerivedFactory" Service="MyService" %>
```

Mantenha as implementações de fábrica o mais leve possível. Se você tiver muita lógica personalizada, seu código será mais reutilizável se você colocar essa lógica dentro de seu host em vez de dentro da fábrica.

Por exemplo, para habilitar um ponto de extremidade habilitado para AJAX para `MyService` , especifique o <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> para o valor do `Factory` atributo, em vez do padrão <xref:System.ServiceModel.Activation.ServiceHostFactory> , na `@ServiceHost` diretiva, conforme mostrado no exemplo a seguir:

```aspx-csharp
<% @ServiceHost
Service="MyService"
Language="C#"
Debug="true"
Factory="WebScriptServiceHostFactory"
%>
```

## <a name="see-also"></a>Consulte também

- [Host de serviço personalizado](../../../wcf/samples/custom-service-host.md)
