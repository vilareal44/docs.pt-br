---
title: Validação de segurança
ms.date: 03/30/2017
ms.assetid: 48dcd496-0c4f-48ce-8b9b-0e25b77ffa58
ms.openlocfilehash: 70408976469b1cbcf9c4679bd91d81872ec74ae1
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599965"
---
# <a name="security-validation"></a>Validação de segurança
Este exemplo demonstra como usar um comportamento personalizado para validar serviços em um computador para garantir que eles atendam a critérios específicos. Neste exemplo, os serviços são validados pelo comportamento personalizado examinando cada ponto de extremidade no serviço e verificando se eles contêm elementos de associação seguros. Este exemplo é baseado na [introdução](getting-started-sample.md).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
## <a name="endpoint-validation-custom-behavior"></a>Comportamento personalizado de validação do ponto de extremidade  
 Ao adicionar o código de usuário ao `Validate` método contido na <xref:System.ServiceModel.Description.IServiceBehavior> interface, o comportamento personalizado pode ser dado a um serviço ou ponto de extremidade para executar ações definidas pelo usuário. O código a seguir é usado para executar um loop por meio de cada ponto de extremidade contido em um serviço, que pesquisa suas coleções de associação para associações seguras.  
  
```csharp
public void Validate(ServiceDescription serviceDescription,
                                       ServiceHostBase serviceHostBase)  
{  
    // Loop through each endpoint individually, gathering their
    // binding elements.  
    foreach (ServiceEndpoint endpoint in serviceDescription.Endpoints)  
    {  
        secureElementFound = false;  
  
        // Retrieve the endpoint's binding element collection.  
        BindingElementCollection bindingElements =
            endpoint.Binding.CreateBindingElements();  
  
        // Look to see if the binding elements collection contains any
        // secure binding elements. Transport, Asymmetric, and Symmetric
        // binding elements are all derived from SecurityBindingElement.  
        if ((bindingElements.Find<SecurityBindingElement>() != null) || (bindingElements.Find<HttpsTransportBindingElement>() != null) || (bindingElements.Find<WindowsStreamSecurityBindingElement>() != null) || (bindingElements.Find<SslStreamSecurityBindingElement>() != null))  
        {  
            secureElementFound = true;  
        }  
  
    // Send a message to the system event viewer when an endpoint is deemed insecure.  
    if (!secureElementFound)  
        throw new Exception(System.DateTime.Now.ToString() + ": The endpoint \"" + endpoint.Name + "\" has no secure bindings.");  
    }  
}  
```  
  
 Adicionar o código a seguir ao arquivo Web. config adiciona a `serviceValidate` extensão de comportamento para o serviço reconhecer.  
  
```xml  
<system.serviceModel>  
    <extensions>  
        <behaviorExtensions>  
            <add name="endpointValidate" type="Microsoft.ServiceModel.Samples.EndpointValidateElement, endpointValidate, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null" />  
        </behaviorExtensions>  
    </extensions>
    ...
</system.serviceModel>
```  
  
 Depois que a extensão de comportamento é adicionada ao serviço, agora é possível adicionar o `endpointValidate` comportamento à lista de comportamentos no arquivo Web. config e, portanto, ao serviço.  
  
```xml  
<behaviors>  
    <serviceBehaviors>  
        <behavior name="CalcServiceSEB1">  
            <serviceMetadata httpGetEnabled="true" />  
            <endpointValidate />  
        </behavior>  
    </serviceBehaviors>  
</behaviors>  
```  
  
 Os comportamentos e suas extensões que são adicionados ao arquivo Web. config aplicam comportamentos a serviços individuais, enquanto quando adicionados ao arquivo Machine. config aplicam o comportamento a todos os serviços ativos no computador.  
  
> [!NOTE]
> Ao adicionar o comportamento a todos os serviços, é recomendável fazer backup do arquivo Machine. config antes de fazer qualquer alteração.  
  
 Agora, execute o cliente fornecido no diretório client\bin deste exemplo. Uma exceção é lançada com a seguinte mensagem: "o serviço solicitado, ' http://localhost/servicemodelsamples/service.svc ' não pôde ser ativado." Isso é esperado porque um ponto de extremidade é considerado inseguro pelo comportamento de validação do ponto de extremidade e impede que o serviço seja iniciado. O comportamento também gera uma exceção interna que descreve qual ponto de extremidade é inseguro e grava uma mensagem no sistema Visualizador de Eventos na origem "System. ServiceModel 4.0.0.0" e na categoria "Webhost". Também é possível ativar o rastreamento no serviço neste exemplo. Isso permite que o usuário exiba as exceções geradas pelo comportamento de validação de ponto de extremidade abrindo os rastreamentos de serviço resultantes usando a ferramenta Visualizador de rastreamento de serviço.  
  
### <a name="view-failed-endpoint-validation-exception-messages-in-the-event-viewer"></a>Exibir mensagens de exceção de validação de ponto de extremidade com falha no Visualizador de Eventos  
  
1. Clique no menu **Iniciar** e selecione **executar...**.  
  
2. Digite `eventvwr` e clique em **OK**.  
  
3. Na janela Visualizador de Eventos, clique em **aplicativo**.  
  
4. Clique duas vezes no evento "System. ServiceModel 4.0.0.0" recentemente adicionado na categoria "Webhost" na janela do **aplicativo** para exibir mensagens de ponto de extremidade inseguras.  
  
## <a name="set-up-build-and-run-the-sample"></a>Configurar, compilar e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\ServiceValidation`  
  
## <a name="see-also"></a>Consulte também

- [AppFabric que monitora Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
