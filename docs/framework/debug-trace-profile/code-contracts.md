---
title: Contratos de código
description: Explore os contratos de código, que fornecem uma maneira de especificar pré-condições, condições e invariáveis de objeto em seu código .NET.
ms.date: 09/05/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Code contracts
ms.assetid: 84526045-496f-489d-8517-a258cf76f040
ms.openlocfilehash: 60f794373af75bd3f745c224e0a8c7a84192e4c4
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904137"
---
# <a name="code-contracts"></a>Contratos de código

Os contratos de código fornecem uma maneira de especificar pré-condições, pós-condições e invariáveis de objeto no código. As pré-condições são requisitos que devem ser atendidos ao inserir um método ou uma propriedade. As pós-condições descrevem as expectativas no momento em que o código do método ou da propriedade é fechado. As invariáveis de objeto descrevem o estado esperado de uma classe que está em um bom estado.

Os contratos de código incluem classes para marcação do código, um analisador estático para análise em tempo de compilação e um analisador de runtime. As classes dos contratos de código podem ser encontradas no namespace <xref:System.Diagnostics.Contracts>.

Os benefícios dos contratos de código incluem os seguintes:

- Testes aprimorados: os contratos de código fornecem verificação de contrato estático, verificação de runtime e geração de documentação.

- Ferramentas de teste automático: use contratos de código para gerar testes de unidade mais significativos filtrando argumentos de teste sem sentido que não atendem às pré-condições.

- Verificação estática: o verificador estático pode decidir se há violações de contrato sem executar o programa. Ele verifica se há contratos implícitos, como desreferências nulas e limites da matriz, além de contratos explícitos.

