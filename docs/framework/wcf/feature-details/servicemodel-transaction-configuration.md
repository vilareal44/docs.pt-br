---
title: Configuração de transação de ServiceModel
ms.date: 03/30/2017
helpviewer_keywords:
- transactions [WCF], ServiceModel configuration
ms.assetid: 5636067a-7fbd-4485-aaa2-8141c502acf3
ms.openlocfilehash: 1d04a7bb756cccb33b436c1f57decc0249764828
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600329"
---
# <a name="servicemodel-transaction-configuration"></a>Configuração de transação de ServiceModel
Windows Communication Foundation (WCF) fornece três atributos para configurar transações para um serviço: `transactionFlow` , `transactionProtocol` e `transactionTimeout` .  
  
## <a name="configuring-transactionflow"></a>Configurando o transactionFlow  
 A maioria das associações predefinidas que o WCF fornece contém os `transactionFlow` `transactionProtocol` atributos e, para que você possa configurar a associação para aceitar as transações de entrada de um ponto de extremidade específico usando um protocolo de fluxo de transação específico. Além disso, você pode usar o `transactionFlow` elemento e seu `transactionProtocol` atributo para criar sua própria associação personalizada. Para obter mais informações sobre como definir os elementos de configuração, consulte [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) e [esquema de configuração do WCF](../../configure-apps/file-schema/wcf/index.md).  
  
 O `transactionFlow` atributo especifica se o fluxo de transações está habilitado para pontos de extremidade de serviço que usam a associação.  
  
## <a name="configuring-transactionprotocol"></a>Configurando o transactionProtocol  
 O `transactionProtocol` atributo especifica o protocolo de transação a ser usado com pontos de extremidade de serviço que usam a associação.  
  
 Veja a seguir um exemplo de uma seção de configuração que configura a associação especificada para dar suporte ao fluxo de transações, bem como a usar o protocolo WS-AtomicTransaction.  
  
```xml  
<netNamedPipeBinding>  
   <binding name="test"  
      closeTimeout="00:00:10"  
      openTimeout="00:00:20"
      receiveTimeout="00:00:30"  
      sendTimeout="00:00:40"  
      transactionFlow="true"  
      transactionProtocol="WSAtomicTransactionOctober2004"  
      hostNameComparisonMode="WeakWildcard"  
      maxBufferSize="1001"  
      maxConnections="123"
      maxReceivedMessageSize="1000">  
   </binding>  
</netNamedPipeBinding>  
```  
  
## <a name="configuring-transactiontimeout"></a>Configurando o transactionTimeout  
 Você pode configurar o `transactionTimeout` atributo para seu serviço WCF no `behavior` elemento do arquivo de configuração. O código a seguir demonstra como fazer isso.  
  
```xml  
<configuration>  
   <system.serviceModel>  
      <behaviors>  
         <behavior name="NewBehavior" transactionTimeout="00:01:00" /> <!-- 1 minute timeout -->  
      </behaviors>  
   </system.serviceModel>  
</configuration>  
```  
  
 O `transactionTimeout` atributo especifica o período de tempo no qual uma nova transação criada no serviço deve ser concluída. Ele é usado como o <xref:System.Transactions.TransactionScope> tempo limite para qualquer operação que estabeleça uma nova transação e, se <xref:System.ServiceModel.OperationBehaviorAttribute> for aplicado, a <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> propriedade será definida como `true` .  
  
 O tempo limite especifica a duração do tempo desde a criação da transação até a conclusão da fase 1 no protocolo de confirmação de duas fases.  
  
 Se esse atributo for definido dentro de uma `service` seção de configuração, você deverá aplicar pelo menos um método do serviço correspondente com <xref:System.ServiceModel.OperationBehaviorAttribute> o, no qual a <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionScopeRequired%2A> propriedade é definida como `true` .  
  
 Observe que o valor de tempo limite usado é o valor menor entre esse `transactionTimeout` parâmetro de configuração e qualquer <xref:System.ServiceModel.ServiceBehaviorAttribute.TransactionTimeout%2A> propriedade.  
  
## <a name="see-also"></a>Consulte também

- [\<binding>](../../configure-apps/file-schema/wcf/bindings.md)
- [Esquema de configuração do WCF](../../configure-apps/file-schema/wcf/index.md)
