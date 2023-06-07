---
title: Expedição por elemento Body
ms.date: 03/30/2017
ms.assetid: f64a3c04-62b4-47b2-91d9-747a3af1659f
ms.openlocfilehash: 19913cdaa47d766f62a313e216a653ac69633a99
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594693"
---
# <a name="dispatch-by-body-element"></a>Expedição por elemento Body
Este exemplo demonstra como implementar um algoritmo alternativo para atribuir mensagens de entrada a operações.  
  
 Por padrão, o distribuidor do modelo de serviço seleciona o método de tratamento apropriado para uma mensagem de entrada com base no cabeçalho "ação" do WS-Addressing da mensagem ou nas informações equivalentes na solicitação HTTP SOAP.  
  
 Algumas pilhas de serviços Web SOAP 1,1 que não seguem as diretrizes WS-I Basic Profile 1,1 não expedem mensagens com base no URI de ação, mas com base no nome qualificado XML do primeiro elemento dentro do corpo SOAP. Da mesma forma, o lado do cliente dessas pilhas pode enviar mensagens com um cabeçalho SOAPAction HTTP vazio ou arbitrário, que era permitido pela especificação SOAP 1,1.  
  
 Para alterar a maneira como as mensagens são expedidas para métodos, o exemplo implementa a <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector> interface de extensibilidade no `DispatchByBodyElementOperationSelector` . Essa classe seleciona operações baseadas no primeiro elemento do corpo da mensagem.  
  
 O construtor de classe espera que um dicionário seja preenchido com pares de `XmlQualifiedName` cadeias de caracteres e, no qual os nomes qualificados indicam o nome do primeiro filho do corpo SOAP e as cadeias de caracteres indicam o nome da operação correspondente. O `defaultOperationName` é o nome da operação que recebe todas as mensagens que não podem ser correspondidas em relação a este dicionário:  
  
```csharp
class DispatchByBodyElementOperationSelector : IDispatchOperationSelector  
{  
    Dictionary<XmlQualifiedName, string> dispatchDictionary;  
    string defaultOperationName;  
  
    public DispatchByBodyElementOperationSelector(Dictionary<XmlQualifiedName,string> dispatchDictionary, string defaultOperationName)  
    {  
        this.dispatchDictionary = dispatchDictionary;  
        this.defaultOperationName = defaultOperationName;  
    }  
}
```  
  
 <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector>as implementações são muito simples de criar, pois há apenas um método na interface: <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector.SelectOperation%2A> . O trabalho desse método é inspecionar uma mensagem de entrada e retornar uma cadeia de caracteres que seja igual ao nome de um método no contrato de serviço para o ponto de extremidade atual.  
  
 Neste exemplo, o seletor de operação adquire um <xref:System.Xml.XmlDictionaryReader> para o corpo da mensagem de entrada usando <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A> . Esse método já posiciona o leitor no primeiro filho do corpo da mensagem para que seja suficiente obter o nome do elemento atual e o URI do namespace e combiná-los em um `XmlQualifiedName` que é usado para pesquisar a operação correspondente do dicionário mantido pelo seletor de operação.  
  
```csharp
public string SelectOperation(ref System.ServiceModel.Channels.Message message)  
{  
    XmlDictionaryReader bodyReader = message.GetReaderAtBodyContents();  
    XmlQualifiedName lookupQName = new  
       XmlQualifiedName(bodyReader.LocalName, bodyReader.NamespaceURI);  
    message = CreateMessageCopy(message,bodyReader);  
    if (dispatchDictionary.ContainsKey(lookupQName))  
    {  
         return dispatchDictionary[lookupQName];  
    }  
    else  
    {  
        return defaultOperationName;  
    }  
}  
```  
  
 Acessar o corpo da mensagem com <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A> ou qualquer um dos outros métodos que fornecem acesso ao conteúdo do corpo da mensagem faz com que a mensagem seja marcada como "leitura", o que significa que a mensagem é inválida para qualquer processamento adicional. Portanto, o seletor de operação cria uma cópia da mensagem de entrada com o método mostrado no código a seguir. Como a posição do leitor não foi alterada durante a inspeção, ela pode ser referenciada pela mensagem recém-criada para a qual as propriedades de mensagem e os cabeçalhos de mensagem também são copiados, o que resulta em um clone exato da mensagem original:  
  
