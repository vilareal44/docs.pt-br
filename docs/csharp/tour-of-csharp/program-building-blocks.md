---
title: Os blocos de construção de programas em C# "
description: Saiba mais sobre membros, expressões e instruções C#. Os tipos contêm membros que você escreve. Esses membros são criados a partir de instruções e expressões.
ms.date: 08/06/2020
ms.openlocfilehash: 142fe7b5a3424a8925638bfb4e4437392347f4c6
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88268134"
---
# <a name="program-building-blocks"></a>Blocos de construção de programas

Os tipos descritos no artigo anterior são criados usando estes blocos de construção: [***Membros***](../programming-guide/classes-and-structs/members.md), [ ***expressões***e ***instruções***](../programming-guide/statements-expressions-operators/index.md).

## <a name="members"></a>Membros

Os membros de a `class` são membros ***estáticos*** ou ***membros de instância***. Os membros estáticos pertencem às classes e os membros de instância pertencem aos objetos (instâncias de classes).

A lista a seguir fornece uma visão geral dos tipos de membros que uma classe pode conter.

- **Constantes**: valores constantes associados à classe
- **Campos**: variáveis que estão associadas à classe
- **Métodos**: ações que podem ser executadas pela classe
- **Propriedades**: ações associadas à leitura e à gravação de propriedades nomeadas da classe
- **Indexadores**: ações associadas à indexação de instâncias da classe como uma matriz
- **Eventos**: notificações que podem ser geradas pela classe
- **Operadores**: conversões e operadores de expressão com suporte na classe
- **Construtores**: ações necessárias para inicializar instâncias da classe ou a própria classe
- **Finalizadores**: ações executadas antes de instâncias da classe serem descartadas permanentemente
- **Tipos**: tipos aninhados declarados pela classe

## <a name="accessibility"></a>Acessibilidade

Cada membro de uma classe tem uma acessibilidade associada, que controla as regiões do texto do programa que podem acessar o membro. Existem seis formas possíveis de acessibilidade. Os modificadores de acesso são resumidos abaixo.

- `public`: O acesso não é limitado.
- `private`: O acesso é limitado a essa classe.
- `protected`: O acesso é limitado a esta classe ou classes derivadas desta classe.
- `internal`: O acesso é limitado ao assembly atual ( `.exe` ou `.dll` ).
- `protected internal`: O acesso é limitado a essa classe, classes derivadas dessa classe ou classes dentro do mesmo assembly.
- `private protected`: O acesso é limitado a essa classe ou classes derivadas desse tipo dentro do mesmo assembly.

## <a name="fields"></a>Campos

Um *campo* é uma variável que está associada a uma classe ou a uma instância de uma classe.

Um campo declarado com o modificador estático define um campo estático. Um campo estático identifica exatamente um local de armazenamento. Não importa quantas instâncias de uma classe são criadas, há apenas uma cópia de um campo estático.

Um campo declarado sem o modificador estático define um campo de instância. Cada instância de uma classe contém uma cópia separada de todos os campos de instância dessa classe.

No exemplo a seguir, cada instância da `Color` classe tem uma cópia separada dos campos de `r` instância,, `g` e `b` , mas há apenas uma cópia dos `Black` `White` `Red` `Green` campos estáticos,,, e `Blue` :

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="ColorClassDefinition":::

Conforme mostrado no exemplo anterior, os *campos somente leitura* podem ser declarados com um modificador `readonly`. A atribuição a um campo somente leitura só pode ocorrer como parte da declaração do campo ou em um construtor na mesma classe.

## <a name="methods"></a>Métodos

Um *método* é um membro que implementa um cálculo ou uma ação que pode ser executada por um objeto ou classe. Os *métodos estáticos* são acessados pela classe. Os *métodos de instância* são acessados pelas instâncias da classe.

Os métodos podem ter uma lista de *parâmetros*, que representam valores ou referências de variáveis passadas para o método. Os métodos têm um *tipo de retorno*, que especifica o tipo do valor calculado e retornado pelo método. O tipo de retorno de um método é `void` se ele não retornar um valor.

Como os tipos, os métodos também podem ter um conjunto de parâmetros de tipo, para que os quais os argumentos de tipo devem ser especificados quando o método é chamado. Ao contrário dos tipos, os argumentos de tipo geralmente podem ser inferidos de argumentos de uma chamada de método e não precisam ser fornecidos explicitamente.

