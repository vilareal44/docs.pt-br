---
title: Métodos – Guia de C#
description: Visão geral dos métodos, parâmetros de método e valores retornados de método
ms.technology: csharp-fundamentals
ms.date: 05/21/2018
ms.assetid: 577a8527-1081-4b36-9b9e-0685b6553c6e
ms.openlocfilehash: 09a287b3d74e1b8dbdaf4a53cb207dfe1fad8a0c
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063348"
---
# <a name="methods"></a>Métodos

Um método é um bloco de código que contém uma série de instruções. Um programa faz com que as instruções sejam executadas chamando o método e especificando os argumentos de método necessários. No C#, todas as instruções executadas são realizadas no contexto de um método. O método `Main` é o ponto de entrada para todos os aplicativos C# e é chamado pelo CLR (Common Language Runtime) quando o programa é iniciado.

> [!NOTE]
> Este tópico aborda os métodos nomeados. Para obter informações sobre funções anônimas, consulte [Funções anônimas](programming-guide/statements-expressions-operators/anonymous-functions.md).

<a name="signatures"></a>

## <a name="method-signatures"></a>Assinaturas de método

Os métodos são declarados em uma `class` ou `struct` especificando:

- Um nível de acesso opcional, como `public` ou `private`. O padrão é `private`.
- Modificadores opcionais como `abstract` ou `sealed`.
- O valor retornado ou `void` se o método não tiver nenhum.
- O nome do método.
- Quaisquer parâmetros de método. Os parâmetros de método estão entre parênteses e separados por vírgulas. Parênteses vazios indicam que o método não requer parâmetros.

Essas partes juntas formam a assinatura do método.

> [!NOTE]
> Um tipo de retorno de um método não faz parte da assinatura do método para fins de sobrecarga de método. No entanto, ele faz parte da assinatura do método ao determinar a compatibilidade entre um delegado e o método para o qual ele aponta.

O exemplo a seguir define uma classe chamada `Motorcycle` que contém cinco métodos:

