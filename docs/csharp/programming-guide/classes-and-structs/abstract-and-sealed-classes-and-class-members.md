---
title: Classes e membros de classes abstract e sealed – Guia de Programação em C#
description: A palavra-chave Abstract em C# cria classes incompletas e membros de classe. A palavra-chave sealed impede a herança de classes virtuais ou membros de classe anteriormente.
ms.date: 07/20/2015
helpviewer_keywords:
- abstract classes [C#]
- sealed classes [C#]
- C# language, abstract classes
- C# language, sealed
ms.assetid: 99aa52f7-b435-43f9-936e-2470af734c4e
ms.openlocfilehash: 391a8ccbb1fbe6626d1cd5a4b6fcfd9ace3506e6
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474482"
---
# <a name="abstract-and-sealed-classes-and-class-members-c-programming-guide"></a>Classes e membros de classes abstract e sealed (Guia de Programação em C#)
A palavra-chave [abstract](../../language-reference/keywords/abstract.md) permite que você crie classes e membros de [classe](../../language-reference/keywords/class.md) que estão incompletos e devem ser implementados em uma classe derivada.  
  
 A palavra-chave [sealed](../../language-reference/keywords/sealed.md) permite evitar a herança de uma classe ou de determinados membros de classe que foram marcados anteriormente com [virtual](../../language-reference/keywords/virtual.md).  
  
## <a name="abstract-classes-and-class-members"></a>Classes abstratas e membros de classe  
 As classes podem ser declaradas como abstratas, colocando a palavra-chave `abstract` antes da definição de classe. Por exemplo:  
  
 [!code-csharp[csProgGuideInheritance#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#13)]  
  
 Uma classe abstrata não pode ser instanciada. A finalidade de uma classe abstrata é fornecer uma definição comum de uma classe base que pode ser compartilhada por várias classes derivadas. Por exemplo, uma biblioteca de classes pode definir uma classe abstrata que serve como um parâmetro para muitas de suas funções e exige que os programadores que usam essa biblioteca forneçam sua própria implementação da classe, criando uma classe derivada.  
  
 As classes abstratas também podem definir métodos abstratos. Isso é realizado através da adição da palavra-chave `abstract` antes do tipo de retorno do método. Por exemplo:  
  
 [!code-csharp[csProgGuideInheritance#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#14)]  
  
 Os métodos abstratos não têm implementação, portanto, a definição do método é seguida por um ponto e vírgula, em vez de um bloco de método normal. As classes derivadas da classe abstrata devem implementar todos os métodos abstratos. Quando uma classe abstrata herda um método virtual de uma classe base, a classe abstrata pode substituir o método virtual por um método abstrato. Por exemplo:  
  
 [!code-csharp[csProgGuideInheritance#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#15)]  
  
 Se um método `virtual` for declarado `abstract`, ele ainda será virtual para qualquer classe que herdar da classe abstrata. Uma classe que herda um método abstrato não pode acessar a implementação original do método. No exemplo anterior, `DoWork` na classe F não pode chamar `DoWork` na classe D. Dessa forma, uma classe abstrata pode forçar classes derivadas a fornecerem novas implementações de método para métodos virtuais.  
  
## <a name="sealed-classes-and-class-members"></a>Classes e membros de classes sealed  
 As classes podem ser declaradas como [sealed](../../language-reference/keywords/sealed.md), colocando a palavra-chave `sealed` antes da definição de classe. Por exemplo:  
  
 [!code-csharp[csProgGuideInheritance#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#16)]  
  
 Uma classe sealed não pode ser usada como uma classe base. Por esse motivo, também não pode ser uma classe abstrata. As classes sealed impedem a derivação. Como elas nunca podem ser usadas como uma classe base, algumas otimizações em tempo de execução podem tornar a chamada a membros de classe sealed ligeiramente mais rápida.  
  
 Um método, um indexador, uma propriedade ou um evento em uma classe derivada que está substituindo um membro virtual da classe base, pode declarar esse membro como sealed. Isso anula o aspecto virtual do membro para qualquer outra classe derivada. Isso é realizado através da colocação da palavra-chave `sealed` antes da palavra-chave [override](../../language-reference/keywords/override.md) na declaração de membro de classe. Por exemplo:  
  
 [!code-csharp[csProgGuideInheritance#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#17)]  
  
## <a name="see-also"></a>Confira também

- [Guia de programação C#](../index.md)
- [Classes e structs](./index.md)
- [Herança](./inheritance.md)
- [Métodos](./methods.md)
- [Fields](./fields.md)
- [Como definir propriedades abstract](./how-to-define-abstract-properties.md)
