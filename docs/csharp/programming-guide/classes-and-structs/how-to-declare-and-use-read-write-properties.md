---
title: Como declarar e usar as propriedades de leitura/gravação – guia de programação em C#
description: Saiba como usar as propriedades de leitura/gravação em C#. Este exemplo inclui duas propriedades, cada uma das quais tem acessadores get e Set, para que as propriedades sejam de leitura/gravação.
ms.date: 07/20/2015
helpviewer_keywords:
- get accessor [C#], declaring properties
- set accessor [C#]
- properties [C#], declaring
- read/write properties [C#]
- accessors [C#], declaring properties with
ms.assetid: a4962fef-af7e-4c4b-a929-4ae4d646ab8a
ms.openlocfilehash: 08bdaa9446491d473cfb16e3b82bac41d7af5b79
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864443"
---
# <a name="how-to-declare-and-use-read-write-properties-c-programming-guide"></a>Como declarar e usar propriedades de leitura/gravação (guia de programação C#)
As propriedades oferecem a conveniência de membros de dados públicos sem os riscos associados ao acesso sem proteção, sem controle e não verificado aos dados de um objeto. Isso é feito por meio de *acessadores*: métodos especiais que atribuem e recuperam valores do membro de dados subjacente. O acessador [set](../../language-reference/keywords/set.md) habilita a atribuição de membros de dados e o acessador [get](../../language-reference/keywords/get.md) recupera valores do membro de dados.  
  
 Este exemplo mostra uma classe `Person` que tem duas propriedades: `Name` (string) e `Age` (int). Ambas as propriedades fornecem acessadores `get` e `set`, portanto, são consideradas propriedades de leitura/gravação.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[csProgGuideObjects#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#33)]  
  
## <a name="robust-programming"></a>Programação robusta  
 No exemplo anterior, as propriedades `Name` e `Age` são [públicas](../../language-reference/keywords/public.md) e incluem os acessadores `get` e `set`. Isso permite que qualquer objeto leia e grave essas propriedades. No entanto, às vezes é desejável excluir um os acessadores. Omitir o acessador `set`, por exemplo, torna a propriedade somente leitura:  
  
 [!code-csharp[csProgGuideObjects#87](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#87)]  
  
 Como alternativa, é possível expor um acessador publicamente, porém, tornando o outro privado ou protegido. Para obter mais informações, consulte [Acessibilidade do Acessador Assimétrico](./restricting-accessor-accessibility.md).  
  
 Depois de serem declaradas, as propriedades podem ser usadas como campos da classe. Isso permite uma sintaxe muito natural na obtenção e configuração do valor de uma propriedade, conforme as instruções a seguir:  
  
 [!code-csharp[csProgGuideObjects#35](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#35)]  
  
 Observe que em um método de propriedade `set`, uma variável especial `value` está disponível. Essa variável contém o valor que o usuário especificou, por exemplo:  
  
 [!code-csharp[csProgGuideObjects#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#36)]  
  
 Observe a sintaxe normal para incrementar a propriedade `Age` em um objeto `Person`:  
  
 [!code-csharp[csProgGuideObjects#37](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#37)]  
  
 Se métodos `set` e `get` separados fossem usados para modelar propriedades, o código equivalente se pareceria com isto:  
  
```csharp  
person.SetAge(person.GetAge() + 1);
```  
  
 O método `ToString` é substituído neste exemplo:  
  
 [!code-csharp[csProgGuideObjects#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#38)]  
  
 Observe que `ToString` não é usado explicitamente no programa. Ele é invocado por padrão pelas chamadas `WriteLine`.  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Propriedades](./properties.md)
- [Classes e structs](./index.md)
