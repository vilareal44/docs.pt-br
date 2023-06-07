---
title: Formatação HTTP Web WCF
ms.date: 03/30/2017
ms.assetid: e2414896-5463-41cd-b0a6-026a713eac2c
ms.openlocfilehash: 011ff4f2e667268fac1aa2d82c0a2c4ffefc8dde
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585552"
---
# <a name="wcf-web-http-formatting"></a>Formatação HTTP Web WCF
O modelo de programação Web HTTP do WCF permite que você determine dinamicamente o melhor formato para uma operação de serviço retornar sua resposta. Dois métodos para determinar um formato apropriado são suportados: Automatic e Explicit.  
  
## <a name="automatic-formatting"></a>Formatação automática  
 Quando habilitada, a formatação automática escolhe o melhor formato para retornar a resposta. Ele determina o melhor formato verificando o seguinte, na ordem:  
  
1. Os tipos de mídia no cabeçalho Accept da mensagem de solicitação.  
  
2. O tipo de conteúdo da mensagem de solicitação.  
  
3. A configuração de formato padrão na operação.  
  
4. A configuração de formato padrão no WebHttpBehavior.  
  
 Se a mensagem de solicitação contiver um cabeçalho Accept, a infraestrutura Windows Communication Foundation (WCF) procurará um tipo ao qual ele dá suporte. Se o `Accept` cabeçalho especificar prioridades para seus tipos de mídia, eles serão cumpridos. Se nenhum formato adequado for encontrado no `Accept` cabeçalho, o tipo de conteúdo da mensagem de solicitação será usado. Se nenhum tipo de conteúdo adequado for especificado, a configuração de formato padrão para a operação será usada. O formato padrão é definido com o `ResponseFormat` parâmetro dos <xref:System.ServiceModel.Web.WebGetAttribute> atributos e <xref:System.ServiceModel.Web.WebInvokeAttribute> . Se nenhum formato padrão for especificado na operação, o valor da <xref:System.ServiceModel.Description.WebHttpBehavior.DefaultOutgoingResponseFormat%2A> propriedade será usado. A formatação automática depende da <xref:System.ServiceModel.Description.WebHttpBehavior.AutomaticFormatSelectionEnabled%2A> propriedade. Quando essa propriedade é definida como `true` , a infraestrutura do WCF determina o melhor formato a ser usado. A seleção automática de formato é desabilitada por padrão para compatibilidade com versões anteriores. A seleção automática de formato pode ser habilitada programaticamente ou por meio da configuração. O exemplo a seguir mostra como habilitar a seleção automática de formato no código.  
  
```csharp
// This code assumes the service name is MyService and the service contract is IMyContract
Uri baseAddress = new Uri("http://localhost:8000");  
  
WebServiceHost host = new WebServiceHost(typeof(MyService), baseAddress)  
try  
{  
   ServiceEndpoint sep = host.AddServiceEndpoint(typeof(IMyContract), new WebHttpBinding(), "");  
   // Check it see if the WebHttpBehavior already exists  
   WebHttpBehavior whb = sep.Behaviors.Find<WebHttpBehavior>();  
  
   if (whb != null)  
   {  
      whb.AutomaticFormatSelectionEnabled = true;  
   }  
   else  
   {  
      WebHttpBehavior webBehavior = new WebHttpBehavior();  
      webBehavior.AutomaticFormatSelectionEnabled = true;  
      sep.Behaviors.Add(webBehavior);  
   }  
         // Open host to start listening for messages  
   host.Open();
  
  // ...  
}  
  catch(CommunicationException ex)  
  {  
     Console.WriteLine("An exception occurred: " + ex.Message());  
  }  
```  
  
 A formatação automática também pode ser habilitada por meio da configuração. Você pode definir a <xref:System.ServiceModel.Description.WebHttpBehavior.AutomaticFormatSelectionEnabled%2A> propriedade diretamente no <xref:System.ServiceModel.Description.WebHttpBehavior> ou usando o <xref:System.ServiceModel.Description.WebHttpEndpoint> . O exemplo a seguir mostra como habilitar a seleção de formato automático no <xref:System.ServiceModel.Description.WebHttpBehavior> .  
  
```xml  
<system.serviceModel>  
  <behaviors>  
    <endpointBehaviors>  
      <behavior>  
        <webHttp automaticFormatSelectionEnabled="true" />  
      </behavior>  
    </endpointBehaviors>  
  </behaviors>  
  <standardEndpoints>  
    <webHttpEndpoint>  
      <!-- the "" standard endpoint is used by WebServiceHost for auto creating a web endpoint. -->  
      <standardEndpoint name="" helpEnabled="true" />  
    </webHttpEndpoint>  
  </standardEndpoints>  
</system.serviceModel>  
```  
  
 O exemplo a seguir mostra como habilitar a seleção automática de formato usando <xref:System.ServiceModel.Description.WebHttpEndpoint> .  
  
