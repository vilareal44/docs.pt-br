---
title: Fluxo de transação WS
ms.date: 03/30/2017
helpviewer_keywords:
- Transactions
ms.assetid: f8eecbcf-990a-4dbb-b29b-c3f9e3b396bd
ms.openlocfilehash: 1fbde53289c147d8ea273b9c86e65cbb8e262b30
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596404"
---
# <a name="ws-transaction-flow"></a>Fluxo de transação WS
Este exemplo demonstra o uso de uma transação coordenada pelo cliente e as opções de cliente e servidor para o fluxo de transações usando a transação WS-Atomic ou o protocolo OleTransactions. Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora, mas as operações são atribuídas para demonstrar o uso do `TransactionFlowAttribute` com a enumeração **TransactionFlowOption** para determinar para qual grau o fluxo de transações está habilitado. Dentro do escopo da transação fluida, um log das operações solicitadas é gravado em um banco de dados e persiste até que a transação coordenada do cliente seja concluída – se a transação do cliente não for concluída, a transação do serviço Web garantirá que as atualizações apropriadas ao banco de dados não sejam confirmadas.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Depois de iniciar uma conexão com o serviço e uma transação, o cliente acessa várias operações de serviço. O contrato para o serviço é definido da seguinte maneira com cada uma das operações que demonstram uma configuração diferente para o `TransactionFlowOption` .  

```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract]  
    [TransactionFlow(TransactionFlowOption.Mandatory)]  
    double Add(double n1, double n2);  
    [OperationContract]  
    [TransactionFlow(TransactionFlowOption.Allowed)]  
    double Subtract(double n1, double n2);  
    [OperationContract]  
    [TransactionFlow(TransactionFlowOption.NotAllowed)]  
    double Multiply(double n1, double n2);  
    [OperationContract]  
    double Divide(double n1, double n2);
}  
```

 Isso define as operações na ordem em que elas devem ser processadas:  
  
- Uma `Add` solicitação de operação deve incluir uma transação fluida.  
  
- Uma `Subtract` solicitação de operação pode incluir uma transação fluida.  
  
- Uma `Multiply` solicitação de operação não deve incluir uma transação fluida por meio da configuração não permitido explicitamente.  
  
- Uma `Divide` solicitação de operação não deve incluir uma transação fluida por meio da omissão de um `TransactionFlow` atributo.  
  
 Para habilitar o fluxo de transações, as associações com a [\<transactionFlow>](../../configure-apps/file-schema/wcf/transactionflow.md) propriedade habilitado devem ser usadas além dos atributos de operação apropriados. Neste exemplo, a configuração do serviço expõe um ponto de extremidade TCP e um ponto de extremidade HTTP, além de um ponto de extremidade de troca de metadados. O ponto de extremidade TCP e o ponto de extremidade HTTP usam as seguintes associações, ambas com a [\<transactionFlow>](../../configure-apps/file-schema/wcf/transactionflow.md) propriedade habilitado.  
  
```xml  
<bindings>  
  <netTcpBinding>  
    <binding name="transactionalOleTransactionsTcpBinding"  
             transactionFlow="true"  
             transactionProtocol="OleTransactions"/>  
  </netTcpBinding>  
  <wsHttpBinding>  
    <binding name="transactionalWsatHttpBinding"  
             transactionFlow="true" />  
  </wsHttpBinding>  
</bindings>  
```  
  
> [!NOTE]
> O netTcpbinding fornecido pelo sistema permite a especificação do transactionProtocol, enquanto que o wsHttpBinding fornecido pelo sistema usa apenas o protocolo WSAtomicTransactionOctober2004 mais interoperável. O protocolo OleTransactions só está disponível para uso por clientes Windows Communication Foundation (WCF).  
  
 Para a classe que implementa a `ICalculator` interface, todos os métodos são atribuídos com <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> a propriedade definida como `true` . Essa configuração declara que todas as ações executadas dentro do método ocorrem dentro do escopo de uma transação. Nesse caso, as ações executadas incluem a gravação no banco de dados de log. Se a solicitação de operação incluir uma transação fluida, as ações ocorrerão dentro do escopo da transação de entrada ou um novo escopo de transação será gerado automaticamente.  
  
> [!NOTE]
> A <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> propriedade define o comportamento local para as implementações do método de serviço e não define a capacidade do cliente ou o requisito para o fluxo de uma transação.  

