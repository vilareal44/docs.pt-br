---
title: Usando contratos de dados
description: Saiba mais sobre um contrato de dados, que define, para cada parâmetro ou tipo de retorno, quais dados são serializados para serem trocados entre um cliente e servidor WCF.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataContractAttribute class
- WCF, data
- data contracts [WCF]
ms.assetid: a3ae7b21-c15c-4c05-abd8-f483bcbf31af
ms.openlocfilehash: 80ea2a8bd67c627fbe11ee07e640704c1a41ef7b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244719"
---
# <a name="using-data-contracts"></a>Usando contratos de dados
Um *contrato de dados* é um contrato formal entre um serviço e um cliente que descreve de forma abstrata os dados a serem trocados. Ou seja, para se comunicar, o cliente e o serviço não precisam compartilhar os mesmos tipos, apenas os mesmos contratos de dados. Um contrato de dados define precisamente, para cada parâmetro ou tipo de retorno, quais dados são serializados (convertidos em XML) para serem trocados.  
  
## <a name="data-contract-basics"></a>Noções básicas do contrato de dados  
 O Windows Communication Foundation (WCF) usa um mecanismo de serialização chamado de serializador de contrato de dados por padrão para serializar e desserializar dados (convertê-los de e para XML). Todos os tipos primitivos .NET Framework, como inteiros e cadeias de caracteres, bem como determinados tipos tratados como primitivos, como <xref:System.DateTime> e <xref:System.Xml.XmlElement> , podem ser serializados sem nenhuma outra preparação e são considerados como tendo os contratos de dados padrão. Muitos tipos de .NET Framework também têm contratos de dados existentes. Para obter uma lista completa de tipos serializáveis, consulte [tipos com suporte no serializador de contrato de dados](types-supported-by-the-data-contract-serializer.md).  
  
 Novos tipos complexos que você criar devem ter um contrato de dados definido para que eles sejam serializáveis. Por padrão, o <xref:System.Runtime.Serialization.DataContractSerializer> infere o contrato de dados e serializa todos os tipos publicamente visíveis. Todas as propriedades públicas de leitura/gravação e os campos do tipo são serializados. Você pode recusar membros da serialização usando o <xref:System.Runtime.Serialization.IgnoreDataMemberAttribute> . Você também pode criar explicitamente um contrato de dados usando <xref:System.Runtime.Serialization.DataContractAttribute> <xref:System.Runtime.Serialization.DataMemberAttribute> atributos e. Isso normalmente é feito aplicando o <xref:System.Runtime.Serialization.DataContractAttribute> atributo ao tipo. Esse atributo pode ser aplicado a classes, estruturas e enumerações. O <xref:System.Runtime.Serialization.DataMemberAttribute> atributo deve ser aplicado a cada membro do tipo de contrato de dados para indicar que é um *membro de dados*, ou seja, deve ser serializado. Para obter mais informações, consulte [tipos serializáveis](serializable-types.md).  
  
