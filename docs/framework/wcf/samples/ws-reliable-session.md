---
title: Sessão confiável de WS
ms.date: 03/30/2017
helpviewer_keywords:
- Reliable session
ms.assetid: 86e914f2-060b-432b-bd17-333695317745
ms.openlocfilehash: 68123ba9a273bf2c1eaa7b3747930ebca386064b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84589689"
---
# <a name="ws-reliable-session"></a>Sessão confiável de WS
Este exemplo demonstra o uso de sessões confiáveis. As sessões confiáveis oferecem suporte para mensagens e sessões confiáveis. O sistema de mensagens confiável repete a comunicação em caso de falha e permite que as garantias de entrega sejam especificadas, como a chegada de mensagens em ordem. As sessões mantêm o estado para clientes entre chamadas. O exemplo implementa sessões para manter o estado do cliente e especifica garantias de entrega em ordem.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\wsReliableSession`  
  
 Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora. Os recursos de sessão confiáveis são habilitados e configurados nos arquivos de configuração do aplicativo para o cliente e o serviço.  
  
 Neste exemplo, o serviço é hospedado no Serviços de Informações da Internet (IIS) e o cliente é um aplicativo de console (. exe).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O exemplo usa o `wsHttpBinding` . A associação é especificada nos arquivos de configuração para o cliente e o serviço. O tipo de associação é especificado no atributo do elemento do ponto de extremidade `binding` , conforme mostrado na seguinte configuração de exemplo.  
  
```xml  
<endpoint address=""  
          binding="wsHttpBinding"  
          bindingConfiguration="Binding1"
          contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
 O ponto de extremidade contém um `bindingConfiguration` atributo que faz referência a uma configuração de associação chamada "Binding1". A configuração de associação permite sessões confiáveis definindo o `enabled` atributo do [\<reliableSession>](../../configure-apps/file-schema/wcf/reliablesession.md) para `true` . As garantias de entrega para sessões ordenadas são controladas definindo o atributo ordenado como `true` ou `false` . O padrão é `true`.  
  
```xml  
<bindings>  
    <wsHttpBinding>  
        <binding name="Binding1">  
            <reliableSession enabled="true" />  
        </binding>  
    </wsHttpBinding>  
</bindings>  
```  
  
 A classe de implementação de serviço implementa <xref:System.ServiceModel.InstanceContextMode.PerSession> a instanciação para manter uma instância de classe separada para cada cliente, conforme mostrado no código de exemplo a seguir.  

```csharp
[ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)] public class CalculatorService : ICalculator  
{  
    ...  
}  
```
  
 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Instale o ASP.NET 4,0 usando o comando a seguir.  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
