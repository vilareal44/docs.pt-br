---
title: Ordem de membro de dados
description: Saiba mais sobre a ordem dos membros de dados no WCF. Os aplicativos podem precisar saber ou alterar a ordem na qual os membros de dados são enviados ou esperados.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data contracts [WCF], ordering members
ms.assetid: 0658a47d-b6e5-4ae0-ba72-ababc3c6ff33
ms.openlocfilehash: 5c192d3bda65a7364345df4310dccd96cbe04056
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247358"
---
# <a name="data-member-order"></a>Ordem de membro de dados
Em alguns aplicativos, é útil saber a ordem na qual os dados dos vários membros de dados são enviados ou devem ser recebidos (como a ordem na qual os dados aparecem no XML serializado). Às vezes, pode ser necessário alterar essa ordem. Este tópico explica as regras de ordenação.  
  
## <a name="basic-rules"></a>Regras básicas  
 As regras básicas para a ordenação de dados incluem:  
  
- Se um tipo de contrato de dados for uma parte de uma hierarquia de herança, os membros de dados de seus tipos base serão sempre primeiro na ordem.  
  
- A seguir, na ordem, estão os membros de dados do tipo atual que não têm a <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A> Propriedade do <xref:System.Runtime.Serialization.DataMemberAttribute> conjunto de atributos, em ordem alfabética.  
  
- A seguir estão todos os membros de dados que têm a <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A> Propriedade do <xref:System.Runtime.Serialization.DataMemberAttribute> conjunto de atributos. Eles são ordenados primeiro pelo valor da `Order` propriedade e, em seguida, em ordem alfabética, se houver mais de um membro de um determinado `Order` valor. Os valores de ordem podem ser ignorados.  
  
 A ordem alfabética é estabelecida chamando o <xref:System.String.CompareOrdinal%2A> método.  
  
## <a name="examples"></a>Exemplos  
 Considere o código a seguir.  
  
 [!code-csharp[C_DataContractNames#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#4)]
 [!code-vb[C_DataContractNames#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#4)]  
  
 O XML produzido é semelhante ao seguinte.  
  
```xml  
<DerivedType>  
    <!-- Zebra is a base data member, and appears first. -->  
    <zebra/>
  
    <!-- Cat has no Order, appears alphabetically first. -->  
    <cat/>  
  
   <!-- Dog has no Order, appears alphabetically last. -->  
    <dog/>
  
    <!-- Bird is the member with the smallest Order value -->  
    <bird/>  
  
    <!-- Albatross has the next Order value, alphabetically first. -->  
    <albatross/>  
  
    <!-- Parrot, with the next Order value, alphabetically last. -->  
     <parrot/>  
  
    <!-- Antelope is the member with the highest Order value. Note that   
    Order=2 is skipped -->  
     <antelope/>
</DerivedType>  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.Serialization.DataContractAttribute>
- [Equivalência de contrato de dados](data-contract-equivalence.md)
- [Usando contratos de dados](using-data-contracts.md)
