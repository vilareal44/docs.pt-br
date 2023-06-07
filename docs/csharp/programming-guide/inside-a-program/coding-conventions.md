---
title: Convenções de codificação em C# – Guia de Programação em C#
description: Saiba mais sobre convenções de codificação em C#. As convenções de codificação criam uma aparência consistente para o código e facilitam a cópia, a alteração e a manutenção do código.
ms.date: 07/20/2015
helpviewer_keywords:
- coding conventions, C#
- Visual C#, coding conventions
- C# language, coding conventions
ms.assetid: f4f60de9-d49b-4fb6-bab1-20e19ea24710
ms.openlocfilehash: 772aebff0b8c7aebe7c7d5c7634cd2931f4570b1
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301847"
---
# <a name="c-coding-conventions-c-programming-guide"></a>Convenções de codificação em C# (Guia de Programação em C#)

As convenções de codificação atendem às seguintes finalidades:  
  
- Criam uma aparência consistente para o código, para que os leitores possam se concentrar no conteúdo e não no layout.  
  
- Permitem que os leitores entendam o código com mais rapidez, fazendo suposições com base na experiência anterior.  
  
- Facilitam a cópia, a alteração e a manutenção do código.  
  
- Demonstram as práticas recomendadas do C#.  

As diretrizes neste artigo são usadas pela Microsoft para desenvolver exemplos e documentação.  
  
## <a name="naming-conventions"></a>Convenções de nomenclatura  
  