A *assinatura* de um método deve ser exclusiva na classe na qual o método é declarado. A assinatura de um método consiste no nome do método, no número de parâmetros de tipo e no número, nos modificadores e nos tipos de seus parâmetros. A assinatura de um método não inclui o tipo de retorno.

Quando um corpo de método é uma única expressão, o método pode ser definido usando um formato de expressão Compact, conforme mostrado no exemplo a seguir:

```csharp
public override ToString() => "This is an object";
```

### <a name="parameters"></a>Parâmetros

Os parâmetros são usados para passar valores ou referências de variável aos métodos. Os parâmetros de um método obtêm seus valores reais de *argumentos* que são especificados quando o método é invocado. Há quatro tipos de parâmetros: parâmetros de valor, parâmetros de referência, parâmetros de saída e matrizes de parâmetros.

Um *parâmetro de valor* é usado para passar argumentos de entrada. Um parâmetro de valor corresponde a uma variável local que obtém seu valor inicial do argumento passado para o parâmetro. Modificações em um parâmetro de valor não afetam o argumento que foi passado para o parâmetro.

Os parâmetros de valor podem ser opcionais, especificando um valor padrão para que os argumentos correspondentes possam ser omitidos.

Um *parâmetro de referência* é usado para passar argumentos por referência. O argumento passado para um parâmetro de referência deve ser uma variável com um valor definido. Durante a execução do método, o parâmetro de referência representa o mesmo local de armazenamento que a variável de argumento. Um parâmetro de referência é declarado com o modificador `ref`. O exemplo a seguir mostra o uso de parâmetros `ref`.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="RefExample":::

Um *parâmetro de saída* é usado para passar argumentos por referência. Ele é semelhante a um parâmetro de referência, exceto que ele não requer que você atribua explicitamente um valor ao argumento fornecido pelo chamador. Um parâmetro de saída é declarado com o modificador `out`. O exemplo a seguir mostra o uso de parâmetros `out` usando a sintaxe introduzida no C# 7.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="OutExample":::

Uma *matriz de parâmetros* permite que um número variável de argumentos sejam passados para um método. Uma matriz de parâmetro é declarada com o modificador `params`. Somente o último parâmetro de um método pode ser uma matriz de parâmetros e o tipo de uma matriz de parâmetros deve ser um tipo de matriz unidimensional. Os `Write` `WriteLine` métodos e da <xref:System.Console?displayProperty=nameWithType> classe são bons exemplos de uso de matriz de parâmetros. Eles são declarados da seguinte maneira.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="ConsoleExtract":::

Dentro de um método que usa uma matriz de parâmetros, a matriz de parâmetros se comporta exatamente como um parâmetro regular de um tipo de matriz. No entanto, em uma invocação de um método com uma matriz de parâmetros, é possível passar um único argumento do tipo de matriz de parâmetro ou qualquer número de argumentos do tipo de elemento da matriz de parâmetros. No último caso, uma instância de matriz é automaticamente criada e inicializada com os argumentos determinados. Esse exemplo

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="UseParamsArgs":::

é equivalente ao escrito a seguir.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="CompilerParams":::

### <a name="method-body-and-local-variables"></a>Corpo do método e variáveis locais

Um corpo do método especifica as instruções para execução quando o método é invocado.

Um corpo de método pode declarar variáveis que são específicas para a invocação do método. Essas variáveis são chamadas de *variáveis locais*. Uma declaração de variável local especifica um nome de tipo, um nome de variável e, possivelmente, um valor inicial. O exemplo a seguir declara uma variável local `i` com um valor inicial de zero e uma variável local `j` sem valor inicial.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="SquaresClass":::

O C# requer que uma variável local seja *atribuída definitivamente* antes de seu valor poder ser obtido. Por exemplo, se a declaração do anterior `i` não incluísse um valor inicial, o compilador relataria um erro para os usos mais recentes do porque não seria `i` `i` definitivamente atribuído a esses pontos no programa.

Um método pode usar instruções `return` para retornar o controle é pelo chamador. Em um método que retorna `void` , as `return` instruções não podem especificar uma expressão. Em um método que retorna não nulo, as instruções `return` devem incluir uma expressão que calcula o valor retornado.

### <a name="static-and-instance-methods"></a>Métodos estáticos e de instância

Um método declarado com um `static` modificador é um *método estático*. Um método estático não opera em uma instância específica e só pode acessar diretamente membros estáticos.

