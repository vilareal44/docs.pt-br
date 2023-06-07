---
title: Construtores
description: 'Saiba como definir e usar construtores em F # para criar e inicializar objetos de classe e estrutura.'
ms.date: 05/16/2016
ms.openlocfilehash: be8fc3f1d82a9a7c778a6d094139f14150a28813
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303433"
---
# <a name="constructors"></a>Construtores

Este artigo descreve como definir e usar construtores para criar e inicializar objetos de classe e estrutura.

## <a name="construction-of-class-objects"></a>Construção de objetos de classe

Objetos de tipos de classe têm construtores. Há dois tipos de construtores. Um é o Construtor principal, cujos parâmetros aparecem entre parênteses logo após o nome do tipo. Você especifica outros construtores opcionais adicionais usando a `new` palavra-chave. Qualquer Construtor adicional deve chamar o Construtor principal.

O Construtor principal contém `let` e `do` associações que aparecem no início da definição de classe. Uma `let` Associação declara campos privados e métodos da classe; uma `do` Associação executa o código. Para obter mais informações sobre `let` associações em construtores de classe, consulte [ `let` associações em classes](let-bindings-in-classes.md). Para obter mais informações sobre `do` associações em construtores, consulte [ `do` associações em classes](do-bindings-in-classes.md).

Independentemente de o construtor que você deseja chamar ser um construtor primário ou um Construtor adicional, você pode criar objetos usando uma `new` expressão, com ou sem a `new` palavra-chave opcional. Você inicializa os objetos junto com argumentos de construtor, seja listando os argumentos em ordem e separados por vírgulas e entre parênteses, ou usando argumentos nomeados e valores entre parênteses. Você também pode definir propriedades em um objeto durante a construção do objeto usando os nomes de propriedade e atribuindo valores da mesma forma que usa argumentos de Construtor nomeados.

O código a seguir ilustra uma classe que tem um construtor e várias maneiras de criar objetos:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3501.fs)]

A saída é da seguinte maneira:

```console
Initialized object that has coordinates (1, 2, 3)
Initialized object that has coordinates (4, 5, 6)
Initialized object that has coordinates (7, 8, 9)
Initialized object that has coordinates (0, 0, 0)
```

## <a name="construction-of-structures"></a>Construção de estruturas

As estruturas seguem todas as regras de classes. Portanto, você pode ter um construtor principal e pode fornecer construtores adicionais usando o `new` . No entanto, há uma diferença importante entre estruturas e classes: estruturas podem ter um construtor sem parâmetros (ou seja, um sem argumentos) mesmo que nenhum construtor principal seja definido. O construtor sem parâmetros inicializa todos os campos para o valor padrão desse tipo, geralmente zero ou seu equivalente. Todos os construtores que você definir para estruturas devem ter pelo menos um argumento para que não entrem em conflito com o construtor sem parâmetros.

Além disso, as estruturas geralmente têm campos que são criados usando a `val` palavra-chave; as classes também podem ter esses campos. Estruturas e classes que têm campos definidos usando a `val` palavra-chave também podem ser inicializadas em construtores adicionais usando expressões de registro, conforme mostrado no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3502.fs)]

Para obter mais informações, consulte [campos explícitos: a `val` palavra-chave](explicit-fields-the-val-keyword.md).

## <a name="executing-side-effects-in-constructors"></a>Executando efeitos colaterais em construtores

Um construtor principal em uma classe pode executar código em uma `do` associação. No entanto, e se você tiver que executar o código em um Construtor adicional, sem uma `do` Associação? Para fazer isso, use a `then` palavra-chave.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3503.fs)]

Os efeitos colaterais do construtor primário ainda são executados. Portanto, a saída é a seguinte:

```console
Created a person object.
Created a person object.
Created an invalid person object.
```

O motivo pelo qual `then` é necessário em vez de outro `do` é que a `do` palavra-chave tem seu significado padrão de delimitar uma `unit` expressão de retorno quando presente no corpo de um Construtor adicional. Ele tem apenas um significado especial no contexto de construtores primários.

## <a name="self-identifiers-in-constructors"></a>Identificadores automáticos em construtores

Em outros membros, você fornece um nome para o objeto atual na definição de cada membro. Você também pode colocar o identificador automático na primeira linha da definição de classe usando a `as` palavra-chave imediatamente após os parâmetros do construtor. O exemplo a seguir ilustra essa sintaxe.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3504.fs)]

Em construtores adicionais, você também pode definir um autoidentifier colocando a `as` cláusula logo após os parâmetros do construtor. O exemplo a seguir ilustra essa sintaxe:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3505.fs)]

Podem ocorrer problemas quando você tenta usar um objeto antes que ele seja totalmente definido. Portanto, os usos do autoidentifier podem fazer com que o compilador emita um aviso e insira verificações adicionais para garantir que os membros de um objeto não sejam acessados antes de o objeto ser inicializado. Você só deve usar o autoidentifier nas `do` associações do Construtor principal ou depois da `then` palavra-chave em construtores adicionais.

O nome do autoidentifier não precisa ser `this` . Pode ser qualquer identificador válido.

## <a name="assigning-values-to-properties-at-initialization"></a>Atribuindo valores a propriedades na inicialização

Você pode atribuir valores às propriedades de um objeto de classe no código de inicialização acrescentando uma lista de atribuições do formulário `property = value` à lista de argumentos para um construtor. Isso é demonstrado no exemplo de código a seguir:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3506.fs)]

A seguinte versão do código anterior ilustra a combinação de argumentos comuns, argumentos opcionais e configurações de propriedade em uma chamada de construtor:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3507.fs)]

## <a name="constructors-in-inherited-class"></a>Construtores na classe herdada

Ao herdar de uma classe base que tem um construtor, você deve especificar seus argumentos na cláusula Inherit. Para obter mais informações, consulte [construtores e herança](../inheritance.md#constructors-and-inheritance).

## <a name="static-constructors-or-type-constructors"></a>Construtores estáticos ou construtores de tipo

Além de especificar o código para criar objetos, static `let` e `do` bindings podem ser criados em tipos de classe que são executados antes de o tipo ser usado pela primeira vez para executar a inicialização no nível de tipo. Para obter mais informações, consulte [ `let` associações em classes](let-bindings-in-classes.md) e [ `do` associações em classes](do-bindings-in-classes.md).

## <a name="see-also"></a>Confira também

- [Membros](index.md)