- Em exemplos curtos que não incluem [diretivas using](../../language-reference/keywords/using-directive.md), use as qualificações do namespace. Se você souber que um namespace é importado por padrão em um projeto, não precisará qualificar totalmente os nomes desse namespace. Nomes qualificados podem ser interrompidos após um ponto (.) se forem muito longos para uma única linha, conforme mostrado no exemplo a seguir.  
  
     [!code-csharp[csProgGuideCodingConventions#1](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#1)]  
  
- Não é necessário alterar os nomes de objetos que foram criados usando as ferramentas de designer do Visual Studio para adequá-los a outras diretrizes.  
  
## <a name="layout-conventions"></a>Convenções de Layout  

Um bom layout usa formatação para enfatizar a estrutura do código e para facilitar a leitura de código. Exemplos e amostras Microsoft estão em conformidade com as seguintes convenções:  
  
- Use as configurações padrão do Editor de códigos (recuo inteligente, recuos de quatro caracteres, guias salvas como espaços). Para obter mais informações, consulte [Opções, Editor de Texto, C#, Formatação](/visualstudio/ide/reference/options-text-editor-csharp-formatting).  
  
- Gravar apenas uma instrução por linha.  
  
- Gravar apenas uma declaração por linha.  
  
- Se as linhas de continuação não devem recuar automaticamente, recue-as uma tabulação (quatro espaços).  
  
- Adicione pelo menos uma linha em branco entre as definições de método e de propriedade.  
  
- Use parênteses para criar cláusulas em uma expressão aparente, conforme mostrado no código a seguir.  
  
     [!code-csharp[csProgGuideCodingConventions#2](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#2)]  
  
## <a name="commenting-conventions"></a>Comentando Convenções  
  
- Coloque o comentário em uma linha separada, não no final de uma linha de código.  
  
- Comece o texto do comentário com uma letra maiúscula.  
  
- Termine o texto do comentário com um ponto final.  
  
- Insira um espaço entre o delimitador de comentário (/ /) e o texto do comentário, conforme mostrado no exemplo a seguir.  
  
     [!code-csharp[csProgGuideCodingConventions#3](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#3)]  
  
- Não crie blocos de asteriscos formatados em torno dos comentários.  
  
## <a name="language-guidelines"></a>Diretrizes de Linguagem  

As seções a seguir descrevem práticas que a equipe de C# segue para preparar exemplos e amostras do código.  
  
### <a name="string-data-type"></a>Tipo de dados da cadeia de caracteres  
  
- Use a [interpolação de cadeia de caracteres](../../language-reference/tokens/interpolated.md) para concatenar cadeias de caracteres curtas, como é mostrado no código a seguir.  
  
     [!code-csharp[csProgGuideCodingConventions#6](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#6)]  
  
- Para acrescentar cadeias de caracteres em loops, especialmente quando você estiver trabalhando com grandes quantidades de texto, use um objeto <xref:System.Text.StringBuilder>.  
  
     [!code-csharp[csProgGuideCodingConventions#7](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#7)]  
  
### <a name="implicitly-typed-local-variables"></a>Variáveis Locais Tipadas Implicitamente  
  
- Use a [digitação implícita](../classes-and-structs/implicitly-typed-local-variables.md) para variáveis locais quando o tipo da variável for óbvio do lado direito da atribuição ou quando o tipo exato não for importante.  
  
     [!code-csharp[csProgGuideCodingConventions#8](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#8)]  
  
- Não use [var](../../language-reference/keywords/var.md) quando o tipo não estiver aparente no lado direito da atribuição.  
  
     [!code-csharp[csProgGuideCodingConventions#9](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#9)]  
  
- Não se baseie no nome da variável para especificar o tipo dela. Ele pode não estar correto.  
  
     [!code-csharp[csProgGuideCodingConventions#10](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#10)]  
  
- Evite o uso de `var` em vez de [dynamic](../../language-reference/builtin-types/reference-types.md).  
  
- Use a digitação implícita para determinar o tipo da variável de loop nos loops [for](../../language-reference/keywords/for.md) .  
  
     O exemplo a seguir usa digitação implícita em uma instrução `for`.  
  
     [!code-csharp[csProgGuideCodingConventions#7](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#7)]  

- Não use a digitação implícita para determinar o tipo da variável de loop em loops [foreach](../../language-reference/keywords/foreach-in.md) .

     O exemplo a seguir usa a digitação explícita em uma `foreach` instrução.

     [!code-csharp[csProgGuideCodingConventions#12](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#12)]

     > [!NOTE]
     > Tenha cuidado para não alterar acidentalmente um tipo de elemento da coleção iterável. Por exemplo, é fácil alternar de <xref:System.Linq.IQueryable?displayProperty=nameWithType> para <xref:System.Collections.IEnumerable?displayProperty=nameWithType> em uma `foreach` instrução, que altera a execução de uma consulta.

### <a name="unsigned-data-type"></a>Tipo de Dados Sem Sinal  
  
Em geral, use `int` em vez de tipos sem assinatura. O uso de `int` é comum em todo o C#, e é mais fácil interagir com outras bibliotecas ao usar `int`.  
  
### <a name="arrays"></a>Matrizes  
  
Use a sintaxe concisa ao inicializar matrizes na linha da declaração.  
  
[!code-csharp[csProgGuideCodingConventions#13](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#13)]  
  
### <a name="delegates"></a>Delegados  
  
Use a sintaxe concisa ao criar instâncias de um tipo delegado.  
  
[!code-csharp[csProgGuideCodingConventions#14](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#14)]  
  
[!code-csharp[csProgGuideCodingConventions#15](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#15)]  
  
### <a name="try-catch-and-using-statements-in-exception-handling"></a>try-catch e instruções de uso no tratamento de exceção  
  
- Use uma instrução [try-catch](../../language-reference/keywords/try-catch.md) para a maioria da manipulação de exceções.  
  
     [!code-csharp[csProgGuideCodingConventions#16](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#16)]  
  
- Simplifique o código usando a [instrução using](../../language-reference/keywords/using-statement.md) do #C. Se você tiver uma instrução [try-finally](../../language-reference/keywords/try-finally.md) na qual o único código do bloco `finally` é uma chamada para o método <xref:System.IDisposable.Dispose%2A>, use, em vez disso, uma instrução `using`.  
  
     [!code-csharp[csProgGuideCodingConventions#17](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#17)]  
  
### <a name="-and-124124-operators"></a>Operadores && e &#124;&#124;  
  
Para evitar exceções e aumentar o desempenho ignorando comparações desnecessárias, use [&&](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-and-operator-) em vez de [&](../../language-reference/operators/boolean-logical-operators.md#logical-and-operator-) e [&#124;&#124;](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-or-operator-) em vez de [&#124;](../../language-reference/operators/boolean-logical-operators.md#logical-or-operator-) quando você executa comparações, conforme mostrado no exemplo a seguir.  
  
[!code-csharp[csProgGuideCodingConventions#18](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#18)]  
  
### <a name="new-operator"></a>Operador New  
  
- Use um formulário conciso de instanciação de objeto com digitação implícita, conforme mostrado na declaração a seguir.  
  
     [!code-csharp[csProgGuideCodingConventions#19](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#19)]  
  
     A linha anterior é equivalente à declaração a seguir.  
  
     [!code-csharp[csProgGuideCodingConventions#20](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#20)]  
  
- Use inicializadores de objeto para simplificar a criação do objeto.  
  
     [!code-csharp[csProgGuideCodingConventions#21](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#21)]  
  
### <a name="event-handling"></a>Tratamento de Evento  
  
Se você estiver definindo um manipulador de eventos que não necessita ser removido posteriormente, use uma expressão lambda.  
  
[!code-csharp[csProgGuideCodingConventions#22](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#22)]  
  
[!code-csharp[csProgGuideCodingConventions#23](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#23)]  
  
### <a name="static-members"></a>Membros Estáticos  
  
Chame membros [estáticos](../../language-reference/keywords/static.md) usando o nome de classe: *ClassName.StaticMember*. Essa prática torna o código mais legível, tornando o acesso estático limpo.  Não qualifique um membro estático definido em uma classe base com o nome de uma classe derivada.  Enquanto esse código é compilado, a leitura do código fica incorreta e o código poderá ser danificado no futuro se você adicionar um membro estático com o mesmo nome da classe derivada.  
  
### <a name="linq-queries"></a>Consultas LINQ  
  
- Use nomes significativos para variáveis de consulta. O exemplo a seguir usa `seattleCustomers` para os clientes que estão localizados em Seattle.  
  
     [!code-csharp[csProgGuideCodingConventions#25](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#25)]  
  
- Use aliases para se certificar de que os nomes de propriedades de tipos anônimos sejam colocados corretamente em maiúsculas, usando o padrão Pascal-Case.  
  
     [!code-csharp[csProgGuideCodingConventions#26](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#26)]  
  
- Renomeie propriedades quando os nomes de propriedades no resultado forem ambíguos. Por exemplo, se a sua consulta retornar um nome de cliente e uma ID de distribuidor, em vez de deixá-los como `Name` e `ID` no resultado, renomeie-os para esclarecer que `Name` é o nome de um cliente, e `ID` é a identificação de um distribuidor.  
  
     [!code-csharp[csProgGuideCodingConventions#27](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#27)]  
  
- Usa a digitação implícita na declaração de variáveis de consulta e de intervalo.  
  
     [!code-csharp[csProgGuideCodingConventions#25](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#25)]  
  
- Alinhe cláusulas de consulta na cláusula [from](../../language-reference/keywords/from-clause.md), conforme mostrado nos exemplos anteriores.  
  
- Use cláusulas [where](../../language-reference/keywords/where-clause.md) antes de outras cláusulas de consulta para garantir que cláusulas de consulta posteriores operem no conjunto de dados filtrado e reduzido.  
  
     [!code-csharp[csProgGuideCodingConventions#29](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#29)]  
  
- Use várias cláusulas `from` em vez de uma cláusula [join](../../language-reference/keywords/join-clause.md) para acessar coleções internas. Por exemplo, cada coleção de objetos `Student` pode conter um conjunto de pontuações no teste. Quando a próxima consulta for executada, ela retorna cada pontuação que seja acima de 90, juntamente com o sobrenome do estudante que recebeu a pontuação.  
  
     [!code-csharp[csProgGuideCodingConventions#30](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#30)]  
  
## <a name="security"></a>Segurança  

Siga as diretrizes em [Diretrizes de codificação segura](../../../standard/security/secure-coding-guidelines.md).  
  
## <a name="see-also"></a>Veja também

- [Convenções de codificação do Visual Basic](../../../visual-basic/programming-guide/program-structure/coding-conventions.md)
- [Diretrizes de codificação segura](../../../standard/security/secure-coding-guidelines.md)
