---
title: Serviço de AJAX com exemplo de JSON e XML
ms.date: 03/30/2017
ms.assetid: 8ea5860d-0c42-4ae9-941a-e07efdd8e29c
ms.openlocfilehash: 8f70b6aa2e61d01a075a6edb3fe490ef593e73b0
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575947"
---
# <a name="ajax-service-with-json-and-xml-sample"></a>Serviço de AJAX com exemplo de JSON e XML

Este exemplo demonstra como usar o Windows Communication Foundation (WCF) para criar um serviço de JavaScript e XML (AJAX) assíncrono que retorna dados de JavaScript Object Notation (JSON) ou XML. Você pode acessar um serviço AJAX usando o código JavaScript de um cliente de navegador da Web. Este exemplo baseia-se no exemplo de [serviço básico Ajax](basic-ajax-service.md) .

Ao contrário dos outros exemplos do AJAX, este exemplo não usa o ASP.NET AJAX e o <xref:System.Web.UI.ScriptManager> controle. Com algumas configurações adicionais, os serviços WCF AJAX podem ser acessados de qualquer página HTML por meio de JavaScript, e esse cenário é mostrado aqui. Para obter um exemplo de como usar o WCF com o ASP.NET AJAX, consulte [exemplos de AJAX](ajax.md).

Este exemplo mostra como alternar o tipo de resposta de uma operação entre JSON e XML. Essa funcionalidade está disponível independentemente de o serviço estar configurado para ser acessado pelo ASP.NET AJAX ou por uma página de cliente HTML/JavaScript.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

Para habilitar o uso de clientes non-ASP.NET AJAX, use <xref:System.ServiceModel.Activation.WebServiceHostFactory> (não <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory> ) no arquivo. svc. <xref:System.ServiceModel.Activation.WebServiceHostFactory>Adiciona um <xref:System.ServiceModel.Description.WebHttpEndpoint> ponto de extremidade padrão ao serviço. O ponto de extremidade está configurado em um endereço vazio relativo ao arquivo. svc; Isso significa que o endereço do serviço é `http://localhost/ServiceModelSamples/service.svc` , sem nenhum sufixo adicional além do nome da operação.

`<%@ServiceHost language="c#" Debug="true" Service="Microsoft.Samples.XmlAjaxService.CalculatorService" Factory="System.ServiceModel.Activation.WebServiceHostFactory" %>`

A seção a seguir no Web. config pode ser usada para fazer alterações de configuração adicionais no ponto de extremidade. Ela poderá ser removida se nenhuma alteração extra for necessária.

```xml
<system.serviceModel>
  <standardEndpoints>
    <webHttpEndpoint>
      <!-- Use this element to configure the endpoint -->
      <standardEndpoint name="" />
    </webHttpEndpoint>
  </standardEndpoints>
</system.serviceModel>
```

O formato de dados padrão para <xref:System.ServiceModel.Description.WebHttpEndpoint> o é XML, enquanto o formato de dados padrão para <xref:System.ServiceModel.Description.WebScriptEndpoint> é JSON. Para obter mais informações, consulte [CREATING WCF Ajax Services with ASP.net](../feature-details/creating-wcf-ajax-services-without-aspnet.md).

O serviço no exemplo a seguir é um serviço padrão do WCF com duas operações. Ambas as operações exigem o <xref:System.ServiceModel.Web.WebMessageBodyStyle.Wrapped> estilo do corpo <xref:System.ServiceModel.Web.WebGetAttribute> nos <xref:System.ServiceModel.Web.WebInvokeAttribute> atributos ou, que são específicos do `webHttp` comportamento e não têm nenhuma influência sobre a opção de formato de dados JSON/XML.

```csharp
[OperationContract]
[WebInvoke(ResponseFormat = WebMessageFormat.Xml, BodyStyle = WebMessageBodyStyle.Wrapped)]
MathResult DoMathXml(double n1, double n2);
```

O formato de resposta para a operação é especificado como XML, que é a configuração padrão para o [\<webHttp>](../../configure-apps/file-schema/wcf/webhttp.md) comportamento. No entanto, é uma prática recomendada especificar explicitamente o formato de resposta.

A outra operação usa o `WebInvokeAttribute` atributo e especifica explicitamente JSON em vez de XML para a resposta.

```csharp
[OperationContract]
[WebInvoke(ResponseFormat = WebMessageFormat.Json, BodyStyle = WebMessageBodyStyle.Wrapped)]
MathResult DoMathJson(double n1, double n2);
```

Observe que em ambos os casos as operações retornam um tipo complexo, `MathResult` , que é um tipo de contrato de dados padrão do WCF.

A página da Web do cliente XmlAjaxClientPage. htm contém o código JavaScript que invoca uma das duas operações anteriores quando o usuário clica nos botões **executar cálculo (retornar JSON)** ou **executar cálculo (XML de retorno)** na página. O código para invocar o serviço constrói um corpo JSON e o envia usando HTTP POST. A solicitação é criada manualmente em JavaScript, diferentemente do exemplo de [serviço AJAX básico](basic-ajax-service.md) e de outros exemplos usando o ASP.NET AJAX.

```csharp
// Create HTTP request
var xmlHttp;
// Request instantiation code omitted…
// Result handler code omitted…

// Build the operation URL
var url = "service.svc/ajaxEndpoint/";
url = url + operation;

// Build the body of the JSON message
var body = '{"n1":';
body = body + document.getElementById("num1").value + ',"n2":';
body = body + document.getElementById("num2").value + '}';

// Send the HTTP request
xmlHttp.open("POST", url, true);
xmlHttp.setRequestHeader("Content-type", "application/json");
xmlHttp.send(body);
```

Quando o serviço responde, a resposta é exibida sem nenhum processamento adicional em uma caixa de texto na página. Isso é implementado para fins de demonstração para permitir que você observe diretamente os formatos de dados XML e JSON usados.

```javascript
// Create result handler
xmlHttp.onreadystatechange=function(){
     if(xmlHttp.readyState == 4){
          document.getElementById("result").value = xmlHttp.responseText;
     }
}
```

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\AJAX\XmlAjaxService`

#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Crie a solução XmlAjaxService. sln, conforme descrito em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Navegue até `http://localhost/ServiceModelSamples/XmlAjaxClientPage.htm` (não abra XmlAjaxClientPage. htm no navegador do diretório do projeto).

## <a name="see-also"></a>Consulte também

- [Serviço AJAX utilizando HTTP POST](ajax-service-using-http-post.md)
