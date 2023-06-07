---
title: Tuplas
ms.date: 04/23/2017
helpviewer_keywords:
- tuples [Visual Basic]
ms.assetid: 3e66cd1b-3432-4e1d-8c37-5ebacae8f53f
ms.openlocfilehash: b169a1c13b3f20d7b5e2a1386cfb28a9cc093dcd
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559083"
---
# <a name="tuples-visual-basic"></a>Tuplas (Visual Basic)

A partir do Visual Basic 2017, a linguagem de Visual Basic oferece suporte interno para tuplas que facilita a criação de tuplas e o acesso aos elementos de tuplas. Uma tupla é uma estrutura de dados leve que tem um número específico e uma sequência de valores. Ao instanciar a tupla, você define o número e o tipo de dados de cada valor (ou elemento). Por exemplo, um 2 tupla (ou par) tem dois elementos. O primeiro pode ser um `Boolean` valor, enquanto o segundo é um `String` . Como as tuplas facilitam o armazenamento de vários valores em um único objeto, elas são geralmente usadas como uma maneira leve de retornar vários valores de um método.

> [!IMPORTANT]
> O suporte à tupla requer o <xref:System.ValueTuple> tipo. Se o .NET Framework 4,7 não estiver instalado, você deverá adicionar o pacote NuGet `System.ValueTuple` , que está disponível na galeria do NuGet. Sem esse pacote, você pode obter um erro de compilação semelhante a, "o tipo predefinido ' ValueTuple (Of,,,) ' não está definido ou importado."

## <a name="instantiating-and-using-a-tuple"></a>Criando uma instância e usando uma tupla

Você cria uma instância de uma tupla ao colocar seus valores delimitados por vírgula entre parênteses. Cada um desses valores se torna um campo da tupla. Por exemplo, o código a seguir define um triplo (ou três tuplas) com um `Date` como seu primeiro valor, um `String` como seu segundo e um `Boolean` como terceiro.

