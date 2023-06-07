---
title: Novidades no C# 7.0 – Guia do C#
description: Obtenha uma visão geral dos novos recursos na versão 7.0 da linguagem C#.
ms.date: 02/20/2019
ms.assetid: fd41596d-d0c2-4816-b94d-c4d00a5d0243
ms.openlocfilehash: 38b1afebf6d4fa69c46424c2d9a3631e8f3a8707
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174738"
---
# <a name="whats-new-in-c-70"></a>Novidades no C# 7.0

O C# 7.0 adiciona vários recursos novos à linguagem C#:

- [`out`as](#out-variables)
  - Declare valores `out` embutidos como argumentos para o método em que eles são usados.
- [Tuplas](#tuples)
  - Você pode criar tipos simples e sem nome que contêm vários campos públicos. Compiladores e ferramentas IDE entendem a semântica desses tipos.
- [Descartes](#discards)
  - Descartes são variáveis temporárias de somente gravação usadas em atribuições quando o valor atribuído não tem importância. Eles são mais úteis ao desconstruir tuplas e tipos definidos pelo usuário, bem como ao chamar métodos com parâmetros `out`.
- [Correspondência de padrões](#pattern-matching)
  - Você pode criar a lógica de ramificação com base em tipos e valores arbitrários dos membros desses tipos.
- [`ref`locais e retorna](#ref-locals-and-returns)
  - As variáveis locais do método e os valores de retorno podem ser referências a outros armazenamentos.
- [Funções locais](#local-functions)
  - Você pode aninhar funções dentro de outras funções para limitar seu escopo e visibilidade.
- [Mais membros aptos para expressão](#more-expression-bodied-members)
  - A lista de membros que podem ser criados usando expressões cresceu.
- [`throw`Expressões](#throw-expressions)
  - Gere exceções em constructos de código que anteriormente não eram permitidos devido ao fato de `throw` ser uma instrução.
- [Tipos de retorno assíncrono generalizado](#generalized-async-return-types)
  - Os métodos declarados com o modificador `async` podem retornar outros tipos além de `Task` e `Task<T>`.
- [Aprimoramentos da sintaxe de literais numéricos](#numeric-literal-syntax-improvements)
  - Novos tokens aprimoram a legibilidade para constantes numéricas.

O restante deste artigo fornece uma visão geral de cada recurso. Para cada recurso, você aprenderá o raciocínio por trás dele. Você aprenderá a sintaxe. Você pode explorar esses recursos em seu ambiente usando a ferramenta global `dotnet try`:

1. Instale a ferramenta global [dotnet-try](https://github.com/dotnet/try/blob/master/README.md#setup).
1. Clone o repositório [dotnet/try-samples](https://github.com/dotnet/try-samples).
1. Definir o diretório atual do subdiretório *csharp7* para o repositório *try-samples*.
1. Execute `dotnet try`.

## <a name="out-variables"></a>Variáveis `out`

A sintaxe existente que dá suporte a parâmetros `out` foi aperfeiçoada nesta versão. Agora você pode declarar variáveis `out` na lista de argumentos de uma chamada de método, em vez de escrever uma instrução de declaração separada:

[!code-csharp[OutVariableDeclarations](~/samples/snippets/csharp/new-in-7/program.cs#OutVariableDeclarations "Out variable declarations")]

Você talvez queira especificar o tipo da variável `out` para maior clareza, conforme mostrado acima. No entanto, a linguagem dá suporte ao uso de variável local de tipo implícito:

[!code-csharp[OutVarVariableDeclarations](~/samples/snippets/csharp/new-in-7/program.cs#OutVarVariableDeclarations "Implicitly typed Out variable")]

- O código é mais fácil de ler.
  - Você declara a variável out onde a usa, não em outra linha acima.
- Não é necessário atribuir um valor inicial.
  - Ao declarar a variável `out` no local em que ela é usada em uma chamada de método, você não pode usá-la acidentalmente antes de ela ser atribuída.

## <a name="tuples"></a>Tuplas

O C# fornece uma sintaxe avançada para classes e structs que são usados para explicar a intenção do design. Mas, às vezes, essa sintaxe avançada requer trabalho adicional com poucas vantagens. Geralmente, você pode escrever métodos que precisam de uma estrutura simples que contém mais de um elemento de dados. Para dar suporte a esses cenários foram adicionadas *tuplas* ao C#. As tuplas são estruturas de dados leves que contêm vários campos para representar os membros de dados.
Os campos não são validados, e você não pode definir seus próprios métodos

> [!NOTE]
> As tuplas estavam disponíveis antes do C# 7.0, mas elas eram ineficientes e não tinham nenhum suporte de linguagem.
> Isso significava que os elementos de tupla só podiam ser referenciados como `Item1`, `Item2` e assim por diante. O C# 7.0 introduz o suporte de linguagem para tuplas, que permite nomes semânticos para os campos de uma tupla, usando tipos de tupla novos e mais eficientes.

Você pode criar uma tupla atribuindo um valor a cada membro e, opcionalmente, fornecendo nomes semânticos para cada um dos membros da tupla:

[!code-csharp[NamedTuple](~/samples/snippets/csharp/new-in-7/program.cs#NamedTuple "Named tuple")]

A tupla `namedLetters` contém campos denominados `Alpha` e `Beta`. Esses nomes existem somente no momento da compilação e não são preservados, por exemplo, ao inspecionar a tupla usando reflexão em tempo de execução.

Em uma atribuição de tupla, você também pode especificar os nomes dos campos no lado direito da atribuição:

[!code-csharp[ImplicitNamedTuple](~/samples/snippets/csharp/new-in-7/program.cs#ImplicitNamedTuple "Implicitly named tuple")]

Pode haver ocasiões em que você deseja descompactar os membros de uma tupla que foram retornados de um método.  Você pode fazer isso declarando variáveis separadas para cada um dos valores na tupla. Essa descompactação é chamada *desconstrução* da tupla:

[!code-csharp[CallingWithDeconstructor](~/samples/snippets/csharp/new-in-7/program.cs#CallingWithDeconstructor "Deconstructing a tuple")]

Você também pode fornecer uma desconstrução semelhante para qualquer tipo no .NET. Escreva um método `Deconstruct` como um membro da classe. esse método `Deconstruct` fornece um conjunto de argumentos `out` para cada uma das propriedades que você deseja extrair. Considere essa classe `Point` que fornece um método desconstrutor que extrai as coordenadas `X` e `Y`:

[!code-csharp[PointWithDeconstruction](~/samples/snippets/csharp/new-in-7/point.cs#PointWithDeconstruction "Point with deconstruction method")]

Extraia os campos individuais atribuindo um `Point` a uma tupla:

[!code-csharp[DeconstructPoint](~/samples/snippets/csharp/new-in-7/program.cs#DeconstructPoint "Deconstruct a point")]

Para obter mais informações, consulte [tipos de tupla](../language-reference/builtin-types/value-tuples.md).

## <a name="discards"></a>Descartes

Geralmente, ao desconstruir uma tupla ou chamar um método com parâmetros `out`, você é forçado a definir uma variável cujo valor não é importante e você não pretende usar. O C# adiciona suporte para *descartes* para lidar com esse cenário. Um descarte é uma variável de somente gravação cujo nome é `_` (o caractere de sublinhado); você pode atribuir todos os valores que você pretende descartar à variável única. Um descarte é como uma variável não atribuída. Além da instrução de atribuição, o descarte não pode ser usado no código.

Os descartes são compatíveis com os seguintes cenários:

- Ao desconstruir tuplas ou tipos definidos pelo usuário.
- Ao chamar métodos com parâmetros [out](../language-reference/keywords/out-parameter-modifier.md).
- Em uma operação de correspondência de padrões com as instruções [is](../language-reference/keywords/is.md) e [switch](../language-reference/keywords/switch.md).
- Como um identificador autônomo quando você deseja identificar explicitamente o valor de uma atribuição como um descarte.

O exemplo a seguir define um método `QueryCityDataForYears` que retorna uma tupla de 6 que contém dados de dois anos diferentes para uma cidade. A chamada do método no exemplo é relacionada somente com os dois valores de população retornados pelo método e, por isso, trata os valores restantes na tupla como descartes ao desconstruir a tupla.

[!code-csharp[Tuple-discard](~/samples/snippets/csharp/programming-guide/deconstructing-tuples/discard-tuple1.cs)]

Para obter mais informações, consulte [Descartes](../discards.md).

## <a name="pattern-matching"></a>Correspondência de padrões

*Correspondência de padrões* é um recurso que permite que você implemente a expedição do método em propriedades diferentes do tipo de um objeto. Você provavelmente já está familiarizado com a expedição de método com base no tipo de um objeto. Na programação orientada a objeto, os métodos virtuais e de substituição fornecem a sintaxe da linguagem para implementar a expedição de método com base no tipo de um objeto. As classes base e derivada fornecem implementações diferentes.
As expressões de correspondência de padrões estendem esse conceito, de modo que você possa implementar com facilidade padrões de expedição semelhantes para elementos de dados e tipos que não são relacionados por meio de uma hierarquia de herança.

A correspondência de padrões tem suporte a expressões `is` e `switch`. Cada uma delas permite inspecionar um objeto e suas propriedades para determinar se esse objeto satisfaz o padrão procurado. Você usa a palavra-chave `when` para especificar regras adicionais para o padrão.

A `is` expressão de padrão estende o [ `is` operador](../language-reference/keywords/is.md#pattern-matching-with-is) familiar para consultar um objeto sobre seu tipo e atribuir o resultado em uma instrução. O seguinte código verifica se uma variável é um `int` e, nesse caso, adiciona-a à soma atual:

```csharp
if (input is int count)
    sum += count;
```

O pequeno exemplo anterior demonstra as melhorias na expressão `is`. Você pode realizar o teste nos tipos de valor, bem como nos tipos de referência, e atribuir o resultado com êxito a uma nova variável do tipo correto.

A expressão de correspondência switch tem uma sintaxe conhecida, baseada na instrução `switch` que já faz parte da linguagem C#. A instrução switch atualizada tem vários novos constructos:

- O tipo controlador de uma expressão `switch` não está mais restrito a tipos integrais, tipos `Enum`, `string` ou a um tipo que permite valor nulo correspondente a um desses tipos. Qualquer tipo pode ser usado.
- Teste o tipo da expressão `switch` em cada rótulo `case`. Assim como ocorre com a expressão `is`, você pode atribuir uma nova variável a esse tipo.
- Adicione uma cláusula `when` para testar mais condições de teste nessa variável.
- A ordem dos rótulos `case` agora é importante. O primeiro branch para correspondência é executado; os outros são ignorados.

O seguinte código demonstra esses novos recursos:

```csharp
public static int SumPositiveNumbers(IEnumerable<object> sequence)
{
    int sum = 0;
    foreach (var i in sequence)
    {
        switch (i)
        {
            case 0:
                break;
            case IEnumerable<int> childSequence:
            {
                foreach(var item in childSequence)
                    sum += (item > 0) ? item : 0;
                break;
            }
            case int n when n > 0:
                sum += n;
                break;
            case null:
                throw new NullReferenceException("Null found in sequence");
            default:
                throw new InvalidOperationException("Unrecognized type");
        }
    }
    return sum;
}
```

- `case 0:` é o padrão de constante conhecido.
- `case IEnumerable<int> childSequence:` é um padrão de tipo.
- `case int n when n > 0:` é um padrão de tipo com uma condição `when` adicional.
- `case null:` é o padrão de nulo.
- `default:` é o caso padrão conhecido.

Saiba mais sobre a correspondência de padrões em [Correspondência de padrões em C#](../pattern-matching.md).

## <a name="ref-locals-and-returns"></a>Locais e retornos de ref

Esse recurso permite que os algoritmos que usam e retornam referências para as variáveis definidas em outro lugar. Um exemplo é trabalhar com matrizes grandes e localizar um único local com determinadas características. O seguinte método retorna uma **referência** a esse armazenamento na matriz:

[!code-csharp[FindReturningRef](~/samples/snippets/csharp/new-in-7/MatrixSearch.cs#FindReturningRef "Find returning by reference")]

Você pode declarar o valor retornado como uma `ref` e modificar esse valor na matriz, conforme mostrado no seguinte código:

[!code-csharp[AssignRefReturn](~/samples/snippets/csharp/new-in-7/Program.cs#AssignRefReturn "Assign ref return")]

A linguagem C# tem várias regras que protegem contra o uso indevido de locais e retornos de `ref`:

- É necessário adicionar a palavra-chave `ref` à assinatura do método e a todas as instruções `return` em um método.
  - Isso torna claro que o método é retornado por referência em todo o método.
- Um `ref return` pode ser atribuído a uma variável de valor ou a uma variável `ref`.
  - O chamador controla se o valor retornado é copiado ou não. A omissão do modificador `ref` ao atribuir o valor retornado indica que o chamador deseja obter uma cópia do valor, não uma referência ao armazenamento.
- Não é possível atribuir um valor retornado do método padrão a uma variável local de `ref`.
  - Isso proíbe que instruções como `ref int i = sequence.Count();`
- Não é possível retornar um `ref` para uma variável cujo tempo de vida não se estende para além da execução do método.
  - Isso significa que não é possível retornar uma referência a uma variável local ou a uma variável com um escopo semelhante.
- O locais e retornos de `ref` não podem ser usados com métodos assíncronos.
  - O compilador não consegue saber se a variável referenciada foi definida com o valor final quando o método assíncrono retorna.

A adição de locais e retornos de ref permite algoritmos que são mais eficientes evitando a cópia de valores ou a execução múltipla de operações de desreferenciamento.

Adicionar `ref` ao valor retornado é uma [alteração compatível com a origem](version-update-considerations.md#source-compatible-changes). O código existente é compilado, mas o valor retornado ref é copiado quando atribuído. Os chamadores devem atualizar o armazenamento para o valor retornado para uma variável local `ref` para armazenar o retorno como uma referência.

Para saber mais, confira o artigo [Palavra-chave ref](../language-reference/keywords/ref.md).

## <a name="local-functions"></a>Funções locais

Muitos designs para classes incluem métodos que são chamados de apenas um local. Esses métodos privados adicionais mantêm cada método pequeno e focado. As *funções locais* permitem que você declare métodos dentro do contexto de outro método. Funções locais tornam mais fácil para os leitores da classe verem que o método local é chamado apenas do contexto em que é declarado.

Há dois casos de uso muito comuns para funções locais: métodos iteradores públicos e métodos assíncronos públicos. Ambos os tipos de métodos de geram código que relata os erros mais tarde do que os programadores podem esperar. Em métodos iteradores, as exceções são observadas apenas ao chamar o código que enumera a sequência retornada. Em métodos assíncronos, as exceções são observadas apenas quando a `Task` retornada é aguardada. O seguinte exemplo demonstra a validação de parâmetro de separação da implementação do iterador usando uma função local:

[!code-csharp[22_IteratorMethodLocal](~/samples/snippets/csharp/new-in-7/Iterator.cs#IteratorMethodLocal "Iterator method with local function")]

A mesma técnica pode ser empregada com métodos `async` para garantir que as exceções decorrentes da validação de argumento sejam lançadas antes de o trabalho assíncrono começar:

[!code-csharp[TaskExample](~/samples/snippets/csharp/new-in-7/AsyncWork.cs#TaskExample "Task returning method with local function")]

> [!NOTE]
> Alguns dos designs com suporte das funções locais também podem ser realizados usando *expressões lambda*. Para obter mais informações, consulte [funções locais versus expressões lambda](../programming-guide/classes-and-structs/local-functions.md#local-functions-vs-lambda-expressions).

## <a name="more-expression-bodied-members"></a>Mais membros aptos para expressão

O C# 6 introduziu [membros aptos para expressão](csharp-6.md#expression-bodied-function-members) para funções de membro e propriedades somente leitura. O C# 7.0 expande os membros permitidos que podem ser implementados como expressões. No C# 7.0, você pode implementar *construtores*, *finalizadores* e acessadores `get` e `set` em *propriedades* e *indexadores*. O código a seguir mostra exemplos de cada um:

[!code-csharp[ExpressionBodiedMembers](~/samples/snippets/csharp/new-in-7/expressionmembers.cs#ExpressionBodiedEverything "new expression-bodied members")]

> [!NOTE]
> Este exemplo não precisa de um finalizador, mas ele é mostrado para demonstrar a sintaxe. Você não deve implementar um finalizador em sua classe a menos que seja necessário para liberar recursos não gerenciados. Você também deve considerar o uso da classe <xref:System.Runtime.InteropServices.SafeHandle> em vez de gerenciar recursos não gerenciados diretamente.

Esses novos locais para membros aptos para expressão representam uma etapa importante para a linguagem C#: esses recursos foram implementados por membros da comunidade trabalhando no projeto [Roslyn](https://github.com/dotnet/Roslyn) de software livre.

A alteração de um método para um membro de corpo da expressão é uma [alteração compatível com binário](version-update-considerations.md#binary-compatible-changes).

## <a name="throw-expressions"></a>Expressões throw

No C#, `throw` sempre foi uma instrução. Como `throw` é uma instrução, não uma expressão, havia constructos do C# em que não era possível usá-la. Eles incluíam expressões condicionais, expressões de união nulas e algumas expressões lambda. A adição de membros aptos para expressão inclui mais locais em que as expressões `throw` seriam úteis. Para que você possa escrever qualquer um desses construtos, o C# 7.0 apresenta [*expressões throw*](../language-reference/keywords/throw.md#the-throw-expression).

Essa adição facilita a escrita de um código mais baseado em expressão. Você não precisa de instruções adicionais para a verificação de erros.

## <a name="generalized-async-return-types"></a>Tipos de retorno assíncrono generalizado

Retornar um objeto `Task` de métodos assíncronos pode introduzir gargalos de desempenho em determinados caminhos. `Task` é um tipo de referência, portanto, usá-lo significa alocar um objeto. Em casos em que um método declarado com o modificador `async` retorna um resultado armazenado em cache ou é concluído de forma síncrona, as alocações extras podem se tornar um custo de tempo significativo em seções críticas de desempenho de código. Isso pode se tornar caro se essas alocações ocorrem em loops rígidos.

O novo recurso de linguagem significa que os tipos de retorno do método assíncrono não se limitam a `Task`, `Task<T>` e `void`. O tipo retornado ainda deve satisfazer o padrão assíncrono, o que significa que um método `GetAwaiter` deve ser acessível. Como um exemplo concreto, o `ValueTask` tipo foi adicionado ao .net para fazer uso desse novo recurso de linguagem:

[!code-csharp[UsingValueTask](~/samples/snippets/csharp/new-in-7/AsyncWork.cs#UsingValueTask "Using ValueTask")]

> [!NOTE]
> Você precisa adicionar o pacote NuGet [`System.Threading.Tasks.Extensions`](https://www.nuget.org/packages/System.Threading.Tasks.Extensions/) para usar o <xref:System.Threading.Tasks.ValueTask%601> tipo.

Essa melhoria é mais útil para autores de biblioteca impedirem a alocação de uma `Task` em um código crítico para o desempenho.

## <a name="numeric-literal-syntax-improvements"></a>Aprimoramentos da sintaxe de literais numéricos

A leitura incorreta das constantes numéricas pode fazer com que seja difícil entender o código ao lê-lo pela primeira vez. Bitmasks ou outros valores simbólicos são propensos a equívocos. O C# 7.0 inclui dois novos recursos para a escrita de números da maneira mais legível possível para o uso pretendido: *literais binários* e *separadores de dígito*.

Para ocasiões em que você estiver criando bitmasks ou sempre que uma representação binária de um número tornar o código mais legível, escreva esse número em binário:

[!code-csharp[ThousandSeparators](~/samples/snippets/csharp/new-in-7/Program.cs#ThousandSeparators "Thousands separators")]

O `0b` no início da constante indica que o número é escrito como um número binário. Números binários podem ficar longos e, portanto, é mais fácil ver os padrões de bit introduzindo o `_` como um separador de dígito, conforme mostrado acima na constante binária. O separador de dígitos pode aparecer em qualquer lugar na constante. Para números de base 10, é comum usá-lo como um separador de milhar:

[!code-csharp[LargeIntegers](~/samples/snippets/csharp/new-in-7/Program.cs#LargeIntegers "Large integer")]

O separador de dígito pode ser usado com os tipos `decimal`, `float` e `double` também:

[!code-csharp[OtherConstants](~/samples/snippets/csharp/new-in-7/Program.cs#OtherConstants "non-integral constants")]

Juntos, você pode declarar constantes numéricas com muito mais facilidade de leitura.
