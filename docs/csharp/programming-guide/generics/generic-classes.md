---
title: Classes genéricas – Guia de Programação em C#
description: Saiba mais sobre classes genéricas usadas em coleções, como listas vinculadas, tabelas de hash, pilhas, filas e árvores.
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, generic classes
- generics [C#], classes
ms.assetid: 27d6f256-cd61-41e3-bc6e-b990a53b0224
ms.openlocfilehash: 308f4328540e1001018942738d931be3d8be53ed
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301912"
---
# <a name="generic-classes-c-programming-guide"></a>Classes genéricas (Guia de Programação em C#)
As classes genéricas encapsulam operações que não são específicas de um determinado tipo de dados. O uso mais comum das classes genéricas é com coleções, como listas vinculadas, tabelas de hash, pilhas, filas, árvores e assim por diante. As operações como adicionar e remover itens da coleção são realizadas basicamente da mesma maneira, independentemente do tipo de dados que estão sendo armazenados.  
  
 Na maioria dos cenários que exigem classes de coleção, a abordagem recomendada é usar as que são fornecidas na biblioteca de classes do .NET. Para obter mais informações sobre o uso dessas classes, consulte [Coleções genéricas no .NET](../../../standard/generics/collections.md).  
  
 Em geral, você cria classes genéricas iniciando com uma classe concreta existente e alterando os tipos para parâmetros de tipo, um por vez, até alcançar o equilíbrio ideal de generalização e usabilidade. Ao criar suas próprias classes genéricas, observe as seguintes considerações importantes:  
  
- Quais tipos generalizar em parâmetros de tipo.  
  
     Como uma regra, quanto mais tipos você puder parametrizar, mais flexível e reutilizável seu código se tornará. No entanto, generalização em excesso poderá criar um código que seja difícil de ser lido ou entendido por outros desenvolvedores.  
  
- Quais restrições, se houver, aplicar aos parâmetros de tipo (consulte [Restrições a parâmetros de tipo](./constraints-on-type-parameters.md)).  
  
     Uma boa regra é aplicar o máximo de restrições, de maneira que ainda seja possível manipular os tipos que você precisa manipular. Por exemplo, se você souber que a classe genérica é destinada a ser usada apenas com tipos de referência, aplique a restrição da classe. Isso impedirá o uso não intencional de sua classe com tipos de valor e permitirá que você use o operador `as` em `T` e verificar se há valores nulos.  
  
- Se deve-se levar em consideração o comportamento genérico em subclasses e classes base.  
  
     Como as classes genéricas podem servir como classes base, as mesmas considerações de design aplicam-se nesse caso, como com as classes não genéricas. Consulte as regras sobre heranças de classes base genéricas mais adiante neste tópico.  
  
- Se implementar uma ou mais interfaces genéricas.  
  
     Por exemplo, se você estiver projetando uma classe que será usada para criar itens em uma coleção com base em classes genéricas, poderá ser necessário implementar uma interface como a <xref:System.IComparable%601>, em que `T` é o tipo de sua classe.  
  
 Para obter um exemplo de uma classe genérica simples, consulte [Introdução aos genéricos](./index.md).  
  
 As regras para parâmetros de tipo e restrições têm várias implicações para o comportamento de classes genéricas, especialmente em relação à acessibilidade de membro e herança. Antes de prosseguir, você deve compreender alguns termos. Para uma classe genérica `Node<T>,`, o código cliente pode fazer referência à classe, especificando um argumento de tipo para criar um tipo construído fechado (`Node<int>`). Como alternativa, ele pode deixar o parâmetro de tipo não especificado para criar um tipo construído aberto (`Node<T>`), como ao especificar uma classe base genérica. As classes genéricas podem herdar de classes base construídas concretas, fechadas ou abertas:  
  
 [!code-csharp[csProgGuideGenerics#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#16)]  
  
 As classes não genéricas ou em outras palavras, classes concretas, podem herdar de classes base construídas fechadas, mas não de classes construídas abertas ou de parâmetros de tipo, porque não há maneiras de o código cliente fornecer o argumento de tipo necessário para instanciar a classe base em tempo de execução.  
  
 [!code-csharp[csProgGuideGenerics#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#17)]  
  
 As classes genéricas que herdam de tipos construídos abertos devem fornecer argumentos de tipo para qualquer parâmetro de tipo de classe base que não é compartilhado pela classe herdeira, conforme demonstrado no código a seguir:  
  
 [!code-csharp[csProgGuideGenerics#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#18)]  
  
 As classes genéricas que herdam de tipos construídos abertos devem especificar restrições que são um superconjunto ou sugerem, as restrições no tipo base:  
  
 [!code-csharp[csProgGuideGenerics#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#19)]  
  
 Os tipos genéricos podem usar vários parâmetros de tipo e restrições, da seguinte maneira:  
  
 [!code-csharp[csProgGuideGenerics#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#20)]  
  
 Tipos construídos abertos e construídos fechados podem ser usados como parâmetros de método:  
  
 [!code-csharp[csProgGuideGenerics#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#21)]  
  
 Se uma classe genérica implementa uma interface, todas as instâncias dessa classe podem ser convertidas nessa interface.  
  
 As classes genéricas são invariáveis. Em outras palavras, se um parâmetro de entrada especifica um `List<BaseClass>`, você receberá um erro em tempo de compilação se tentar fornecer um `List<DerivedClass>`.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Collections.Generic>
- [Guia de programação C#](../index.md)
- [Genéricos](./index.md)
- [Salvar o estado de enumeradores](https://docs.microsoft.com/archive/blogs/wesdyer/saving-the-state-of-enumerators)
- [Um enigma de herança, parte 1](https://docs.microsoft.com/archive/blogs/ericlippert/an-inheritance-puzzle-part-one)