[!code-csharp[csSnippets.Methods#40](../../samples/snippets/csharp/concepts/methods/methods40.cs#40)]

Observe que a classe `Motorcycle` inclui um método sobrecarregado, `Drive`. Dois métodos têm o mesmo nome, mas devem ser diferenciados por seus tipos de parâmetro.

<a name="invocation"></a>

## <a name="method-invocation"></a>Invocação de método

Os métodos podem ser de *instância* ou *estáticos*. Invocar um método de instância requer que você crie uma instância de um objeto e chame o método nesse objeto. Um método de instância opera nessa instância e seus dados. Você invoca um método estático referenciando o nome do tipo ao qual o método pertence; os métodos estáticos não operam nos dados da instância. Tentar chamar um método estático por meio de uma instância do objeto gera um erro do compilador.

Chamar um método é como acessar um campo. Após o nome do objeto (se você estiver chamando um método de instância) ou o nome do tipo (se você estiver chamando um método `static`), adicione um ponto, o nome do método e parênteses. Os argumentos são listados dentro dos parênteses e são separados por vírgulas.

A definição do método especifica os nomes e tipos de quaisquer parâmetros obrigatórios. Quando um chamador invoca o método, ele fornece valores concretos, chamados argumentos, para cada parâmetro. Os argumentos devem ser compatíveis com o tipo de parâmetro, mas o nome do argumento, se for usado no código de chamada, não precisa ser o mesmo que o parâmetro denominado definido no método. No exemplo a seguir, o método `Square` inclui um único parâmetro do tipo `int` chamado *i*. A primeira chamada do método passa para o método `Square` uma variável do tipo `int` chamada *num*, a segunda, uma constante numérica e a terceira, uma expressão.

[!code-csharp[csSnippets.Methods#74](../../samples/snippets/csharp/concepts/methods/params74.cs#74)]

A forma mais comum de invocação de método usa argumentos posicionais, ela fornece os argumentos na mesma ordem que os parâmetros de método. Os métodos da classe `Motorcycle`, podem, portanto, ser chamados como no exemplo a seguir. A chamada para o método `Drive`, por exemplo, inclui dois argumentos que correspondem aos dois parâmetros na sintaxe do método. O primeiro se torna o valor do parâmetro `miles`, o segundo o valor do parâmetro `speed`.

[!code-csharp[csSnippets.Methods#41](../../samples/snippets/csharp/concepts/methods/methods40.cs#41)]

Você também pode usar *argumentos nomeados* em vez de argumentos posicionais ao invocar um método. Ao usar argumentos nomeados, você especifica o nome do parâmetro seguido por dois pontos (":") e o argumento. Os argumentos do método podem aparecer em qualquer ordem, desde que todos os argumentos necessários estejam presentes. O exemplo a seguir usa argumentos nomeados para invocar o método `TestMotorcycle.Drive`. Neste exemplo, os argumentos nomeados são passados na ordem oposta da lista de parâmetros do método.

[!code-csharp[csSnippets.Methods#45](../../samples/snippets/csharp/concepts/methods/named1.cs#45)]

Você pode invocar um método usando argumentos posicionais e argumentos nomeados. No entanto, um argumento posicional não pode seguir um argumento nomeado. O exemplo a seguir invoca o método `TestMotorcycle.Drive` do exemplo anterior usando um argumento posicional e um argumento nomeado.

[!code-csharp[csSnippets.Methods#46](../../samples/snippets/csharp/concepts/methods/named2.cs#46)]

<a name="inherited"></a>

## <a name="inherited-and-overridden-methods"></a>Métodos herdados e substituídos

Além dos membros que são definidos explicitamente em um tipo, um tipo herda membros definidos em suas classes base. Como todos os tipos no sistema de tipos gerenciado são herdados direta ou indiretamente da classe <xref:System.Object>, todos os tipos herdam seus membros, como <xref:System.Object.Equals(System.Object)>, <xref:System.Object.GetType> e <xref:System.Object.ToString>. O exemplo a seguir define uma classe `Person`, instancia dois objetos `Person` e chama o método `Person.Equals` para determinar se os dois objetos são iguais. O método `Equals`, no entanto, não é definido na classe `Person`, ele é herdado do <xref:System.Object>.

[!code-csharp[csSnippets.Methods#104](../../samples/snippets/csharp/concepts/methods/inherited1.cs#104)]

Tipos podem substituir membros herdados usando a palavra-chave `override` e fornecendo uma implementação para o método substituído. A assinatura do método precisa ser igual à do método substituído. O exemplo a seguir é semelhante ao anterior, exceto que ele substitui o método <xref:System.Object.Equals(System.Object)>. (Ele também substitui o método <xref:System.Object.GetHashCode>, uma vez que os dois métodos destinam-se a fornecer resultados consistentes.)

[!code-csharp[csSnippets.Methods#105](../../samples/snippets/csharp/concepts/methods/overridden1.cs#105)]

<a name="passing"></a>

## <a name="passing-parameters"></a>Passando parâmetros

Os tipos no C# são *tipos de valor* ou *tipos de referência*. Para obter uma lista de tipos de valor internos, consulte [tipos](./tour-of-csharp/types.md). Por padrão, os tipos de referência e tipos de valor são passados para um método por valor.

<a name="byval"></a>

### <a name="passing-parameters-by-value"></a>Passando parâmetros por valor

Quando um tipo de valor é passado para um método por valor, uma cópia do objeto, em vez do próprio objeto, é passada para o método. Portanto, as alterações no objeto do método chamado não têm efeito no objeto original quando o controle retorna ao chamador.

O exemplo a seguir passa um tipo de valor para um método por valor e o método chamado tenta alterar o valor do tipo de valor. Ele define uma variável do tipo `int`, que é um tipo de valor, inicializa o valor para 20 e o passa para um método chamado `ModifyValue` que altera o valor da variável para 30. No entanto, quando o método retorna, o valor da variável permanece inalterado.

[!code-csharp[csSnippets.Methods#10](../../samples/snippets/csharp/concepts/methods/byvalue10.cs#10)]

Quando um objeto do tipo de referência é passado para um método por valor, uma referência ao objeto é passada por valor. Ou seja, o método recebe não o objeto em si, mas um argumento que indica o local do objeto. Se você alterar um membro do objeto usando essa referência, a alteração será refletida no objeto quando o controle retornar para o método de chamada. No entanto, substituir o objeto passado para o método não tem efeito no objeto original quando o controle retorna para o chamador.

O exemplo a seguir define uma classe (que é um tipo de referência) chamada `SampleRefType`. Ele cria uma instância de um objeto `SampleRefType`, atribui 44 ao seu campo `value` e passa o objeto para o método `ModifyObject`. Este exemplo faz essencialmente a mesma coisa que o exemplo anterior, ele passa um argumento por valor para um método. Mas como um tipo de referência é usado, o resultado é diferente. A modificação feita em `ModifyObject` para o campo `obj.value` também muda o campo `value` do argumento, `rt`, no método `Main` para 33, como a saída do exemplo mostra.

[!code-csharp[csSnippets.Methods#42](../../samples/snippets/csharp/concepts/methods/byvalue42.cs#42)]

<a name="byref"></a>

### <a name="passing-parameters-by-reference"></a>Passando parâmetros por referência

Você passa um parâmetro por referência quando deseja alterar o valor de um argumento em um método e deseja refletir essa alteração quando o controle retorna para o método de chamada. Para passar um parâmetro por referência, use a [`ref`](language-reference/keywords/ref.md) [`out`](language-reference/keywords/out-parameter-modifier.md) palavra-chave ou. Você também pode passar um valor por referência para evitar copiar, mas ainda evitar modificações usando a [`in`](language-reference/keywords/in-parameter-modifier.md) palavra-chave.

O exemplo a seguir é idêntico ao anterior, exceto que o valor é passado por referência para o método `ModifyValue`. Quando o valor do parâmetro é modificado no método `ModifyValue`, a alteração no valor é refletida quando o controle retorna ao chamador.

[!code-csharp[csSnippets.Methods#106](../../samples/snippets/csharp/concepts/methods/byref106.cs#106)]

Um padrão comum que usa parâmetros pela referência envolve a troca os valores das variáveis. Você passa duas variáveis para um método por referência e o método troca seus conteúdos. O exemplo a seguir troca valores inteiros.

[!code-csharp[csSnippets.Methods#106](../../samples/snippets/csharp/concepts/methods/swap107.cs#107)]

Passar um parâmetro de tipo de referência permite que você altere o valor da própria referência, em vez de o valor de seus campos ou elementos individuais.

<a name="paramarray"></a>

### <a name="parameter-arrays"></a>Matrizes de parâmetros

Às vezes, o requisito de que você especifique o número exato de argumentos para o método é restritivo. Usando a palavra-chave `params` para indicar que um parâmetro é uma matriz de parâmetros, você permite que o método seja chamado com um número variável de argumentos. O parâmetro marcado com a palavra-chave `params` deve ser um tipo de matriz e ele deve ser o último parâmetro na lista de parâmetros do método.

Um chamador pode, então, invocar o método de uma das três maneiras:

- Passando uma matriz do tipo apropriado que contém o número de elementos desejado.
- Passando uma lista separada por vírgulas de argumentos individuais do tipo apropriado para o método.
- Não fornecendo um argumento para a matriz de parâmetros.

O exemplo a seguir define um método chamado `GetVowels` que retorna todas as vogais de uma matriz de parâmetros. O método `Main` ilustra todas as três maneiras de invocar o método. Os chamadores não precisam fornecer argumentos para parâmetros que incluem o modificador `params`. Nesse caso, o parâmetro é `null`.

[!code-csharp[csSnippets.Methods#75](~/samples/snippets/csharp/concepts/methods/params75.cs#75)]

<a name="optional"></a>

## <a name="optional-parameters-and-arguments"></a>Parâmetros e argumentos opcionais

Uma definição de método pode especificar que os parâmetros são obrigatórios ou que são opcionais. Por padrão, os parâmetros são obrigatórios. Os parâmetros opcionais são especificados incluindo o valor padrão do parâmetro na definição do método. Quando o método for chamado, se nenhum argumento for fornecido para um parâmetro opcional, o valor padrão será usado em vez disso.

O valor padrão do parâmetro deve ser atribuído por um dos tipos de expressões a seguir:

- Uma constante, como um número ou uma cadeia de caracteres literal.
- Uma expressão da forma `new ValType()`, em que `ValType` é um tipo de valor. Isso invoca o construtor sem parâmetros implícito do tipo de valor, que não é de fato um membro do tipo.
- Uma expressão da forma `default(ValType)`, em que `ValType` é um tipo de valor.

Se um método inclui parâmetros obrigatórios e opcionais, os parâmetros opcionais são definidos no final da lista de parâmetros, após todos os parâmetros obrigatórios.

O exemplo a seguir define um método, `ExampleMethod`, que tem um parâmetro obrigatório e dois opcionais.

[!code-csharp[csSnippets.Methods#21](../../samples/snippets/csharp/concepts/methods/optional1.cs#21)]

Se um método com vários argumentos opcionais for invocado usando argumentos posicionais, o chamador deverá fornecer um argumento para todos os parâmetros opcionais do primeiro ao último para o qual um argumento é fornecido. No caso do método `ExampleMethod`, por exemplo, se o chamador fornecer um argumento para o parâmetro `description`, ele deverá fornecer também um para o parâmetro `optionalInt`. `opt.ExampleMethod(2, 2, "Addition of 2 and 2");` é uma chamada de método válida, `opt.ExampleMethod(2, , "Addition of 2 and 0");` gera um erro do compilador de “Argumento ausente”.

Se um método for chamado usando argumentos nomeados ou uma combinação de argumentos posicionais e nomeados, o chamador poderá omitir todos os argumentos após o último argumento posicional na chamada do método.

A exemplo a seguir chama o método `ExampleMethod` três vezes.  As duas primeiras chamadas de método usam argumentos posicionais. O primeiro omite ambos os argumentos opcionais, enquanto o segundo omite o último argumento. A terceira chamada de método fornece um argumento posicional para o parâmetro obrigatório, mas usa um argumento nomeado para fornecer um valor para o parâmetro `description` enquanto omite o argumento `optionalInt`.

[!code-csharp[csSnippets.Methods#22](../../samples/snippets/csharp/concepts/methods/optional1.cs#22)]

O uso de parâmetros opcionais afeta a *resolução de sobrecarga* ou a maneira em que o compilador C# determina qual sobrecarga específica deve ser invocada pela invocada de método, da seguinte maneira:

- Um método, indexador ou construtor é um candidato para a execução se cada um dos parâmetros é opcional ou corresponde, por nome ou posição, a um único argumento na instrução de chamada e esse argumento pode ser convertido para o tipo do parâmetro.
- Se mais de um candidato for encontrado, as regras de resolução de sobrecarga de conversões preferenciais serão aplicadas aos argumentos que são especificados explicitamente. Os argumentos omitidos para parâmetros opcionais são ignorados.
- Se dois candidatos são considerados igualmente bons, a preferência vai para um candidato que não tem parâmetros opcionais para os quais argumentos foram omitidos na chamada. Esta é uma consequência da preferência geral na resolução de sobrecarga de candidatos que têm menos parâmetros.

<a name="return"></a>

## <a name="return-values"></a>Valores retornados

Os métodos podem retornar um valor para o chamador. Se o tipo de retorno (o tipo listado antes do nome do método) não for `void`, o método poderá retornar o valor usando palavra-chave `return`. Uma instrução com a palavra-chave `return` seguida por uma variável, constante ou expressão que corresponde ao tipo de retorno retornará esse valor para o chamador do método. Métodos com um tipo de retorno não nulo devem usar a palavra-chave `return` para retornar um valor. A palavra-chave `return` também interrompe a execução do método.

Se o tipo de retorno for `void`, uma instrução `return` sem um valor ainda será útil para interromper a execução do método. Sem a palavra-chave `return`, a execução do método será interrompida quando chegar ao final do bloco de código.

Por exemplo, esses dois métodos usam a palavra-chave `return` para retornar inteiros:

[!code-csharp[csSnippets.Methods#44](../../samples/snippets/csharp/concepts/methods/return44.cs#44)]

Para usar um valor retornado de um método, o método de chamada pode usar a chamada de método em si em qualquer lugar que um valor do mesmo tipo seria suficiente. Você também pode atribuir o valor retornado a uma variável. Por exemplo, os dois exemplos de código a seguir obtêm a mesma meta:

[!code-csharp[csSnippets.Methods#45](../../samples/snippets/csharp/concepts/methods/return44.cs#45)]

[!code-csharp[csSnippets.Methods#46](../../samples/snippets/csharp/concepts/methods/return44.cs#46)]

Usar uma variável local, nesse caso, `result`, para armazenar um valor é opcional. Isso pode ajudar a legibilidade do código ou pode ser necessário se você precisar armazenar o valor original do argumento para todo o escopo do método.

Às vezes, você deseja que seu método retorne mais de um único valor. A partir do C# 7.0, você pode fazer isso facilmente usando *tipos de tupla* e *literais de tupla*. O tipo de tupla define os tipos de dados dos elementos da tupla. Os literais de tupla fornecem os valores reais da tupla retornada. No exemplo a seguir, `(string, string, string, int)` define o tipo de tupla que é retornado pelo método `GetPersonalInfo`. A expressão `(per.FirstName, per.MiddleName, per.LastName, per.Age)` é a tupla literal, o método retorna o nome, o nome do meio e o sobrenome, juntamente com a idade, de um objeto `PersonInfo`.

```csharp
public (string, string, string, int) GetPersonalInfo(string id)
{
    PersonInfo per = PersonInfo.RetrieveInfoById(id);
    return (per.FirstName, per.MiddleName, per.LastName, per.Age);
}
```

O chamador pode então consumir a tupla retornada com o código semelhante ao seguinte:

```csharp
var person = GetPersonalInfo("111111111")
Console.WriteLine("{person.Item1} {person.Item3}: age = {person.Item4}");
```

Os nomes também podem ser atribuídos aos elementos da tupla na definição de tipo de tupla. O exemplo a seguir mostra uma versão alternativa do método `GetPersonalInfo` que usa elementos nomeados:

```csharp
public (string FName, string MName, string LName, int Age) GetPersonalInfo(string id)
{
    PersonInfo per = PersonInfo.RetrieveInfoById(id);
    return (per.FirstName, per.MiddleName, per.LastName, per.Age);
}
```

A chamada anterior para o método `GetPersonInfo` pode ser modificada da seguinte maneira:

```csharp
var person = GetPersonalInfo("111111111");
Console.WriteLine("{person.FName} {person.LName}: age = {person.Age}");
```

Se um método passa uma matriz como um argumento e modifica o valor de elementos individuais, o método não precisa retornar a matriz, embora você possa optar por fazer isso para obter um bom estilo ou um fluxo de valores funcional.  Isso ocorre porque o C# passa todos os tipos de referência por valor e o valor de uma referência de matriz é o ponteiro para a matriz. No exemplo a seguir, as alterações no conteúdo da matriz `values` realizados pelo método `DoubleValues` são observáveis por qualquer código que faz referência à matriz.

[!code-csharp[csSnippets.Methods#101](../../samples/snippets/csharp/concepts/methods/returnarray1.cs#101)]

<a name="extension"></a>

## <a name="extension-methods"></a>Métodos de extensão

Normalmente, há duas maneiras de adicionar um método a um tipo existente:

- Modificar o código-fonte para esse tipo. Você não pode fazer isso, é claro, se não possui o código-fonte do tipo. E isso se torna uma alteração significativa se você também adicionar campos de dados privados para dar suporte ao método.
- Definir o novo método em uma classe derivada. Não é possível adicionar um método dessa forma usando a herança para outros tipos, como estruturas e enumerações. Isso também não pode ser usado para “adicionar” um método a uma classe selada.

Os métodos de extensão permitem que você “adicione” um método a um tipo existente sem modificar o tipo em si ou implementar o novo método em um tipo herdado. O método de extensão também não precisa residir no mesmo assembly que o tipo que ele estende. Você chama um método de extensão como se fosse um membro definido de um tipo.

Para obter mais informações, consulte [Métodos de extensão](programming-guide/classes-and-structs/extension-methods.md).

<a name="async"></a>

## <a name="async-methods"></a>Métodos assíncronos

Usando o recurso async, você pode invocar métodos assíncronos sem usar retornos de chamada explícitos ou dividir manualmente seu código entre vários métodos ou expressões lambda.

Se marcar um método com o modificador [async](language-reference/keywords/async.md), você poderá usar o operador [await](language-reference/operators/await.md) no método. Quando o controle atingir uma expressão `await` no método assíncrono, o controle retornará para o chamador se a tarefa aguardada não estiver concluída e o progresso no método com a palavra-chave `await` será suspenso até a tarefa aguardada ser concluída. Quando a tarefa for concluída, a execução poderá ser retomada no método.

> [!NOTE]
> Um método assíncrono retorna para o chamador quando encontra o primeiro objeto esperado que ainda não está completo ou chega ao final do método assíncrono, o que ocorrer primeiro.

Um método assíncrono pode ter um tipo de retorno <xref:System.Threading.Tasks.Task%601>, <xref:System.Threading.Tasks.Task> ou `void`. O tipo de retorno `void` é usado principalmente para definir manipuladores de eventos, nos quais o tipo de retorno `void` é necessário. Um método assíncrono que retorna `void` não pode ser aguardado e o chamador de um método de retorno nulo não pode capturar as exceções que esse método gera. Começando com o C# 7.0, um método assíncrono pode ter [qualquer tipo de retorno como os de tarefa](./whats-new/csharp-7.md#generalized-async-return-types).

No exemplo a seguir, `DelayAsync` é um método assíncrono que contém uma instrução return que retorna um inteiro. Como é um método assíncrono, sua declaração de método deve ter um tipo de retorno de `Task<int>`. Como o tipo de retorno é `Task<int>`, a avaliação da expressão `await` em `DoSomethingAsync` produz um inteiro, como a instrução `int result = await delayTask` a seguir demonstra.

[!code-csharp[csSnippets.Methods#102](../../samples/snippets/csharp/concepts/methods/async1.cs#102)]

Um método assíncrono não pode declarar os parâmetros [in](language-reference/keywords/in-parameter-modifier.md), [ref](language-reference/keywords/ref.md) nem [out](language-reference/keywords/out-parameter-modifier.md), mas pode chamar métodos que tenham esses parâmetros.

 Para obter mais informações sobre os métodos assíncronos, consulte [Programação assíncrona com async e await](async.md), [Fluxo de controle em programas assíncronos](programming-guide/concepts/async/control-flow-in-async-programs.md) e [Tipos de retorno assíncronos](programming-guide/concepts/async/async-return-types.md).

<a name="expr"></a>

## <a name="expression-bodied-members"></a>Membros aptos para expressão

É comum ter definições de método que simplesmente retornam imediatamente com o resultado de uma expressão ou que têm uma única instrução como o corpo do método.  Há um atalho de sintaxe para definir esses métodos usando `=>`:

```csharp
public Point Move(int dx, int dy) => new Point(x + dx, y + dy);
public void Print() => Console.WriteLine(First + " " + Last);
// Works with operators, properties, and indexers too.
public static Complex operator +(Complex a, Complex b) => a.Add(b);
public string Name => First + " " + Last;
public Customer this[long id] => store.LookupCustomer(id);
```

Se o método retornar `void` ou for um método assíncrono, o corpo do método deverá ser uma expressão de instrução (igual aos lambdas).  Para propriedades e indexadores, eles devem ser somente leitura e não usar a palavra-chave do acessador `get`.

<a name="iterators"></a>

## <a name="iterators"></a>Iterators

Um iterador realiza uma iteração personalizada em uma coleção, como uma lista ou uma matriz. Um iterador usa a instrução [yield return](language-reference/keywords/yield.md) para retornar um elemento de cada vez. Quando uma instrução `yield return` for atingida, o local atual será lembrado para que o chamador possa solicitar o próximo elemento na sequência.

O tipo de retorno de um iterador pode ser <xref:System.Collections.IEnumerable>, <xref:System.Collections.Generic.IEnumerable%601>, <xref:System.Collections.IEnumerator> ou <xref:System.Collections.Generic.IEnumerator%601>.

Para obter mais informações, consulte [Iteradores](programming-guide/concepts/iterators.md).

## <a name="see-also"></a>Consulte também

- [Modificadores de acesso](language-reference/keywords/access-modifiers.md)
- [Classes static e membros de classes static](programming-guide/classes-and-structs/static-classes-and-static-class-members.md)
- [Herança](programming-guide/classes-and-structs/inheritance.md)
- [Classes e membros de classes abstract e sealed](programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members.md)
- [params](language-reference/keywords/params.md)
- [fora](language-reference/keywords/out-parameter-modifier.md)
- [ref](language-reference/keywords/ref.md)
- [no](language-reference/keywords/in-parameter-modifier.md)
- [Passando parâmetros](programming-guide/classes-and-structs/passing-parameters.md)