Um método declarado sem um `static` modificador é um *método de instância*. Um método de instância opera em uma instância específica e pode acessar membros estáticos e de instância. A instância em que um método de instância foi invocado pode ser explicitamente acessada como `this`. É um erro fazer referência a `this` um método estático.

A seguinte classe `Entity` tem membros estáticos e de instância.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="EntityClass":::

Cada `Entity` instância contém um número de série (e, presumivelmente, algumas outras informações que não são mostradas aqui). O construtor `Entity` (que é como um método de instância) inicializa a nova instância com o próximo número de série disponível. Como o construtor é um membro de instância, ele tem permissão para acessar o `_serialNo` campo de instância e o `s_nextSerialNo` campo estático.

Os métodos estáticos `GetNextSerialNo` e `SetNextSerialNo` podem acessar o campo estático `s_nextSerialNo`, mas seria um erro para eles acessar diretamente o campo de instância `_serialNo`.

O exemplo a seguir mostra o uso da `Entity` classe.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="UsingEntity":::

Os `SetNextSerialNo` `GetNextSerialNo` métodos estáticos e são invocados na classe, enquanto o `GetSerialNo` método de instância é invocado em instâncias da classe.

### <a name="virtual-override-and-abstract-methods"></a>Métodos abstratos, virtuais e de substituição

Quando uma declaração de método de instância inclui um modificador `virtual`, o método deve ser um *método virtual*. Quando nenhum modificador virtual estiver presente, o método será um *método não virtual*.

Quando um método virtual é invocado, o *tipo de tempo de execução* da instância para o qual essa invocação ocorre determina a implementação real do método para invocar. Em uma invocação de método não virtual, o *tipo de tempo de compilação* da instância é o fator determinante.

Um método virtual pode ser *substituído* em uma classe derivada. Quando uma declaração de método de instância inclui um modificador de substituição, o método substitui um método virtual herdado com a mesma assinatura. A declaração de método virtual AA introduz um novo método. Uma declaração de método de substituição especializa um método virtual herdado existente fornecendo uma nova implementação desse método.

Um *método abstrato* é um método virtual sem implementação. Um método abstract é declarado com o `abstract` modificador e é permitido somente em uma classe abstrata. Um método abstrato deve ser substituído em cada classe derivada não abstrata.

O exemplo a seguir declara uma classe abstrata, `Expression`, que representa um nó de árvore de expressão e três classes derivadas, `Constant`, `VariableReference` e `Operation`, que implementam nós de árvore de expressão para operações aritméticas, referências de variável e constantes. (Este exemplo é semelhante a, mas não relacionado aos tipos de árvore de expressão).

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="WorkingWithExpressions":::

As quatro classes anteriores podem ser usadas para modelar expressões aritméticas. Por exemplo, usando instâncias dessas classes, a expressão `x + 3` pode ser representada da seguinte maneira.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="UseExpressions":::

O método `Evaluate` de uma instância `Expression` é chamado para avaliar a expressão especificada e produzir um valor `double`. O método recebe um argumento `Dictionary` que contém nomes de variáveis (como chaves das entradas) e valores (como valores das entradas). Como `Evaluate` é um método abstrato, classes não abstratas derivadas de `Expression` devem substituir `Evaluate`.

Uma implementação de `Evaluate` do `Constant` retorna apenas a constante armazenada. Uma implementação de `VariableReference` consulta o nome de variável no dicionário e retorna o valor resultante. Uma implementação de `Operation` primeiro avalia os operandos esquerdo e direito (chamando recursivamente seus métodos `Evaluate`) e, em seguida, executa a operação aritmética determinada.

O seguinte programa usa as classes `Expression` para avaliar a expressão `x * (y + 2)` para valores diferentes de `x` e `y`.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="UsingExpressions":::

### <a name="method-overloading"></a>Sobrecarga de método

A *sobrecarga* de método permite que vários métodos na mesma classe tenham o mesmo nome, contanto que tenham assinaturas exclusivas. Ao compilar uma invocação de um método sobrecarregado, o compilador usa a *resolução de sobrecarga* para determinar o método específico para invocar. A resolução de sobrecarga encontra um método que melhor corresponde aos argumentos. Se não for possível encontrar uma única melhor correspondência, um erro será relatado. O exemplo a seguir mostra a resolução de sobrecarga em vigor. O comentário para cada invocação no `UsageExample` método mostra qual método é invocado.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="Overloading":::

Conforme mostrado pelo exemplo, um método específico sempre pode ser selecionado por meio da conversão explícita dos argumentos para os tipos de parâmetro e argumentos de tipo exatos.