[!code-vb[Instantiate](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple1.vb#1)]

Por padrão, o nome de cada campo em uma tupla consiste na cadeia de caracteres `Item` junto com a posição com base em um campo na tupla. Para essa 3 tupla, o campo é `Date` `Item1` , o `String` campo é `Item2` , e o `Boolean` campo é `Item3` . O exemplo a seguir exibe os valores dos campos da tupla instanciados na linha de código anterior

[!code-vb[Instantiate](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple1.vb#2)]

Os campos de uma tupla de Visual Basic são de leitura/gravação; Depois de criar uma instância de uma tupla, você pode modificar seus valores. O exemplo a seguir modifica dois dos três campos da tupla criada no exemplo anterior e exibe o resultado.

[!code-vb[Instantiate](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple1.vb#3)]

## <a name="instantiating-and-using-a-named-tuple"></a>Criando uma instância e usando uma tupla nomeada

Em vez de usar nomes padrão para os campos de uma tupla, você pode criar uma instância de uma *tupla nomeada* atribuindo seus próprios nomes aos elementos da tupla. Os campos da tupla podem ser acessados por seus nomes atribuídos *ou* por seus nomes padrão. O exemplo a seguir instancia a mesma 3 tupla como anteriormente, exceto que ela nomeia explicitamente o primeiro campo `EventDate` , o segundo `Name` e o terceiro `IsHoliday` . Em seguida, ele exibe os valores de campo, modifica-os e exibe os valores de campo novamente.

[!code-vb[Instantiate](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple1.vb#4)]

## <a name="inferred-tuple-element-names"></a>Nomes de elementos de tupla inferidos

A partir do Visual Basic 15,3, Visual Basic pode inferir os nomes dos elementos de tupla; Você não precisa atribuí-los explicitamente. Nomes de tupla deduzidos são úteis quando você Inicializa uma tupla de um conjunto de variáveis e você deseja que o nome do elemento de tupla seja igual ao nome da variável.

O exemplo a seguir cria uma `stateInfo` tupla que contém três elementos explicitamente nomeados, `state` , `stateName` e `capital` . Observe que, ao nomear os elementos, a instrução de inicialização de tupla simplesmente atribui os elementos nomeados aos valores das variáveis nomeadas de forma idêntica.

[!code-vb[ExplicitlyNamed](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/named-tuples/program.vb#1)]

Como elementos e variáveis têm o mesmo nome, o compilador de Visual Basic pode inferir os nomes dos campos, como mostra o exemplo a seguir.

[!code-vb[ExplicitlyNamed](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/named-tuples/program.vb#2)]

Para habilitar nomes de elementos de tupla deduzidos, você deve definir a versão do compilador de Visual Basic a ser usada no arquivo de projeto Visual Basic ( \* . vbproj):

```xml
<PropertyGroup>
  <LangVersion>15.3</LangVersion>
</PropertyGroup>
```

O número de versão pode ser qualquer versão do compilador de Visual Basic começando com 15,3. Em vez de embutir em código uma versão específica do compilador, você também pode especificar "mais recente" como o valor de `LangVersion` para compilar com a versão mais recente do compilador de Visual Basic instalado no seu sistema.

Para obter mais informações, consulte [definindo a versão do idioma do Visual Basic](../../../language-reference/configure-language-version.md).

Em alguns casos, o compilador Visual Basic não pode inferir o nome do elemento de tupla do nome candidato, e o campo de tupla só pode ser referenciado usando seu nome padrão, como `Item1` , `Item2` etc. Eles incluem:

- O nome do candidato é o mesmo que o nome de um membro de tupla, como `Item3` , `Rest` ou `ToString` .

- O nome do candidato está duplicado na tupla.

Quando a inferência de nome de campo falha, Visual Basic não gera um erro de compilador, nem é uma exceção lançada em tempo de execução. Em vez disso, os campos de tupla devem ser referenciados por seus nomes predefinidos, como `Item1` e `Item2` .

## <a name="tuples-versus-structures"></a>Tuplas versus estruturas

Uma tupla de Visual Basic é um tipo de valor que é uma instância de um dos tipos genéricos **System. ValueTuple** . Por exemplo, a `holiday` tupla definida no exemplo anterior é uma instância da <xref:System.ValueTuple%603> estrutura. Ele foi projetado para ser um contêiner leve de dados. Como a tupla visa facilitar a criação de um objeto com vários itens de dados, ele não tem alguns dos recursos que uma estrutura personalizada pode ter. Eles incluem:

- Membros personalizados. Você não pode definir suas próprias propriedades, métodos ou eventos para uma tupla.

- Confirmação. Você não pode validar os dados atribuídos a campos.

- Imutabilidade. Visual Basic tuplas são mutáveis. Por outro lado, uma estrutura personalizada permite que você controle se uma instância é mutável ou imutável.

Se membros personalizados, a validação de propriedade e de campo ou a imutabilidade forem importantes, você deverá usar a instrução Visual Basic [Structure](../../../language-reference/statements/structure-statement.md) para definir um tipo de valor personalizado.

Uma tupla Visual Basic herda os membros de seu tipo **ValueTuple** . Além de seus campos, eles incluem os seguintes métodos:

| Método | Descrição |
| ---|---|
| CompareTo | Compara a tupla atual com outra tupla com o mesmo número de elementos. |
| É igual a | Determina se a tupla atual é igual a outra tupla ou objeto. |
| GetHashCode | Calcula o código hash para a instância atual. |
| ToString | Retorna a representação da cadeia de caracteres dessa tupla, que assume o formulário `(Item1, Item2...)` , onde `Item1` e `Item2` representa os valores dos campos da tupla. |

Além disso, os tipos **ValueTuple** implementam <xref:System.Collections.IStructuralComparable> e <xref:System.Collections.IStructuralEquatable> interfaces, que permitem que você defina os comparadores personalizados.

## <a name="assignment-and-tuples"></a>Atribuição e tuplas

Visual Basic dá suporte à atribuição entre os tipos de tupla que têm o mesmo número de campos. Os tipos de campo podem ser convertidos se uma das seguintes opções for verdadeira:

- O campo de origem e destino são do mesmo tipo.

- Uma conversão de alargamento (ou implícita) do tipo de origem para o tipo de destino é definida.

- `Option Strict` é `On` , e uma conversão de restrição (ou explícita) do tipo de origem para o tipo de destino é definida. Essa conversão pode gerar uma exceção se o valor de origem estiver fora do intervalo do tipo de destino.

Outras conversões não são consideradas para atribuições. Vamos examinar os tipos de atribuições que são permitidos entre tipos de tupla.

Considere estas variáveis usadas nos exemplos a seguir:

[!code-vb[Assign](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple3.vb#1)]

As duas primeiras variáveis `unnamed` e `anonymous` não têm nomes semânticos fornecidos para os campos. Os nomes de campo são o padrão `Item1` e `Item2` . As duas últimas variáveis `named` e `differentName` têm nomes de campo semânticos. Observe que essas duas tuplas têm nomes diferentes para os campos.

Todas as quatro tuplas têm o mesmo número de campos (chamados de ' aridade '), e os tipos desses campos são idênticos. Portanto, todas essas atribuições funcionam:

[!code-vb[Assign](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple3.vb#2)]

Observe que os nomes das tuplas não são atribuídos. Os valores dos campos são atribuídos na ordem dos campos na tupla.

Por fim, observe que podemos atribuir a `named` tupla à `conversion` tupla, embora o primeiro campo de `named` seja um `Integer` , e o primeiro campo de `conversion` seja um `Long` . Essa atribuição é realizada com sucesso porque a conversão de um `Integer` para um `Long` é uma conversão de ampliação.

[!code-vb[Assign](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple3.vb#3)]

Tuplas com números diferentes de campos não podem ser atribuídas:

```vb
' Does not compile.
' VB30311: Value of type '(Integer, Integer, Integer)' cannot be converted
'          to '(Answer As Integer, Message As String)'
var differentShape = (1, 2, 3)
named = differentShape
```

## <a name="tuples-as-method-return-values"></a>Tuplas como valores retornados do método

Um método pode retornar apenas um único valor. Com frequência, no entanto, você gostaria que uma chamada de método retornasse vários valores. Há várias maneiras de contornar essa limitação:

- Você pode criar uma classe personalizada ou estrutura cujas propriedades ou campos representam valores retornados pelo método. Portanto, é uma solução pesada; Ele requer que você defina um tipo personalizado cujo único objetivo é recuperar valores de uma chamada de método.

- Você pode retornar um único valor do método e retornar os valores restantes passando-os por referência ao método. Isso envolve a sobrecarga de instanciar uma variável e riscos, inadvertidamente, substituir o valor da variável que você passa por referência.

- Você pode usar uma tupla, que fornece uma solução leve para recuperar vários valores de retorno.

Por exemplo, os métodos **TryParse** no .net retornam um `Boolean` valor que indica se a operação de análise foi bem-sucedida. O resultado da operação de análise é retornado em uma variável passada por referência ao método. Normalmente, uma chamada para o método de análise, como <xref:System.Int32.TryParse%2A?displayProperty=nameWithType> é semelhante ao seguinte:

[!code-vb[Return](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple-returns.vb#1)]

Podemos retornar uma tupla da operação de análise se encapsularmos a chamada para o <xref:System.Int32.TryParse%2A?displayProperty=nameWithType> método em nosso próprio método. No exemplo a seguir, `NumericLibrary.ParseInteger` chama o <xref:System.Int32.TryParse%2A?displayProperty=nameWithType> método e retorna uma tupla nomeada com dois elementos.

[!code-vb[Return](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple-returns.vb#2)]

Em seguida, você pode chamar o método com um código como o seguinte:

[!code-vb[Return](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple-returns.vb#3)]

## <a name="visual-basic-tuples-and-tuples-in-the-net-framework"></a>Visual Basic tuplas e tuplas no .NET Framework

Uma tupla Visual Basic é uma instância de um dos tipos genéricos **System. ValueTuple** , que foram introduzidos no .NET Framework 4,7. O .NET Framework também inclui um conjunto de classes genéricas **System. tupla** . No entanto, essas classes diferem das tuplas Visual Basic e dos tipos genéricos **System. ValueTuple** de várias maneiras:

- Os elementos das classes de **tupla** são propriedades nomeadas `Item1` , `Item2` e assim por diante. Em Visual Basic tuplas e os tipos **ValueTuple** , os elementos de tupla são campos.

- Você não pode atribuir nomes significativos aos elementos de uma instância de **tupla** ou de uma instância de **ValueTuple** . Visual Basic permite atribuir nomes que comunicam o significado dos campos.

- As propriedades de uma instância de **tupla** são somente leitura; as tuplas são imutáveis. Em Visual Basic tuplas e os tipos **ValueTuple** , os campos de tupla são de leitura/gravação; as tuplas são mutáveis.

- Os tipos de **tupla** genérica são tipos de referência. Usar esses tipos de **tupla** significa alocar objetos. Em caminhos de acesso, isso pode ter um impacto mensurável no desempenho do aplicativo. Visual Basic tuplas e os tipos de **ValueTuple** são tipos de valor.

Os métodos de extensão na classe facilitam <xref:System.TupleExtensions> a conversão entre Visual Basic tuplas e objetos de **tupla** .net. O método **totupla** converte uma tupla de Visual Basic em um objeto de **tupla** do .net e o método **ToValueTuple** converte um objeto de **tupla** do .net em uma tupla de Visual Basic.

O exemplo a seguir cria uma tupla, converte-a em um objeto de **tupla** do .net e a converte de volta para uma tupla de Visual Basic. Em seguida, o exemplo compara essa tupla com a original para garantir que elas sejam iguais.

[!code-vb[Convert](../../../../../samples/snippets/visualbasic/programming-guide/language-features/data-types/tuple2.vb#1)]

## <a name="see-also"></a>Confira também

- [Referência de linguagem de Visual Basic](index.md)
