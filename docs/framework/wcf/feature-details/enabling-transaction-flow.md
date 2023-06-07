---
title: Ativando o fluxo de transações
ms.date: 03/30/2017
helpviewer_keywords:
- transactions [WCF], enabling flow
ms.assetid: a03f5041-5049-43f4-897c-e0292d4718f7
ms.openlocfilehash: 5cea72e503087ac2a8f3b6ff2a07c2919ee00630
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597417"
---
# <a name="enabling-transaction-flow"></a>Ativando o fluxo de transações
O Windows Communication Foundation (WCF) fornece opções altamente flexíveis para controlar o fluxo de transações. As configurações de fluxo de transações de um serviço podem ser expressas usando uma combinação de atributos e configuração.  
  
## <a name="transaction-flow-settings"></a>Configurações de fluxo de transações  
 As configurações de fluxo de transações são geradas para um ponto de extremidade de serviço como resultado da interseção dos três valores a seguir:  
  
- O <xref:System.ServiceModel.TransactionFlowAttribute> atributo especificado para cada método no contrato de serviço.  
  
- A `TransactionFlow` propriedade de associação na associação específica.  
  
- A `TransactionFlowProtocol` propriedade de associação na associação específica. A `TransactionFlowProtocol` Propriedade Binding permite que você escolha entre dois protocolos de transação diferentes que você pode usar para fluir uma transação. As seções a seguir descrevem brevemente cada uma delas.  
  
### <a name="ws-atomictransaction-protocol"></a>Protocolo WS-AtomicTransaction  
 O protocolo WS-AtomicTransaction (WS-AT) é útil para cenários em que a interoperabilidade com pilhas de protocolos de terceiros é necessária.  
  
### <a name="oletransactions-protocol"></a>Protocolo OleTransactions  
 O protocolo OleTransactions é útil para cenários em que a interoperabilidade com pilhas de protocolos de terceiros não é necessária e o implantador de um serviço sabe antecipadamente que o serviço de protocolo WS-AT está desabilitado localmente ou a topologia de rede existente não favorece o uso de WS-AT.  
  
 A tabela a seguir mostra os diferentes tipos de fluxos de transação que podem ser gerados usando essas várias combinações.  
  
|TransactionFlow<br /><br /> associação|Propriedade de associação TransactionFlow|Protocolo de associação TransactionFlowProtocol|Tipo de fluxo de transações|  
|---------------------------------|--------------------------------------|----------------------------------------------|------------------------------|  
|Obrigatório|true|WS-AT|A transação deve ser fluida no formato interoperável WS-AT.|  
|Obrigatório|true|OleTransactions|A transação deve ser fluida no formato WCF OleTransactions.|  
|Obrigatório|false|Não aplicável|Não aplicável porque esta é uma configuração inválida.|  
|Permitido|true|WS-AT|A transação pode estar fluindo no formato interoperável WS-AT.|  
|Permitido|true|OleTransactions|A transação pode estar fluindo no formato WCF OleTransactions.|  
|Permitido|false|Qualquer valor|Uma transação não está fluindo.|  
|NotAllowed|Qualquer valor|Qualquer valor|Uma transação não está fluindo.|  
  
 A tabela a seguir resume o resultado do processamento da mensagem.  
  
|Mensagem de entrada|Configuração de TransactionFlow|Cabeçalho da transação|Resultado do processamento de mensagens|  
|----------------------|-----------------------------|------------------------|-------------------------------|  
|A transação corresponde ao formato de protocolo esperado|Permitido ou obrigatório|`MustUnderstand` é igual a `true`.|Processo|  
|A transação não corresponde ao formato de protocolo esperado|Obrigatório|`MustUnderstand` é igual a `false`.|Rejeitado porque uma transação é necessária|  
|A transação não corresponde ao formato de protocolo esperado|Permitido|`MustUnderstand` é igual a `false`.|Rejeitado porque o cabeçalho não é compreendido|  
|Transação usando qualquer formato de protocolo|NotAllowed|`MustUnderstand` é igual a `false`.|Rejeitado porque o cabeçalho não é compreendido|  
|Nenhuma transação|Obrigatório|N/D|Rejeitado porque uma transação é necessária|  
|Nenhuma transação|Permitido|N/D|Processo|  
|Nenhuma transação|NotAllowed|N/D|Processo|  
  
 Embora cada método em um contrato possa ter diferentes requisitos de fluxo de transações, a configuração do protocolo de fluxo de transações é delimitada no nível da associação. Isso significa que todos os métodos que compartilham o mesmo ponto de extremidade (e, portanto, a mesma associação) também compartilham a mesma política, permitindo ou exigindo o fluxo de transações, bem como o mesmo protocolo de transação, se aplicável.  
  
