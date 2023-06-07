---
title: Como configurar configurações de serviço de COM+
ms.date: 03/30/2017
helpviewer_keywords:
- COM+ [WCF], configuring service settings
ms.assetid: f42a55a8-3af8-4394-9fdd-bf12a93780eb
ms.openlocfilehash: 3fb4b31038845d223248e72d32b3e7413f2aef63
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597170"
---
# <a name="how-to-configure-com-service-settings"></a>Como configurar configurações de serviço de COM+
Quando uma interface de aplicativo é adicionada ou removida usando a ferramenta de configuração de serviço COM+, a configuração do serviço Web é atualizada no arquivo de configuração do aplicativo. No modo hospedado COM+, o arquivo Application. config é colocado no diretório raiz do aplicativo (%PROGRAMFILES%\ComPlus Applications \\ {AppID} é o padrão). Em qualquer um dos modos hospedados na Web, o arquivo Web. config é colocado no diretório vroot especificado.  
  
> [!NOTE]
> A assinatura de mensagens deve ser usada para proteger contra violação de mensagens entre um cliente e um servidor. Além disso, a criptografia da camada de transporte ou de mensagem deve ser usada para proteger contra a divulgação de informações de mensagens entre um cliente e um servidor. Assim como acontece com os serviços do Windows Communication Foundation (WCF), você deve usar a limitação para limitar o número de chamadas simultâneas, conexões, instâncias e operações pendentes. Isso ajuda a evitar o excesso de consumo de recursos. O comportamento de limitação é especificado por meio das configurações do arquivo de configuração de serviço.  
  
## <a name="example"></a>Exemplo  
 Considere um componente que implementa a seguinte interface:  
  
```csharp
[Guid("C551FBA9-E3AA-4272-8C2A-84BD8D290AC7")]  
public interface IFinances  
{  
    string Debit(string accountNo, double amount);  
    string Credit(string accountNo, double amount);  
}  
```  
  
 Se o componente for exposto como um serviço Web, o contrato de serviço correspondente que é exposto e os clientes precisarem estar em conformidade, será o seguinte:  
  
```csharp
[ServiceContract(Session = true,  
Namespace = "http://tempuri.org/C551FBA9-E3AA-4272-8C2A-84BD8D290AC7",  
Name = "IFinances")]  
public interface IFinancesContract : IDisposable  
{  
    [OperationContract]  
    string Debit(string accountNo, double amount);  
    [OperationContract]  
    string Credit(string accountNo, double amount);  
}  
```  
  
> [!NOTE]
> Os formulários de IID fazem parte do namespace inicial do contrato.  
  
 Os aplicativos cliente que usam esse serviço precisariam estar em conformidade com esse contrato, juntamente com o uso de uma associação compatível com aquela especificada na configuração do aplicativo.  
  
 O exemplo de código a seguir mostra um arquivo de configuração padrão. Sendo um serviço Web Windows Communication Foundation (WCF), isso está em conformidade com o esquema de configuração do modelo de serviço padrão e pode ser editado da mesma forma que outros arquivos de configuração de serviços WCF.  
  
 As modificações típicas incluem:  
  
- Alterar o endereço do ponto de extremidade do formulário ApplicationName/ComponentName/InterfaceName padrão para um formulário mais utilizável.  
  
- Modificar o namespace do serviço do `http://tempuri.org/InterfaceID` formulário padrão para um formulário mais relevante.  
  
- Alterando o ponto de extremidade para usar uma associação de transporte diferente.  
  
     No caso hospedado no COM+, o transporte de pipes nomeados é usado por padrão, mas um transporte fora da máquina, como o TCP, pode ser usado em vez disso.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
    <system.serviceModel>  
        <bindings>  
            <netNamedPipeBinding>  
                <binding name="comNonTransactionalBinding" />  
                <binding name="comTransactionalBinding" transactionFlow="true" />  
            </netNamedPipeBinding>  
        </bindings>  
        <comContracts>  
            <comContract contract="{C551FBA9-E3AA-4272-8C2A-84BD8D290AC7}"  
                name="IFinances" namespace="http://tempuri.org/C551FBA9-E3AA-4272-8C2A-84BD8D290AC7"  
                requiresSession="true">  
                <exposedMethods>  
                    <add exposedMethod="Debit" />  
                    <add exposedMethod="Credit" />  
                </exposedMethods>  
            </comContract>  
        </comContracts>  
        <services>  
            <service name="{DCDB24CC-0B19-4534-95CD-FBBFF4D67DD9},{C942B840-AD54-4A44-B5F7-928130980AB9}">  
                <endpoint address="IFinances" binding="netNamedPipeBinding" bindingConfiguration="comNonTransactionalBinding"  
                    contract="{C551FBA9-E3AA-4272-8C2A-84BD8D290AC7}" />  
                <host>  
                    <baseAddresses>  
                        <add baseAddress="net.pipe://localhost/ServiceModelDocSampleApp/ServiceModelDocSample.esFinance" />  
                    </baseAddresses>  
                </host>  
            </service>  
        </services>  
    </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Consulte também

- [Integração com aplicativos COM+](integrating-with-com-plus-applications.md)