### <a name="example"></a>Exemplo  
 O exemplo a seguir mostra um contrato de serviço (uma interface) ao qual os <xref:System.ServiceModel.ServiceContractAttribute> atributos e foram <xref:System.ServiceModel.OperationContractAttribute> aplicados explicitamente. O exemplo mostra que os tipos primitivos não exigem um contrato de dados, enquanto um tipo complexo faz.  
  
 [!code-csharp[C_DataContract#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontract/cs/source.cs#1)]
 [!code-vb[C_DataContract#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontract/vb/source.vb#1)]  
  
 O exemplo a seguir mostra como um contrato de dados para o `MyTypes.PurchaseOrder` tipo é criado aplicando os <xref:System.Runtime.Serialization.DataContractAttribute> <xref:System.Runtime.Serialization.DataMemberAttribute> atributos e à classe e seus membros.  
  
 [!code-csharp[C_DataContract#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontract/cs/source.cs#2)]
 [!code-vb[C_DataContract#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontract/vb/source.vb#2)]  
  
### <a name="notes"></a>Observações  
 As observações a seguir fornecem itens a serem considerados ao criar contratos de dados:  
  
- O <xref:System.Runtime.Serialization.IgnoreDataMemberAttribute> atributo só é respeitado quando usado com tipos desmarcados. Isso inclui tipos que não são marcados com um dos <xref:System.Runtime.Serialization.DataContractAttribute> atributos, <xref:System.SerializableAttribute> , <xref:System.Runtime.Serialization.CollectionDataContractAttribute> , ou <xref:System.Runtime.Serialization.EnumMemberAttribute> marcados como serializáveis por outros meios (como <xref:System.Xml.Serialization.IXmlSerializable> ).  
  
- Você pode aplicar o <xref:System.Runtime.Serialization.DataMemberAttribute> atributo a campos e propriedades.  
  
- Os níveis de acessibilidade do membro (interno, privado, protegido ou público) não afetam o contrato de dados de forma alguma.  
  
- O <xref:System.Runtime.Serialization.DataMemberAttribute> atributo será ignorado se for aplicado a membros estáticos.  
  
- Durante a serialização, o código de obtenção de propriedade é chamado para membros de dados de propriedade para obter o valor das propriedades a serem serializadas.  
  
- Durante a desserialização, um objeto não inicializado é criado pela primeira vez, sem chamar nenhum construtor no tipo. Em seguida, todos os membros de dados são desserializados.  
  
- Durante a desserialização, o código do conjunto de propriedades é chamado para membros de dados de propriedade para definir as propriedades como o valor que está sendo desserializado.  
  
- Para que um contrato de dados seja válido, deve ser possível serializar todos os seus membros de dados. Para obter uma lista completa de tipos serializáveis, consulte [tipos com suporte no serializador de contrato de dados](types-supported-by-the-data-contract-serializer.md).  
  
     Os tipos genéricos são tratados exatamente da mesma forma que os tipos não genéricos. Não há requisitos especiais para parâmetros genéricos. Por exemplo, considere o seguinte tipo.  
  
 [!code-csharp[C_DataContract#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontract/cs/source.cs#3)]
 [!code-vb[C_DataContract#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontract/vb/source.vb#3)]  
  
 Esse tipo é serializável se o tipo usado para o parâmetro de tipo genérico ( `T` ) é serializável ou não. Como é necessário serializar todos os membros de dados, o tipo a seguir só será serializável se o parâmetro de tipo genérico também for serializável, como mostrado no código a seguir.  
  
 [!code-csharp[C_DataContract#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontract/cs/source.cs#4)]
 [!code-vb[C_DataContract#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontract/vb/source.vb#4)]  
  
 Para obter um exemplo de código completo de um serviço WCF que define um contrato de dados, consulte o exemplo de [contrato de dados básico](../samples/basic-data-contract.md) .  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.Serialization.DataMemberAttribute>
- <xref:System.Runtime.Serialization.DataContractAttribute>
- [Tipos serializáveis](serializable-types.md)
- [Nomes de contrato de dados](data-contract-names.md)
- [Equivalência de contrato de dados](data-contract-equivalence.md)
- [Ordem de membro de dados](data-member-order.md)
- [Tipos conhecidos de contrato de dados](data-contract-known-types.md)
- [Contratos de dados compatíveis por encaminhamento](forward-compatible-data-contracts.md)
- [Controle de versão de contrato de dados](data-contract-versioning.md)
- [Retornos de chamada de serialização tolerantes à versão](version-tolerant-serialization-callbacks.md)
- [Valores padrões de membro de dados](data-member-default-values.md)
- [Tipos com suporte fornecido pelo serializador de contrato de dados](types-supported-by-the-data-contract-serializer.md)
- [Como criar um contrato de dados básicos para uma classe ou estrutura](how-to-create-a-basic-data-contract-for-a-class-or-structure.md)