```csharp
private Message CreateMessageCopy(Message message,
                                     XmlDictionaryReader body)  
{  
    Message copy = Message.CreateMessage(message.Version,message.Headers.Action,body);  
    copy.Headers.CopyHeaderFrom(message,0);  
    copy.Properties.CopyProperties(message.Properties);  
    return copy;  
}  
```  
  
## <a name="adding-an-operation-selector-to-a-service"></a>Adicionando um seletor de operação a um serviço  
 Os seletores de operação de expedição de serviço são extensões para o Dispatcher do Windows Communication Foundation (WCF). Para selecionar métodos no canal de retorno de chamada de contratos duplex, também há seletores de operação do cliente, que funcionam muito como os seletores de operação de expedição descritos aqui, mas que não são explicitamente abordados neste exemplo.  
  
 Assim como a maioria das extensões de modelo de serviço, os seletores de operação de expedição são adicionados ao dispatcher usando comportamentos. Um *comportamento* é um objeto de configuração, que adiciona uma ou mais extensões ao tempo de execução de expedição (ou ao tempo de execução do cliente) ou altera suas configurações.  
  
 Como os seletores de operação têm o escopo do contrato, o comportamento apropriado a ser implementado aqui é o <xref:System.ServiceModel.Description.IContractBehavior> . Como a interface é implementada em uma <xref:System.Attribute> classe derivada, conforme mostrado no código a seguir, o comportamento pode ser adicionado declarativamente a qualquer contrato de serviço. Sempre que um <xref:System.ServiceModel.ServiceHost> é aberto e o tempo de execução de expedição é criado, todos os comportamentos encontrados como atributos em contratos, operações e implementações de serviço ou como elemento na configuração do serviço são adicionados automaticamente e, posteriormente, solicitados a contribuir com as extensões ou modificar a configuração padrão.  
  
 Para resumir, o trecho de código a seguir mostra apenas a implementação do método <xref:System.ServiceModel.Description.IContractBehavior.ApplyDispatchBehavior%2A> , que afeta as alterações de configuração do Dispatcher neste exemplo. Os outros métodos não são mostrados porque retornam ao chamador sem fazer nenhum trabalho.  
  
```csharp
[AttributeUsage(AttributeTargets.Class|AttributeTargets.Interface)]  
class DispatchByBodyElementBehaviorAttribute : Attribute, IContractBehavior  
{  
    // public void AddBindingParameters(...)
    // public void ApplyClientBehavior(...)  
    // public void Validate(...)  
```  
  
 Primeiro, a <xref:System.ServiceModel.Description.IContractBehavior.ApplyDispatchBehavior%2A> implementação configura o dicionário de pesquisa para o seletor de operação Iterando sobre os <xref:System.ServiceModel.Description.OperationDescription> elementos nos pontos de extremidade de serviço <xref:System.ServiceModel.Description.ContractDescription> . Em seguida, cada descrição da operação é inspecionada para a presença do `DispatchBodyElementAttribute` comportamento, uma implementação do <xref:System.ServiceModel.Description.IOperationBehavior> que também é definida neste exemplo. Embora essa classe também seja um comportamento, ela é passiva e não contribui ativamente com nenhuma alteração de configuração no tempo de execução de expedição. Todos os seus métodos retornam ao chamador sem realizar nenhuma ação. O comportamento da operação existe apenas para que os metadados necessários para o novo mecanismo de expedição, ou seja, o nome qualificado do elemento body em cuja ocorrência de uma operação seja selecionada, possam ser associados às respectivas operações.  
  
 Se esse comportamento for encontrado, um par de valores criado a partir do nome qualificado XML ( `QName` Propriedade) e o nome da operação ( `Name` Propriedade) será adicionado ao dicionário.  
  
 Depois que o dicionário é populado, um novo `DispatchByBodyElementOperationSelector` é construído com essas informações e definido como o seletor de operação do tempo de execução de expedição:  
  
