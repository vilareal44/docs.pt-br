---
title: Contratos de dados compatíveis por encaminhamento
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data contracts [WCF], forward compatibility
ms.assetid: 413c9044-26f8-4ecb-968c-18495ea52cd9
ms.openlocfilehash: 34bde56b78ec0148cf6b924f8edd29343b97faa4
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597378"
---
# <a name="forward-compatible-data-contracts"></a>Contratos de dados compatíveis por encaminhamento
Um recurso do sistema de contrato de dados do Windows Communication Foundation (WCF) é que os contratos podem evoluir ao longo do tempo de maneiras não separáveis. Ou seja, um cliente com uma versão mais antiga de um contrato de dados pode se comunicar com um serviço com uma versão mais recente do mesmo contrato de dados ou um cliente com uma versão mais recente de um contrato de dados pode se comunicar com uma versão mais antiga do mesmo contrato de dados. Para obter mais informações, consulte [práticas recomendadas: controle de versão de contrato de dados](../best-practices-data-contract-versioning.md).  
  
 Você pode aplicar a maioria dos recursos de controle de versão de acordo com a necessidade quando novas versões de um contrato de dados existente são criadas. No entanto, um recurso de controle de versão, ida e *volta*, deve ser incorporado ao tipo da primeira versão para funcionar corretamente.  
  
## <a name="round-tripping"></a>Ida e volta  
 A viagem de ida e volta ocorre quando os dados passam de uma nova versão para uma versão antiga e para a nova versão de um contrato de dados. A ida e volta garante que nenhum dado seja perdido. Habilitar a ida e volta torna o tipo compatível com as alterações futuras com suporte do modelo de controle de versão do contrato de dados.  
  
 Para habilitar a ida e volta para um tipo específico, o tipo deve implementar a <xref:System.Runtime.Serialization.IExtensibleDataObject> interface. A interface contém uma propriedade <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A> (retornando o <xref:System.Runtime.Serialization.ExtensionDataObject> tipo). A propriedade armazena todos os dados de versões futuras do contrato de dados que são desconhecidos para a versão atual.  
  
### <a name="example"></a>Exemplo  
 O contrato de dados a seguir não é compatível com o encaminhamento com alterações futuras.  
  
 [!code-csharp[C_DataContract#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontract/cs/source.cs#7)]
 [!code-vb[C_DataContract#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontract/vb/source.vb#7)]  
  
 Para tornar o tipo compatível com alterações futuras (como adicionar um novo membro de dados chamado "phoneNumber"), implemente a <xref:System.Runtime.Serialization.IExtensibleDataObject> interface.  
  
 [!code-csharp[C_DataContract#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontract/cs/source.cs#8)]
 [!code-vb[C_DataContract#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontract/vb/source.vb#8)]  
  
 Quando a infraestrutura do WCF encontra dados que não fazem parte do contrato de dados original, os dados são armazenados na propriedade e preservados. Ele não é processado de nenhuma outra forma, exceto o armazenamento temporário. Se o objeto for retornado de volta para onde foi originado, os dados originais (desconhecidos) também serão retornados. Portanto, os dados fizeram uma viagem de ida e volta para o ponto de extremidade de origem sem perda. Observe, no entanto, que se o ponto de extremidade de origem exigir que os dados sejam processados, essa expectativa será não atendida e o ponto de extremidade deverá, de alguma forma, detectar e acomodar a alteração.  
  
 O <xref:System.Runtime.Serialization.ExtensionDataObject> tipo não contém propriedades ou métodos públicos. Portanto, é impossível obter acesso direto aos dados armazenados dentro da <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A> propriedade.  
  
 O recurso de ida e volta pode ser desativado, seja definindo `ignoreExtensionDataObject` `true` no <xref:System.Runtime.Serialization.DataContractSerializer> Construtor ou definindo a <xref:System.ServiceModel.ServiceBehaviorAttribute.IgnoreExtensionDataObject%2A> propriedade como `true` no <xref:System.ServiceModel.ServiceBehaviorAttribute> . Quando esse recurso estiver desativado, o desserializador não preencherá a <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A> propriedade e o serializador não emitirá o conteúdo da propriedade.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Serialization.IExtensibleDataObject>
- <xref:System.Runtime.Serialization.ExtensionDataObject>
- [Controle de versão de contrato de dados](data-contract-versioning.md)
- [Práticas recomendadas: controle de versão de contrato de dados](../best-practices-data-contract-versioning.md)
