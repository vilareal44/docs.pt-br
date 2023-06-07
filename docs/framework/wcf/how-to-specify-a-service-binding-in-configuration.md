---
title: Como especificar uma associação de serviço em configuração
description: Saiba como configurar um ponto de extremidade para um serviço WCF em um arquivo de configuração. Um contrato é definido para um serviço e implementado em uma classe.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 885037f7-1c2b-4d7a-90d9-06b89be172f2
ms.openlocfilehash: 3b9dd12f2a28ae2d420e82013459613cee8140f1
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051942"
---
# <a name="how-to-specify-a-service-binding-in-configuration"></a>Como especificar uma associação de serviço em configuração
Neste exemplo, um `ICalculator` contrato é definido para um serviço de calculadora básica, o serviço é implementado na `CalculatorService` classe e, em seguida, seu ponto de extremidade é configurado no arquivo de Web.config, onde é especificado que o serviço usa o <xref:System.ServiceModel.BasicHttpBinding> . Para obter uma descrição de como configurar esse serviço usando o código em vez de uma configuração, consulte [como especificar uma associação de serviço no código](how-to-specify-a-service-binding-in-code.md).  
  
 Geralmente, é a prática recomendada especificar a associação e as informações de endereço de forma declarativa na configuração, em vez de imperativa no código. A definição de pontos de extremidade no código geralmente não é prática porque as associações e os endereços para um serviço implantado são normalmente diferentes daqueles usados enquanto o serviço está sendo desenvolvido. Em geral, manter as informações de vinculação e endereçamento do código permite que elas sejam alteradas sem a necessidade de recompilar ou reimplantar o aplicativo.  
  
 Todas as etapas de configuração a seguir podem ser executadas usando a [ferramenta do editor de configuração (SvcConfigEditor.exe)](configuration-editor-tool-svcconfigeditor-exe.md).  
  
 Para a cópia de origem deste exemplo, consulte [BasicBinding](./samples/basicbinding.md).  
  
## <a name="to-specify-the-basichttpbinding-to-use-to-configure-the-service"></a>Para especificar o BasicHttpBinding a ser usado para configurar o serviço  
  
1. Defina um contrato de serviço para o tipo de serviço.  
  
     [!code-csharp[C_HowTo_ConfigureServiceBinding#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureservicebinding/cs/source.cs#1)]
     [!code-vb[C_HowTo_ConfigureServiceBinding#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_configureservicebinding/vb/source.vb#1)]  
  
2. Implemente o contrato de serviço em uma classe de serviço.  
  
     [!code-csharp[C_HowTo_ConfigureServiceBinding#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_configureservicebinding/cs/source.cs#2)]
     [!code-vb[C_HowTo_ConfigureServiceBinding#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto_configureservicebinding/vb/source.vb#2)]  
  
    > [!NOTE]
    > As informações de endereço ou de associação não são especificadas dentro da implementação do serviço. Além disso, o código não precisa ser escrito para buscar essas informações do arquivo de configuração.  
  
3. Crie um arquivo de Web.config para configurar um ponto de extremidade para o `CalculatorService` que usa o <xref:System.ServiceModel.WSHttpBinding> .  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.serviceModel>  
        <services>  
          <service name=" CalculatorService" >  

            <!-- Leave the address blank to be populated by default -->
            <!-- from the hosting environment,in this case IIS, so -->
            <!-- the address will just be that of the IIS Virtual -->
            <!-- Directory. -->

            <!-- Specify the binding configuration name for that -->
            <!-- binding type. This is optional but useful if you -->
            <!-- want to modify the properties of the binding. -->
            <!-- The bindingConfiguration name Binding1 is defined -->
            <!-- below in the bindings element. -->
            <endpoint
                address=""
                binding="wsHttpBinding"  
                bindingConfiguration="Binding1"  
                contract="ICalculator" />  
          </service>  
        </services>  
        <bindings>  
          <wsHttpBinding>  
            <binding name="Binding1">  
              <!-- Binding property values can be modified here. -->  
              <!-- See the next procedure. -->  
            </binding>  
          </wsHttpBinding>  
       </bindings>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
4. Crie um arquivo Service. svc que contenha a linha a seguir e coloque-o em seu diretório virtual Serviços de Informações da Internet (IIS).  
  
    ```aspx-csharp
    <%@ServiceHost language=c# Service="CalculatorService" %>
    ```  
  
## <a name="to-modify-the-default-values-of-the-binding-properties"></a>Para modificar os valores padrão das propriedades de associação  
  
1. Para modificar um dos valores de propriedade padrão do <xref:System.ServiceModel.WSHttpBinding> , crie um novo nome de configuração de associação – `<binding name="Binding1">` dentro do [\<wsHttpBinding>](../configure-apps/file-schema/wcf/wshttpbinding.md) elemento e defina os novos valores para os atributos da Associação nesse elemento de associação. Por exemplo, para alterar os valores de tempo limite de abertura e fechamento padrão de 1 minuto para 2 minutos, adicione o seguinte ao arquivo de configuração.  
  
    ```xml  
    <wsHttpBinding>  
      <binding name="Binding1"  
               closeTimeout="00:02:00"  
               openTimeout="00:02:00">  
      </binding>  
    </wsHttpBinding>  
    ```  
  
## <a name="see-also"></a>Consulte também

- [Usando associações para configurar serviços e clientes](using-bindings-to-configure-services-and-clients.md)
- [Especificando um endereço do ponto de extremidade](specifying-an-endpoint-address.md)
