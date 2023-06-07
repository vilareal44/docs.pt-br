---
title: Como implementar explicitamente membros de interface – guia de programação C#
description: Saiba como implementar explicitamente membros de interface neste exemplo de C#. Os membros são acessados por meio da instância de interface.
ms.date: 07/20/2015
helpviewer_keywords:
- interfaces [C#], explicitly implementing
ms.assetid: 514cde76-f981-474e-8b40-9493619f899c
ms.openlocfilehash: 35b512ff6cbee1dd942f5b3476db660481808297
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303069"
---
# <a name="how-to-explicitly-implement-interface-members-c-programming-guide"></a>Como implementar explicitamente membros de interface (guia de programação C#)
Este exemplo declara uma [interface](../../language-reference/keywords/interface.md), `IDimensions` e uma classe, `Box` , que implementa explicitamente os membros da interface `GetLength` e `GetWidth` . Os membros são acessados por meio da instância `dimensions` da interface.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[csProgGuideInheritance#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#8)]  
  
## <a name="robust-programming"></a>Programação robusta  
  
- Observe que as seguintes linhas, no método `Main`, foram comentadas, pois produziriam erros de compilação. Um membro de interface implementado explicitamente não pode ser acessado de uma instância de [classe](../../language-reference/keywords/class.md):  
  
     [!code-csharp[csProgGuideInheritance#45](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#45)]  
  
- Observe também que as linhas a seguir, no método `Main`, imprimem com êxito as dimensões da caixa, pois os métodos estão sendo chamados de uma instância da interface:  
  
     [!code-csharp[csProgGuideInheritance#46](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#46)]  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Classes e structs](../classes-and-structs/index.md)
- [Interfaces](./index.md)
- [Como implementar membros de duas interfaces explicitamente](./how-to-explicitly-implement-members-of-two-interfaces.md)
