---
title: Desconstruindo tuplas e outros tipos
description: Saiba como desconstruir tuplas e outros tipos.
ms.technology: csharp-fundamentals
ms.date: 11/23/2017
ms.assetid: 0b0c4b0f-4a47-4f66-9b8e-f5c63b195960
ms.openlocfilehash: 8defd75a7cdff3490d2b0a6097ec2a898576e113
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174160"
---
# <a name="deconstructing-tuples-and-other-types"></a>Desconstruindo tuplas e outros tipos

Uma tupla fornece uma maneira leve de recuperar vários valores de uma chamada de método. Mas depois de recuperar a tupla, você precisa lidar com seus elementos individuais. Fazer isso para cada elemento é incômodo, conforme mostra o exemplo a seguir. O método `QueryCityData` retorna uma tupla de 3 e cada um de seus elementos é atribuído a uma variável em uma operação separada.

[!code-csharp[WithoutDeconstruction](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple1.cs)]

Recuperar vários valores de propriedade e de campo de um objeto pode ser igualmente complicado: é necessário atribuir um valor de campo ou de propriedade a uma variável, membro por membro.

Começando com o C# 7.0, você pode recuperar vários elementos de uma tupla ou recuperar vários valores calculados, de campo e de propriedade de um objeto em uma única operação *deconstruct*. Quando você desconstrói uma tupla, você atribui os elementos dela a variáveis individuais. Quando você desconstrói um objeto, você atribui os elementos dela a variáveis individuais.

## <a name="deconstructing-a-tuple"></a>Desconstruir uma tupla

O C# conta com suporte interno à desconstrução de tuplas, que permite que você descompacte todos os itens em uma tupla em uma única operação. A sintaxe geral para desconstruir uma tupla é semelhante à sintaxe para definir uma: coloque as variáveis para as quais cada elemento deve ser atribuído entre parênteses no lado esquerdo de uma instrução de atribuição. Por exemplo, a instrução a seguir atribui os elementos de uma tupla de 4 a quatro variáveis separadas:

```csharp
var (name, address, city, zip) = contact.GetAddressInfo();
```

Há três maneiras de desconstruir uma tupla:

