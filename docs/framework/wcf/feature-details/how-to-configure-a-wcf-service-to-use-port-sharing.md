---
title: Como configurar um serviço do Windows Communication Foundation para utilizar o compartilhamento de porta
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6400bc71-a858-4ac2-8d5a-caa72d3b5482
ms.openlocfilehash: 28f2858d68de99839d7fec66b0fe4528d7e42325
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84579521"
---
# <a name="how-to-configure-a-windows-communication-foundation-service-to-use-port-sharing"></a>Como configurar um serviço do Windows Communication Foundation para utilizar o compartilhamento de porta
A maneira mais fácil de usar net. TCP://compartilhamento de porta em seu aplicativo Windows Communication Foundation (WCF) é expor um serviço usando o <xref:System.ServiceModel.NetTcpBinding> .  
  
 Essa associação fornece uma <xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A> propriedade que controla se o compartilhamento de porta Net. TCP://está habilitado para o serviço que está sendo configurado com essa associação.  
  
 O procedimento a seguir mostra como usar a <xref:System.ServiceModel.NetTcpBinding> classe para abrir um ponto de extremidade no Uniform Resource Identifier (URI) net. TCP://localhost/MyService, primeiro no código e, em seguida, usando elementos de configuração.  
  
### <a name="to-enable-nettcp-port-sharing-on-a-nettcpbinding-in-code"></a>Para habilitar o compartilhamento net. TCP://porta em um NetTcpbinding no código  
  
1. Crie um serviço para implementar um contrato chamado `IMyService` e chamá-lo `MyService` ,.  
  
     [!code-csharp[c_ConfigurePortSharing#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_configureportsharing/cs/source.cs#1)]
     [!code-vb[c_ConfigurePortSharing#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_configureportsharing/vb/source.vb#1)]  
  
2. Crie uma instância da <xref:System.ServiceModel.NetTcpBinding> classe e defina a <xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A> propriedade como `true` .  
  
     [!code-csharp[c_ConfigurePortSharing#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_configureportsharing/cs/source.cs#2)]
     [!code-vb[c_ConfigurePortSharing#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_configureportsharing/vb/source.vb#2)]  
  
3. Crie um <xref:System.ServiceModel.ServiceHost> e adicione o ponto de extremidade de serviço a ele para `MyService` que use o compartilhamento de porta habilitado <xref:System.ServiceModel.NetTcpBinding> e que escuta no URI do endereço do ponto de extremidade "net. TCP://localhost/MyService".  
  
     [!code-csharp[c_ConfigurePortSharing#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_configureportsharing/cs/source.cs#3)]
     [!code-vb[c_ConfigurePortSharing#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_configureportsharing/vb/source.vb#3)]  
  
    > [!NOTE]
    > Este exemplo usa a porta TCP padrão de 808 porque o URI do endereço do ponto de extremidade não especifica um número de porta diferente. Como o compartilhamento de porta está explicitamente habilitado na associação de transporte, esse serviço pode compartilhar a porta 808 com outros serviços em outros processos. Se o compartilhamento de porta não fosse permitido e outro aplicativo já estava usando a porta 808, esse serviço geraria um <xref:System.ServiceModel.AddressAlreadyInUseException> quando fosse aberto.  
  
### <a name="to-enable-nettcp-port-sharing-on-a-nettcpbinding-in-configuration"></a>Para habilitar o compartilhamento de porta Net. TCP://em uma NetTcpbinding na configuração  
  
1. O exemplo a seguir mostra como habilitar o compartilhamento de porta e adicionar o ponto de extremidade de serviço usando elementos de configuração.  
  
```xml  
<system.serviceModel>  
  <bindings>  
    <netTcpBinding name="portSharingBinding"
                   portSharingEnabled="true" />  
  </bindings>  
  <services>  
    <service name="MyService">  
        <endpoint address="net.tcp://localhost/MyService"  
                  binding="netTcpBinding"  
                  contract="IMyService"  
                  bindingConfiguration="portSharingBinding" />  
    </service>  
  </services>  
</system.serviceModel>  
```  
  
## <a name="see-also"></a>Consulte também

- [Compartilhamento de porta Net.TCP](net-tcp-port-sharing.md)
- [Como habilitar o serviço de compartilhamento de porta Net.TCP](how-to-enable-the-net-tcp-port-sharing-service.md)
