---
title: Delegados genéricos – Guia de Programação em C#
description: Saiba mais sobre como usar delegados genéricos em C#. Consulte exemplos de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], delegates
- delegates [C#], generic
ms.assetid: bdea509c-44c1-4309-aaa9-15c7aee009df
ms.openlocfilehash: d99271ca9f12e95743d633caac16aaa4151e9c41
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301899"
---
# <a name="generic-delegates-c-programming-guide"></a>Delegados genéricos (Guia de Programação em C#)
Um [delegado](../../language-reference/builtin-types/reference-types.md) pode definir seus próprios parâmetros de tipo. O código que referencia o delegado genérico pode especificar o argumento de tipo para criar um tipo construído fechado, assim como quando uma classe genérica é instanciada ou quando um método genérico é chamado, conforme mostrado no exemplo a seguir:  
  
 [!code-csharp[csProgGuideGenerics#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#36)]  
  
 A versão 2.0 do C# tem um novo recurso chamado conversão de grupo de método, que pode ser aplicada a tipos concretos e de delegado genérico e habilita a gravação da linha anterior com esta sintaxe simplificada:  
  
 [!code-csharp[csProgGuideGenerics#37](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#37)]  
  
 Os delegados definidos em uma classe genérica podem usar os parâmetros de tipo da classe genérica da mesma forma que os métodos da classe.  
  
 [!code-csharp[csProgGuideGenerics#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#38)]  
  
 O código que referencia o delegado deve especificar o argumento de tipo da classe recipiente, da seguinte maneira:  
  
 [!code-csharp[csProgGuideGenerics#39](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#39)]  
  
 Os delegados genéricos são especialmente úteis na definição de eventos com base no padrão de design comum, pois o argumento do remetente pode ser fortemente tipado e não precisa ser convertido de e para <xref:System.Object>.  
  
 [!code-csharp[csProgGuideGenerics#40](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#40)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Collections.Generic>
- [Guia de programação C#](../index.md)
- [Introdução aos genéricos](./index.md)
- [Métodos genéricos](./generic-methods.md)
- [Classes genéricas](./generic-classes.md)
- [Interfaces genéricas](./generic-interfaces.md)
- [Representantes](../delegates/index.md)
- [Genéricos](../../../standard/generics/index.md)
