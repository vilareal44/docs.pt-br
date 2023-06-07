---
title: Comportamento de transação de serviço
ms.date: 03/30/2017
helpviewer_keywords:
- Service Transaction Behavior Sample [Windows Communication Foundation]
ms.assetid: 1a9842a3-e84d-427c-b6ac-6999cbbc2612
ms.openlocfilehash: 0be5bf0dbe6416febb898fb5150c5a516c8b0969
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591521"
---
# <a name="service-transaction-behavior"></a>Comportamento de transação de serviço

Este exemplo demonstra o uso de uma transação coordenada pelo cliente e as configurações de ServiceBehaviorAttribute e OperationBehaviorAttribute para controlar o comportamento da transação de serviço. Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora, mas é estendido para manter um log de servidor das operações realizadas em uma tabela de banco de dados e um total de execução com estado para as operações de calculadora. As gravações persistentes na tabela de log do servidor dependem do resultado de uma transação coordenada do cliente – se a transação do cliente não for concluída, a transação do serviço Web garantirá que as atualizações do banco de dados não sejam confirmadas.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

O contrato para o serviço define que todas as operações exigem que uma transação flua com solicitações:

```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples",
                    SessionMode = SessionMode.Required)]
public interface ICalculator
{
    [OperationContract]
    [TransactionFlow(TransactionFlowOption.Mandatory)]
    double Add(double n);
    [OperationContract]
    [TransactionFlow(TransactionFlowOption.Mandatory)]
    double Subtract(double n);
    [OperationContract]
    [TransactionFlow(TransactionFlowOption.Mandatory)]
    double Multiply(double n);
    [OperationContract]
    [TransactionFlow(TransactionFlowOption.Mandatory)]
    double Divide(double n);
}
```

Para habilitar o fluxo de transações de entrada, o serviço é configurado com o wsHttpBinding fornecido pelo sistema com o atributo transactionFlow definido como `true` . Essa associação usa o protocolo interoperável WSAtomicTransactionOctober2004:

```xml
<bindings>
  <wsHttpBinding>
    <binding name="transactionalBinding" transactionFlow="true" />
  </wsHttpBinding>
</bindings>
```

Depois de iniciar uma conexão com o serviço e uma transação, o cliente acessa várias operações de serviço dentro do escopo dessa transação e, em seguida, conclui a transação e fecha a conexão:

```csharp
// Create a client
CalculatorClient client = new CalculatorClient();

// Create a transaction scope with the default isolation level of Serializable
using (TransactionScope tx = new TransactionScope())
{
    Console.WriteLine("Starting transaction");

    // Call the Add service operation.
    double value = 100.00D;
    Console.WriteLine("  Adding {0}, running total={1}",
                                        value, client.Add(value));

    // Call the Subtract service operation.
    value = 45.00D;
    Console.WriteLine("  Subtracting {0}, running total={1}",
                                        value, client.Subtract(value));

    // Call the Multiply service operation.
    value = 9.00D;
    Console.WriteLine("  Multiplying by {0}, running total={1}",
                                        value, client.Multiply(value));

    // Call the Divide service operation.
    value = 15.00D;
    Console.WriteLine("  Dividing by {0}, running total={1}",
                                        value, client.Divide(value));

    Console.WriteLine("  Completing transaction");
    tx.Complete();
}

Console.WriteLine("Transaction committed");

// Closing the client gracefully closes the connection and cleans up resources
client.Close();
```

No serviço, há três atributos que afetam o comportamento da transação de serviço e fazem isso das seguintes maneiras:

