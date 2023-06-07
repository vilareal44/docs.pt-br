---
title: Nomes de contrato de dados
description: Descubra as regras de nomenclatura de membro e de contrato de dados e o comportamento padrão da infraestrutura do WCF, que oferece suporte à comunicação usando contratos de dados equivalentes.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data contracts [WCF], naming
ms.assetid: 31f87e6c-247b-48f5-8e94-b9e1e33d8d09
ms.openlocfilehash: 85c533d683558520d46f259db0bdb34dcb1214c9
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247397"
---
# <a name="data-contract-names"></a>Nomes de contrato de dados

Às vezes, um cliente e um serviço não compartilham os mesmos tipos. Eles ainda podem passar dados entre si, desde que os contratos de dados sejam equivalentes em ambos os lados. A [equivalência do contrato de dados](data-contract-equivalence.md) é baseada em nomes de membros de dados e de contrato de dados e, portanto, um mecanismo é fornecido para mapear tipos e membros para esses nomes. Este tópico explica as regras para nomear contratos de dados, bem como o comportamento padrão da infraestrutura de Windows Communication Foundation (WCF) ao criar nomes.

## <a name="basic-rules"></a>Regras básicas
As regras básicas sobre a nomenclatura de contratos de dados incluem:

- Um nome de contrato de dados totalmente qualificado consiste em um namespace e um nome.

- Os membros de dados têm apenas nomes, mas nenhum namespace.

- Ao processar contratos de dados, a infraestrutura do WCF diferencia maiúsculas de minúsculas para os namespaces e os nomes de contratos de dados e membros de dados.

## <a name="data-contract-namespaces"></a>Namespaces de contrato de dados
Um namespace de contrato de dados assume a forma de um Uniform Resource Identifier (URI). O URI pode ser absoluto ou relativo. Por padrão, os contratos de dados para um tipo específico recebem um namespace proveniente do namespace Common Language Runtime (CLR) desse tipo.

Por padrão, qualquer namespace CLR específico (no formato *CLR. namespace*) é mapeado para o namespace `http://schemas.datacontract.org/2004/07/Clr.Namespace` . Para substituir esse padrão, aplique o <xref:System.Runtime.Serialization.ContractNamespaceAttribute> atributo a todo o módulo ou assembly. Como alternativa, para controlar o namespace de contrato de dados para cada tipo, defina a <xref:System.Runtime.Serialization.DataContractAttribute.Namespace%2A> Propriedade do <xref:System.Runtime.Serialization.DataContractAttribute> .

> [!NOTE]
> O `http://schemas.microsoft.com/2003/10/Serialization` namespace é reservado e não pode ser usado como um namespace de contrato de dados.

> [!NOTE]
> Não é possível substituir o namespace padrão em tipos de contrato de dados que contenham `delegate` declarações.

## <a name="data-contract-names"></a>Nomes de contrato de dados
O nome padrão de um contrato de dados para um determinado tipo é o nome desse tipo. Para substituir o padrão, defina a <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> propriedade de <xref:System.Runtime.Serialization.DataContractAttribute> como um nome alternativo. Regras especiais para tipos genéricos são descritas em "nomes de contrato de dados para tipos genéricos", posteriormente neste tópico.

## <a name="data-member-names"></a>Nomes de membro de dados
O nome padrão de um membro de dados para um determinado campo ou propriedade é o nome desse campo ou propriedade. Para substituir o padrão, defina a <xref:System.Runtime.Serialization.DataMemberAttribute.Name%2A> propriedade de <xref:System.Runtime.Serialization.DataMemberAttribute> como um valor alternativo.

### <a name="examples"></a>Exemplos
O exemplo a seguir mostra como você pode substituir o comportamento de nomenclatura padrão de contratos de dados e membros de dados.

[!code-csharp[C_DataContractNames#1](~/samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#1)]
[!code-vb[C_DataContractNames#1](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#1)]

## <a name="data-contract-names-for-generic-types"></a>Nomes de contrato de dados para tipos genéricos
Existem regras especiais para determinar nomes de contratos de dados para tipos genéricos. Essas regras ajudam a evitar colisões de nome de contrato de dados entre dois genéricos fechados do mesmo tipo genérico.

Por padrão, o nome do contrato de dados para um tipo genérico é o nome do tipo, seguido pela cadeia de caracteres "of", seguido pelos nomes dos contratos de dados dos parâmetros genéricos, seguido por um *hash* calculado usando os namespaces de contrato de dados dos parâmetros genéricos. Um hash é o resultado de uma função matemática que atua como uma "impressão digital" que identifica exclusivamente uma parte dos dados. Quando todos os parâmetros genéricos são tipos primitivos, o hash é omitido.

Por exemplo, consulte os tipos no exemplo a seguir.

[!code-csharp[C_DataContractNames#2](~/samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#2)]
[!code-vb[C_DataContractNames#2](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#2)]

Neste exemplo, o tipo `Drawing<Square,RegularRedBrush>` tem o nome do contrato de dados "DrawingOfSquareRedBrush5HWGAU6h", onde "5HWGAU6h" é um hash dos namespaces "urn: Shapes" e "urn: default". O tipo `Drawing<Square,SpecialRedBrush>` tem o nome do contrato de dados "DrawingOfSquareRedBrushjpB5LgQ_S", onde "jpB5LgQ_S" é um hash dos namespaces "urn: Shapes" e "urn: Special". Observe que, se o hash não for usado, os dois nomes serão idênticos e, portanto, ocorrerá uma colisão de nome.

## <a name="customizing-data-contract-names-for-generic-types"></a>Personalizando nomes de contrato de dados para tipos genéricos

Às vezes, os nomes de contrato de dados gerados para tipos genéricos, conforme descrito anteriormente, são inaceitáveis. Por exemplo, você pode saber com antecedência que não irá executar colisões de nome e talvez queira remover o hash. Nesse caso, você pode usar a <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A?displayProperty=nameWithType> propriedade para especificar uma maneira diferente de gerar nomes. Você pode usar números entre chaves dentro da `Name` propriedade para se referir aos nomes de contrato de dados dos parâmetros genéricos. (0 refere-se ao primeiro parâmetro, 1 refere-se ao segundo e assim por diante.) Você pode usar um sinal de número (#) dentro de chaves para fazer referência ao hash. Você pode usar cada uma dessas referências várias vezes ou não.

Por exemplo, o tipo genérico anterior `Drawing` poderia ter sido declarado como mostrado no exemplo a seguir.

[!code-csharp[c_DataContractNames#3](~/samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#3)]
[!code-vb[c_DataContractNames#3](~/samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#3)]

Nesse caso, o tipo `Drawing<Square,RegularRedBrush>` tem o nome do contrato de dados "Drawing_using_RedBrush_brush_and_Square_shape". Observe que, como há um "{#}" na <xref:System.Runtime.Serialization.DataContractAttribute.Name%2A> propriedade, o hash não é uma parte do nome e, portanto, o tipo é suscetível a colisões de nomenclatura; por exemplo, o tipo `Drawing<Square,SpecialRedBrush>` teria exatamente o mesmo nome de contrato de dados.

## <a name="see-also"></a>Veja também

- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- <xref:System.Runtime.Serialization.ContractNamespaceAttribute>
- [Usando contratos de dados](using-data-contracts.md)
- [Equivalência de contrato de dados](data-contract-equivalence.md)
- [Nomes de contrato de dados](data-contract-names.md)
- [Controle de versão de contrato de dados](data-contract-versioning.md)
