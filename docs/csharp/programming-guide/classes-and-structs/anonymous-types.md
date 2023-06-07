---
title: Tipos anônimos – Guia de Programação em C#
description: Tipos anônimos em C# encapsulam um conjunto de propriedades somente leitura em um objeto sem precisar definir explicitamente um tipo. O compilador gera um nome.
ms.date: 07/20/2015
helpviewer_keywords:
- anonymous types [C#]
- C# Language, anonymous types
ms.assetid: 59c9d7a4-3b0e-475e-b620-0ab86c088e9b
ms.openlocfilehash: ccdee1a86ab0c292721e69346cb7cedb03d4c28b
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474495"
---
# <a name="anonymous-types-c-programming-guide"></a>Tipos anônimos (Guia de Programação em C#)

Os tipos anônimos fornecem um modo conveniente de encapsular um conjunto de propriedades somente leitura em um único objeto sem a necessidade de primeiro definir explicitamente um tipo. O nome do tipo é gerado pelo compilador e não está disponível no nível do código-fonte. O tipo de cada propriedade é inferido pelo compilador.  
  
 Crie tipos anônimos, usando o operador [new](../../language-reference/operators/new-operator.md) com um inicializador de objeto. Para obter mais informações sobre inicializadores de objeto, consulte [Inicializadores de Objeto e Coleção](./object-and-collection-initializers.md).  
  
 O exemplo a seguir mostra um tipo anônimo que é inicializado com duas propriedades chamadas `Amount` e `Message`.  
  
```csharp  
var v = new { Amount = 108, Message = "Hello" };  
  
// Rest the mouse pointer over v.Amount and v.Message in the following  
// statement to verify that their inferred types are int and string.  
Console.WriteLine(v.Amount + v.Message);  
```  
  
 Os tipos anônimos são normalmente usados na cláusula [select](../../language-reference/keywords/select-clause.md) de uma expressão de consulta para retornar um subconjunto das propriedades de cada objeto na sequência de origem. Para obter mais informações sobre consultas, consulte [LINQ em C#](../../linq/index.md).  
  
 Os tipos anônimos contêm uma ou mais propriedades públicas somente leitura. Nenhum outro tipo de membros da classe, como métodos ou eventos, é válido. A expressão que é usada para inicializar uma propriedade não pode ser `null`, uma função anônima ou um tipo de ponteiro.  
  
 O cenário mais comum é inicializar um tipo anônimo com propriedades de outro tipo. No exemplo a seguir, suponha que existe uma classe com o nome `Product`. A classe `Product` inclui as propriedades `Color` e `Price`, além de outras propriedades que não lhe interessam. A variável `products` é uma coleção de objetos do `Product`. A declaração do tipo anônimo começa com a palavra-chave `new`. A declaração inicializa um novo tipo que usa apenas duas propriedades de `Product`. Isso faz com que uma menor quantidade de dados seja retornada na consulta.  
  
 Quando você não especifica os nomes de membros no tipo anônimo, o compilador dá aos membros de tipo anônimo o mesmo nome da propriedade que está sendo usada para inicializá-los. Forneça um nome para a propriedade que está sendo inicializada com uma expressão, como mostrado no exemplo anterior. No exemplo a seguir, os nomes das propriedades do tipo anônimo são `Color` e `Price` .  
  
 [!code-csharp[csRef30Features#81](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csRef30Features/CS/csref30.cs#81)]  
  
 Normalmente, ao usar um tipo anônimo para inicializar uma variável, a variável é declarada como uma variável local de tipo implícito usando [var](../../language-reference/keywords/var.md). O nome do tipo não pode ser especificado na declaração da variável, porque apenas o compilador tem acesso ao nome subjacente do tipo anônimo. Para obter mais informações sobre `var`, consulte [Variáveis de local digitadas implicitamente](./implicitly-typed-local-variables.md).  
  
 Você pode criar uma matriz de elementos de tipo anônimo combinando uma variável local de tipo implícito e uma matriz de tipo implícito, como mostrado no exemplo a seguir.  
  
```csharp  
var anonArray = new[] { new { name = "apple", diam = 4 }, new { name = "grape", diam = 1 }};  
```  
  
## <a name="remarks"></a>Comentários  
 Os tipos anônimos são tipos [class](../../language-reference/keywords/class.md) que derivam diretamente de [object](../../language-reference/builtin-types/reference-types.md) e que não podem ser convertidos para qualquer tipo, exceto [object](../../language-reference/builtin-types/reference-types.md). O compilador fornece um nome para cada tipo anônimo, embora o seu aplicativo não possa acessá-lo. Do ponto de vista do Common Language Runtime, um tipo anônimo não é diferente de qualquer outro tipo de referência.  
  
 Se dois ou mais inicializadores de objeto anônimos em um assembly especificarem uma sequência de propriedades que estão na mesma ordem e que têm os mesmos nomes e tipos, o compilador tratará os objetos como instâncias do mesmo tipo. Eles compartilham o mesmo tipo de informação gerado pelo compilador.  
  
 Você não pode declarar que um campo, uma propriedade, um evento ou um tipo de retorno de um método tem um tipo anônimo. Da mesma forma, não pode declarar que um parâmetro formal de um método, propriedade, construtor ou indexador tem um tipo anônimo. Para passar um tipo anônimo ou uma coleção que contenha tipos anônimos, como um argumento para um método, você pode declarar o parâmetro como objeto type. No entanto, isso anula a finalidade dos tipos fortes. Se você precisa armazenar os resultados da consulta ou passá-los fora do limite do método, considere o uso de uma estrutura ou classe com denominação comum em vez de um tipo anônimo.  
  
 Como os métodos <xref:System.Object.Equals%2A> e <xref:System.Object.GetHashCode%2A> em tipos anônimos são definidos em termos dos métodos das propriedades `Equals` e `GetHashCode`, duas instâncias do mesmo tipo anônimo são iguais somente se todas as suas propriedades forem iguais.  
  
## <a name="see-also"></a>Consulte também

- [Guia de programação C#](../index.md)
- [Inicializadores de objeto e coleção](./object-and-collection-initializers.md)
- [Introdução a LINQ em C#](../concepts/linq/index.md)
- [LINQ em C#](../../linq/index.md)
