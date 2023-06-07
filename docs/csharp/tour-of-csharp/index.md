---
title: Um tour pelo C# – Guia do C#
description: Novato em C#? Conheça os fundamentos da linguagem.
ms.date: 08/06/2020
ms.openlocfilehash: 9fa292e8e85832d831f36cf0f21512aa0cf32580
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656222"
---
# <a name="a-tour-of-the-c-language"></a>Um tour pela linguagem C#

O C# (pronuncia-se "Veja nítido") é uma linguagem de programação moderna, orientada a objeto e de tipo seguro. O C# tem suas raízes na família de linguagens C e os programadores em C, C++, Java e JavaScript a reconhecerão imediatamente. Este tour fornece uma visão geral dos principais componentes do idioma no C# 8 e versões anteriores. Se você quiser explorar a linguagem por meio de exemplos interativos, experimente a [introdução aos](../tutorials/intro-to-csharp/index.md) tutoriais do C#.

O C# é uma linguagem de programação ***orientada*** a objetos e orientada a componentes. O c# fornece construções de linguagem para dar suporte direto a esses conceitos, tornando o C# uma linguagem natural para criar e usar componentes de software. Desde sua origem, o C# adicionou recursos para dar suporte a novas cargas de trabalho e práticas de design de software emergentes.

Vários recursos do C# auxiliam na construção de aplicativos robustos e duráveis. A [***coleta de lixo***](../../standard/garbage-collection/index.md) recupera automaticamente a memória ocupada por objetos não utilizados inacessíveis. A [***manipulação de exceção***](../programming-guide/exceptions/index.md) fornece uma abordagem estruturada e extensível para detecção e recuperação de erros. As [***expressões lambda***](../language-reference/operators/lambda-expressions.md) dão suporte a técnicas de programação funcional. A [***sintaxe de consulta***](../linq/index.md) cria um padrão comum para trabalhar com dados de qualquer fonte. O suporte a idiomas para [***operações assíncronas***](../programming-guide/concepts/async/index.md) fornece a sintaxe para a criação de sistemas distribuídos. A [***correspondência de padrões***](..//pattern-matching.md) fornece sintaxe para separar facilmente os dados de algoritmos em sistemas distribuídos modernos. O C# tem um [***sistema de tipos unificado***](../programming-guide/types/index.md). Todos os tipos do C#, incluindo tipos primitivos, como `int` e `double`, herdam de um único tipo de `object` raiz. Todos os tipos compartilham um conjunto de operações comuns. Os valores de qualquer tipo podem ser armazenados, transportados e operados de maneira consistente. Além disso, o C# dá suporte a tipos de referência definidos pelo usuário e tipos de valor. O C# permite a alocação dinâmica de objetos e o armazenamento em linha de estruturas leves.

O C# enfatiza o ***controle de versão*** para garantir que programas e bibliotecas possam evoluir ao longo do tempo de maneira compatível. Aspectos do design do C# que foram influenciados diretamente pelas considerações de controle de versão incluem os `virtual` `override` modificadores and separados, as regras para resolução de sobrecarga de método e suporte para declarações de membro de interface explícitas.

## <a name="hello-world"></a>Hello world

O programa "Hello, World" é usado tradicionalmente para introduzir uma linguagem de programação. Este é para C#:

:::code language="csharp" interactive="try-dotnet" source="./snippets/shared/HelloWorld.cs":::

O programa "Hello, World" começa com uma diretiva `using` que faz referência ao namespace `System`. Namespaces fornecem um meio hierárquico de organizar bibliotecas e programas em C#. Os namespaces contêm tipos e outros namespaces — por exemplo, o namespace `System` contém uma quantidade de tipos, como a classe `Console` referenciada no programa e diversos outros namespaces, como `IO` e `Collections`. A diretiva `using` que faz referência a um determinado namespace permite o uso não qualificado dos tipos que são membros desse namespace. Devido à diretiva `using`, o programa pode usar `Console.WriteLine` como um atalho para `System.Console.WriteLine`.

A classe `Hello` declarada pelo programa "Hello, World" tem um único membro, o método chamado `Main`. O `Main` método é declarado com o `static` modificador. Embora os métodos de instância possam fazer referência a uma determinada instância de objeto delimitador usando a palavra-chave `this`, métodos estáticos operam sem referência a um objeto específico. Por convenção, um método estático chamado `Main` serve como o ponto de entrada de um programa em C#.