- Documentação de referência: o gerador de documentação amplia os arquivos de documentação XML existentes com informações de contrato. Também há folhas de estilos que podem ser usadas com o [Sandcastle](https://github.com/EWSoftware/SHFB) para que as páginas de documentação geradas tenham seções de contrato.

Todas as linguagens do .NET Framework podem aproveitar os contratos; imediatamente: não é necessário escrever um analisador ou compilador especial. Um suplemento do Visual Studio permite especificar o nível da análise do contrato de código a ser executado. Os analisadores podem confirmar se os contratos estão bem formados (verificação de tipo e resolução de nomes) e podem produzir um formato compilado dos contratos no formato da MSIL (Microsoft Intermediate Language). A criação de contratos no Visual Studio permite aproveitar o IntelliSense padrão fornecido pela ferramenta.

A maioria dos métodos da classe de contrato é compilada condicionalmente; ou seja, o compilador emite chamadas para esses métodos somente quando um símbolo especial, CONTRACTS_FULL, é definido, usando a diretiva `#define`. CONTRACTS_FULL permite escrever contratos no código sem o uso de diretivas `#ifdef`; é possível produzir diferentes builds, alguns com contratos e outras sem eles.

Para obter ferramentas e instruções detalhadas sobre como usar contratos de código, consulte [contratos de código](https://marketplace.visualstudio.com/items?itemName=RiSEResearchinSoftwareEngineering.CodeContractsforNET) no site do Marketplace do Visual Studio.

## <a name="preconditions"></a>Pré-condições

É possível expressar pré-condições usando o método <xref:System.Diagnostics.Contracts.Contract.Requires%2A?displayProperty=nameWithType>. As pré-condições especificam o estado quando um método é invocado. Geralmente, elas são usadas para especificar valores de parâmetro válidos. Todos os membros mencionados nas pré-condições devem ser, pelo menos, tão acessíveis quanto o próprio método; caso contrário, a pré-condição pode não ser compreendida por todos os chamadores de um método. A condição não deve ter efeitos colaterais. O comportamento em runtime de pré-condições com falha é determinado pelo analisador de runtime.

Por exemplo, a pré-condição a seguir expressa que o parâmetro `x` não deve ser nulo.

```csharp
Contract.Requires(x != null);
```

Se o código precisar gerar uma exceção específica em caso de falha de uma pré-condição, use a sobrecarga genérica de <xref:System.Diagnostics.Contracts.Contract.Requires%2A>, conforme mostrado a seguir.

```csharp
Contract.Requires<ArgumentNullException>(x != null, "x");
```

### <a name="legacy-requires-statements"></a>Instruções Requires herdadas

A maior parte do código contém alguma validação de parâmetro na forma do código `if`-`then`-`throw`. As ferramentas de contrato reconhecem essas instruções como pré-condições nos seguintes casos:

- As instruções aparecem antes das outras instruções em um método.

- O conjunto inteiro de instruções desse tipo é seguido por uma chamada de método <xref:System.Diagnostics.Contracts.Contract> explícita, como uma chamada ao método <xref:System.Diagnostics.Contracts.Contract.Requires%2A>, <xref:System.Diagnostics.Contracts.Contract.Ensures%2A>, <xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A> ou <xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A>.

Quando as instruções `if`-`then`-`throw` aparecem neste formato, as ferramentas as reconhecem como instruções `requires` herdadas. Se nenhum outro contrato seguir a sequência `if`-`then`-`throw`, encerre o código com o método <xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A?displayProperty=nameWithType>.

```csharp
if (x == null) throw new ...
Contract.EndContractBlock(); // All previous "if" checks are preconditions
```

Observe que a condição no teste anterior é uma pré-condição negada. (A pré-condição real seria `x != null` .) Uma pré-condição negada é altamente restrita: ela deve ser escrita conforme mostrado no exemplo anterior; ou seja, ele não deve conter `else` cláusulas e o corpo da `then` cláusula deve ser uma única `throw` instrução. O teste `if` está sujeito às regras de pureza e visibilidade (consulte [Diretrizes de uso](#usage_guidelines)), mas a expressão `throw` está sujeita apenas às regras de pureza. No entanto, o tipo da exceção gerada deve estar tão visível quanto o método no qual ocorre o contrato.

## <a name="postconditions"></a>Pós-condições

Pós-condições são contratos para o estado de um método quando ele termina. A pós-condição é verificada logo antes do fechamento de um método. O comportamento em runtime de pós-condições com falha é determinado pelo analisador de runtime.

Ao contrário das pré-condições, as pós-condições podem referenciar membros com menos visibilidade. Um cliente pode não conseguir entender nem fazer uso de algumas das informações expressas por uma pós-condição usando o estado particular, mas isso não afeta a capacidade do cliente de usar o método corretamente.

### <a name="standard-postconditions"></a>Pós-condições padrão

É possível expressar pós-condições padrão usando o método <xref:System.Diagnostics.Contracts.Contract.Ensures%2A>. As pós-condições expressam uma condição que deve ser `true` após o término normal do método.

```csharp
Contract.Ensures(this.F > 0);
```

### <a name="exceptional-postconditions"></a>Pós-condições excepcionais

Pós-condições excepcionais são pós-condições que devem ser `true` quando uma exceção específica é gerada por um método. É possível especificar essas pós-condições usando o método <xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A?displayProperty=nameWithType>, como mostra o exemplo a seguir.

```csharp
Contract.EnsuresOnThrow<T>(this.F > 0);
```

O argumento é a condição que deve ser `true` sempre que uma exceção que é um subtipo de `T` é gerada.

Há alguns tipos de exceção que são difíceis de serem usados em uma pós-condição excepcional. Por exemplo, o uso do tipo <xref:System.Exception> para `T` exige o método para garantir a condição, independentemente do tipo de exceção gerado, mesmo se ele for um excedente de pilha ou outra exceção impossível de ser controlada. Você deve usar pós-condições excepcionais somente para exceções específicas que podem ser geradas quando um membro é chamado, por exemplo, quando uma <xref:System.InvalidTimeZoneException> é gerada para uma chamada de método <xref:System.TimeZoneInfo>.

### <a name="special-postconditions"></a>Pós-condições especiais

Os seguintes métodos podem ser usados apenas em pós-condições:

- É possível se referir aos valores retornados do método nas pós-condições usando a expressão `Contract.Result<T>()`, em que `T` é substituído pelo tipo de retorno do método. Quando o compilador não puder inferir o tipo, você deverá fornecê-lo explicitamente. Por exemplo, o compilador do C# não pode inferir tipos de métodos que não usam nenhum argumento. Portanto, ele exige a seguinte pós-condição: métodos `Contract.Ensures(0 <Contract.Result<int>())` com um tipo de retorno `void` não podem se referir a `Contract.Result<T>()` em suas pós-condições.

- Um valor de pré-estado em uma pós-condição refere-se ao valor de uma expressão no início de um método ou uma propriedade. Ele usa a expressão `Contract.OldValue<T>(e)`, em que `T` é o tipo de `e`. É possível omitir o argumento de tipo genérico sempre que o compilador pode inferir seu tipo. (Por exemplo, o compilador C# sempre infere o tipo porque ele usa um argumento). Há várias restrições sobre o que pode ocorrer no `e` e os contextos nos quais uma expressão antiga pode aparecer. Uma expressão antiga não pode conter outra expressão antiga. O mais importante é que uma expressão antiga deve se referir a um valor que existia no estado de pré-condição do método. Em outras palavras, ela deve ser uma expressão que possa ser avaliada, desde que a pré-condição do método seja `true`. Veja a seguir várias instâncias dessa regra.

  - O valor deve existir no estado de pré-condição do método. Para fazer referência a um campo em um objeto, as pré-condições devem garantir que o objeto seja sempre não nulo.

  - Não é possível se referir ao valor retornado do método em uma expressão antiga:

      ```csharp
      Contract.OldValue(Contract.Result<int>() + x) // ERROR
      ```

  - Não é possível se referir aos parâmetros `out` em uma expressão antiga.

  - Uma expressão antiga não poderá depender da variável associada de um quantificador se o intervalo do quantificador depender do valor retornado do método:

      ```csharp
      Contract.ForAll(0, Contract.Result<int>(), i => Contract.OldValue(xs[i]) > 3); // ERROR
      ```

  - Uma expressão antiga não pode se referir ao parâmetro do representante anônimo em uma chamada <xref:System.Diagnostics.Contracts.Contract.ForAll%2A> ou <xref:System.Diagnostics.Contracts.Contract.Exists%2A>, a menos que ela seja usada como um indexador ou um argumento para uma chamada de método:

      ```csharp
      Contract.ForAll(0, xs.Length, i => Contract.OldValue(xs[i]) > 3); // OK
      Contract.ForAll(0, xs.Length, i => Contract.OldValue(i) > 3); // ERROR
      ```

  - Uma expressão antiga não pode ocorrer no corpo de um representante anônimo se o valor da expressão antiga depender de um dos parâmetros do representante anônimo, a menos que o representante anônimo seja um argumento para o método <xref:System.Diagnostics.Contracts.Contract.ForAll%2A> ou <xref:System.Diagnostics.Contracts.Contract.Exists%2A>:

      ```csharp
      Method(... (T t) => Contract.OldValue(... t ...) ...); // ERROR
      ```

  - Os parâmetros `Out` apresentam um problema porque os contratos aparecem antes do corpo do método e a maioria dos compiladores não permite referências aos parâmetros `out` em pós-condições. Para resolver esse problema, a classe <xref:System.Diagnostics.Contracts.Contract> fornece o método <xref:System.Diagnostics.Contracts.Contract.ValueAtReturn%2A>, que permite uma pós-condição com base em um parâmetro `out`.

      ```csharp
      public void OutParam(out int x)
      {
          Contract.Ensures(Contract.ValueAtReturn(out x) == 3);
          x = 3;
      }
      ```

      Assim como ocorre com o método <xref:System.Diagnostics.Contracts.Contract.OldValue%2A>, é possível omitir o parâmetro de tipo genérico sempre que o compilador pode inferir seu tipo. O reescritor de contrato substitui a chamada de método pelo valor do parâmetro `out`. O método <xref:System.Diagnostics.Contracts.Contract.ValueAtReturn%2A> pode aparecer somente em pós-condições. O argumento para o método deve ser um parâmetro `out` ou um campo de um parâmetro `out` de estrutura. Esse último também é útil ao se referir a campos na pós-condição de um construtor de estrutura.

      > [!NOTE]
      > Atualmente, as ferramentas de análise de contrato de código não verificam se os parâmetros `out` são inicializados corretamente e desconsideram sua menção na pós-condição. Portanto, no exemplo anterior, se a linha após o contrato tiver usado o valor `x` em vez de atribuir um inteiro a ele, um compilador não emitirá o erro correto. No entanto, em um build no qual o símbolo do pré-processador CONTRACTS_FULL não está definido (como o build de versão ASA), o compilador emitirá um erro.

## <a name="invariants"></a>Invariáveis

Invariáveis de objeto são condições que devem ser verdadeiras para cada instância de uma classe, sempre que o objeto está visível para um cliente. Elas expressam as condições nas quais o objeto é considerado correto.

Os métodos invariáveis são identificados sendo marcados com o atributo <xref:System.Diagnostics.Contracts.ContractInvariantMethodAttribute>. Os métodos invariáveis não devem conter nenhum código, exceto uma sequência de chamadas ao método <xref:System.Diagnostics.Contracts.Contract.Invariant%2A>, cada uma delas especificando uma invariável individual, conforme mostrado no exemplo a seguir.

```csharp
[ContractInvariantMethod]
protected void ObjectInvariant ()
{
    Contract.Invariant(this.y >= 0);
    Contract.Invariant(this.x > this.y);
    ...
}
```

As invariáveis são definidas condicionalmente pelo símbolo do pré-processador CONTRACTS_FULL. Durante a verificação em tempo de execução, as invariáveis são verificadas ao final de cada método público. Se uma invariável mencionar um método público na mesma classe, a verificação de invariáveis que normalmente ocorre ao final do método público será desabilitada. Em vez disso, a verificação ocorrerá somente ao final da chamada de método externa para essa classe. Isso também ocorrerá se a classe for inserida novamente devido a uma chamada a um método em outra classe. As invariáveis não são verificadas para um finalizador de objeto e uma <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> implementação.

<a name="usage_guidelines"></a>

## <a name="usage-guidelines"></a>Diretrizes de uso

### <a name="contract-ordering"></a>Ordenação de contrato

A tabela a seguir mostra a ordem dos elementos que deve ser usada ao escrever contratos de método.

|`If-then-throw statements`|Pré-condições públicas compatíveis com versões anteriores|
|-|-|
|<xref:System.Diagnostics.Contracts.Contract.Requires%2A>|Todas as pré-condições públicas.|
|<xref:System.Diagnostics.Contracts.Contract.Ensures%2A>|Todas as pós-condições públicas (normais).|
|<xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A>|Todas as pós-condições excepcionais públicas.|
|<xref:System.Diagnostics.Contracts.Contract.Ensures%2A>|Todas as pós-condições particulares/internas (normais).|
|<xref:System.Diagnostics.Contracts.Contract.EnsuresOnThrow%2A>|Todas as pós-condições excepcionais particulares/internas.|
|<xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A>|Se você estiver usando pré-condições de estilo `if`-`then`-`throw` sem nenhum outro contrato, faça uma chamada a <xref:System.Diagnostics.Contracts.Contract.EndContractBlock%2A> para indicar que as verificações anteriores são pré-condições.|

<a name="purity"></a>

### <a name="purity"></a>Pureza

Todos os métodos chamados em um contrato devem ser puros; ou seja, eles não devem atualizar nenhum estado preexistente. Um método puro tem permissão para modificar os objetos que foram criados após a entrada no método puro.

Atualmente, as ferramentas de contrato de código supõem que os seguintes elementos de código são puros:

- Métodos marcados com o <xref:System.Diagnostics.Contracts.PureAttribute>.

- Tipos marcados com o <xref:System.Diagnostics.Contracts.PureAttribute> (o atributo se aplica a todos os métodos do tipo).

- Acessadores get da propriedade.

- Operadores (métodos estáticos cujos nomes começam com “op” e que têm um ou dois parâmetros e um tipo de retorno não nulo).

- Qualquer método cujo nome totalmente qualificado começa com “System.Diagnostics.Contracts.Contract”, “System.String”, “System.IO.Path” ou “System.Type”.

- Qualquer representante invocado, desde que o próprio tipo de representante seja atribuído com o <xref:System.Diagnostics.Contracts.PureAttribute>. Os tipos de representante <xref:System.Predicate%601?displayProperty=nameWithType> e <xref:System.Comparison%601?displayProperty=nameWithType> são considerados puros.

<a name="visibility"></a>

### <a name="visibility"></a>Visibilidade

Todos os membros mencionados em um contrato devem ser, pelo menos, tão visíveis quanto o método no qual aparecem. Por exemplo, um campo particular não pode ser mencionado em uma pré-condição de um método público; os clientes não podem validar um contrato desse tipo antes de chamarem o método. No entanto, se o campo estiver marcado com o <xref:System.Diagnostics.Contracts.ContractPublicPropertyNameAttribute>, ele estará isento dessas regras.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra o uso de contratos de código.

[!code-csharp[ContractExample#1](../../../samples/snippets/csharp/VS_Snippets_CLR/contractexample/cs/program.cs#1)]
[!code-vb[ContractExample#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/contractexample/vb/program.vb#1)]
