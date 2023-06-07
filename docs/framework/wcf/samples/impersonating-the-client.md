---
title: Representando o cliente
ms.date: 03/30/2017
helpviewer_keywords:
- service behaviors, impersonation sample
- Impersonating the Client Sample [Windows Communication Foundation]
- impersonation, Windows Communication Foundation sample
ms.assetid: 8bd974e1-90db-4152-95a3-1d4b1a7734f8
ms.openlocfilehash: b5272d8b4dbac60e14fe87accbb08a2073ed65ab
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594628"
---
# <a name="impersonating-the-client"></a>Representando o cliente
O exemplo de representação demonstra como representar o aplicativo chamador no serviço para que o serviço possa acessar recursos do sistema em nome do chamador.  
  
 Este exemplo é baseado no exemplo de [hospedagem interna](self-host.md) . Os arquivos de configuração do serviço e do cliente são os mesmos do exemplo de [hospedagem interna](self-host.md) .  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O código de serviço foi modificado de modo que o `Add` método no serviço represente o chamador usando o <xref:System.ServiceModel.OperationBehaviorAttribute> , conforme mostrado no código de exemplo a seguir.  
  
```csharp
[OperationBehavior(Impersonation = ImpersonationOption.Required)]  
public double Add(double n1, double n2)  
{  
    double result = n1 + n2;  
    Console.WriteLine("Received Add({0},{1})", n1, n2);  
    Console.WriteLine("Return: {0}", result);  
    DisplayIdentityInformation();  
    return result;  
}  
```  
  
 Como resultado, o contexto de segurança do thread em execução é alternado para representar o chamador antes de inserir o `Add` método e revertido ao sair do método.  
  
 O `DisplayIdentityInformation` método mostrado no código de exemplo a seguir é uma função de utilitário que exibe a identidade do chamador.  
  
```csharp
static void DisplayIdentityInformation()  
{  
    Console.WriteLine("\t\tThread Identity            :{0}",  
         WindowsIdentity.GetCurrent().Name);  
    Console.WriteLine("\t\tThread Identity level  :{0}",
         WindowsIdentity.GetCurrent().ImpersonationLevel);  
    Console.WriteLine("\t\thToken                     :{0}",  
         WindowsIdentity.GetCurrent().Token.ToString());  
    return;  
}  
```  
  
 O `Subtract` método no serviço representa o chamador usando chamadas obrigatórias, conforme mostrado no código de exemplo a seguir.  
  
```csharp
public double Subtract(double n1, double n2)  
{  
    double result = n1 - n2;  
    Console.WriteLine("Received Subtract({0},{1})", n1, n2);  
    Console.WriteLine("Return: {0}", result);  
    Console.WriteLine("Before impersonating");  
    DisplayIdentityInformation();  
  
    if (ServiceSecurityContext.Current.WindowsIdentity.ImpersonationLevel == TokenImpersonationLevel.Impersonation ||  
        ServiceSecurityContext.Current.WindowsIdentity.ImpersonationLevel == TokenImpersonationLevel.Delegation)  
    {  
        // Impersonate.  
        using (ServiceSecurityContext.Current.WindowsIdentity.Impersonate())  
        {  
            // Make a system call in the caller's context and ACLs
            // on the system resource are enforced in the caller's context.
            Console.WriteLine("Impersonating the caller imperatively");  
            DisplayIdentityInformation();  
        }  
    }  
    else  
    {  
        Console.WriteLine("ImpersonationLevel is not high enough to perform this operation.");  
    }  
  
    Console.WriteLine("After reverting");  
    DisplayIdentityInformation();  
    return result;  
}  
```  
  
 Observe que, nesse caso, o chamador não é representado para a chamada inteira, mas é representado apenas por uma parte da chamada. Em geral, é preferível representar para o menor escopo para a operação inteira.  
  
 Os outros métodos não representam o chamador.  
  
 O código do cliente foi modificado para definir o nível de representação como <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation> . O cliente especifica o nível de representação a ser usado pelo serviço, usando a <xref:System.Security.Principal.TokenImpersonationLevel> enumeração. A enumeração oferece suporte aos seguintes valores <xref:System.Security.Principal.TokenImpersonationLevel.None> : <xref:System.Security.Principal.TokenImpersonationLevel.Anonymous> , <xref:System.Security.Principal.TokenImpersonationLevel.Identification> , <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation> e <xref:System.Security.Principal.TokenImpersonationLevel.Delegation> . Para executar uma verificação de acesso ao acessar um recurso do sistema no computador local que é protegido usando ACLs do Windows, o nível de representação deve ser definido <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation> como, conforme mostrado no código de exemplo a seguir.  
  
```csharp
// Create a client with given client endpoint configuration  
CalculatorClient client = new CalculatorClient();  
  
client.ClientCredentials.Windows.AllowedImpersonationLevel = TokenImpersonationLevel.Impersonation;  
```  
  
 Quando você executa o exemplo, as solicitações e respostas da operação são exibidas nas janelas do console do cliente e do serviço. Pressione ENTER em cada janela do console para desligar o serviço e o cliente.  
  
> [!NOTE]
> O serviço deve ser executado em uma conta administrativa ou a conta em que ele é executado deve receber direitos para registrar o `http://localhost:8000/ServiceModelSamples` URI com a camada http. Esses direitos podem ser concedidos Configurando uma [reserva de namespace](/windows/win32/http/namespace-reservations-registrations-and-routing) usando a [ferramenta Httpcfg. exe](/windows/win32/http/httpcfg-exe).  
  
> [!NOTE]
> Em computadores que executam o Windows Server 2003, haverá suporte para a representação apenas se o aplicativo host. exe tiver o privilégio de representação. (Por padrão, somente os administradores têm essa permissão.) Para adicionar esse privilégio a uma conta em que o serviço está sendo executado, vá para **Ferramentas administrativas**, abra **política de segurança local**, abra **políticas locais**, clique em **atribuição de direitos de usuário**e selecione **representar um cliente após a autenticação** e clique duas vezes em **Propriedades** para adicionar um usuário ou grupo.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
4. Para demonstrar que o serviço representa o chamador, execute o cliente em uma conta diferente daquela em que o serviço está sendo executado. Para fazer isso, no prompt de comando, digite:  
  
    ```console  
    runas /user:<machine-name>\<user-name> client.exe  
    ```  
  
     Em seguida, você será solicitado a fornecer uma senha. Insira a senha para a conta que você especificou anteriormente.  
  
5. Quando você executa o cliente, observe a identidade antes e depois de executá-la com credenciais diferentes.  
