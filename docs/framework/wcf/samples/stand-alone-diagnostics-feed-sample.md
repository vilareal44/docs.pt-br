---
title: Exemplo de feed de diagnóstico independente
ms.date: 03/30/2017
ms.assetid: d31c6c1f-292c-4d95-8e23-ed8565970ea5
ms.openlocfilehash: 0402805b7eb5b0b224db32eb07780743e5f32fb3
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600913"
---
# <a name="stand-alone-diagnostics-feed-sample"></a>Exemplo de feed de diagnóstico independente
Este exemplo demonstra como criar um feed RSS/Atom para a distribuição com o Windows Communication Foundation (WCF). É um programa "Olá, Mundo" básico que mostra as noções básicas do modelo de objeto e como configurá-lo em um serviço de Windows Communication Foundation (WCF).  
  
 Os modelos do WCF são feeds de agregação como operações de serviço que retornam um tipo de dados especial, <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> . As instâncias do <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> podem serializar um feed nos formatos RSS 2,0 e Atom 1,0. O código de exemplo a seguir mostra o contrato usado.  
  
```csharp  
[ServiceContract(Namespace = "")]  
    interface IDiagnosticsService  
    {  
        [OperationContract]  
        //The [WebGet] attribute controls how WCF dispatches  
        //HTTP requests to service operations based on a URI suffix  
        //(the part of the request URI after the endpoint address)  
        //using the HTTP GET method. The UriTemplate specifies a relative  
        //path of 'feed', and specifies that the format is  
        //supplied using a query string.
        [WebGet(UriTemplate="feed?format={format}")]  
        [ServiceKnownType(typeof(Atom10FeedFormatter))]  
        [ServiceKnownType(typeof(Rss20FeedFormatter))]  
        SyndicationFeedFormatter GetProcesses(string format);  
    }  
```  
  
 A `GetProcesses` operação é anotada com o <xref:System.ServiceModel.Web.WebGetAttribute> atributo que permite controlar como o WCF distribui solicitações HTTP Get para operações de serviço e especifica o formato das mensagens enviadas.  
  
 Como qualquer serviço WCF, os feeds de distribuição podem ser hospedados automaticamente em qualquer aplicativo gerenciado. Os serviços de distribuição exigem uma associação específica (o <xref:System.ServiceModel.WebHttpBinding> ) e um comportamento de ponto de extremidade específico (o <xref:System.ServiceModel.Description.WebHttpBehavior> ) para funcionar corretamente. A nova <xref:System.ServiceModel.Web.WebServiceHost> classe fornece uma API conveniente para criar esses pontos de extremidade sem configuração específica.  
  
```csharp  
WebServiceHost host = new WebServiceHost(typeof(ProcessService), new Uri("http://localhost:8000/diagnostics"));  
  
            //The WebServiceHost will automatically provide a default endpoint at the base address  
            //using the proper binding (the WebHttpBinding) and endpoint behavior (the WebHttpBehavior)  
```  
  
 Como alternativa, você pode usar <xref:System.ServiceModel.Activation.WebServiceHostFactory> de dentro de um arquivo. svc hospedado pelo IIS para fornecer funcionalidade equivalente (essa técnica não é demonstrada neste código de exemplo).  
  
```xml
<% @ServiceHost Language="C#|VB" Debug="true" Service="ProcessService" %>
```
  
 Como esse serviço recebe solicitações usando o HTTP GET padrão, você pode usar qualquer cliente com reconhecimento de RSS ou ATOM para acessar o serviço. Por exemplo, você pode exibir a saída desse serviço navegando para `http://localhost:8000/diagnostics/feed/?format=atom` ou `http://localhost:8000/diagnostics/feed/?format=rss` em um navegador com reconhecimento de RSS.
  
 Você também pode usar a [forma como o modelo de objeto de distribuição do WCF mapeia para Atom e RSS](../feature-details/how-the-wcf-syndication-object-model-maps-to-atom-and-rss.md) para ler dados agregados e processá-los usando código imperativo.  
  
```csharp
XmlReader reader = XmlReader.Create( "http://localhost:8000/diagnostics/feed/?format=rss",
    new XmlReaderSettings()
    {
        //MaxCharactersInDocument can be used to control the maximum amount of data
        //read from the reader and helps prevent OutOfMemoryException
        MaxCharactersInDocument = 1024 * 64
    } );

SyndicationFeed feed = SyndicationFeed.Load(reader);

foreach (SyndicationItem i in feed.Items)
{
    XmlSyndicationContent content = i.Content as XmlSyndicationContent;
    ProcessData pd = content.ReadContent<ProcessData>();

    Console.WriteLine(i.Title.Text);
    Console.WriteLine(pd.ToString());
}
```
  
## <a name="set-up-build-and-run-the-sample"></a>Configurar, compilar e executar o exemplo
  
1. Verifique se você tem a permissão de registro de endereço correta para HTTP e HTTPS no computador, conforme explicado no procedimento configurar instruções no [processo de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Compile a solução.

3. Execute o aplicativo de console.

4. Enquanto o aplicativo de console estiver em execução, navegue até `http://localhost:8000/diagnostics/feed/?format=atom` ou `http://localhost:8000/diagnostics/feed/?format=rss` use um navegador com reconhecimento de RSS.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Syndication\DiagnosticsFeed`

## <a name="see-also"></a>Consulte também

- [Modelo de programação WCF Web HTTP](../feature-details/wcf-web-http-programming-model.md)
- [Sindicalização do WCF](../feature-details/wcf-syndication.md)
