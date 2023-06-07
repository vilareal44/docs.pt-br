---
title: Controle estendido através de relatórios e tratamento de erro
ms.date: 03/30/2017
ms.assetid: 45f996a7-fa00-45cb-9d6f-b368f5778aaa
ms.openlocfilehash: c7ca8d85220d65905bc4d9d220de366c331504a4
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600536"
---
# <a name="extending-control-over-error-handling-and-reporting"></a>Controle estendido através de relatórios e tratamento de erro
Este exemplo demonstra como estender o controle sobre o tratamento de erros e o relatório de erros em um serviço Windows Communication Foundation (WCF) usando a <xref:System.ServiceModel.Dispatcher.IErrorHandler> interface. O exemplo se baseia na [introdução](getting-started-sample.md) com algum código adicional adicionado ao serviço para lidar com erros. O cliente força várias condições de erro. O serviço intercepta os erros e os registra em um arquivo.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Os serviços podem interceptar erros, executar o processamento e afetar como os erros são relatados usando a <xref:System.ServiceModel.Dispatcher.IErrorHandler> interface. A interface tem dois métodos que podem ser implementados: <xref:System.ServiceModel.Dispatcher.IErrorHandler.ProvideFault%28System.Exception%2CSystem.ServiceModel.Channels.MessageVersion%2CSystem.ServiceModel.Channels.Message%40%29> e <xref:System.ServiceModel.Dispatcher.IErrorHandler.HandleError%2A> . O <xref:System.ServiceModel.Dispatcher.IErrorHandler.ProvideFault%28System.Exception%2CSystem.ServiceModel.Channels.MessageVersion%2CSystem.ServiceModel.Channels.Message%40%29> método permite adicionar, modificar ou suprimir uma mensagem de falha gerada em resposta a uma exceção. O <xref:System.ServiceModel.Dispatcher.IErrorHandler.HandleError%2A> método permite que o processamento de erro ocorra em caso de erro e controle se o tratamento de erros adicional pode ser executado.  
  
 Neste exemplo, o `CalculatorErrorHandler` tipo implementa a <xref:System.ServiceModel.Dispatcher.IErrorHandler> interface. No  
  
 <xref:System.ServiceModel.Dispatcher.IErrorHandler.HandleError%2A>, o `CalculatorErrorHandler` grava um log do erro em um arquivo de texto Error. txt em c:\Logs. Observe que o exemplo registra em log a falha e não a suprimi, permitindo que ela seja relatada de volta ao cliente.  
  
```csharp
public class CalculatorErrorHandler : IErrorHandler
{
    // Provide a fault. The Message fault parameter can be replaced, or set to
    // null to suppress reporting a fault.

    public void ProvideFault(Exception error, MessageVersion version, ref Message fault)
    {
    }

    // HandleError. Log an error, then allow the error to be handled as usual.
    // Return true if the error is considered as already handled

    public bool HandleError(Exception error)
    {
        using (TextWriter tw = File.AppendText(@"c:\logs\error.txt"))
        {
            if (error != null)
            {
                tw.WriteLine("Exception: " + error.GetType().Name + " - " + error.Message);
            }
            tw.Close();
        }
        return true;
    }
}  
```  
  
 O `ErrorBehaviorAttribute` existe como um mecanismo para registrar um manipulador de erro com um serviço. Esse atributo usa um único parâmetro de tipo. Esse tipo deve implementar a <xref:System.ServiceModel.Dispatcher.IErrorHandler> interface e deve ter um construtor público e vazio. Em seguida, o atributo cria uma instância desse tipo de manipulador de erros e o instala no serviço. Ele faz isso implementando a <xref:System.ServiceModel.Description.IServiceBehavior> interface e, em seguida, usando o <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A> método para adicionar instâncias do manipulador de erros ao serviço.  
  
