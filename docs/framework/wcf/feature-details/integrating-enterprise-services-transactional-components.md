---
title: Integração de componentes transacionais de Enterprise Services
ms.date: 03/30/2017
ms.assetid: 05dab277-b8b2-48cf-b40c-826be128b175
ms.openlocfilehash: 1c4fabfadb113c79b216fa10ff80b551ba0f9716
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596845"
---
# <a name="integrating-enterprise-services-transactional-components"></a>Integração de componentes transacionais de Enterprise Services

O Windows Communication Foundation (WCF) fornece um mecanismo automático para a integração com os serviços corporativos (consulte [integração com aplicativos com+](integrating-with-com-plus-applications.md)). No entanto, talvez você queira a flexibilidade para desenvolver serviços que usam internamente componentes transacionais hospedados nos serviços corporativos. Como o recurso de transações do WCF é criado na <xref:System.Transactions> infraestrutura, o processo de integração dos serviços corporativos com o WCF é idêntico ao de especificar a interoperabilidade entre <xref:System.Transactions> o e os serviços corporativos, conforme descrito em [interoperabilidade com os serviços corporativos e as transações com+](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.0/ms229974(v=vs.85)).  
  
 Para fornecer o nível desejado de interoperabilidade entre a transação de fluxo de entrada e a transação de contexto COM+, a implementação do serviço deve criar uma <xref:System.Transactions.TransactionScope> instância e usar o valor apropriado da <xref:System.Transactions.EnterpriseServicesInteropOption> enumeração.  
  
## <a name="integrating-enterprise-services-with-a-service-operation"></a>Integrando serviços corporativos a uma operação de serviço  
 O código a seguir demonstra uma operação, com fluxo de transações permitido, que cria um <xref:System.Transactions.TransactionScope> com a <xref:System.Transactions.EnterpriseServicesInteropOption.Full> opção. As seguintes condições se aplicam neste cenário:  
  
- Se o cliente fluir uma transação, a operação, incluindo a chamada para o componente Enterprise Services, será executada dentro do escopo dessa transação. O uso de <xref:System.Transactions.EnterpriseServicesInteropOption.Full> garante que a transação seja sincronizada com o <xref:System.EnterpriseServices> contexto, o que significa que a transação de ambiente para <xref:System.Transactions> e a <xref:System.EnterpriseServices> é a mesma.  
  
- Se o cliente não fluir em uma transação, <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> a configuração para `true` criará um novo escopo de transação para a operação. Da mesma forma, o uso <xref:System.Transactions.EnterpriseServicesInteropOption.Full> de garante que a transação da operação seja igual à transação usada dentro do <xref:System.EnterpriseServices> contexto do componente.  
  
 Todas as chamadas de método adicionais também ocorrem dentro do escopo da transação da mesma operação.  
  
```csharp
[ServiceContract()]  
public interface ICustomerServiceContract  
{  
   [OperationContract]  
   [TransactionFlow(TransactionFlowOption.Allowed)]  
   void UpdateCustomerNameOperation(int customerID, string newCustomerName);  
}  
  
[ServiceBehavior(TransactionIsolationLevel = System.Transactions.IsolationLevel.Serializable)]  
public class CustomerService : ICustomerServiceContract  
{  
   [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]  
   public void UpdateCustomerNameOperation(int customerID, string newCustomerName)  
   {  
   // Create a transaction scope with full ES interop  
      using (TransactionScope ts = new TransactionScope(  
                     TransactionScopeOption.Required,  
                     new TransactionOptions(),  
                     EnterpriseServicesInteropOption.Full))  
      {  
         // Create an Enterprise Services component  
         // Call UpdateCustomer method on an Enterprise Services
         // component
  
         // Call UpdateOtherCustomerData method on an Enterprise
         // Services component
         ts.Complete();  
      }  
  
      // Do UpdateAdditionalData on an non-Enterprise Services  
      // component  
   }  
}  
```  
  
 Se nenhuma sincronização for necessária entre a transação atual de uma operação e as chamadas para componentes de serviços corporativos transacionais, use a <xref:System.Transactions.EnterpriseServicesInteropOption.None> opção ao instanciar a <xref:System.Transactions.TransactionScope> instância.  
  
## <a name="integrating-enterprise-services-with-a-client"></a>Integrando serviços corporativos a um cliente  
 O código a seguir demonstra o código do cliente usando uma <xref:System.Transactions.TransactionScope> instância com a <xref:System.Transactions.EnterpriseServicesInteropOption.Full> configuração. Nesse cenário, as chamadas para operações de serviço que dão suporte ao fluxo de transações ocorrem dentro do escopo da mesma transação que as chamadas para componentes de serviços corporativos.  
  
```csharp
static void Main()  
{  
    // Create a client  
    CalculatorClient client = new CalculatorClient();  
  
    // Create a transaction scope with full ES interop  
    using (TransactionScope ts = new TransactionScope(  
          TransactionScopeOption.Required,  
          new TransactionOptions(),  
          EnterpriseServicesInteropOption.Full))  
    {  
        // Call Add calculator service operation  
  
        // Create an Enterprise Services component  
  
        // Call UpdateCustomer method on an Enterprise Services
        // component
  
        ts.Complete();  
    }  
  
    // Closing the client gracefully closes the connection and
    // cleans up resources  
    client.Close();  
}  
```  
  
## <a name="see-also"></a>Consulte também

- [Integração com aplicativos COM+](integrating-with-com-plus-applications.md)
- [Integração com aplicativos COM](integrating-with-com-applications.md)
