---
title: Segurança de mensagens do Windows
ms.date: 03/30/2017
helpviewer_keywords:
- WS Security
ms.assetid: d2221d1c-c9cb-48d1-b044-a3b4445c7f05
ms.openlocfilehash: 7b5b9ba0cc9a6d867b0478720b6151c7a561da16
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584708"
---
# <a name="message-security-windows"></a>Segurança de mensagens do Windows
Este exemplo demonstra como configurar uma <xref:System.ServiceModel.WSHttpBinding> Associação para usar a segurança em nível de mensagem com a autenticação do Windows. Este exemplo é baseado na [introdução](getting-started-sample.md). Neste exemplo, o serviço é hospedado no Serviços de Informações da Internet (IIS) e o cliente é um aplicativo de console (. exe).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 A segurança padrão para o [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) é a segurança de mensagem usando a autenticação do Windows. Os arquivos de configuração neste exemplo definem explicitamente o `mode` atributo de [\<security>](../../configure-apps/file-schema/wcf/security-of-wshttpbinding.md) para `Message` e o `clientCredentialType` atributo como `Windows` . Esses valores são os valores padrão para essa associação, mas foram explicitamente configurados, conforme mostrado na configuração de exemplo a seguir para demonstrar seu uso.  
  
```xml  
<bindings>  
    <wsHttpBinding>  
        <binding>  
            <security mode="Message">  
                <message clientCredentialType="Windows"/>  
            </security>  
        </binding>  
    </wsHttpBinding>  
</bindings>  
```  
  
 A configuração de ponto de extremidade do cliente consiste em um endereço absoluto para o ponto de extremidade de serviço, a associação e o contrato. A associação de cliente é configurada com o `securityMode` e o `authenticationMode` .  
  
```xml  
<system.serviceModel>  
  <client>  
    <endpoint address=  
            "http://localhost/servicemodelsamples/service.svc"
            binding="wsHttpBinding"
            bindingConfiguration="Binding1"
            contract="Microsoft.ServiceModel.Samples.ICalculator" />  
  </client>  
  
  <bindings>  
    <wsHttpBinding>  
      <!-- The default security for the WSHttpBinding is -->  
      <!-- Message security using Windows authentication. -->  
      <!-- This configuration explicitly defines the security mode -->  
      <!-- as Message and the clientCredentialType as Windows -->  
      <!-- for demonstration purposes. -->  
      <binding name="Binding1">  
        <security mode="Message">  
          <message clientCredentialType="Windows"/>  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
</system.serviceModel>  
```  
  
 O código-fonte do serviço foi modificado para demonstrar como o <xref:System.ServiceModel.OperationContext.ServiceSecurityContext%2A> pode ser usado para acessar a identidade do chamador.  

```csharp
public string GetCallerIdentity()  
{  
    // The Windows identity of the caller can be accessed on the ServiceSecurityContext.WindowsIdentity.  
    return OperationContext.Current.ServiceSecurityContext.WindowsIdentity.Name;  
}  
```

 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. O primeiro método chamado- `GetCallerIdentity` -retorna o nome da identidade do chamador de volta para o cliente. Pressione ENTER na janela do console para desligar o cliente.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