A saída do programa é produzida pelo método `WriteLine` da classe `Console` no namespace `System`. Essa classe é fornecida pelas bibliotecas de classe padrão, que, por padrão, são referenciadas automaticamente pelo compilador.

## <a name="types-and-variables"></a>Tipos e variáveis

Há dois tipos em C#: *tipos de referência* e *tipos de valor*. As variáveis de tipos de valor contêm diretamente seus dados enquanto variáveis de tipos de referência armazenam referências a seus dados, o último sendo conhecido como objetos. Com os tipos de referência, é possível que duas variáveis referenciem o mesmo objeto e possíveis operações em uma variável afetem o objeto referenciado pela outra variável. Com os tipos de valor, as variáveis têm sua própria cópia dos dados, e não é possível que as operações em um afetem a outra (exceto `ref` para `out` variáveis de parâmetro e).

Um ***identificador*** é um nome de variável. Um identificador é uma sequência de caracteres Unicode sem qualquer espaço em branco. Um identificador pode ser uma palavra reservada em C#, se for prefixada pelo `@` . Isso pode ser útil ao interagir com outras linguagens.

Os tipos de valor do C# são divididos em *tipos simples*, *tipos de enumeração*, *tipos de struct*e *tipos de valor anulável*. Os tipos de referência do C# são divididos em *tipos de classe*, *tipos de interface*, *tipos de matriz*e *tipos delegados*.

A seguinte estrutura de tópicos fornece uma visão geral do sistema de tipos do C#.

