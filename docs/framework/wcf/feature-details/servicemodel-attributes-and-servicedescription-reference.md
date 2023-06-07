---
title: Atributos de ServiceModel e referência de ServiceDescription
ms.date: 03/30/2017
ms.assetid: 4ab86b17-eab9-4846-a881-0099f9a7cc64
ms.openlocfilehash: 5e39a63d399edccc580b27ad4bfbc9ab05015ef9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600342"
---
# <a name="servicemodel-attributes-and-servicedescription-reference"></a>Atributos de ServiceModel e referência de ServiceDescription
A *árvore de descrição* é a hierarquia de tipos (começando com a <xref:System.ServiceModel.Description.ServiceDescription?displayProperty=nameWithType> classe) que, juntos, descrevem todos os aspectos de um serviço. O Windows Communication Foundation (WCF) usa uma árvore de descrição para criar um tempo de execução de serviço válido para publicar WSDL (Web Services Description Language), XSD (linguagem de definição de esquema XML) e asserções de política (metadados) sobre o serviço que os clientes podem usar para se conectar e usar o serviço e gerar várias representações de código e de arquivo de configuração dos valores de árvore de descrição  
  
 Este tópico descreve como as propriedades relacionadas ao contrato são obtidas do contrato de serviço e como elas são implementadas e adicionadas à árvore de descrição. Em alguns casos, os valores de atributo são convertidos em Propriedades de comportamento e o comportamento é inserido na árvore de descrição. Para obter mais informações sobre como os valores de árvore de descrição são convertidos em metadados, consulte [ServiceDescription e referência WSDL](servicedescription-and-wsdl-reference.md).  
  
## <a name="mapping-operations-to-the-description-tree"></a>Mapeando operações para a árvore de descrição  
 Em aplicativos WCF, os contratos de serviço são modelados por interfaces (ou classes) que usam atributos para marcar a interface ou classe e seus métodos como um agrupamento de operações. Quando uma <xref:System.ServiceModel.ServiceHost> classe é aberta, todos os contratos de serviço e implementações são refletidos e mesclados com informações de configuração em uma árvore de descrição.  
  
 Há dois tipos de modelos de operação: o modelo de *parâmetro* e o modelo de *contrato de mensagem* . O modelo de parâmetro usa métodos gerenciados que não têm um parâmetro ou tipo de valor de retorno que é marcado pela <xref:System.ServiceModel.MessageContractAttribute?displayProperty=nameWithType> classe. Nesse modelo, os desenvolvedores controlam a serialização de parâmetros e valores de retorno, mas o WCF gera os valores que são usados para preencher a árvore de descrição para o serviço e seu contrato.  
  
 As associações especificadas nos arquivos de configuração são carregadas diretamente na <xref:System.ServiceModel.Description.ServiceEndpoint.Binding%2A?displayProperty=nameWithType> propriedade.  
  
|Propriedade ServiceBehaviorAttribute|Valor de árvore de descrição afetado|  
|---------------------------------------|-------------------------------------|  
|Nome|<xref:System.ServiceModel.Description.ServiceDescription.Name%2A>|  
|Namespace|<xref:System.ServiceModel.Description.ServiceDescription.Namespace%2A>|  
|ConfigurationName|<xref:System.ServiceModel.Description.ServiceDescription.ConfigurationName%2A>|  
|IgnoreExtensionDataObject|Define a <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior.IgnoreExtensionDataObject%2A> propriedade para todas as operações.|  
|MaxItemsInObjectGraph|Define a <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior.MaxItemsInObjectGraph%2A> propriedade para todas as operações.|  
  
|Propriedade ServiceContractAttribute|Valor de árvore de descrição afetado|  
|---------------------------------------|-------------------------------------|  
|CallbackContract|<xref:System.ServiceModel.Description.ContractDescription.CallbackContractType%2A>, <xref:System.ServiceModel.Description.MessageDescription> adicionado a todas as operações <xref:System.ServiceModel.Description.OperationDescription.Messages%2A> .|  
|ConfigurationName|<xref:System.ServiceModel.Description.ContractDescription.ConfigurationName%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.ContractDescription.ProtectionLevel%2A>e, possivelmente, os níveis de proteção filhos. Para obter mais informações sobre a hierarquia de nível de proteção, consulte [noções básicas](../understanding-protection-level.md)sobre o nível de proteção.|  
|SessionMode|<xref:System.ServiceModel.Description.ContractDescription.SessionMode%2A>|  
  
