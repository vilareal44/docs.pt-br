---
title: Tipos de enumeração em contratos de dados
description: Saiba mais sobre como o modelo de contrato de dados expressa enumerações como parte do modelo de programação WFC.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data contracts [WCF], enumeration types
ms.assetid: b5d694da-68cb-4b74-a5fb-75108a68ec3b
ms.openlocfilehash: ff3184a285e88d47d4545a38a6c74b2f209827fb
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247293"
---
# <a name="enumeration-types-in-data-contracts"></a>Tipos de enumeração em contratos de dados
As enumerações podem ser expressas no modelo de contrato de dados. Este tópico descreve vários exemplos que explicam o modelo de programação.  
  
## <a name="enumeration-basics"></a>Noções básicas de enumeração  
 Uma maneira de usar tipos de enumeração no modelo de contrato de dados é aplicar o <xref:System.Runtime.Serialization.DataContractAttribute> atributo ao tipo. Em seguida, você deve aplicar o <xref:System.Runtime.Serialization.EnumMemberAttribute> atributo a cada membro que deve ser incluído no contrato de dados.  
  
 O exemplo a seguir mostra duas classes. O primeiro usa a enumeração e a segunda define a enumeração.  
  
 [!code-csharp[c_DataContractEnumerations#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#1)]
 [!code-vb[c_DataContractEnumerations#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#1)]  
  
 Uma instância da `Car` classe pode ser enviada ou recebida somente se o `condition` campo for definido como um dos valores `New` , `Used` ou `Rental` . Se `condition` for `Broken` ou `Stolen` , um <xref:System.Runtime.Serialization.SerializationException> será lançado.  
  
 Você pode usar as <xref:System.Runtime.Serialization.DataContractAttribute> Propriedades ( <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> e <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A> ) normalmente para contratos de dados de enumeração.  
  
### <a name="enumeration-member-values"></a>Valores de membro de enumeração  
 Geralmente, o contrato de dados inclui nomes de membros de enumeração, não valores numéricos. No entanto, ao usar o modelo de contrato de dados, se o lado de recebimento for um cliente WCF, o esquema exportado preservará os valores numéricos. Observe que esse não é o caso ao usar o [usando a classe XmlSerializer](using-the-xmlserializer-class.md).  
  
 No exemplo anterior, se `condition` é definido como `Used` e os dados são serializados para XML, o XML resultante é `<condition>Used</condition>` e não `<condition>1</condition>` . Portanto, o contrato de dados a seguir é equivalente ao contrato de dados do `CarConditionEnum` .  
  
 [!code-csharp[c_DataContractEnumerations#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#2)]
 [!code-vb[c_DataContractEnumerations#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#2)]  
  
 Por exemplo, você pode usar `CarConditionEnum` no lado de envio e `CarConditionWithNumbers` no lado de recebimento. Embora o lado de envio use o valor "1" para `Used` e o lado de recebimento use o valor "20", a representação XML é `<condition>Used</condition>` para ambos os lados.  
  
 Para ser incluído no contrato de dados, você deve aplicar o <xref:System.Runtime.Serialization.EnumMemberAttribute> atributo. Na .NET Framework, você sempre pode aplicar o valor especial 0 (zero) a uma enumeração, que também é o valor padrão para qualquer enumeração. No entanto, mesmo esse valor zero especial não pode ser serializado, a menos que esteja marcado com o <xref:System.Runtime.Serialization.EnumMemberAttribute> atributo.  
  
 Há duas exceções a isso:  
  
- Sinalize enumerações (discutidas posteriormente neste tópico).  
  
- Membros de dados de enumeração com a <xref:System.Runtime.Serialization.DataMemberAttribute.EmitDefaultValue%2A> propriedade definida como `false` (nesse caso, a enumeração com o valor zero é omitida dos dados serializados).  
  
### <a name="customizing-enumeration-member-values"></a>Personalizando valores de membro de enumeração  
 Você pode personalizar o valor do membro de enumeração que forma uma parte do contrato de dados usando a <xref:System.Runtime.Serialization.EnumMemberAttribute.Value%2A> Propriedade do <xref:System.Runtime.Serialization.EnumMemberAttribute> atributo.  
  
 Por exemplo, o contrato de dados a seguir também é equivalente ao contrato de dados do `CarConditionEnum` .  
  
 [!code-csharp[c_DataContractEnumerations#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#3)]
 [!code-vb[c_DataContractEnumerations#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#3)]  
  
 Quando serializado, o valor de `PreviouslyOwned` tem a representação XML `<condition>Used</condition>` .  
  
## <a name="simple-enumerations"></a>Enumerações simples  
 Você também pode serializar tipos de enumeração aos quais o <xref:System.Runtime.Serialization.DataContractAttribute> atributo não foi aplicado. Esses tipos de enumeração são tratados exatamente como descrito anteriormente, exceto pelo fato de que cada membro (que não tem o <xref:System.NonSerializedAttribute> atributo aplicado) é tratado como se o <xref:System.Runtime.Serialization.EnumMemberAttribute> atributo tenha sido aplicado. Por exemplo, a seguinte enumeração tem implicitamente um contrato de dados equivalente ao `CarConditionEnum` exemplo anterior.  
  
 [!code-csharp[c_DataContractEnumerations#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#6)]
 [!code-vb[c_DataContractEnumerations#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#6)]  
  
 Você pode usar enumerações simples quando não precisar personalizar o nome do contrato de dados da enumeração e o namespace e os valores de membro da enumeração.  
  
#### <a name="notes-on-simple-enumerations"></a>Observações sobre enumerações simples  
 A aplicação do <xref:System.Runtime.Serialization.EnumMemberAttribute> atributo a enumerações simples não tem nenhum efeito.  
  
 Não faz diferença se o <xref:System.SerializableAttribute> atributo é aplicado ou não à enumeração.  
  
 O fato de que a <xref:System.Runtime.Serialization.DataContractSerializer> classe honra o <xref:System.NonSerializedAttribute> atributo aplicado aos membros da enumeração é diferente do comportamento do <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> e do <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter> . Ambos os serializadores ignoram o <xref:System.NonSerializedAttribute> atributo.  
  
## <a name="flag-enumerations"></a>Enumerações de sinalizador  
 Você pode aplicar o <xref:System.FlagsAttribute> atributo a enumerações. Nesse caso, uma lista de zero ou mais valores de enumeração pode ser enviada ou recebida simultaneamente.  
  
 Para fazer isso, aplique o <xref:System.Runtime.Serialization.DataContractAttribute> atributo à enumeração de sinalizador e marque todos os membros que são potências de dois com o <xref:System.Runtime.Serialization.EnumMemberAttribute> atributo. Observe que, para usar uma enumeração de sinalizador, a progressão deve ser uma sequência ininterrupta de potências de 2 (por exemplo, 1, 2, 4, 8, 16, 32, 64).  
  
 As etapas a seguir se aplicam ao envio do valor de enumeração de um sinalizador:  
  
1. Tentativa de localizar um membro de enumeração (com o <xref:System.Runtime.Serialization.EnumMemberAttribute> atributo aplicado) que é mapeado para o valor numérico. Se for encontrado, envie uma lista que contenha apenas esse membro.  
  
2. Tentativa de quebrar o valor numérico em uma soma de modo que haja membros de enumeração (cada um com o <xref:System.Runtime.Serialization.EnumMemberAttribute> atributo aplicado) que é mapeado para cada parte da soma. Envie a lista de todos esses membros. Observe que o *algoritmo de ávido* é usado para encontrar essa soma e, portanto, não há nenhuma garantia de que essa soma seja encontrada, mesmo que esteja presente. Para evitar esse problema, certifique-se de que os valores numéricos dos membros da enumeração sejam potências de dois.  
  
3. Se as duas etapas anteriores falharem e o valor numérico for diferente de zero, lance a <xref:System.Runtime.Serialization.SerializationException> . Se o valor numérico for zero, envie a lista vazia.  
  
### <a name="example"></a>Exemplo  
 O exemplo de enumeração a seguir pode ser usado em uma operação de sinalizador.  
  
 [!code-csharp[c_DataContractEnumerations#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#4)]
 [!code-vb[c_DataContractEnumerations#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#4)]  
  
 Os valores de exemplo a seguir são serializados conforme indicado.  
  
 [!code-csharp[c_DataContractEnumerations#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractenumerations/cs/source.cs#5)]
 [!code-vb[c_DataContractEnumerations#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractenumerations/vb/source.vb#5)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- [Usando contratos de dados](using-data-contracts.md)
- [Especificando transferência de dados em contratos de serviço](specifying-data-transfer-in-service-contracts.md)