## <a name="other-function-members"></a>Outros membros da função

Os membros que contêm código executável são conhecidos coletivamente como *membros de função* de uma classe. A seção anterior descreve os métodos, que são os principais tipos de membros de função. Esta seção descreve os outros tipos de membros da função com suporte do C#: construtores, propriedades, indexadores, eventos, operadores e finalizadores.

O exemplo a seguir mostra uma classe genérica chamada `MyList<T>` , que implementa uma lista de objetos que aumenta. A classe contém vários exemplos dos tipos mais comuns de membros da função.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="ListExample":::

### <a name="constructors"></a>Construtores

O C# dá suporte aos construtores estáticos e de instância. Um *construtor de instância* é um membro que implementa as ações necessárias para inicializar uma instância de uma classe. Um *construtor estático* é um membro que implementa as ações necessárias para inicializar uma classe em si quando ela é carregada pela primeira vez.

Um construtor é declarado como um método sem nenhum tipo de retorno e o mesmo nome que a classe continente. Se uma declaração de Construtor incluir um `static` modificador, ele declara um construtor estático. Caso contrário, ela declara um construtor de instância.

Construtores de instância podem ser sobrecarregados e ter parâmetros opcionais. Por exemplo, a classe `MyList<T>` declara um construtor de instância com um único parâmetro `int` opcional. Os construtores de instância são invocados usando o operador `new`. As seguintes instruções alocam duas instâncias `MyList<string>` usando o construtor da classe `MyList` com e sem o argumento opcional.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="CreateLists":::

Ao contrário de outros membros, os construtores de instância não são herdados. Uma classe não tem construtores de instância diferentes dos construtores realmente declarados na classe. Se nenhum construtor de instância for fornecido para uma classe, então um construtor vazio sem parâmetros será fornecido automaticamente.

### <a name="properties"></a>Propriedades

As *propriedades* são uma extensão natural dos campos. Elas são denominadas membros com tipos associados, e a sintaxe para acessar os campos e as propriedades é a mesma. No entanto, ao contrário dos campos, as propriedades não denotam locais de armazenamento. Em vez disso, as propriedades têm *acessadores* que especificam as instruções executadas quando seus valores são lidos ou gravados.

Uma propriedade é declarada como um campo, exceto que a declaração termina com um acessador get ou um acessador set gravado entre os delimitadores `{` e `}` em vez de terminar em um ponto e vírgula. Uma propriedade que tem um acessador get e um acessador set é uma *propriedade de leitura-gravação*. Uma propriedade que tem apenas um acessador get é uma *propriedade somente leitura*, e uma propriedade que tem apenas um acessador set é uma *propriedade somente gravação*.

Um acessador get corresponde a um método sem parâmetros com um valor retornado do tipo de propriedade. Um acessador set corresponde a um método com um parâmetro único chamado valor e nenhum tipo de retorno. O acessador get computa o valor da propriedade. O acessador set fornece um novo valor para a propriedade. Quando a propriedade é o destino de uma atribuição, ou o operando de `++` ou `--` , o acessador set é invocado. Em outros casos em que a propriedade é referenciada, o acessador get é invocado.

 Quando uma propriedade é referenciada como o destino de uma atribuição ou como o operando do + + ou --, o acessador set é invocado com um argumento que fornece o novo valor.

A classe `MyList<T>` declara duas propriedades, `Count` e `Capacity`, que são somente leitura e leitura/gravação, respectivamente. O código a seguir é um exemplo de uso dessas propriedades:

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="AccessProperties":::

Como nos campos e métodos, o C# dá suporte a propriedades de instância e a propriedades estáticas. As propriedades estáticas são declaradas com o modificador estático e as propriedades de instância são declaradas sem ele.

Os acessadores de uma propriedade podem ser virtuais. Quando uma declaração de propriedade inclui um modificador `virtual`, `abstract` ou `override`, ela se aplica aos acessadores da propriedade.

### <a name="indexers"></a>Indexadores

Um *indexador* é um membro que permite que objetos sejam indexados da mesma forma que uma matriz. Um indexador é declarado como uma propriedade, exceto se o nome do membro for `this` seguido por uma lista de parâmetros escrita entre os delimitadores `[` e `]`. Os parâmetros estão disponíveis nos acessadores do indexador. Semelhante às propriedades, os indexadores podem ser de leitura-gravação, somente leitura e somente gravação, e os acessadores de um indexador pode ser virtuais.