```xml  
<system.serviceModel>  
    <standardEndpoints>  
      <webHttpEndpoint>  
        <!-- the "" standard endpoint is used by WebServiceHost for auto creating a web endpoint. -->  
        <standardEndpoint name="" helpEnabled="true" automaticFormatSelectionEnabled="true"  />  
      </webHttpEndpoint>  
    </standardEndpoints>  
  </system.serviceModel>  
```  
  
## <a name="explicit-formatting"></a>Formatação explícita  
 Como o nome indica, em formatação explícita, o desenvolvedor determina o melhor formato a ser usado no código de operação. Se o melhor formato for XML ou JSON, o desenvolvedor será definido <xref:System.ServiceModel.Web.OutgoingWebResponseContext.Format%2A> como <xref:System.ServiceModel.Web.WebMessageFormat.Xml> ou <xref:System.ServiceModel.Web.WebMessageFormat.Json> . Se a <xref:System.ServiceModel.Web.OutgoingWebResponseContext.Format%2A> propriedade não estiver definida explicitamente, o formato padrão da operação será usado.  
  
 O exemplo a seguir verifica o parâmetro de cadeia de caracteres de consulta de formato para um formato a ser usado. Se ele tiver sido especificado, ele definirá o formato da operação usando <xref:System.ServiceModel.Web.OutgoingWebResponseContext.Format%2A> .  
  
```csharp
public class Service : IService  
{  
    [WebGet]  
     public string EchoWithGet(string s)  
    {  
        // if a format query string parameter has been specified, set the response format to that. If no such
        // query string parameter exists the Accept header will be used
        string formatQueryStringValue = WebOperationContext.Current.IncomingRequest.UriTemplateMatch.QueryParameters["format"];  
        if (!string.IsNullOrEmpty(formatQueryStringValue))  
        {  
            if (formatQueryStringValue.Equals("xml", System.StringComparison.OrdinalIgnoreCase))  
            {
                WebOperationContext.Current.OutgoingResponse.Format = WebMessageFormat.Xml;
            }
            else if (formatQueryStringValue.Equals("json", System.StringComparison.OrdinalIgnoreCase))  
            {  
                WebOperationContext.Current.OutgoingResponse.Format = WebMessageFormat.Json;  
            }  
            else  
            {  
                throw new WebFaultException<string>($"Unsupported format '{formatQueryStringValue}'",   HttpStatusCode.BadRequest);
            }  
        }  
        return "You said " + s;  
    }  
```  
  
 Se você precisar dar suporte a formatos diferentes de XML ou JSON, defina sua operação para que ele tenha um tipo de retorno <xref:System.ServiceModel.Channels.Message> . No código da operação, determine o formato apropriado a ser usado e, em seguida, crie um <xref:System.ServiceModel.Channels.Message> objeto usando um dos seguintes métodos:  
  
- `WebOperationContext.CreateAtom10Response`  
  
- `WebOperationContext.CreateJsonResponse`  
  
- `WebOperationContext.CreateStreamResponse`  
  
- `WebOperationContext.CreateTextResponse`  
  
- `WebOperationContext.CreateXmlResponse`  
  
 Cada um desses métodos usa o conteúdo e cria uma mensagem com o formato apropriado. O `WebOperationContext.Current.IncomingRequest.GetAcceptHeaderElements` método pode ser usado para obter uma lista de formatos preferenciais pelo cliente em ordem decrescente de preferência. O exemplo a seguir mostra como usar o `WebOperationContext.Current.IncomingRequest.GetAcceptHeaderElements` para determinar o formato a ser usado e, em seguida, usa o método de criação de resposta apropriado para criar a mensagem de resposta.  
  
```csharp
public class Service : IService  
{  
    public Message EchoListWithGet(string list)  
    {  
        List<string> returnList = new List<string>(list.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries));  
        IList<ContentType> acceptHeaderElements = WebOperationContext.Current.IncomingRequest.GetAcceptHeaderElements();  
        for (int x = 0; x < acceptHeaderElements.Count; x++)  
        {  
            string normalizedMediaType = acceptHeaderElements[x].MediaType.ToLowerInvariant();  
            switch (normalizedMediaType)  
            {  
                case "image/jpeg": return CreateJpegResponse(returnList);  
                case "application/xhtml+xml": return CreateXhtmlResponse(returnList);  
                case "application/atom+xml": return CreateAtom10Response(returnList);  
                case "application/xml": return CreateXmlResponse(returnList);  
                case "application/json": return CreateJsonResponse(returnList);  
          }  
    }  
  
    // Default response format is XML  
    return CreateXmlResponse(returnList);  
    }  
}  
```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.UriTemplate>
- <xref:System.UriTemplateMatch>
- [Modelo de programação WCF Web HTTP](wcf-web-http-programming-model.md)
- [UriTemplate and UriTemplateTable](uritemplate-and-uritemplatetable.md)
- [Visão geral do modelo de programação HTTP Web do WCF](wcf-web-http-programming-model-overview.md)
- [Modelo de objeto de programação HTTP Web do WCF](wcf-web-http-programming-object-model.md)