```csharp
// This attribute can be used to install a custom error handler for a service.  
public class ErrorBehaviorAttribute : Attribute, IServiceBehavior  
{  
    Type errorHandlerType;  
  
    public ErrorBehaviorAttribute(Type errorHandlerType)  
    {  
        this.errorHandlerType = errorHandlerType;  
    }  
  
    void IServiceBehavior.Validate(ServiceDescription description, ServiceHostBase serviceHostBase)  
    {  
    }  
  
    void IServiceBehavior.AddBindingParameters(ServiceDescription description, ServiceHostBase serviceHostBase, System.Collections.ObjectModel.Collection<ServiceEndpoint> endpoints, BindingParameterCollection parameters)  
    {  
    }  
  
    void IServiceBehavior.ApplyDispatchBehavior(ServiceDescription description, ServiceHostBase serviceHostBase)  
    {  
        IErrorHandler errorHandler;  
  
        try  
        {  
            errorHandler = (IErrorHandler)Activator.CreateInstance(errorHandlerType);  
        }  
        catch (MissingMethodException e)  
        {  
            throw new ArgumentException("The errorHandlerType specified in the ErrorBehaviorAttribute constructor must have a public empty constructor.", e);  
        }  
        catch (InvalidCastException e)  
        {  
            throw new ArgumentException("The errorHandlerType specified in the ErrorBehaviorAttribute constructor must implement System.ServiceModel.Dispatcher.IErrorHandler.", e);  
        }  
  
        foreach (ChannelDispatcherBase channelDispatcherBase in serviceHostBase.ChannelDispatchers)  
        {  
            ChannelDispatcher channelDispatcher = channelDispatcherBase as ChannelDispatcher;  
            channelDispatcher.ErrorHandlers.Add(errorHandler);  
        }
    }  
}  
```  
  
 O exemplo implementa um serviço de calculadora. O cliente deliberadamente faz com que dois erros ocorram no serviço fornecendo parâmetros com valores ilegais. O `CalculatorErrorHandler` usa a <xref:System.ServiceModel.Dispatcher.IErrorHandler> interface para registrar os erros em um arquivo local e, em seguida, permite que eles sejam relatados de volta ao cliente. O cliente força uma divisão por zero e uma condição fora do intervalo de argumento.  
  
```csharp
try
{  
    Console.WriteLine("Forcing an error in Divide");  
    // Call the Divide service operation - trigger a divide by 0 error.  
    value1 = 22;  
    value2 = 0;  
    result = proxy.Divide(value1, value2);  
    Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
}  
catch (FaultException e)  
{  
    Console.WriteLine("FaultException: " + e.GetType().Name + " - " + e.Message);  
}  
catch (Exception e)  
{  
    Console.WriteLine("Exception: " + e.GetType().Name + " - " + e.Message);  
}  
```  
  
 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Você vê a divisão por zero e as condições de argumento fora do intervalo sendo relatadas como falhas. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
Add(15,3) = 18  
Subtract(145,76) = 69  
Multiply(9,81) = 729  
Forcing an error in Divide  
FaultException: FaultException - Invalid Argument: The second argument must not be zero.  
Forcing an error in Factorial  
FaultException: FaultException - Invalid Argument: The argument must be greater than zero.  
  
Press <ENTER> to terminate client.  
```  
  
 O arquivo c:\logs\errors.txt contém as informações registradas sobre os erros pelo serviço. Observe que, para o serviço gravar no diretório, você deve certificar-se de que o processo no qual o serviço está sendo executado (normalmente ASP.NET ou Network Service) tenha permissão para gravar no diretório.  
  
```txt
Fault: Reason = Invalid Argument: The second argument must not be zero.  
Fault: Reason = Invalid Argument: The argument must be greater than zero.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Verifique se você criou o diretório c:\Logs para o arquivo error. txt. Ou modifique o nome do arquivo usado em `CalculatorErrorHandler.HandleError` .  
  
4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\ErrorHandling`  