- No `ServiceBehaviorAttribute` :

  - A `TransactionTimeout` propriedade especifica o período de tempo no qual uma transação deve ser concluída. Neste exemplo, ele é definido como 30 segundos.

  - A `TransactionIsolationLevel` propriedade especifica o nível de isolamento ao qual o serviço dá suporte. Isso é necessário para corresponder ao nível de isolamento do cliente.

  - A `ReleaseServiceInstanceOnTransactionComplete` propriedade especifica se a instância de serviço é reciclada quando uma transação é concluída. Ao configurá-lo como `false` , o serviço mantém a mesma instância de serviço nas solicitações de operação. Isso é necessário para manter o total acumulado. Se definido como `true` , uma nova instância será gerada após cada ação concluída.

  - A `TransactionAutoCompleteOnSessionClose` propriedade especifica se as transações pendentes são concluídas quando a sessão é fechada. Ao defini-lo como `false` , as operações individuais são necessárias para definir a <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete?displayProperty=nameWithType> propriedade como `true` or para exigir explicitamente uma chamada para o <xref:System.ServiceModel.OperationContext.SetTransactionComplete?displayProperty=nameWithType> método para concluir as transações. Este exemplo demonstra as duas abordagens.

- No `ServiceContractAttribute` :

  - A `SessionMode` propriedade especifica se o serviço correlaciona as solicitações apropriadas em uma sessão lógica. Como esse serviço inclui operações em que a propriedade de TransactionAutoComplete OperationBehaviorAttribute está definida como `false` (multiplicar e dividir), `SessionMode.Required` deve ser especificada. Por exemplo, a operação de multiplicação não conclui sua transação e, em vez disso, depende de uma chamada posterior para ser concluída usando o `SetTransactionComplete` método; o serviço deve ser capaz de determinar que essas operações estão ocorrendo dentro da mesma sessão.

- No `OperationBehaviorAttribute` :

  - A `TransactionScopeRequired` propriedade especifica se as ações da operação devem ser executadas em um escopo de transação. Isso é definido como `true` para todas as operações neste exemplo e, como o cliente flui sua transação para todas as operações, as ações ocorrem dentro do escopo dessa transação do cliente.

  - A `TransactionAutoComplete` propriedade especifica se a transação na qual o método é executado será automaticamente concluída se nenhuma exceção não tratada ocorrer. Conforme descrito anteriormente, isso é definido como `true` para as operações adicionar e subtrair, mas `false` para as operações de multiplicar e dividir. As operações adicionar e subtrair concluem suas ações automaticamente, a divisão conclui suas ações por meio de uma chamada explícita para o `SetTransactionComplete` método, e a multiplicação não conclui suas ações, mas, em vez disso, se baseia e requer uma chamada posterior, como a divisão, para concluir as ações.

A implementação do serviço atribuído é a seguinte:

```csharp
[ServiceBehavior(
    TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable,
    TransactionTimeout = "00:00:30",
    ReleaseServiceInstanceOnTransactionComplete = false,
    TransactionAutoCompleteOnSessionClose = false)]
public class CalculatorService : ICalculator
{
    double runningTotal;

    public CalculatorService()
    {
        Console.WriteLine("Creating new service instance...");
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public double Add(double n)
    {
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Adding {0} to {1}", n, runningTotal));
        runningTotal = runningTotal + n;
        return runningTotal;
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public double Subtract(double n)
    {
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Subtracting {0} from {1}", n, runningTotal));
        runningTotal = runningTotal - n;
        return runningTotal;
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = false)]
    public double Multiply(double n)
    {
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Multiplying {0} by {1}", runningTotal, n));
        runningTotal = runningTotal * n;
        return runningTotal;
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = false)]
    public double Divide(double n)
    {
        RecordToLog(String.Format(CultureInfo.CurrentCulture, "Dividing {0} by {1}", runningTotal, n));
        runningTotal = runningTotal / n;
        OperationContext.Current.SetTransactionComplete();
        return runningTotal;
    }

    // Logging method omitted for brevity
}
```

Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

```console
Starting transaction
Performing calculations...
  Adding 100, running total=100
  Subtracting 45, running total=55
  Multiplying by 9, running total=495
  Dividing by 15, running total=33
  Completing transaction
Transaction committed
Press <ENTER> to terminate client.
```

O registro em log das solicitações de operação de serviço é exibido na janela do console do serviço. Pressione ENTER na janela do cliente para desligar o cliente.

```console
Press <ENTER> to terminate service.
Creating new service instance...
  Writing row 1 to database: Adding 100 to 0
  Writing row 2 to database: Subtracting 45 from 100
  Writing row 3 to database: Multiplying 55 by 9
  Writing row 4 to database: Dividing 495 by 15
```

