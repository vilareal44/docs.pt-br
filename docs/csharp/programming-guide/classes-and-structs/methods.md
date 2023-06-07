---
title: Métodos – Guia de Programação em C#
description: Um método em C# é um bloco de código que contém uma série de instruções. Um programa executa as instruções chamando o método e especificando argumentos.
ms.date: 07/20/2015
helpviewer_keywords:
- methods [C#]
- C# language, methods
ms.assetid: cc738f07-e8cd-4683-9585-9f40c0667c37
ms.openlocfilehash: db35b48d4d7e70a54b38342e79fa2881b3857bd7
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864144"
---
# <a name="methods-c-programming-guide"></a>Métodos (Guia de Programação em C#)

Um método é um bloco de código que contém uma série de instruções. Um programa faz com que as instruções sejam executadas chamando o método e especificando os argumentos de método necessários. No C#, todas as instruções executadas são realizadas no contexto de um método. O `Main` método é o ponto de entrada para cada aplicativo C# e é chamado pelo Common Language Runtime (CLR) quando o programa é iniciado.

> [!NOTE]
> Este artigo discute os métodos nomeados. Para obter informações sobre funções anônimas, consulte [Funções anônimas](../statements-expressions-operators/anonymous-functions.md).

## <a name="method-signatures"></a>Assinaturas de método

Os métodos são declarados em uma [classe](../../language-reference/keywords/class.md), [struct](../../language-reference/builtin-types/struct.md)ou [interface](../interfaces/index.md) especificando o nível de acesso, como `public` ou `private` , modificadores opcionais, como `abstract` ou `sealed` , o valor de retorno, o nome do método e qualquer parâmetro de método. Juntas, essas partes são a assinatura do método.

> [!NOTE]
> Um tipo de retorno de um método não faz parte da assinatura do método para fins de sobrecarga de método. No entanto, ele faz parte da assinatura do método ao determinar a compatibilidade entre um delegado e o método para o qual ele aponta.

Os parâmetros de método estão entre parênteses e separados por vírgulas. Parênteses vazios indicam que o método não requer parâmetros. Essa classe contém quatro métodos:

[!code-csharp[csProgGuideObjects#40](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#40)]

## <a name="method-access"></a>Acesso de método

Chamar um método em um objeto é como acessar um campo. Após o nome do objeto, adicione um ponto final, o nome do método e parênteses. Os argumentos são listados dentro dos parênteses e são separados por vírgulas. Os métodos da classe `Motorcycle` podem, portanto, ser chamados como no exemplo a seguir:

[!code-csharp[csProgGuideObjects#41](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#41)]

## <a name="method-parameters-vs-arguments"></a>Parâmetros do método vs. argumentos

A definição do método especifica os nomes e tipos de quaisquer parâmetros obrigatórios. Quando o código de chamada chama o método, ele fornece valores concretos, chamados argumentos, para cada parâmetro. Os argumentos devem ser compatíveis com o tipo de parâmetro, mas o nome do argumento (se houver) usado no código de chamada não precisa ser o mesmo que o parâmetro nomeado definido no método. Por exemplo:

[!code-csharp[csProgGuideObjects#74](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#74)]

## <a name="passing-by-reference-vs-passing-by-value"></a>Passando por referência vs. passando por valor

Por padrão, quando uma instância de um [tipo de valor](../../language-reference/builtin-types/value-types.md) é passada para um método, sua cópia é passada em vez da própria instância. Portanto, as alterações no argumento não têm efeito sobre a instância original no método de chamada. Para passar uma instância de tipo de valor por referência, use a `ref` palavra-chave. Para obter mais informações, consulte [Passando parâmetros de tipo de valor](./passing-value-type-parameters.md).

Quando um objeto de tipo de referência é passado para um método, uma referência ao objeto é passada. Ou seja, o método recebe não o objeto em si, mas um argumento que indica o local do objeto. Se você alterar um membro do objeto usando essa referência, a alteração será refletida no argumento no método de chamada, ainda que você passe o objeto por valor.

Você cria um tipo de referência usando a `class` palavra-chave, como mostra o exemplo a seguir:

[!code-csharp[csProgGuideObjects#42](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#42)]

Agora, se você passar um objeto com base nesse tipo para um método, uma referência ao objeto será passada. O exemplo a seguir passa um objeto do tipo `SampleRefType` para o método `ModifyObject` :

[!code-csharp[csProgGuideObjects#75](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#75)]

O exemplo faz essencialmente a mesma coisa que o exemplo anterior, pois ele passa um argumento por valor para um método. No entanto, como um tipo de referência é usado, o resultado é diferente. A modificação feita em `ModifyObject` para o campo `value` do parâmetro, `obj`, também altera o campo `value` do argumento, `rt`, no método `TestRefType`. O método `TestRefType` exibe 33 como a saída.

Para obter mais informações sobre como passar tipos de referência por referência e por valor, consulte [Passando parâmetros de tipo de referência](./passing-reference-type-parameters.md) e [Tipos de referência](../../language-reference/keywords/reference-types.md).

## <a name="return-values"></a>Valores retornados

Os métodos podem retornar um valor para o chamador. Se o tipo de retorno, o tipo listado antes do nome do método, não for `void`, o método poderá retornar o valor usando a palavra-chave `return`. Uma instrução com a palavra-chave `return` seguida por uma variável que corresponde ao tipo de retorno retornará esse valor ao chamador do método.

O valor pode ser retornado ao chamador por valor ou, começando com o C# 7.0, [por referência](ref-returns.md). Valores são retornados ao chamador por referência se a `ref` palavra-chave é usada na assinatura do método e segue cada palavra-chave `return`. Por exemplo, a instrução de retorno e a assinatura de método a seguir indicam que o método retorna uma variável chamada `estDistance` por referência para o chamador.

```csharp
public ref double GetEstimatedDistance()
{
    return ref estDistance;
}
```

A palavra-chave `return` também interrompe a execução do método. Se o tipo de retorno for `void`, uma instrução `return` sem um valor ainda será útil para interromper a execução do método. Sem a palavra-chave `return`, a execução do método será interrompida quando chegar ao final do bloco de código. Métodos com um tipo de retorno não nulo devem usar a palavra-chave `return` para retornar um valor. Por exemplo, esses dois métodos usam a palavra-chave `return` para retornar inteiros:

[!code-csharp[csProgGuideObjects#44](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#44)]

Para usar um valor retornado de um método, o método de chamada pode usar a chamada de método em si em qualquer lugar que um valor do mesmo tipo seria suficiente. Você também pode atribuir o valor retornado a uma variável. Por exemplo, os dois exemplos de código a seguir obtêm a mesma meta:

[!code-csharp[csProgGuideObjects#45](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#45)]

[!code-csharp[csProgGuideObjects#46](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#46)]

Usar uma variável local, nesse caso, `result`, para armazenar um valor é opcional. Isso pode ajudar a legibilidade do código ou pode ser necessário se você precisar armazenar o valor original do argumento para todo o escopo do método.

Para usar o valor retornado de um método por referência, você deve declarar uma variável [ref local](ref-returns.md#ref-locals) se você pretende modificar seu valor. Por exemplo, se o método `Planet.GetEstimatedDistance` retorna um valor <xref:System.Double> por referência, você pode defini-lo como uma variável ref local com código semelhante ao seguinte:

```csharp
ref int distance = plant
```

Retornar uma matriz multidimensional de um método, `M`, que modifica o conteúdo da matriz, não é necessário se a função de chamada passou a matriz para `M`.  Você pode retornar a matriz resultante de `M` para um bom estilo ou fluxo funcional de valores, mas isso não é necessário porque o C# passa todos os tipos de referência por valor e o valor de uma referência de matriz é o ponteiro para a matriz. No método `M` , qualquer alteração no conteúdo da matriz é observável por qualquer código que tenha uma referência à matriz, conforme mostrado no exemplo a seguir:

```csharp
static void Main(string[] args)
{
    int[,] matrix = new int[2, 2];
    FillMatrix(matrix);
    // matrix is now full of -1
}

public static void FillMatrix(int[,] matrix)
{
    for (int i = 0; i < matrix.GetLength(0); i++)
    {
        for (int j = 0; j < matrix.GetLength(1); j++)
        {
            matrix[i, j] = -1;
        }
    }
}
```

Para obter mais informações, consulte [return](../../language-reference/keywords/return.md).

## <a name="async-methods"></a>Métodos assíncronos

Usando o recurso async, você pode invocar métodos assíncronos sem usar retornos de chamada explícitos ou dividir manualmente seu código entre vários métodos ou expressões lambda.

Se marcar um método com o modificador [async](../../language-reference/keywords/async.md), você poderá usar o operador [await](../../language-reference/operators/await.md) no método. Quando o controle atinge uma expressão await no método assíncrono, ele retorna para o chamador e o progresso no método é suspenso até a tarefa aguardada ser concluída. Quando a tarefa for concluída, a execução poderá ser retomada no método.

> [!NOTE]
> Um método assíncrono retorna para o chamador quando encontra o primeiro objeto esperado que ainda não está completo ou chega ao final do método assíncrono, o que ocorrer primeiro.

Um método assíncrono pode conter um tipo de retorno <xref:System.Threading.Tasks.Task%601>, <xref:System.Threading.Tasks.Task> ou nulo. O tipo de retorno nulo é usado principalmente para definir manipuladores de eventos, em que um tipo de retorno nulo é necessário. Um método assíncrono que retorna nulo não pode ser aguardado e o chamador de um método de retorno nulo não pode capturar as exceções que esse método gera.

No exemplo a seguir, `DelayAsync` é um método assíncrono que tem um tipo de retorno de <xref:System.Threading.Tasks.Task%601>. `DelayAsync` tem uma instrução `return` que retorna um número inteiro. Portanto, a declaração do método de `Task<int>` deve ter um tipo de retorno de `DelayAsync`. Como o tipo de retorno é `Task<int>`, a avaliação da expressão `await` em `DoSomethingAsync` produz um inteiro, como a instrução a seguir demonstra: `int result = await delayTask`.

O método `startButton_Click` é um exemplo de método assíncrono que tem um tipo de retorno nulo. Como `DoSomethingAsync` é um método assíncrono, a tarefa para a chamada para `DoSomethingAsync` deve ser colocada em espera, como mostra a seguinte instrução: `await DoSomethingAsync();`. O método `startButton_Click` deve ser definido com o modificador `async` porque o método tem uma expressão `await`.

[!code-csharp[csAsyncMethod#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csasyncmethod/cs/mainwindow.xaml.cs#2)]

Um método assíncrono não pode declarar nenhum parâmetro [ref](../../language-reference/keywords/ref.md) ou [out](../../language-reference/keywords/out-parameter-modifier.md), mas pode chamar métodos com tais parâmetros.

Para obter mais informações sobre métodos assíncronos, consulte [programação assíncrona com Async e Await](../concepts/async/index.md), [fluxo de controle em programas assíncronos](../concepts/async/control-flow-in-async-programs.md)e [tipos de retorno assíncronos](../concepts/async/async-return-types.md).

## <a name="expression-body-definitions"></a>Definições de corpo de expressão

É comum ter definições de método que simplesmente retornam imediatamente com o resultado de uma expressão ou que têm uma única instrução como o corpo do método. Há um atalho de sintaxe para definir esses métodos usando `=>`:

```csharp
public Point Move(int dx, int dy) => new Point(x + dx, y + dy);
public void Print() => Console.WriteLine(First + " " + Last);
// Works with operators, properties, and indexers too.
public static Complex operator +(Complex a, Complex b) => a.Add(b);
public string Name => First + " " + Last;
public Customer this[long id] => store.LookupCustomer(id);
```

Se o método retornar `void` ou for um método assíncrono, o corpo do método deverá ser uma expressão de instrução (igual às lambdas). Para propriedades e indexadores, eles devem ser somente leitura e você não usa a palavra-chave do acessador `get`.

## <a name="iterators"></a>Iterators

Um iterador realiza uma iteração personalizada em uma coleção, como uma lista ou uma matriz. Um iterador usa a instrução [yield return](../../language-reference/keywords/yield.md) para retornar um elemento de cada vez. Quando uma instrução [yield return](../../language-reference/keywords/yield.md) for alcançada, o local atual no código será lembrado. A execução será reiniciada desse local quando o iterador for chamado na próxima vez.

Você chama um iterador de um código de cliente usando uma instrução [foreach](../../language-reference/keywords/foreach-in.md).

O tipo de retorno de um iterador pode ser <xref:System.Collections.IEnumerable>, <xref:System.Collections.Generic.IEnumerable%601>, <xref:System.Collections.IEnumerator> ou <xref:System.Collections.Generic.IEnumerator%601>.

Para obter mais informações, consulte [Iteradores](../concepts/iterators.md).

## <a name="c-language-specification"></a>especificação da linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Classes e structs](index.md)
- [Modificadores de acesso](access-modifiers.md)
- [Classes static e membros de classes static](static-classes-and-static-class-members.md)
- [Herança](inheritance.md)
- [Classes e membros de classes abstract e sealed](abstract-and-sealed-classes-and-class-members.md)
- [params](../../language-reference/keywords/params.md)
- [return](../../language-reference/keywords/return.md)
- [fora](../../language-reference/keywords/out.md)
- [ref](../../language-reference/keywords/ref.md)
- [Passando parâmetros](passing-parameters.md)