A classe `MyList<T>` declara um indexador único de leitura-gravação que usa um parâmetro `int`. O indexador possibilita indexar instâncias `MyList<T>` com valores `int`. Por exemplo:

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="ListAccess":::

Os indexadores podem ser sobrecarregados. Uma classe pode declarar vários indexadores, desde que o número ou os tipos de seus parâmetros sejam diferentes.

### <a name="events"></a>Eventos

Um *evento* é um membro que permite que uma classe ou objeto forneça notificações. Um evento é declarado como um campo, exceto que a declaração inclui uma `event` palavra-chave e o tipo deve ser um tipo delegado.

Dentro de uma classe que declara um membro de evento, o evento se comporta exatamente como um campo de um tipo delegado (desde que o evento não seja abstrato e não declare acessadores). O campo armazena uma referência a um delegado que representa os manipuladores de eventos que foram adicionados ao evento. Se nenhum manipulador de evento estiver presente, o campo será `null`.

A classe `MyList<T>` declara um membro único de evento chamado `Changed`, que indica que um novo item foi adicionado à lista. O evento Alterado é gerado pelo método virtual `OnChanged`, que primeiro verifica se o evento é `null` (o que significa que nenhum manipulador está presente). A noção de gerar um evento é precisamente equivalente a invocar o delegado representado pelo evento. Não há construções de linguagem especiais para gerar eventos.

Os clientes reagem a eventos por meio de *manipuladores de eventos*. Os manipuladores de eventos são conectados usando o operador `+=` e removidos usando o operador `-=`. O exemplo a seguir anexa um manipulador de eventos para o evento `Changed` de um `MyList<string>`.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="RespondToEvents":::

Para cenários avançados em que o controle do armazenamento subjacente de um evento é desejado, uma declaração de evento pode fornecer explicitamente e acessadores `add` `remove` , que são semelhantes ao `set` acessador de uma propriedade.

### <a name="operators"></a>Operadores

Um *operador* é um membro que define o significado da aplicação de um operador de expressão específico para instâncias de uma classe. Três tipos de operadores podem ser definidos: operadores unários, operadores binários e operadores de conversão. Todos os operadores devem ser declarados como `public` e `static`.

A `MyList<T>` classe declara dois operadores `operator ==` e `operator !=` . Esses operadores substituídos dão um novo significado a expressões que aplicam esses operadores a `MyList` instâncias. Especificamente, os operadores definem a igualdade de duas `MyList<T>` instâncias como comparar cada um dos objetos contidos usando seus `Equals` métodos. O exemplo a seguir usa o operador `==` para comparar duas instâncias `MyList<int>`.

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="ListAddition":::

O primeiro `Console.WriteLine` gera `True` porque as duas listas contêm o mesmo número de objetos com os mesmos valores na mesma ordem. Como `MyList<T>` não definiu `operator ==`, o primeiro `Console.WriteLine` geraria `False` porque `a` e `b` referenciam diferentes instâncias `MyList<int>`.

### <a name="finalizers"></a>Finalizadores

Um *finalizador* é um membro que implementa as ações necessárias para finalizar uma instância de uma classe. Normalmente, um finalizador é necessário para liberar recursos não gerenciados. Os finalizadores não podem ter parâmetros, eles não podem ter modificadores de acessibilidade e não podem ser invocados explicitamente. O finalizador de uma instância é invocado automaticamente durante a coleta de lixo. Para obter mais detalhes, consulte o artigo sobre [finalizadores](../programming-guide/classes-and-structs/destructors.md).

O coletor de lixo tem latitude ampla ao decidir quando coletar objetos e executar os finalizadores. Especificamente, o tempo das invocações do finalizador não é determinístico e os finalizadores podem ser executados em qualquer thread. Para esses e outros motivos, as classes devem implementar os finalizadores apenas quando não houver outras soluções viáveis.

A instrução `using` fornece uma abordagem melhor para a destruição de objetos.

## <a name="expressions"></a>Expressões

*Expressões* são construídas a partir de *operandos* e *operadores*. Os operadores de uma expressão indicam quais operações devem ser aplicadas aos operandos. Exemplos de operadores incluem `+`, `-`, `*`, `/` e `new`. Exemplos de operandos incluem literais, campos, variáveis locais e expressões.

Quando uma expressão contém vários operadores, a *precedência* dos operadores controla a ordem na qual os operadores individuais são avaliados. Por exemplo, a expressão `x + y * z` é avaliada como `x + (y * z)` porque o operador `*` tem precedência maior do que o operador `+`.

