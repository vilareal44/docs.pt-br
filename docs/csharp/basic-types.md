---
title: Tipos básicos – Guia de C#
description: Saiba mais sobre os tipos principais (numéricos, cadeias de caracteres e objetos) em todos os programas em C#
ms.date: 10/10/2016
ms.technology: csharp-fundamentals
ms.assetid: 95c686ba-ae4f-440e-8e94-0dbd6e04d11f
ms.openlocfilehash: 93a0023969bb8bb089922a9e30fbf599eddc7203
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174173"
---
# <a name="types-variables-and-values"></a>Tipos, variáveis e valores

O C# é uma linguagem fortemente tipada. Todas as variáveis e constantes têm um tipo, assim como cada expressão que é avaliada como um valor. Cada assinatura de método especifica um tipo para cada parâmetro de entrada e para o valor retornado. A biblioteca de classes .NET Framework define um conjunto de tipos numéricos internos, bem como tipos mais complexos que representam uma ampla variedade de constructos lógicos, como o sistema de arquivos, as conexões de rede, as coleções e as matrizes de objetos e as datas. Um programa em C# típico usa tipos da biblioteca de classes, bem como tipos definidos pelo usuário que modelam os conceitos que são específicos para o domínio do problema do programa.  
  
As informações armazenadas em um tipo podem incluir o seguinte:  
  
- O espaço de armazenamento que uma variável do tipo requer.  
  
- Os valores mínimo e máximo que ele pode representar.  
  
- Os membros (métodos, campos, eventos e etc.) que ele contém.  
  
- O tipo base do qual ele herda.  
  
- O local no qual a memória para as variáveis será alocada em tempo de execução.  
  
- Os tipos de operações que são permitidos.  
  
O compilador usa as informações de tipo para garantir que todas as operações que são realizadas em seu código sejam *fortemente tipadas*. Por exemplo, se você declarar uma variável do tipo [int](language-reference/builtin-types/integral-numeric-types.md), o compilador permitirá que você use a variável nas operações de adição e subtração. Se você tentar executar as mesmas operações em uma variável do tipo [bool](language-reference/builtin-types/bool.md), o compilador gerará um erro, como mostrado no exemplo a seguir:  
  
[!code-csharp[Type Safety](../../samples/snippets/csharp/concepts/basic-types/type-safety.cs)]  
  
> [!NOTE]  
> Desenvolvedores de C e C++, observem que, em C#, [bool](language-reference/builtin-types/bool.md) não é conversível em [int](language-reference/builtin-types/integral-numeric-types.md).  
  
O compilador insere as informações de tipo no arquivo executável como metadados. O CLR (Common Language Runtime) usa esses metadados em tempo de execução para assegurar mais segurança de tipos ao alocar e recuperar a memória.  

## <a name="specifying-types-in-variable-declarations"></a>Especificando tipos em declarações de variável

Quando declara uma variável ou constante em um programa, você deve especificar o tipo da variável ou usar a palavra-chave [var](language-reference/keywords/var.md) para permitir que o compilador infira o tipo. O exemplo a seguir mostra algumas declarações de variáveis que usam tipos numéricos internos e tipos complexos definidos pelo usuário:  
  
[!code-csharp[Variable Declaration](../../samples/snippets/csharp/concepts/basic-types/variable-declaration.cs)]  
  
Os tipos de parâmetros de método e valores de retornados são especificados na assinatura do método. A assinatura a seguir mostra um método que requer um [int](language-reference/builtin-types/integral-numeric-types.md) como um argumento de entrada e retorna uma cadeia de caracteres:  
  
[!code-csharp[Method Signature](../../samples/snippets/csharp/concepts/basic-types/method-signature.cs)]  
  
Depois que uma variável é declarada, ela não pode ser declarada novamente com um novo tipo e não pode ter um valor atribuído que não seja compatível com seu tipo declarado. Por exemplo, você não pode declarar um [int](language-reference/builtin-types/integral-numeric-types.md) e, em seguida, atribuir a ele um valor booliano de `true` . No entanto, os valores podem ser convertidos em outros tipos, por exemplo, quando são passados como argumentos de método ou atribuídos a novas variáveis. Uma *conversão de tipo* que não causa a perda de dados é executada automaticamente pelo compilador. Uma conversão que pode causar perda de dados requer um *cast* no código-fonte.

Para obter mais informações, consulte [conversões de conversão e tipo](programming-guide/types/casting-and-type-conversions.md).

## <a name="built-in-types"></a>Tipos internos

O C# fornece um conjunto padrão de tipos numéricos internos para representar números inteiros, valores de ponto flutuante, expressões boolianas, caracteres de texto, valores decimais e outros tipos de dados. Também há tipos internos de **cadeias de caracteres** de **objetos**. Eles estão disponíveis para uso em qualquer programa em C#. Para obter a lista completa dos tipos internos, consulte [tipos internos](language-reference/builtin-types/built-in-types.md).
  
## <a name="custom-types"></a>Tipos personalizados

