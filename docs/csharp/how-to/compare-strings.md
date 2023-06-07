---
title: Como comparar cadeias de caracteres – guia C#
description: Saiba como comparar e ordenar valores de cadeia de caracteres, com ou sem ordem específica de cultura ou de diferenciação entre maiúsculas e minúsculas
ms.date: 10/03/2018
helpviewer_keywords:
- strings [C#], comparison
- comparing strings [C#]
ms.openlocfilehash: d1ea0fc3573714347580a2aaded2d0f3118681a8
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85324178"
---
# <a name="how-to-compare-strings-in-c"></a>Como comparar cadeias de caracteres no C\#

Você compara cadeias de caracteres para responder uma dessas duas perguntas: "Essas duas cadeias de caracteres são iguais?" ou "Em que ordem essas cadeias de caracteres devem ser colocadas ao classificá-las?"

Essas duas perguntas são complicadas devido a fatores que afetam as comparações de cadeia de caracteres:

- Você pode escolher uma comparação ordinal ou linguística.
- Você pode escolher se o uso de maiúsculas faz diferença.
- Você pode escolher comparações específicas de cultura.
- As comparações linguísticas dependem da plataforma e da cultura.

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

Ao comparar cadeias de caracteres, você pode definir uma ordem entre elas. As comparações são usadas para classificar uma sequência de cadeias de caracteres. Com a sequência em uma ordem conhecida, fica mais fácil de pesquisar, tanto para softwares quanto para os usuários. Outras comparações podem verificar se as cadeias de caracteres são iguais. Essas verificações são semelhantes a igualdade, mas algumas diferenças, como diferenças entre maiúsculas e minúsculas, podem ser ignoradas.

## <a name="default-ordinal-comparisons"></a>Comparações ordinárias padrão

Por padrão, as operações mais comuns:

- <xref:System.String.CompareTo%2A?displayProperty=nameWithType>
- <xref:System.String.Equals%2A?displayProperty=nameWithType>
- <xref:System.String.op_Equality%2A?displayProperty=nameWithType>e <xref:System.String.op_Inequality%2A?displayProperty=nameWithType> , ou seja, [operadores `==` de igualdade `!=` e ](../language-reference/operators/equality-operators.md#string-equality), respectivamente

Execute uma comparação ordinal que diferencia maiúsculas de minúsculas e, se necessário, use a cultura atual. O exemplo a seguir demonstra que:

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet1":::

A comparação ordinal padrão não leva em conta as regras linguísticas ao comparar cadeias de caracteres. Ela compara o valor binário de cada objeto <xref:System.Char> em duas cadeias de caracteres. Como resultado, a comparação ordinal padrão também diferencia maiúsculas de minúsculas.

O teste para igualdade com <xref:System.String.Equals%2A?displayProperty=nameWithType> e os `==` `!=` operadores and é diferente da comparação de cadeias de caracteres usando os <xref:System.String.CompareTo%2A?displayProperty=nameWithType> <xref:System.String.Compare(System.String,System.String)?displayProperty=nameWithType)> métodos e. Embora os testes de igualdade executem uma comparação ordinal que diferencia maiúsculas de minúsculas, os métodos de comparação executam uma comparação de diferenciação de maiúsculas e minúsculas, com a cultura atual. Como os métodos de comparação padrão geralmente fazem diferentes tipos de comparações, recomendamos que você sempre esclareça a intenção do seu código, chamando uma sobrecarga que especifica explicitamente o tipo de comparação a ser feita.

## <a name="case-insensitive-ordinal-comparisons"></a>Comparações ordinais que não diferenciam maiúsculas de minúsculas

O método <xref:System.String.Equals(System.String,System.StringComparison)?displayProperty=nameWithType> permite que você especifique um valor <xref:System.StringComparison> de <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType>
para uma comparação ordinal que não diferencia maiúsculas de minúsculas. Há também um método <xref:System.String.Compare(System.String,System.String,System.StringComparison)?displayProperty=nameWithType> estático que fará uma comparação ordinal sem distinção entre maiúsculas e minúsculas se você especificar um valor de <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> para o argumento <xref:System.StringComparison>. Eles são mostrados no código a seguir:

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet2":::

Ao fazer uma comparação ordinal sem distinção entre maiúsculas e minúsculas, esses métodos usam as convenções de maiúsculas e minúsculas da [cultura invariável](xref:System.Globalization.CultureInfo.InvariantCulture).

## <a name="linguistic-comparisons"></a>Comparações linguísticas

As cadeias de caracteres também podem ser ordenadas usando regras linguísticas para a cultura atual.
Às vezes, isso é conhecido como "ordem de classificação de palavra". Quando você executa uma comparação linguística, alguns caracteres Unicode não alfanuméricos podem ter pesos especiais atribuídos. Por exemplo, o hífen "-" pode ter um pequeno peso atribuído para que "co-op" e "Coop" apareçam ao lado um do outro na ordem de classificação. Além disso, alguns caracteres Unicode podem ser equivalentes a uma sequência de instâncias <xref:System.Char>. O exemplo a seguir usa a frase "They dance in the street." em alemão, com o “ss” (U+0073 U+0073) em uma cadeia de caracteres e ‘ß’ (U+00DF) em outra. Linguisticamente (no Windows), "ss" é igual ao caractere 'ß' Esszet alemão nas culturas "en-US" e "de-DE".

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet3":::

Este exemplo demonstra a natureza dependente de sistema operacional das comparações linguísticas. O host da janela interativa é um host Linux. As comparações de linguísticas e ordinais produzem os mesmos resultados. Se você executar esse mesmo exemplo em um host do Windows, verá a seguinte saída:

```console
<coop> is less than <co-op> using invariant culture
<coop> is greater than <co-op> using ordinal comparison
<coop> is less than <cop> using invariant culture
<coop> is less than <cop> using ordinal comparison
<co-op> is less than <cop> using invariant culture
<co-op> is less than <cop> using ordinal comparison
```

No Windows, a ordem de classificação de "cop", "coop" e "co-op" muda quando você muda de uma comparação linguística para uma comparação ordinal. As duas frases em alemão também são comparadas de forma diferente ao usar os tipos diferentes de comparação.

## <a name="comparisons-using-specific-cultures"></a>Comparações usando culturas específicas

Este exemplo armazena objetos <xref:System.Globalization.CultureInfo> para as culturas en-US e de-DE.
As comparações são feitas usando um objeto <xref:System.Globalization.CultureInfo> para garantir uma comparação específica da cultura.

A cultura usada afeta as comparações linguísticas. O exemplo a seguir mostra os resultados da comparação das duas frases em alemão usando a cultura "en-US" e a cultura "de-DE":

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet4":::

As comparações que diferenciam cultura normalmente são usadas para comparar e classificar cadeias de caracteres inseridas por usuários com outras cadeias de caracteres inseridas por usuários. Os caracteres e as convenções de classificação dessas cadeias de caracteres podem variar de acordo com a localidade do computador do usuário. Até mesmo cadeias de caracteres que contêm caracteres idênticos podem ser classificadas de formas diferentes dependendo da cultura do thread atual. Além disso, experimente este código de exemplo localmente em um computador Windows e você obterá os seguintes resultados:

```console
<coop> is less than <co-op> using en-US culture
<coop> is greater than <co-op> using ordinal comparison
<coop> is less than <cop> using en-US culture
<coop> is less than <cop> using ordinal comparison
<co-op> is less than <cop> using en-US culture
<co-op> is less than <cop> using ordinal comparison
```

As comparações linguísticas dependem da cultura atual e são dependentes do SO. Leve isso em conta ao trabalhar com comparações de cadeias de caracteres.

## <a name="linguistic-sorting-and-searching-strings-in-arrays"></a>Classificação linguística e cadeias de caracteres de pesquisa em matrizes

Os exemplos a seguir mostram como classificar e pesquisar cadeias de caracteres em uma matriz usando uma comparação linguística que depende da cultura atual. Use os métodos <xref:System.Array> estáticos que aceitam um parâmetro <xref:System.StringComparer?displayProperty=nameWithType>.

Este exemplo mostra como classificar uma matriz de cadeias de caracteres usando a cultura atual:

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet5":::

Depois que a matriz é classificada, você pode procurar as entradas usando uma pesquisa binária. Uma pesquisa binária é iniciada no meio da coleção para determinar qual metade da coleção contém a cadeia de caracteres procurada. Cada comparação subsequente subdivide a parte restante da coleção na metade.  A matriz é classificada usando o <xref:System.StringComparer.CurrentCulture?displayProperty=nameWithType>. A função local `ShowWhere` exibe informações sobre o local em que a cadeia de caracteres foi encontrada. Se a cadeia de caracteres não foi encontrada, o valor retornado indicará onde seria se fosse encontrado.

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet6":::

## <a name="ordinal-sorting-and-searching-in-collections"></a>Classificação ordinal e pesquisa em coleções

O código a seguir usa a classe de coleção <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> para armazenar cadeias de caracteres. As cadeias de caracteres são classificadas usando o método <xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType>. Esse método precisa de um delegado que compara e ordena as duas cadeias de caracteres. O método <xref:System.String.CompareTo%2A?displayProperty=nameWithType> fornece essa função de comparação. Execute o exemplo e observe a ordem. Essa operação de classificação usa uma classificação ordinal que diferencia maiúsculas de minúsculas. Você usaria os métodos <xref:System.String.Compare%2A?displayProperty=nameWithType> estáticos para especificar regras de comparação diferentes.

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet7":::

Uma vez classificada, a lista de cadeias de caracteres pode ser pesquisada usando uma pesquisa binária. O exemplo a seguir mostra como pesquisar a lista classificada usando a mesma função de comparação. A função local `ShowWhere` mostra o local em que o texto procurado está ou deveria estar:

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet8":::

Use sempre o mesmo tipo de comparação para classificação e pesquisa. O uso de tipos diferentes de comparação para classificação e pesquisa produz resultados inesperados.

Classes de coleção como <xref:System.Collections.Hashtable?displayProperty=nameWithType>, <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> e <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> têm construtores que usam um parâmetro <xref:System.StringComparer?displayProperty=nameWithType> quando o tipo dos elementos ou chaves é `string`. Em geral, você deve usar esses construtores sempre que possível e especificar <xref:System.StringComparer.Ordinal?displayProperty=nameWithType> ou <xref:System.StringComparer.OrdinalIgnoreCase?displayProperty=nameWithType>.

## <a name="reference-equality-and-string-interning"></a>Igualdade de referência e centralização de cadeia de caracteres

Nenhum dos exemplos usou <xref:System.Object.ReferenceEquals%2A>. Esse método determina se duas cadeias de caracteres são o mesmo objeto, o que pode levar a resultados inconsistentes em comparações de cadeias de caracteres. O exemplo a seguir demonstra o recurso de *centralização da cadeia de caracteres* do C#. Quando um programa declara duas ou mais variáveis de cadeia de caracteres idênticas, o compilador armazena todas no mesmo local. Chamando o método <xref:System.Object.ReferenceEquals%2A>, você pode ver que as duas cadeias de caracteres na verdade se referem ao mesmo objeto na memória. Use o método <xref:System.String.Copy%2A?displayProperty=nameWithType> para evitar a centralização. Depois que a cópia for feita, as duas cadeias de caracteres terão locais de armazenamento diferentes, mesmo que tenham o mesmo valor. Execute o exemplo a seguir para mostrar que as cadeias de caracteres `a` e `b` são *centralizadas*, ou seja, que elas compartilham o mesmo armazenamento. As cadeias de caracteres `a` e `c` não são.

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/CompareStrings.cs" id="Snippet9":::

> [!NOTE]
> Quando você testa cadeias de caracteres quanto a igualdade, é necessário usar os métodos que especificam explicitamente o tipo de comparação que você pretende executar. O código fica muito mais legível e fácil de manter. Use as sobrecargas dos métodos das classes <xref:System.String?displayProperty=nameWithType> e <xref:System.Array?displayProperty=nameWithType> que aceitam um parâmetro de enumeração <xref:System.StringComparison>. Você especifica o tipo de comparação a ser executado. Evite usar os operadores `==` e `!=` ao testar a igualdade. Os métodos de instância <xref:System.String.CompareTo%2A?displayProperty=nameWithType> sempre executam uma comparação ordinal que diferencia maiúsculas de minúsculas. Basicamente, eles são adequados para colocar cadeias de caracteres em ordem alfabética.

Você pode internalizar uma cadeia de caracteres ou recuperar uma referência a uma cadeia de caracteres interna existente chamando o método <xref:System.String.Intern%2A?displayProperty=nameWithType>. Para determinar se uma cadeia de caracteres está internalizada, chame o método <xref:System.String.IsInterned%2A?displayProperty=nameWithType>.

## <a name="see-also"></a>Veja também

- <xref:System.Globalization.CultureInfo?displayProperty=nameWithType>
- <xref:System.StringComparer?displayProperty=nameWithType>
- [Cadeias de caracteres](../programming-guide/strings/index.md)
- [Comparação de cadeias de caracteres](../../standard/base-types/comparing.md)
- [Globalizando e localizando aplicativos](/visualstudio/ide/globalizing-and-localizing-applications)