- Você pode declarar explicitamente o tipo de cada campo dentro de parênteses. O exemplo a seguir usa essa abordagem para desconstruir a tupla de 3 retornada pelo método `QueryCityData`.

    [!code-csharp[Deconstruction-Explicit](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple2.cs#1)]

- Você pode usar a palavra-chave `var` de modo que o C# infira o tipo de cada variável. Você coloca a palavra-chave `var` fora dos parênteses. O exemplo a seguir usa a inferência de tipos ao desconstruir a tupla de 3 retornada pelo método `QueryCityData`.

    [!code-csharp[Deconstruction-Infer](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple3.cs#1)]

    Você também pode usar a palavra-chave `var` individualmente com qualquer uma ou todas as declarações de variável dentro dos parênteses.

    [!code-csharp[Deconstruction-Infer-Some](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple4.cs#1)]

    Isso é difícil e não é recomendado.

- Por fim, você pode desconstruir a tupla em variáveis que já foram declaradas.

    [!code-csharp[Deconstruction-Declared](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple5.cs#1)]

Observe que você não poderá especificar um tipo específico fora dos parênteses, mesmo se todos os campos na tupla tiverem o mesmo tipo. Isso gera o erro do compilador CS8136, "O formulário de desconstrução 'var (...)' não permite um tipo específico para 'var'.".

Observe que você também deve atribuir cada elemento da tupla a uma variável. Se você omitir qualquer elemento, o compilador gerará o erro CS8132, "Não é possível desconstruir uma tupla de 'x' elementos em 'y' variáveis."

Observe que não é possível combinar declarações e as atribuições com as variáveis existentes do lado esquerdo de uma desconstrução. O compilador gera o erro CS8184, "uma desconstrução não pode combinar declarações e expressões no lado esquerdo." quando os membros incluem variáveis recém-declaradas e existentes.

## <a name="deconstructing-tuple-elements-with-discards"></a>Desconstruir elementos de tupla com descartes

Geralmente, ao desconstruir uma tupla, você está interessado nos valores de apenas alguns elementos. Começando com o C# 7.0, você pode aproveitar o suporte do C# para *descartes*, que são variáveis somente gravação cujos valores você opta por ignorar. Um descarte é designado por um caractere de sublinhado ("\_") em uma atribuição. Você pode descartar tantos valores quantos desejar; todos são representados pelo descarte único, `_`.

O exemplo a seguir ilustra o uso de tuplas com descartes. O método `QueryCityDataForYears` a seguir retorna uma tupla de 6 com o nome de uma cidade, sua área, um ano, a população da cidade nesse ano, um segundo ano e população da cidade nesse segundo ano. O exemplo mostra a alteração na população entre esses dois anos. Entre os dados disponíveis da tupla, não estamos preocupados com a área da cidade e sabemos o nome da cidade e as duas datas em tempo de design. Como resultado, estamos interessados apenas nos dois valores de população armazenados na tupla e podemos lidar com seus valores restantes como descartes.  

[!code-csharp[Tuple-discard](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/discard-tuple1.cs)]

## <a name="deconstructing-user-defined-types"></a>Desconstruindo tipos definidos pelo usuário

O C# não oferece suporte interno para desconstrução de tipos não tupla. No entanto, como o autor de uma classe, um struct ou uma interface, você pode permitir instâncias do tipo a ser desconstruído implementando um ou mais métodos `Deconstruct`. O método retorna void e cada valor a ser desconstruído é indicado por um parâmetro [out](language-reference/keywords/out-parameter-modifier.md) na assinatura do método. Por exemplo, o método `Deconstruct` a seguir de uma classe `Person` retorna o nome, o segundo nome e o sobrenome:

[!code-csharp[Class-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-class1.cs#1)]

Em seguida, você pode desconstruir uma instância da classe `Person` denominada `p` com uma atribuição semelhante à seguinte:

[!code-csharp[Class-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-class1.cs#2)]

O exemplo a seguir sobrecarrega o método `Deconstruct` para retornar várias combinações de propriedades de um objeto `Person`. As sobrecargas individuais retornam:

- Um nome e um sobrenome.
- Um nome, um sobrenome e um segundo nome.
- Um nome, um sobrenome, um nome de cidade e um nome de estado.

[!code-csharp[Class-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-class2.cs)]

Já que você pode sobrecarregar o método `Deconstruct` para refletir os grupos de dados que geralmente são extraídos de um objeto, você deve ter cuidado ao definir métodos `Deconstruct` com assinaturas que são diferentes e não ambíguas. Vários métodos `Deconstruct` que têm o mesmo número de parâmetros `out` ou com o mesmo número e tipo de parâmetros `out` em uma ordem diferente podem causar confusão.

O método `Deconstruct` sobrecarregado no exemplo a seguir ilustra uma possível fonte de confusão. A primeira sobrecarga retorna o primeiro nome, o segundo nome, o sobrenome e idade de um objeto `Person`, nessa ordem. A segunda sobrecarga retorna informações de nome apenas junto com a renda anual, mas o nome, o segundo nome e o sobrenome estão em uma ordem diferente. Isso torna fácil confundir a ordem dos argumentos ao desconstruir uma instância de `Person`.

[!code-csharp[Deconstruct-ambiguity](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-ambiguous.cs)]

## <a name="deconstructing-a-user-defined-type-with-discards"></a>Desconstruir um tipo definido pelo usuário com descartes

Assim como você faria com [tuplas](#deconstructing-tuple-elements-with-discards), você pode usar descartes para ignorar os itens selecionados retornados por um método `Deconstruct`. Cada descarte é definido por uma variável chamada "\_" e uma única operação de desconstrução pode incluir vários descartes.

O exemplo a seguir desconstrói um objeto `Person` em quatro cadeias de caracteres (os nomes e sobrenomes, a cidade e o estado), mas descarta o sobrenome e o estado.

[!code-csharp[Class-discard](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/class-discard1.cs#1)]

## <a name="deconstructing-a-user-defined-type-with-an-extension-method"></a>Desconstruir um tipo definido pelo usuário com um método de extensão

Se você não criar uma classe, struct ou interface, você ainda poderá decompor objetos desse tipo implementando um ou mais `Deconstruct` [métodos de extensão](programming-guide/classes-and-structs/extension-methods.md) para retornar os valores nos quais você estiver interessado.

O exemplo a seguir define dois métodos de extensão `Deconstruct` para a classe <xref:System.Reflection.PropertyInfo?displayProperty=nameWithType>. O primeiro retorna um conjunto de valores que indicam as características da propriedade, incluindo seu tipo, se ela é estática ou instância, se ela é somente leitura e se é indexada. O segundo indica a acessibilidade da propriedade. Já que a acessibilidade dos acessadores get e set pode ser diferente, valores boolianos indicam se a propriedade acessadores get e set separados e, em caso afirmativo, se eles têm a mesma acessibilidade. Se há apenas um acessador ou ambos os acessadores get e set têm a mesma acessibilidade, a variável `access` indica a acessibilidade da propriedade como um todo. Caso contrário, a acessibilidade dos acessadores get e set é indicada pelas variáveis `getAccess` e `setAccess`.

[!code-csharp[Extension-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-extension1.cs)]

## <a name="see-also"></a>Veja também

- [Descartes](discards.md)
- [Tipos de tupla](language-reference/builtin-types/value-tuples.md)