Observe que, além de manter o total em execução dos cálculos, o serviço relata a criação de instâncias (uma instância para este exemplo) e registra as solicitações de operação em um banco de dados. Como todas as solicitações fluem a transação do cliente, qualquer falha ao concluir a transação resulta em todas as operações de banco de dados que estão sendo revertidas. Isso pode ser demonstrado de várias maneiras:

- Comente a chamada do cliente para `tx.Complete` () e execute novamente-isso faz com que o cliente saia do escopo da transação sem concluir sua transação.

- Comente a chamada para a operação de serviço de divisão – esses resultados impedem que a ação iniciada pela operação de multiplicação seja concluída e, portanto, a transação do cliente também não seja concluída.

- Lance uma exceção sem tratamento em qualquer lugar no escopo da transação do cliente-isso, de maneira semelhante, impede que o cliente conclua sua transação.

O resultado de qualquer um deles é que nenhuma das operações executadas nesse escopo é confirmada e a contagem de linhas mantidas no banco de dados não é incrementada.

> [!NOTE]
> Como parte do processo de compilação, o arquivo de banco de dados é copiado para a pasta bin. Você deve examinar essa cópia do arquivo de banco de dados para observar as linhas que são persistidas no log, em vez do arquivo que está incluído no projeto do Visual Studio.

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você instalou o SQL Server 2005 Express Edition ou o SQL Server 2005. No arquivo app. config do serviço, o banco de dados `connectionString` pode ser definido ou as interações de banco de dados podem ser desabilitadas definindo o valor appSettings `usingSql` como `false` .

2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

Se você executar o exemplo entre computadores, deverá configurar o Microsoft Coordenador de Transações Distribuídas (MSDTC) para habilitar o fluxo de transações de rede e usar a ferramenta WsatConfig. exe para habilitar o suporte à rede de transações de Windows Communication Foundation (WCF).

### <a name="to-configure-the-microsoft-distributed-transaction-coordinator-msdtc-to-support-running-the-sample-across-machines"></a>Para configurar o Microsoft Coordenador de Transações Distribuídas (MSDTC) para dar suporte à execução do exemplo entre computadores

1. No computador de serviço, configure o MSDTC para permitir transações de rede de entrada.

    1. No menu **Iniciar** , navegue até **painel de controle**, **Ferramentas administrativas**e serviços de **componentes**.

    2. Clique com o botão direito do mouse em **meu computador** e selecione **Propriedades**.

    3. Na guia **MSDTC** , clique em **configuração de segurança**.

    4. Verifique o **acesso ao DTC de rede** e permita a **entrada**.

    5. Clique em **Sim** para reiniciar o serviço MS DTC e clique em **OK**.

    6. Clique em **OK** para fechar a caixa de diálogo.

2. Na máquina do serviço e no computador cliente, configure o Firewall do Windows para incluir o Microsoft Coordenador de Transações Distribuídas (MSDTC) na lista de aplicativos isentos:

    1. Execute o aplicativo Firewall do Windows no painel de controle.

    2. Na guia **exceções** , clique em **Adicionar programa**.

    3. Navegue até a pasta C:\WINDOWS\System32.

    4. Selecione MSDTC. exe e clique em **abrir**.

    5. Clique em **OK** para fechar a caixa de diálogo **Adicionar programa** e clique em **OK** novamente para fechar o miniaplicativo Firewall do Windows.

3. No computador cliente, configure o MSDTC para permitir transações de rede de saída:

    1. No menu **Iniciar** , navegue até **painel de controle**, **Ferramentas administrativas**e serviços de **componentes**.

    2. Clique com o botão direito do mouse em **meu computador** e selecione **Propriedades**.

    3. Na guia **MSDTC** , clique em **configuração de segurança**.

    4. Verifique o **acesso ao DTC de rede** e permita a **saída**.

    5. Clique em **Sim** para reiniciar o serviço MS DTC e clique em **OK**.

    6. Clique em **OK** para fechar a caixa de diálogo.

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Behaviors\Transactions`