|Valor de ServiceKnownTypesAttribute|Valor de árvore de descrição afetado|  
|--------------------------------------|-------------------------------------|  
|MethodName|<xref:System.ServiceModel.Description.OperationDescription.KnownTypes%2A>|  
  
|Valor de OperationContractAttribute|Valor de árvore de descrição afetado|  
|--------------------------------------|-------------------------------------|  
|Ação|<xref:System.ServiceModel.Description.MessageDescription.Action%2A>para a mensagem de saída ou a mensagem de entrada, dependendo do contrato de contrato/retorno de chamada.|  
|AsyncPattern|Se for true, <xref:System.ServiceModel.Description.OperationDescription.BeginMethod%2A> e<xref:System.ServiceModel.Description.OperationDescription.EndMethod%2A>|  
|IsOneWay|Mapeia para um único <xref:System.ServiceModel.Description.MessageDescription> no<xref:System.ServiceModel.Description.OperationDescription.Messages%2A>|  
|IsInitiating|<xref:System.ServiceModel.Description.OperationDescription.IsInitiating%2A>|  
|IsTerminating|<xref:System.ServiceModel.Description.OperationDescription.IsTerminating%2A>|  
|Nome|<xref:System.ServiceModel.Description.OperationDescription.Name%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.OperationDescription.ProtectionLevel%2A>e, possivelmente, os níveis de proteção filhos. Para obter mais informações sobre a hierarquia de nível de proteção, consulte [noções básicas](../understanding-protection-level.md)sobre o nível de proteção.|  
|ReplyAction|<xref:System.ServiceModel.Description.MessageDescription.Action%2A>para a mensagem de saída ou a mensagem de entrada, dependendo do contrato de contrato/retorno de chamada.|  
  
|Valor de FaultContractAttribute|Valor de árvore de descrição afetado|  
|----------------------------------|-------------------------------------|  
|Ação|<xref:System.ServiceModel.Description.FaultDescription.Action%2A>dependendo do contrato de contrato/retorno de chamada.|  
|DetailType|<xref:System.ServiceModel.Description.FaultDescription.DetailType%2A>|  
|Nome|<xref:System.ServiceModel.Description.FaultDescription.Name%2A>|  
|Namespace|<xref:System.ServiceModel.Description.FaultDescription.Namespace%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.FaultDescription.ProtectionLevel%2A>|  
  
|Valor de DataContractFormatAttribute|Valor de árvore de descrição afetado|  
|---------------------------------------|-------------------------------------|  
|Uso|O <xref:System.ServiceModel.DataContractFormatAttribute.Style%2A> valor é definido no <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> para a operação.|  
  
|Valor de XmlSerializerFormatAttribute|Valor de árvore de descrição afetado|  
|----------------------------------------|-------------------------------------|  
|Estilo|Essa <xref:System.ServiceModel.XmlSerializerFormatAttribute> propriedade é definida no <xref:System.ServiceModel.Description.XmlSerializerOperationBehavior> para a operação.|  
|Uso|O <xref:System.ServiceModel.XmlSerializerFormatAttribute> é definido no <xref:System.ServiceModel.Description.XmlSerializerOperationBehavior> para a operação.|  
  
|Valor de TransactionFlowAttribute|Valor de árvore de descrição afetado|  
|------------------------------------|-------------------------------------|  
|TransactionFlowOption|O <xref:System.ServiceModel.TransactionFlowAttribute> é adicionado como um comportamento de operação à <xref:System.ServiceModel.Description.OperationDescription.Behaviors%2A> propriedade.|  
  