```csharp
public void ApplyDispatchBehavior(ContractDescription contractDescription, ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.DispatchRuntime dispatchRuntime)  
{  
    Dictionary<XmlQualifiedName,string> dispatchDictionary =
                     new Dictionary<XmlQualifiedName,string>();  
    foreach( OperationDescription operationDescription in
                              contractDescription.Operations )  
    {  
        DispatchBodyElementAttribute dispatchBodyElement =
   operationDescription.Behaviors.Find<DispatchBodyElementAttribute>();  
        if ( dispatchBodyElement != null )  
        {  
             dispatchDictionary.Add(dispatchBodyElement.QName,
                              operationDescription.Name);  
        }  
    }  
    dispatchRuntime.OperationSelector =
            new DispatchByBodyElementOperationSelector(  
               dispatchDictionary,
               dispatchRuntime.UnhandledDispatchOperation.Name);  
    }  
}  
```  
  
## <a name="implementing-the-service"></a>Implementando o serviço  
 O comportamento implementado neste exemplo afeta diretamente como as mensagens da transmissão são interpretadas e expedidas, que é uma função do contrato de serviço. Consequentemente, o comportamento deve ser declarado no nível de contrato de serviço em qualquer implementação de serviço que escolha usá-lo.  
  
 O serviço de projeto de exemplo aplica o `DispatchByBodyElementBehaviorAttribute` comportamento do contrato ao `IDispatchedByBody` contrato de serviço e rotula cada uma das duas operações `OperationForBodyA()` e `OperationForBodyB()` com um `DispatchBodyElementAttribute` comportamento de operação. Quando um host de serviço para um serviço que implementa esse contrato é aberto, esses metadados são coletados pelo Dispatcher Builder conforme descrito anteriormente.  
  
 Como o seletor de operação é expedido exclusivamente com base no elemento do corpo da mensagem e ignora a "ação", é necessário informar ao tempo de execução para não verificar o cabeçalho "ação" nas respostas retornadas atribuindo o curinga "*" à `ReplyAction` propriedade de <xref:System.ServiceModel.OperationContractAttribute> . Além disso, é necessário ter uma operação padrão que tenha a propriedade "Action" definida como o curinga " \* ". A operação padrão recebe todas as mensagens que não podem ser expedidas e não tem um `DispatchBodyElementAttribute` :  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples"),  
                            DispatchByBodyElementBehavior]  
public interface IDispatchedByBody  
{  
    [OperationContract(ReplyAction="*"),
     DispatchBodyElement("bodyA","http://tempuri.org")]  
    Message OperationForBodyA(Message msg);  
    [OperationContract(ReplyAction = "*"),
     DispatchBodyElement("bodyB", "http://tempuri.org")]  
    Message OperationForBodyB(Message msg);  
    [OperationContract(Action="*", ReplyAction="*")]  
    Message DefaultOperation(Message msg);  
}  
```  
  
 A implementação do serviço de exemplo é simples. Cada método encapsula a mensagem recebida em uma mensagem de resposta e a ecoa de volta para o cliente.  
  
## <a name="running-and-building-the-sample"></a>Executando e compilando o exemplo  
 Quando você executa o exemplo, o conteúdo do corpo das respostas da operação é exibido na janela do console do cliente semelhante à saída (formatada) a seguir.  
  
 O cliente envia três mensagens para o serviço cujo elemento de conteúdo do corpo é nomeado `bodyA` , `bodyB` e `bodyX` , respectivamente. Como pode ser adiado da descrição anterior e do contrato de serviço mostrado, a mensagem de entrada com o `bodyA` elemento é expedida para o `OperationForBodyA()` método. Como não há nenhum destino de expedição explícito para a mensagem com o `bodyX` elemento body, a mensagem é expedida para o `DefaultOperation()` . Cada uma das operações de serviço encapsula o corpo da mensagem recebida em um elemento específico do método e a retorna, o que é feito para correlacionar as mensagens de entrada e saída claramente para este exemplo:  
  
```xml  
<?xml version="1.0" encoding="IBM437"?>  
<replyBodyA xmlns="http://tempuri.org">  
   <q:bodyA xmlns:q="http://tempuri.org">test</q:bodyA>  
</replyBodyA>  
<?xml version="1.0" encoding="IBM437"?>  
<replyBodyB xmlns="http://tempuri.org">  
  <q:bodyB xmlns:q="http://tempuri.org">test</q:bodyB>  
</replyBodyB>  
<?xml version="1.0" encoding="IBM437"?>  
<replyDefault xmlns="http://tempuri.org">  
   <q:bodyX xmlns:q="http://tempuri.org">test</q:bodyX>  
</replyDefault>  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Interop\AdvancedDispatchByBody`  