- [Tipos de valor](../language-reference/builtin-types/value-types.md)
  - [Tipos simples](../language-reference/builtin-types/value-types.md#built-in-value-types)
    - [Integral assinada](../language-reference/builtin-types/integral-numeric-types.md): `sbyte` , `short` , `int` , `long`
    - [Integral não assinada](../language-reference/builtin-types/integral-numeric-types.md): `byte` , `ushort` , `uint` , `ulong`
    - [Caracteres Unicode](../../standard/base-types/character-encoding-introduction.md): `char` , que representa uma unidade de código UTF-16
    - [Ponto flutuante de binário do IEEE](../language-reference/builtin-types/floating-point-numeric-types.md): `float` , `double`
    - [Ponto flutuante decimal de alta precisão](../language-reference/builtin-types/floating-point-numeric-types.md): `decimal`
    - Booliano: `bool` , que representa valores Boolianos — valores que são `true` ou `false`
  - [Tipos de enumeração](../language-reference/builtin-types/enum.md)
    - Tipos definidos pelo usuário do formulário `enum E {...}` . Um tipo `enum` é um tipo distinto com constantes nomeadas. Cada tipo `enum` tem um tipo subjacente, que deve ser um dos oito tipos integrais. O conjunto de valores de um tipo `enum` é o mesmo que o conjunto de valores do tipo subjacente.
  - [Tipos struct](../language-reference/builtin-types/struct.md)
    - Tipos definidos pelo usuário do formulário `struct S {...}`
  - [Tipos de valor anuláveis](../language-reference/builtin-types/nullable-value-types.md)
    - Extensões de todos os outros tipos de valor com um valor `null`
  - [Tipos de valor de tupla](../language-reference/builtin-types/value-tuples.md)
    - Tipos definidos pelo usuário do formulário `(T1, T2, ...)`
- [Tipos de referência](../language-reference/keywords/reference-types.md)
  - [Tipos de aula](../language-reference/keywords/class.md)
    - Classe base definitiva de todos os outros tipos: `object`
    - [Cadeias de caracteres Unicode](../../standard/base-types/character-encoding-introduction.md): `string` , que representa uma sequência de unidades de código UTF-16
    - Tipos definidos pelo usuário do formulário `class C {...}`
  - [Tipos de interface](../language-reference/keywords/interface.md)
    - Tipos definidos pelo usuário do formulário `interface I {...}`
  - [Tipos de matriz](../programming-guide/arrays/index.md)
    - Único e multidimensional e irregular, por exemplo,, `int[]` `int[,]` e `int[][]`
  - [Tipos delegados](../language-reference/builtin-types/reference-types.md#the-delegate-type)
    - Tipos definidos pelo usuário do formulário `delegate int D(...)`

Os programas em C# usam *declarações de tipos* para criar novos tipos. Uma declaração de tipo especifica o nome e os membros do novo tipo. Cinco das categorias de tipos do C# são definíveis pelo usuário: tipos de classe, tipos de struct, tipos de interface, tipos de enumeração e tipos delegados.

- Um tipo `class` define uma estrutura de dados que contém membros de dados (campos) e membros de função (métodos, propriedades e outros). Os tipos de classe dão suporte à herança única e ao polimorfismo, mecanismos nos quais as classes derivadas podem estender e especializar as classes base.
- Um tipo `struct` é semelhante a um tipo de classe que representa uma estrutura com membros de dados e membros da função. No entanto, ao contrário das classes, as structs são tipos de valor e normalmente não exigem alocação de heap. Tipos de struct não dão suporte à herança especificada pelo usuário e todos os tipos de struct herdam implicitamente do tipo `object` .
- Um `interface` tipo define um contrato como um conjunto nomeado de membros públicos. Um `class` ou `struct` que implementa um `interface` deve fornecer implementações dos membros da interface. Um `interface` pode herdar de várias interfaces base e um `class` ou `struct` pode implementar várias interfaces.
- Um tipo `delegate` representa referências aos métodos com uma lista de parâmetros e tipo de retorno específicos. Delegados possibilitam o tratamento de métodos como entidades que podem ser atribuídos a variáveis e passadas como parâmetros. Os delegados são análogos aos tipos de função fornecidos pelas linguagens funcionais. Eles também são semelhantes ao conceito de ponteiros de função encontrados em algumas outras linguagens. Diferentemente de ponteiros de função, os delegados são orientados a objeto e são de tipo seguro.

Os `class` `struct` tipos,, e `interface` `delegate` oferecem suporte a genéricos, no qual eles podem ser parametrizados com outros tipos.

O C# dá suporte a matrizes uni e multidimensionais de qualquer tipo. Ao contrário dos tipos listados acima, os tipos de matriz não precisam ser declarados antes que possam ser usados. Em vez disso, os tipos de matriz são construídos seguindo um nome de tipo entre colchetes. Por exemplo, `int[]` é uma matriz unidimensional de `int` , `int[,]` é uma matriz bidimensional de `int` e `int[][]` é uma matriz unidimensional de matriz unidimensional ou matriz "denteada" de `int` .

Tipos anuláveis não exigem uma definição separada. Para cada tipo não anulável `T` , há um tipo anulável correspondente `T?` , que pode conter um valor adicional, `null` . Por exemplo, `int?` é um tipo que pode conter qualquer número inteiro de 32 bits ou o valor `null` e `string?` é um tipo que pode conter qualquer `string` ou o valor `null` .

O sistema de tipos do C# é unificado, de modo que um valor de qualquer tipo pode ser tratado como um `object` . Cada tipo no C#, direta ou indiretamente, deriva do tipo de classe `object`, e `object` é a classe base definitiva de todos os tipos. Os valores de tipos de referência são tratados como objetos simplesmente exibindo os valores como tipo `object`. Os valores de tipos de valor são tratados como objetos, executando *conversão boxing* e *operações de conversão unboxing*. No exemplo a seguir, um valor `int` é convertido em `object` e volta novamente ao `int`.

:::code language="csharp" source="./snippets/shared/Program.cs" ID="boxing" :::

Quando um valor de um tipo de valor é atribuído a uma `object` referência, uma "caixa" é alocada para conter o valor. Essa caixa é uma instância de um tipo de referência, e o valor é copiado para essa caixa. Por outro lado, quando uma `object` referência é convertida em um tipo de valor, é feita uma verificação de que a referência `object` é uma caixa do tipo de valor correto. Se a verificação for realizada com sucesso, o valor na caixa será copiado para o tipo de valor.

O sistema de tipos unificados do C# significa efetivamente que os tipos de valor são tratados como `object` referências "sob demanda". Devido à unificação, as bibliotecas de uso geral que usam `object` o tipo podem ser usadas com todos os tipos que derivam de `object` , incluindo tipos de referência e tipos de valor.

Existem vários tipos de *variáveis* no C#, incluindo campos, elementos de matriz, variáveis locais e parâmetros. As variáveis representam locais de armazenamento. Cada variável tem um tipo que determina quais valores podem ser armazenados na variável, como mostrado abaixo.

- Tipo de valor não nulo
  - Um valor de tipo exato
- Tipos de valor anulável
  - Um valor `null` ou um valor do tipo exato
- objeto
  - Uma referência `null`, uma referência a um objeto de qualquer tipo de referência ou uma referência a um valor de qualquer tipo de valor demarcado
- Tipo de classe
  - Uma referência `null`, uma referência a uma instância desse tipo de classe ou uma referência a uma instância de uma classe derivada desse tipo de classe
- Tipo de interface
  - Uma referência `null`, uma referência a uma instância de um tipo de classe que implementa esse tipo de interface ou uma referência a um valor demarcado de um tipo de valor que implementa esse tipo de interface
- Tipo de matriz
  - Uma referência `null`, uma referência a uma instância desse tipo de matriz ou uma referência a uma instância de um tipo de matriz compatível
- Tipo delegado
  - Uma referência `null` ou uma referência a uma instância de um tipo de delegado compatível

## <a name="program-structure"></a>Estrutura do programa

Os principais conceitos organizacionais do C# são [***programas***](../programming-guide/inside-a-program/index.md), [***namespaces***](../programming-guide/namespaces/index.md), [***tipos***](../programming-guide/types/index.md), [***Membros***](../programming-guide/classes-and-structs/members.md)e [***assemblies***](../../standard/assembly/index.md). Os programas declaram tipos que contêm membros e podem ser organizados em namespaces. Classes, estruturas e interfaces são exemplos de tipos. Campos, métodos, propriedades e eventos são exemplos de membros. Quando programas C# são compilados, eles são fisicamente empacotados em assemblies. Os assemblies normalmente têm a extensão de arquivo `.exe` ou `.dll`, dependendo se eles implementam ***aplicativos*** ou ***bibliotecas***, respectivamente.

Como um pequeno exemplo, considere um assembly que contém o código a seguir:

:::code language="csharp" source="./snippets/shared/AcmeStack.cs":::

O nome totalmente qualificado dessa classe é `Acme.Collections.Stack`. A classe contém vários membros: um campo chamado `top`, dois métodos chamados `Push` e `Pop` e uma classe aninhada chamada `Entry`. A classe `Entry` ainda contém três membros: um campo chamado `next`, um campo chamado `data`e um construtor. O `Stack` é uma classe *genérica* . Ele tem um parâmetro de tipo, `T` que é substituído por um tipo concreto quando usado.

> [!NOTE]
> Uma *pilha* é uma coleção de "primeiro a entrar no final" (filo). Novos elementos são adicionados à parte superior da pilha. Quando um elemento é removido, ele é removido da parte superior da pilha.

Os assemblies contêm código executável na forma de instruções de IL (Linguagem Intermediária) e informações simbólicas na forma de metadados. Antes de ser executado, o compilador JIT (just-in-time) do .NET Common Language Runtime converte o código IL em um assembly para o código específico do processador.

Como um assembly é uma unidade de funcionalidade autodescreveda que contém o código e os metadados, não há necessidade de `#include` diretivas e arquivos de cabeçalho em C#. Os tipos públicos e os membros contidos em um assembly específico são disponibilizados em um programa C# simplesmente fazendo referência a esse assembly ao compilar o programa. Por exemplo, esse programa usa a classe `Acme.Collections.Stack` do assembly `acme.dll`:

:::code language="csharp" source="./snippets/shared/StackUsage.cs":::

Para compilar esse programa, você precisa *fazer referência* ao assembly que contém a classe Stack definida no exemplo anterior.

Os programas em C# podem ser armazenados em vários arquivos de origem. Quando um programa em C# é compilado, todos os arquivos de origem são processados em conjunto e os arquivos de origem podem fazer referência livremente entre si. Conceitualmente, é como se todos os arquivos de origem fossem concatenados em um arquivo grande antes de serem processados. Declarações de encaminhamento nunca são necessárias em C# porque, com poucas exceções, a ordem de declaração é insignificante. O C# não limita um arquivo de origem para declarar apenas um tipo público, nem exige o nome do arquivo de origem para corresponder a um tipo declarado no arquivo de origem.

Artigos adicionais neste Tour explicam esses blocos organizacionais.

>[!div class="step-by-step"]
>[Próximo](types.md)
