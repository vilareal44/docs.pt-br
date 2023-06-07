---
title: Como utilizar a configuração para adicionar um ponto de extremidade AJAX ASP.NET
ms.date: 03/30/2017
ms.assetid: 7cd0099e-dc3a-47e4-a38c-6e10f997f6ea
ms.openlocfilehash: 97f8174161068f2c72b6bd2bc4e8a3044f5bccdd
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051656"
---
# <a name="how-to-use-configuration-to-add-an-aspnet-ajax-endpoint"></a>Como utilizar a configuração para adicionar um ponto de extremidade AJAX ASP.NET
Windows Communication Foundation (WCF) permite que você crie um serviço que disponibiliza um ponto de extremidade habilitado para AJAX ASP.NET que pode ser chamado do JavaScript em um site de cliente. Para criar esse ponto de extremidade, você pode usar um arquivo de configuração, assim como com todos os outros pontos de extremidade do Windows Communication Foundation (WCF) ou usar um método que não exija nenhum elemento de configuração. Este tópico demonstra a abordagem de configuração.  
  
 A parte do procedimento que permite que o ponto de extremidade de serviço se torne ASP.NET habilitado para AJAX consiste em configurar o ponto de extremidade para usar o <xref:System.ServiceModel.WebHttpBinding> e adicionar o [\<enableWebScript>](../../configure-apps/file-schema/wcf/enablewebscript.md) comportamento do ponto de extremidade. Depois de configurar o ponto de extremidade, as etapas para implementar e hospedar o serviço são semelhantes às usadas por qualquer serviço WCF. Para obter um exemplo funcional, consulte o [serviço AJAX usando http post](../samples/ajax-service-using-http-post.md).  
  
 Para obter mais informações sobre como configurar um ponto de extremidade do ASP.NET AJAX sem usar a configuração, consulte [como adicionar um ponto de extremidade do ASP.NET AJAX sem usar a configuração](how-to-add-an-aspnet-ajax-endpoint-without-using-configuration.md).  
  
## <a name="to-create-a-basic-wcf-service"></a>Para criar um serviço WCF básico  
  
1. Defina um contrato de serviço WCF básico com uma interface marcada com o <xref:System.ServiceModel.ServiceContractAttribute> atributo. Marque cada operação com o <xref:System.ServiceModel.OperationContractAttribute> . Certifique-se de definir a <xref:System.ServiceModel.ServiceContractAttribute.Namespace%2A> propriedade.  
  
    ```csharp
    [ServiceContract(Namespace = "MyService")]  
    public interface ICalculator  
    {  
        [OperationContract]  
         // This operation returns the sum of d1 and d2.  
        double Add(double n1, double n2);  
  
        //Other operations omitted…  
  
    }  
    ```  
  
2. Implemente o `ICalculator` contrato de serviço com um `CalculatorService` .  
  
    ```csharp
    public class CalculatorService : ICalculator  
    {  
        public double Add(double n1, double n2)  
        {  
            return n1 + n2;  
        }
        // Other operations omitted…
    }
    ```  
  
3. Defina um namespace para as `ICalculator` `CalculatorService` implementações e encapsulando-as em um bloco de namespace.  
  
    ```csharp
    namespace Microsoft.Ajax.Samples
    {  
        //Include the code for ICalculator and Caculator here.  
    }  
    ```  
  
## <a name="to-create-an-aspnet-ajax-endpoint-for-the-service"></a>Para criar um ponto de extremidade do ASP.NET AJAX para o serviço  
  
1. Crie uma configuração de comportamento e especifique o [\<enableWebScript>](../../configure-apps/file-schema/wcf/enablewebscript.md) comportamento para pontos de extremidade habilitados para AJAX ASP.net do serviço.  
  
    ```xml  
    <system.serviceModel>  
        <behaviors>  
            <endpointBehaviors>  
                <behavior name="AspNetAjaxBehavior">  
                    <enableWebScript />  
                </behavior>  
            </endpointBehaviors>  
        </behaviors>  
    </system.serviceModel>  
    ```  
  
2. Crie um ponto de extremidade para o serviço que usa o <xref:System.ServiceModel.WebHttpBinding> e o comportamento do AJAX ASP.net definido na etapa anterior.  
  
    ```xml  
    <system.serviceModel>  
        <services>  
            <service name="Microsoft.Ajax.Samples.CalculatorService">  
                <endpoint address=""  
                    behaviorConfiguration="AspNetAjaxBehavior"
                    binding="webHttpBinding"  
                    contract="Microsoft.Ajax.Samples.ICalculator" />  
            </service>  
        </services>  
    </system.serviceModel>
    ```  
  
## <a name="to-host-the-service-in-iis"></a>Para hospedar o serviço no IIS  
  
1. Para hospedar o serviço no IIS, crie um novo arquivo chamado serviço com uma extensão. svc no aplicativo. Edite esse arquivo adicionando as informações de diretiva [ \@ ServiceHost](../../configure-apps/file-schema/wcf-directive/servicehost.md) apropriadas para o serviço. Por exemplo, o conteúdo no arquivo de serviço para o `CalculatorService` exemplo contém as informações a seguir.  
  
    ```aspx-csharp
    <%@ServiceHost
    language=c#
    Debug="true"
    Service="Microsoft.Ajax.Samples.CalculatorService"  
    %>  
    ```  
  
2. Para obter mais informações sobre hospedagem no IIS, consulte [como hospedar um serviço WCF no IIS](how-to-host-a-wcf-service-in-iis.md).  
  
## <a name="to-call-the-service"></a>Para chamar o serviço  
  
1. O ponto de extremidade é configurado em um endereço vazio relativo ao arquivo. svc, portanto, o serviço agora está disponível e pode ser invocado enviando solicitações para Service. svc/ \<operation> -por exemplo, Service. svc/Add para a `Add` operação. Você pode usá-lo inserindo a URL do ponto de extremidade na coleção de scripts do controle Gerenciador de script do ASP.NET AJAX. Para obter um exemplo, consulte o [serviço AJAX usando http post](../samples/ajax-service-using-http-post.md).  
  
## <a name="see-also"></a>Consulte também

- [Criando serviços do WCF para o AJAX ASP.NET](creating-wcf-services-for-aspnet-ajax.md)
- [Como migrar serviços habilitados para AJAX ASP.NET para o WCF](how-to-migrate-ajax-enabled-aspnet-web-services-to-wcf.md)