Quando ocorre um operando entre dois operadores com a mesma precedência, a *associatividade* dos operadores controla a ordem na qual as operações são executadas:

* Exceto para os operadores de atribuição e de União nulo, todos os operadores binários são *associativos à esquerda*, o que significa que as operações são executadas da esquerda para a direita. Por exemplo, `x + y + z` é avaliado como `(x + y) + z`.
* Os operadores de atribuição, a União nula `??` e `??=` os operadores e o operador condicional `?:` são *associativos à direita*, o que significa que as operações são executadas da direita para a esquerda. Por exemplo, `x = y = z` é avaliado como `x = (y = z)`.

Precedência e associatividade podem ser controladas usando parênteses. Por exemplo, `x + y * z` primeiro multiplica `y` por `z` e, em seguida, adiciona o resultado a `x`, mas `(x + y) * z` primeiro adiciona `x` e `y` e, em seguida, multiplica o resultado por `z`.

A maioria dos operadores pode ser [*sobrecarregada*](../language-reference/operators/operator-overloading.md). A sobrecarga de operador permite que implementações de operador definidas pelo usuário sejam especificadas para operações em que um ou ambos os operandos são de um tipo struct ou de classe definida pelo usuário.

C# fornece uma série de operadores para realizar operações [aritméticas](../language-reference/operators/arithmetic-operators.md), [lógicas](../language-reference/operators/boolean-logical-operators.md), [bit a bit e shift](../language-reference/operators/bitwise-and-shift-operators.md) e comparações de [igualdade](../language-reference/operators/equality-operators.md) e [ordem](../language-reference/operators/comparison-operators.md).

Para obter a lista completa de operadores do C# ordenada pelo nível de precedência, confira [Operadores do C#](../language-reference/operators/index.md).

## <a name="statements"></a>Instruções

As ações de um programa são expressas usando *instruções*. O C# oferece suporte a vários tipos diferentes de instruções, algumas delas definidas em termos de instruções inseridas.

- Um *bloco* permite a produção de várias instruções em contextos nos quais uma única instrução é permitida. Um bloco é composto por uma lista de instruções escritas entre os delimitadores `{` e `}`.
- *Instruções de declaração* são usadas para declarar constantes e variáveis locais.
- *Instruções de expressão* são usadas para avaliar expressões. As expressões que podem ser usadas como instruções incluem chamadas de método, alocações de objeto usando o operador `new`, atribuições usando `=` e os operadores de atribuição compostos, operações de incremento e decremento usando os operadores `++` e `--` e as expressões `await`.
- *Instruções de seleção* são usadas para selecionar uma dentre várias instruções possíveis para execução com base no valor de alguma expressão. Esse grupo contém as `if` `switch` instruções e.
- *Instruções de iteração* são usadas para executar repetidamente uma instrução inserida. Esse grupo contém as `while` `do` instruções,, `for` e `foreach` .
- *Instruções de salto* são usadas para transferir o controle. Esse grupo contém as `break` `continue` instruções,, `goto` ,, `throw` `return` e `yield` .
- A instrução `try`... `catch` é usada para capturar exceções que ocorrem durante a execução de um bloco, e a instrução `try`... `finally` é usada para especificar o código de finalização que é executado sempre, se uma exceção ocorrer ou não.
- As instruções `checked` e `unchecked` são usadas para controlar o contexto de verificação de estouro para operações e conversões aritméticas do tipo integral.
- A instrução `lock` é usada para obter o bloqueio de exclusão mútua para um determinado objeto, executar uma instrução e, em seguida, liberar o bloqueio.
- A instrução `using` é usada para obter um recurso, executar uma instrução e, em seguida, descartar esse recurso.

O seguinte lista os tipos de instruções que podem ser usadas:

* Declaração de variável local.
* Declaração de constante local.
* Instrução de expressão.
* `if` privacidade.
* `switch` privacidade.
* `while` privacidade.
* `do` privacidade.
* `for` privacidade.
* `foreach` privacidade.
* `break` privacidade.
* `continue` privacidade.
* `goto` privacidade.
* `return` privacidade.
* `yield` privacidade.
* `throw` instruções e `try` instruções.
* `checked``unchecked`instruções and.
* `lock` privacidade.
* `using` privacidade.

>[!div class="step-by-step"]
>[Anterior](types.md) 
> [Avançar](features.md)
