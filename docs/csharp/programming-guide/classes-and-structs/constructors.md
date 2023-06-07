---
title: Construtores – Guia de Programação em C#
description: Um construtor em C# é chamado quando uma classe ou struct é criada. Use construtores para definir padrões, limitar a instanciação e escrever código flexível e de fácil leitura.
ms.date: 05/05/2017
helpviewer_keywords:
- constructors [C#]
- classes [C#], constructors
- C# language, constructors
ms.assetid: df2e2e9d-7998-418b-8e7d-890c17ff6c95
ms.openlocfilehash: 4a731e648143f5e0ecf8860625962d8baa29fe26
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474898"
---
# <a name="constructors-c-programming-guide"></a>Construtores (Guia de Programação em C#)

Sempre que uma [classe](../../language-reference/keywords/class.md) ou [struct](../../language-reference/builtin-types/struct.md) é criada, o construtor é chamado. Uma classe ou struct pode ter vários construtores que usam argumentos diferentes. Os construtores permitem que o programador defina valores padrão, limite a instanciação e grave códigos flexíveis e fáceis de ler. Para obter mais informações e exemplos, consulte [Usando Construtores](./using-constructors.md) e [Construtores de Instância](./instance-constructors.md).  

## <a name="parameterless-constructors"></a>Construtores sem parâmetros
  
Se você não fornecer um construtor para sua classe, o C# criará um por padrão que instancia o objeto e define as variáveis de membro para os valores padrão, conforme listado no artigo [valores padrão de tipos C#](../../language-reference/builtin-types/default-values.md) . Se você não fornecer um construtor para sua estrutura, o C# se baseará em um *Construtor implícito sem parâmetros* para inicializar automaticamente cada campo com seu valor padrão. Para obter mais informações e exemplos, consulte [construtores de instância](instance-constructors.md).  

## <a name="constructor-syntax"></a>Sintaxe do construtor

Um construtor é um método cujo nome é igual ao nome de seu tipo. Sua assinatura do método inclui apenas o nome do método e lista de parâmetros, ele não inclui um tipo de retorno. O exemplo a seguir mostra o construtor para uma classe denominada `Person`.

[!code-csharp[constructors](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/constructors1.cs#1)]  

Se um construtor puder ser implementado como uma única instrução, você poderá usar uma [definição de corpo da expressão](../statements-expressions-operators/expression-bodied-members.md). O exemplo a seguir define uma classe `Location` cujo construtor tem um único parâmetro de cadeia de caracteres chamado *nome*. A definição de corpo da expressão atribui o argumento ao campo `locationName`.

[!code-csharp[expression-bodied-constructor](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/expr-bodied-ctor.cs#1)]  

## <a name="static-constructors"></a>Construtores estáticos

Os exemplos anteriores têm todos os construtores de instância mostrado, que criam um novo objeto. Uma classe ou struct também pode ter um construtor estático, que inicializa membros estáticos do tipo.  Construtores estáticos não têm parâmetros. Se você não fornecer um construtor estático para inicializar campos estáticos, o compilador C# Inicializa campos estáticos para seu valor padrão, conforme listado no artigo [valores padrão de tipos C#](../../language-reference/builtin-types/default-values.md) .

O exemplo a seguir usa um construtor estático para inicializar um campo estático.

[!code-csharp[constructors](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/constructors1.cs#2)]  

Você também pode definir um construtor estático com uma definição de corpo da expressão, como mostra o exemplo a seguir.

[!code-csharp[constructors](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/constructors1.cs#3)]  

Para obter mais informações e exemplos, consulte [Construtores Estáticos](./static-constructors.md).  
  
## <a name="in-this-section"></a>Nesta seção  
 [Usando construtores](./using-constructors.md)  
  
 [Construtores de instância](./instance-constructors.md)  
  
 [Construtores particulares](./private-constructors.md)  
  
 [Construtores estáticos](./static-constructors.md)  
  
 [Como escrever um construtor de cópia](./how-to-write-a-copy-constructor.md)  
  
## <a name="see-also"></a>Consulte também

- [Guia de programação C#](../index.md)
- [Classes e structs](./index.md)
- [Finalizadores](./destructors.md)
- [static](../../language-reference/keywords/static.md)
- [Por que inicializadores são executados na ordem oposta como construtores? Parte um](https://docs.microsoft.com/archive/blogs/ericlippert/why-do-initializers-run-in-the-opposite-order-as-constructors-part-one)