Você usa os constructos [struct](language-reference/builtin-types/struct.md), [classe](language-reference/keywords/class.md), [interface](language-reference/keywords/interface.md) e [enum](language-reference/builtin-types/enum.md) para criar seus próprios tipos personalizados. A biblioteca de classes .NET Framework em si é uma coleção de tipos personalizados fornecidos pela Microsoft que você pode usar em seus próprios aplicativos. Por padrão, os tipos usados com mais frequência na biblioteca de classes estão disponíveis em qualquer programa em C#. Outros ficam disponíveis somente quando você adiciona explicitamente uma referência de projeto ao assembly no qual eles estão definidos. Depois que o compilador tiver uma referência ao assembly, você pode declarar variáveis (e constantes) dos tipos declarados nesse assembly no código-fonte.
  
## <a name="generic-types"></a>Tipos genéricos

Um tipo pode ser declarado com um ou mais *parâmetros de tipo* que servem como um espaço reservado para o tipo real (o *tipo concreto*) que o código cliente fornecerá ao criar uma instância do tipo. Esses tipos são chamados de *tipos genéricos*. Por exemplo, o tipo de .NET Framework <xref:System.Collections.Generic.List%601> tem um parâmetro de tipo que, por convenção, recebe o nome *T*. Ao criar uma instância do tipo, você especifica o tipo dos objetos que a lista conterá, por exemplo, Cadeia de caracteres:  
  
[!code-csharp[Generic types](../../samples/snippets/csharp/concepts/basic-types/generic-type.cs)]
  
O uso do parâmetro de tipo possibilita a reutilização da mesma classe para conter qualquer tipo de elemento sem precisar converter cada elemento em [objeto](language-reference/builtin-types/reference-types.md#the-object-type). As classes de coleção genéricas são chamadas de coleções com rigidez de *tipos* porque o compilador sabe o tipo específico dos elementos da coleção e pode gerar um erro no tempo de compilação se, por exemplo, você tentar adicionar um inteiro ao `strings` objeto no exemplo anterior. Para obter mais informações, consulte [Genéricos](programming-guide/generics/index.md).

## <a name="implicit-types-anonymous-types-and-tuple-types"></a>Tipos implícitos, tipos anônimos e tipos de tupla

Conforme mencionado anteriormente, você pode digitar implicitamente uma variável local (mas não os membros de classe) usando a palavra-chave [var](language-reference/keywords/var.md). A variável ainda recebe um tipo em tempo de compilação, mas o tipo é fornecido pelo compilador. Para obter mais informações, consulte [variáveis de local digitadas implicitamente](programming-guide/classes-and-structs/implicitly-typed-local-variables.md).  
  
Em alguns casos, é inconveniente criar um tipo nomeado para conjuntos simples de valores relacionados que você não pretende armazenar ou transmitir para fora dos limites do método. Você pode criar *tipos anônimos* para essa finalidade. Para obter mais informações, consulte [tipos anônimos](programming-guide/classes-and-structs/anonymous-types.md).

É comum querer retornar mais de um valor de um método. Você pode criar *tipos de tupla* que retornam vários valores em uma única chamada de método. Para obter mais informações, consulte [tipos de tupla](language-reference/builtin-types/value-tuples.md).

## <a name="the-common-type-system"></a>O Common Type System

É importante entender dois pontos fundamentais sobre o sistema de tipo no .NET Framework:  
  
- Ele dá suporte ao conceito de herança. Os tipos podem derivar de outros tipos, chamados *tipos base*. O tipo derivado herda (com algumas restrições) os métodos, as propriedades e outros membros do tipo base. O tipo base, por sua vez, pode derivar de algum outro tipo, nesse caso, o tipo derivado herda os membros de ambos os tipos base na sua hierarquia de herança. Todos os tipos, incluindo tipos numéricos internos, como o <xref:System.Int32> (palavra-chave do C#: `int`), derivam, em última análise, de um único tipo base, que é o <xref:System.Object> (palavra-chave do C#: `object`). Essa hierarquia de tipo unificada é chamada de CTS ( [sistema de tipos comum](../standard/common-type-system.md) ). Para obter mais informações sobre herança em C#, consulte [Herança](programming-guide/classes-and-structs/inheritance.md).  
  
- Cada tipo no CTS é definido como um *tipo de valor* ou um *tipo de referência*. Isso inclui todos os tipos personalizados na biblioteca de classes do .NET, além de tipos personalizados definidos pelo usuário. Os tipos que você define usando a `struct` `enum` palavra-chave ou são tipos de valor. Para obter mais informações sobre tipos de valor, consulte [tipos de valor](language-reference/builtin-types/value-types.md). Os tipos que você define usando a palavra-chave [class](language-reference/keywords/class.md) são tipos de referência. Para obter mais informações sobre tipos de referência, consulte [Classes](programming-guide/classes-and-structs/classes.md). Os tipos de referência e os tipos de valor têm diferentes regras de tempo de compilação e comportamento de tempo de execução diferente.

## <a name="see-also"></a>Veja também

- [Tipos de estrutura](language-reference/builtin-types/struct.md)
- [Tipos de enumeração](language-reference/builtin-types/enum.md)
- [Classes](programming-guide/classes-and-structs/classes.md)