```csharp
// Service class that implements the service contract.  
[ServiceBehavior(TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable)]  
public class CalculatorService : ICalculator  
{  
    [OperationBehavior(TransactionScopeRequired = true)]  
    public double Add(double n1, double n2)  
    {  
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Adding {0} to {1}", n1, n2));  
        return n1 + n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true)]  
    public double Subtract(double n1, double n2)  
    {  
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Subtracting {0} from {1}", n2, n1));  
        return n1 - n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true)]  
    public double Multiply(double n1, double n2)  
    {  
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Multiplying {0} by {1}", n1, n2));  
        return n1 * n2;  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true)]  
    public double Divide(double n1, double n2)  
    {  
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Dividing {0} by {1}", n1, n2));  
        return n1 / n2;  
    }  
  
    // Logging method omitted for brevity  
}  
```

 No cliente, as configurações do serviço `TransactionFlowOption` nas operações são refletidas na definição gerada do cliente da `ICalculator` interface. Além disso, as configurações de Propriedade do serviço `transactionFlow` são refletidas na configuração do aplicativo do cliente. O cliente pode selecionar o transporte e o protocolo selecionando o apropriado `endpointConfigurationName` .  

```csharp
// Create a client using either wsat or oletx endpoint configurations  
CalculatorClient client = new CalculatorClient("WSAtomicTransaction_endpoint");  
// CalculatorClient client = new CalculatorClient("OleTransactions_endpoint");  
```

> [!NOTE]
> O comportamento observado desse exemplo é o mesmo, independentemente de qual protocolo ou transporte é escolhido.  
  
 Depois de iniciar a conexão com o serviço, o cliente cria um novo `TransactionScope` em volta das chamadas para as operações de serviço.  

```csharp
// Start a transaction scope  
using (TransactionScope tx =  
            new TransactionScope(TransactionScopeOption.RequiresNew))  
{  
    Console.WriteLine("Starting transaction");  
  
    // Call the Add service operation  
    //  - generatedClient will flow the required active transaction  
    double value1 = 100.00D;  
    double value2 = 15.99D;  
    double result = client.Add(value1, value2);  
    Console.WriteLine("  Add({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Subtract service operation  
    //  - generatedClient will flow the allowed active transaction  
    value1 = 145.00D;  
    value2 = 76.54D;  
    result = client.Subtract(value1, value2);  
    Console.WriteLine("  Subtract({0},{1}) = {2}", value1, value2, result);  
  
    // Start a transaction scope that suppresses the current transaction  
    using (TransactionScope txSuppress =  
                new TransactionScope(TransactionScopeOption.Suppress))  
    {  
        // Call the Subtract service operation  
        //  - the active transaction is suppressed from the generatedClient  
        //    and no transaction will flow  
        value1 = 21.05D;  
        value2 = 42.16D;  
        result = client.Subtract(value1, value2);  
        Console.WriteLine("  Subtract({0},{1}) = {2}", value1, value2, result);  
  
        // Complete the suppressed scope  
        txSuppress.Complete();  
    }  
  
    // Call the Multiply service operation  
    // - generatedClient will not flow the active transaction  
    value1 = 9.00D;  
    value2 = 81.25D;  
    result = client.Multiply(value1, value2);  
    Console.WriteLine("  Multiply({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Divide service operation.  
    // - generatedClient will not flow the active transaction  
    value1 = 22.00D;  
    value2 = 7.00D;  
    result = client.Divide(value1, value2);  
    Console.WriteLine("  Divide({0},{1}) = {2}", value1, value2, result);  
  
    // Complete the transaction scope  
    Console.WriteLine("  Completing transaction");  
    tx.Complete();  
}  
  
Console.WriteLine("Transaction committed");  
```

 As chamadas para as operações são as seguintes:  
  
- A `Add` solicitação flui a transação necessária para o serviço e as ações do serviço ocorrem dentro do escopo da transação do cliente.  
  
- A primeira `Subtract` solicitação também flui a transação permitida para o serviço e, novamente, as ações do serviço ocorrem dentro do escopo da transação do cliente.  
  
- A segunda `Subtract` solicitação é executada dentro de um novo escopo de transação declarado com a `TransactionScopeOption.Suppress` opção. Isso suprime a transação externa inicial do cliente e a solicitação não flui uma transação para o serviço. Essa abordagem permite que um cliente recuse explicitamente e proteja-se contra o fluxo de uma transação para um serviço quando isso não é necessário. As ações do serviço ocorrem no escopo de uma transação nova e não conectada.  
  
- A `Multiply` solicitação não flui uma transação para o serviço porque a definição gerada pelo cliente da `ICalculator` interface inclui um <xref:System.ServiceModel.TransactionFlowAttribute> definido como <xref:System.ServiceModel.TransactionFlowOption> `NotAllowed` .  
  
- A `Divide` solicitação não flui uma transação para o serviço porque, novamente, a definição de interface gerada pelo `ICalculator` cliente não inclui um `TransactionFlowAttribute` . As ações do serviço ocorrem novamente dentro do escopo de outra transação nova e não conectada.  
  
 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
Starting transaction  
  Add(100,15.99) = 115.99  
  Subtract(145,76.54) = 68.46  
  Subtract(21.05,42.16) = -21.11  
  Multiply(9,81.25) = 731.25  
  Divide(22,7) = 3.14285714285714  
  Completing transaction  