|Valor de MessageContractAttribute|Valor de árvore de descrição afetado|  
|------------------------------------|-------------------------------------|  
|ProtectionLevel|<xref:System.ServiceModel.Description.MessageDescription.ProtectionLevel%2A>|  
|WrapperName|<xref:System.ServiceModel.Description.MessageBodyDescription.WrapperName%2A>|  
|WrapperNamespace|<xref:System.ServiceModel.Description.MessageBodyDescription.WrapperNamespace%2A>|  
  
|Valor de MessageHeaderAttribute|Valor de árvore de descrição afetado|  
|----------------------------------|-------------------------------------|  
|Ator|<xref:System.ServiceModel.Description.MessageHeaderDescription.Actor%2A>para o cabeçalho correspondente em<xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
|MustUnderstand|<xref:System.ServiceModel.Description.MessageHeaderDescription.MustUnderstand%2A>para o cabeçalho correspondente em<xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
|Nome|<xref:System.ServiceModel.Description.MessagePartDescription.Name%2A>para o cabeçalho correspondente em<xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
|Namespace|<xref:System.ServiceModel.Description.MessagePartDescription.Namespace%2A>para o cabeçalho correspondente em<xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.MessagePartDescription.ProtectionLevel%2A>para o cabeçalho correspondente em<xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
|Retransmissão|<xref:System.ServiceModel.Description.MessageHeaderDescription.Relay%2A>para o cabeçalho correspondente em<xref:System.ServiceModel.Description.MessageDescription.Headers%2A>|  
  
|Valor de MessageBodyMemberAttribute|Valor de árvore de descrição afetado|  
|--------------------------------------|-------------------------------------|  
|Nome|<xref:System.ServiceModel.Description.MessagePartDescription.Name%2A>para a parte correspondente em<xref:System.ServiceModel.Description.MessageBodyDescription.Parts%2A>|  
|Namespace|<xref:System.ServiceModel.Description.MessagePartDescription.Namespace%2A>para a parte correspondente em<xref:System.ServiceModel.Description.MessageBodyDescription.Parts%2A>|  
|Order|<xref:System.ServiceModel.Description.MessagePartDescription.Index%2A>para a parte correspondente em<xref:System.ServiceModel.Description.MessageBodyDescription.Parts%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.MessagePartDescription.ProtectionLevel%2A>para a parte correspondente em<xref:System.ServiceModel.Description.MessageBodyDescription.Parts%2A>|  
  
|Valor de MessageHeaderArrayAttribute|Valor de árvore de descrição afetado|  
|---------------------------------------|-------------------------------------|  
|Ator|<xref:System.ServiceModel.Description.MessageHeaderDescription.Actor%2A>|  
|MustUnderstand|<xref:System.ServiceModel.Description.MessageHeaderDescription.MustUnderstand%2A>|  
|Nome|<xref:System.ServiceModel.Description.MessagePartDescription.Name%2A>|  
|Namespace|<xref:System.ServiceModel.Description.MessagePartDescription.Namespace%2A>|  
|ProtectionLevel|<xref:System.ServiceModel.Description.MessagePartDescription.ProtectionLevel%2A>|  
|Retransmissão|<xref:System.ServiceModel.Description.MessageHeaderDescription.Relay%2A>|  
  
|Valor de MessagePropertyAttribute|Valor de árvore de descrição afetado|  
|------------------------------------|-------------------------------------|  
|Nome|<xref:System.ServiceModel.Description.MessagePartDescription.Name%2A>|  
  
|Valor de MessageParameterAttribute|Valor de árvore de descrição afetado|  
|-------------------------------------|-------------------------------------|  
|Nome|<xref:System.ServiceModel.Description.MessagePartDescription.Name%2A>para a parte correspondente em<xref:System.ServiceModel.Description.MessageBodyDescription.Parts%2A>|  
  
 Para obter mais informações sobre como os valores de árvore de descrição são convertidos em metadados, consulte [ServiceDescription e referência WSDL](servicedescription-and-wsdl-reference.md).  
  
## <a name="see-also"></a>Consulte também

- [ServiceDescription and WSDL Reference](servicedescription-and-wsdl-reference.md)
