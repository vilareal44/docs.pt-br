---
title: Como criar um contrato de dados básicos para uma classe ou estrutura
description: Siga este exemplo para aprender a criar um contrato de dados usando uma classe ou estrutura no WCF usando o atributo DataContractAttribute.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataMemberAttribute
- DataContractAttribute class
- data contracts [WCF], creating for a class or structure
ms.assetid: bc464889-3070-4a2f-91d2-e788a0f686a7
ms.openlocfilehash: a45fde58795947c3e46fa45750ae1a3faddd8849
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247163"
---
# <a name="how-to-create-a-basic-data-contract-for-a-class-or-structure"></a>Como criar um contrato de dados básicos para uma classe ou estrutura
Este tópico mostra as etapas básicas para criar um contrato de dados usando uma classe ou estrutura. Para obter mais informações sobre contratos de dados e como eles são usados, consulte [usando contratos de dados](using-data-contracts.md).  
  
 Para obter um tutorial que percorre as etapas de criação de um serviço e cliente do Windows Communication Foundation básico (WCF), consulte o [tutorial de introdução](../getting-started-tutorial.md). Para um aplicativo de exemplo funcional que consiste em um serviço e cliente básicos, consulte [contrato de dados básico](../samples/basic-data-contract.md).  
  
### <a name="to-create-a-basic-data-contract-for-a-class-or-structure"></a>Para criar um contrato de dados básico para uma classe ou estrutura  
  
1. Declare que o tipo tem um contrato de dados aplicando o <xref:System.Runtime.Serialization.DataContractAttribute> atributo à classe. Observe que todos os tipos públicos, incluindo aqueles sem atributos, são serializáveis. O <xref:System.Runtime.Serialization.DataContractSerializer> infere um contrato de dados se o <xref:System.Runtime.Serialization.DataContractAttribute> atributo estiver ausente. Para obter mais informações, consulte [tipos serializáveis](serializable-types.md).  
  
2. Defina os membros (Propriedades, campos ou eventos) que são serializados aplicando o <xref:System.Runtime.Serialization.DataMemberAttribute> atributo a cada membro. Esses membros são chamados de membros de dados. Por padrão, todos os tipos públicos são serializáveis. Para obter mais informações, consulte [tipos serializáveis](serializable-types.md).  
  
    > [!NOTE]
    > Você pode aplicar o <xref:System.Runtime.Serialization.DataMemberAttribute> atributo a campos privados, fazendo com que os dados sejam expostos a outros. Certifique-se de que o membro não contém dados confidenciais.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como criar um contrato de dados para o `Person` tipo aplicando <xref:System.Runtime.Serialization.DataContractAttribute> os <xref:System.Runtime.Serialization.DataMemberAttribute> atributos e à classe e seus membros.  
  
 [!code-csharp[DataContractAttribute#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#2)]
 [!code-vb[DataContractAttribute#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#2)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Runtime.Serialization.DataContractAttribute>
- <xref:System.Runtime.Serialization.DataMemberAttribute>
- [Usando contratos de dados](using-data-contracts.md)
- [Tutorial de Introdução](../getting-started-tutorial.md)
- [Introdução](../samples/getting-started-sample.md)
