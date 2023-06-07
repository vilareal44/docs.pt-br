---
title: Construtores particulares – Guia de Programação em C#
description: Um Construtor privado é um construtor de instância especial em C# usado para restringir como um objeto pode ser criado. Eles podem ser usados com métodos de fábrica ou outros idiomas de construção.
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, private constructors
- private constructors [C#]
ms.assetid: 29eeaa7d-8d81-453c-94b9-0e2800172621
ms.openlocfilehash: a6b86ccb870da0262bcbc516e176e00d17724f9f
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864053"
---
# <a name="private-constructors-c-programming-guide"></a>Construtores particulares (Guia de Programação em C#)
Um construtor particular é um construtor de instância especial. Normalmente, ele é usado em classes que contêm apenas membros estáticos. Se uma classe tiver um ou mais construtores particulares e nenhum construtor público, outras classes (exceto as classes aninhadas) não poderão criar instâncias dessa classe. Por exemplo:  
  
 [!code-csharp[csProgGuideObjects#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#11)]  
  
 A declaração do construtor vazio impede a geração automática de um construtor sem parâmetro. Observe que, se você não usar um modificador de acesso com o construtor, ele ainda será privado por padrão. No entanto, o modificador [private](../../language-reference/keywords/private.md) geralmente é usado explicitamente para deixar claro que a classe não pode ser instanciada.  
  
 Construtores particulares são usados para impedir a criação de instâncias de uma classe quando não há métodos ou campos de instância, como a classe <xref:System.Math> ou quando um método é chamado para obter uma instância de uma classe. Se todos os métodos na classe forem estáticos, considere deixar toda a classe estática. Para obter mais informações [, consulte classes estáticas e membros de classe estática](./static-classes-and-static-class-members.md).  
  
## <a name="example"></a>Exemplo  
 A seguir, temos um exemplo de uma classe usando um construtor particular.  
  
 [!code-csharp[csProgGuideObjects#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#12)]  
  
 Observe que se você remover a marca de comentário da seguinte instrução do exemplo, ela gerará um erro porque o construtor está inacessível devido a seu nível de proteção:  
  
 [!code-csharp[csProgGuideObjects#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#13)]  
  
## <a name="c-language-specification"></a>Especificação da Linguagem C#  

Para obter mais informações, veja [Construtores privados](~/_csharplang/spec/classes.md#private-constructors) na [Especificação da Linguagem C#](/dotnet/csharp/language-reference/language-specification/introduction). A especificação da linguagem é a fonte definitiva para a sintaxe e o uso de C#.
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Classes e structs](./index.md)
- [Construtores](./constructors.md)
- [Finalizadores](./destructors.md)
- [pessoal](../../language-reference/keywords/private.md)
- [público](../../language-reference/keywords/public.md)