## <a name="enabling-transaction-flow-at-the-method-level"></a>Habilitando o fluxo de transações no nível do método  
 Os requisitos de fluxo de transações nem sempre são os mesmos para todos os métodos em um contrato de serviço. Portanto, o WCF também fornece um mecanismo baseado em atributo para permitir que as preferências de fluxo de transações de cada método sejam expressas. Isso é obtido pelo <xref:System.ServiceModel.TransactionFlowAttribute> que especifica o nível no qual uma operação de serviço aceita um cabeçalho de transação. Você deve marcar seus métodos de contrato de serviço com esse atributo se quiser habilitar o fluxo de transações. Esse atributo usa um dos valores da <xref:System.ServiceModel.TransactionFlowOption> enumeração, em que o valor padrão é <xref:System.ServiceModel.TransactionFlowOption.NotAllowed> . Se qualquer valor, exceto <xref:System.ServiceModel.TransactionFlowOption.NotAllowed> for especificado, o método não será necessário para ser unidirecional. Um desenvolvedor pode usar esse atributo para especificar requisitos de fluxo de transações em nível de método ou restrições em tempo de design.  
  
## <a name="enabling-transaction-flow-at-the-endpoint-level"></a>Habilitando o fluxo de transações no nível do ponto de extremidade  
 Além da configuração de fluxo de transações em nível de método que o <xref:System.ServiceModel.TransactionFlowAttribute> atributo fornece, o WCF fornece uma configuração de todo o ponto de extremidade para o fluxo de transações para permitir que os administradores controlem o fluxo de transações em um nível mais alto.  
  
 Isso é conseguido pelo <xref:System.ServiceModel.Channels.TransactionFlowBindingElement> , que permite habilitar ou desabilitar o fluxo de transações de entrada nas configurações de associação de um ponto de extremidade, bem como especificar o formato de protocolo de transação desejado para transações de entrada.  
  
 Se a associação tiver desabilitado o fluxo de transações, mas uma das operações em um contrato de serviço exigir uma transação de entrada, uma exceção de validação será lançada na inicialização do serviço.  
  
 A maioria das ligações que o WCF fornece contém os `transactionFlow` `transactionProtocol` atributos e para permitir que você configure a associação específica para aceitar as transações de entrada. Para obter mais informações sobre como definir os elementos de configuração, consulte [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) .  
  
 Um administrador ou um implantador pode usar o fluxo de transações no nível do ponto de extremidade para configurar restrições ou requisitos de fluxo de transação no momento da implantação usando o arquivo de configuração.  
  
## <a name="security"></a>Segurança  
 Para garantir a segurança e a integridade do sistema, você deve proteger as trocas de mensagens ao fluir transações entre aplicativos. Você não deve fluir ou divulgar detalhes da transação para qualquer aplicativo que não tenha direito a participar da mesma transação.  
  
 Ao gerar clientes WCF para serviços Web desconhecidos ou não confiáveis por meio do uso de troca de metadados, as chamadas para operações nesses serviços Web devem suprimir a transação atual, se possível. O exemplo a seguir demonstra como fazer isso.  
  
```csharp
//client code which has an ambient transaction  
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Suppress))  
{  
    // No transaction will flow to this operation  
    untrustedProxy.Operation1(...);  
    scope.Complete();  
}  
//remainder of client code  
```  
  
 Além disso, os serviços devem ser configurados para aceitar transações de entrada somente de clientes que tenham sido autenticados e autorizados. As transações de entrada só devem ser aceitas se forem provenientes de clientes altamente confiáveis.  
  
## <a name="policy-assertions"></a>Declarações de política  
 O WCF usa declarações de política para controlar o fluxo de transações. As declarações de política podem ser encontradas no documento de política de um serviço, que é gerado pela agregação de contratos, configuração e atributos. O cliente pode obter o documento de política do serviço usando uma resposta de solicitação HTTP GET ou WS-MetadataExchange. Os clientes podem processar o documento de política para determinar quais operações em um contrato de serviço podem dar suporte ou exigir o fluxo de transações.  
  
 As declarações de política de fluxo de transações afetam o fluxo de transações, especificando os cabeçalhos SOAP que um cliente deve enviar a um serviço para representar uma transação. Todos os cabeçalhos de transação devem ser marcados com `MustUnderstand` igual a `true` . Qualquer mensagem com um cabeçalho marcado de outra forma é rejeitada com uma falha de SOAP.  
  
 Somente uma declaração de política relacionada à transação pode estar presente em uma única operação. Os documentos de política com mais de uma declaração de transação em uma operação são considerados inválidos e são rejeitados pelo WCF. Além disso, apenas um único protocolo de transação pode estar presente dentro de cada tipo de porta. Documentos de política com operações que fazem referência a mais de um protocolo de transação dentro de um único tipo de porta são considerados inválidos e são rejeitados pela [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Os documentos de política com declarações de transação presentes em mensagens de saída ou em mensagens de entrada unidirecionais também são considerados inválidos.