Transaction committed  
Press <ENTER> to terminate client.  
```  
  
 O registro em log das solicitações de operação de serviço é exibido na janela do console do serviço. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
Press <ENTER> to terminate the service.  
  Writing row to database: Adding 100 to 15.99  
  Writing row to database: Subtracting 76.54 from 145  
  Writing row to database: Subtracting 42.16 from 21.05  
  Writing row to database: Multiplying 9 by 81.25  
  Writing row to database: Dividing 22 by 7  
```  
  
 Após uma execução bem-sucedida, o escopo da transação do cliente é concluído e todas as ações executadas nesse escopo são confirmadas. Especificamente, os 5 registros anotados são persistidos no banco de dados do serviço. As duas primeiras delas ocorreram dentro do escopo da transação do cliente.  
  
 Se ocorrer uma exceção em qualquer lugar dentro do cliente `TransactionScope` , a transação não poderá ser concluída. Isso faz com que os registros registrados nesse escopo não sejam confirmados no banco de dados. Esse efeito pode ser observado repetindo-se a execução de exemplo depois de comentar a chamada para concluir a externa `TransactionScope` . Nessa execução, apenas as três últimas ações (do segundo `Subtract` , as `Multiply` `Divide` solicitações) são registradas porque a transação do cliente não fluiu para elas.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Para criar a versão C# ou Visual Basic do .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md)  
  
2. Verifique se você instalou o SQL Server Express Edition ou o SQL Server e se a cadeia de conexão foi definida corretamente no arquivo de configuração do aplicativo do serviço. Para executar o exemplo sem usar um banco de dados, defina o `usingSql` valor no arquivo de configuração de aplicativo do serviço como`false`  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
    > [!NOTE]
    > Para configuração entre computadores, habilite o Coordenador de Transações Distribuídas usando as instruções abaixo e use a ferramenta WsatConfig. exe da SDK do Windows para habilitar o suporte à rede de transações do WCF. Para obter informações sobre como configurar o WsatConfig. exe, consulte [Configuring WS-Atomic Transaction support](../feature-details/configuring-ws-atomic-transaction-support.md).  
  
 Se você executar o exemplo no mesmo computador ou em computadores diferentes, deverá configurar o Microsoft Coordenador de Transações Distribuídas (MSDTC) para habilitar o fluxo de transações de rede e usar a ferramenta WsatConfig. exe para habilitar o suporte à rede de transações do WCF.  
  
### <a name="to-configure-the-microsoft-distributed-transaction-coordinator-msdtc-to-support-running-the-sample"></a>Para configurar o Microsoft Coordenador de Transações Distribuídas (MSDTC) para dar suporte à execução do exemplo  
  
1. Em um computador de serviço executando o Windows Server 2003 ou o Windows XP, configure o MSDTC para permitir transações de rede de entrada seguindo estas instruções.  
  
    1. No menu **Iniciar** , navegue até **painel de controle**, **Ferramentas administrativas**e serviços de **componentes**.  
  
    2. Expanda **serviços de componentes**. Abra a pasta **computadores** .  
  
    3. Clique com o botão direito do mouse em **meu computador** e selecione **Propriedades**.  
  
    4. Na guia **MSDTC** , clique em **configuração de segurança**.  
  
    5. Verifique o **acesso ao DTC de rede** e permita a **entrada**.  
  
    6. Clique em **OK**e em **Sim** para reiniciar o serviço MSDTC.  
  
    7. Clique em **OK** para fechar a caixa de diálogo.  
  
2. Em um computador de serviço executando o Windows Server 2008 ou o Windows Vista, configure o MSDTC para permitir transações de rede de entrada seguindo estas instruções.  
  
    1. No menu **Iniciar** , navegue até **painel de controle**, **Ferramentas administrativas**e serviços de **componentes**.  
  
    2. Expanda **serviços de componentes**. Abra a pasta **computadores** . Selecione **Coordenador de transações distribuídas**.  
  
    3. Clique com o botão direito do mouse em **Coordenador DTC** e selecione **Propriedades**.  
  
    4. Na guia **segurança** , marque **acesso DTC de rede** e **permitir entrada**.  
  
    5. Clique em **OK**e em **Sim** para reiniciar o serviço MSDTC.  
  
    6. Clique em **OK** para fechar a caixa de diálogo.  
  
3. No computador cliente, configure o MSDTC para permitir transações de rede de saída:  
  
    1. No menu **Iniciar** , navegue até `Control Panel` , **Ferramentas administrativas**e **serviços de componentes**.  
  
    2. Clique com o botão direito do mouse em **meu computador** e selecione **Propriedades**.  
  
    3. Na guia **MSDTC** , clique em **configuração de segurança**.  
  
    4. Verifique o **acesso ao DTC de rede** e permita a **saída**.  
  
    5. Clique em **OK**e em **Sim** para reiniciar o serviço MSDTC.  
  
    6. Clique em **OK** para fechar a caixa de diálogo.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\TransactionFlow`
