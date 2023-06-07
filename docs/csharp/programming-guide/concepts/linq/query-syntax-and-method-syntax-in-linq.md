---
title: Sintaxe de consulta e sintaxe de método em LINQ (C#)
description: Saiba mais sobre sintaxe de consulta e sintaxe de método no LINQ. Isso inclui métodos de extensão de operador de consulta padrão e expressões lambda.
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ [C#], query syntax vs. method syntax
- queries [LINQ in C#], syntax comparisons
ms.assetid: eedd6dd9-fec2-428c-9581-5b8783810ded
ms.openlocfilehash: 46b0e44182d22158aa5fa54a0f44bae70aa8ddd9
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063036"
---
# <a name="query-syntax-and-method-syntax-in-linq-c"></a>Sintaxe de consulta e sintaxe de método em LINQ (C#)
A maioria das consultas na documentação do LINQ (consulta integrada à linguagem introdutória) é escrita usando a sintaxe de consulta declarativa do LINQ. No entanto, a sintaxe de consulta deve ser convertida em chamadas de método para o CLR (Common Language Runtime) do .NET quando o código for compilado. Essas chamadas de método invocam os operadores de consulta padrão, que têm nomes como `Where`, `Select`, `GroupBy`, `Join`, `Max` e `Average`. Você pode chamá-los diretamente usando a sintaxe de método em vez da sintaxe de consulta.  
  
 A sintaxe de consulta e a sintaxe de método são semanticamente idênticas, mas muitas pessoas acham a sintaxe de consulta mais simples e fácil de ler. Algumas consultas devem ser expressadas como chamadas de método. Por exemplo, você deve usar uma chamada de método para expressar uma consulta que recupera o número de elementos que correspondem a uma condição especificada. Você também deve usar uma chamada de método para uma consulta que recupera o elemento que tem o valor máximo em uma sequência de origem. A documentação de referência para os operadores de consulta padrão no namespace <xref:System.Linq> geralmente usa a sintaxe de método. Portanto, mesmo ao começar a escrever consultas LINQ, é útil estar familiarizado com a forma de usar a sintaxe do método em consultas e nas próprias expressões de consulta.  
  
## <a name="standard-query-operator-extension-methods"></a>Métodos de Extensão do Operador de Consulta Padrão  
 O exemplo a seguir mostra uma *expressão de consulta* simples e a consulta semanticamente equivalente escrita como uma *consulta baseada em método*.  
  
 [!code-csharp[csLINQGettingStarted#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#22)]  
  
 A saída dos dois exemplos é idêntica. Você pode ver que o tipo da variável de consulta é o mesmo em ambas as formas: <xref:System.Collections.Generic.IEnumerable%601>.  
  
 Para entender a consulta baseada em método, vamos examiná-la melhor. No lado direito da expressão, observe que a cláusula `where` agora é expressa como um método de instância no objeto `numbers`, que, como você deve se lembrar, tem um tipo de `IEnumerable<int>`. Se você estiver familiarizado com a interface <xref:System.Collections.Generic.IEnumerable%601> genérica, você saberá que ela não tem um método `Where`. No entanto, se você invocar a lista de conclusão do IntelliSense no IDE do Visual Studio, verá não apenas um método `Where`, mas muitos outros métodos como `Select`, `SelectMany`, `Join` e `Orderby`. Esses são todos os operadores de consulta padrão.  
  
 ![Captura de tela que mostra todos os operadores de consulta padrão no IntelliSense.](./media/query-syntax-and-method-syntax-in-linq/standard-query-operators.png)  
  
 Embora pareça como se <xref:System.Collections.Generic.IEnumerable%601> tivesse sido redefinido para incluir esses métodos adicionais, na verdade esse não é o caso. Os operadores de consulta padrão são implementados como um novo tipo de método chamado *métodos de extensão*. Métodos de extensão "estendem" um tipo existente, eles podem ser chamados como se fossem métodos de instância no tipo. Os operadores de consulta padrão estendem <xref:System.Collections.Generic.IEnumerable%601> e que é por esse motivo que você pode escrever `numbers.Where(...)`.  
  
 Para começar a usar o LINQ, tudo o que você realmente precisa saber sobre métodos de extensão é como colocá-los em escopo em seu aplicativo usando as `using` diretivas corretas. Do ponto de vista do aplicativo, um método de extensão e um método de instância normal são iguais.  
  
 Para obter mais informações sobre os métodos de extensão, consulte [Métodos de extensão](../../classes-and-structs/extension-methods.md). Para obter mais informações sobre os operadores de consulta padrão, consulte [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md). Alguns provedores de LINQ, como [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] e [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] , implementam seus próprios operadores de consulta padrão e métodos de extensão adicionais para outros tipos, além de <xref:System.Collections.Generic.IEnumerable%601> .  
  
## <a name="lambda-expressions"></a>Expressões lambda  
 No exemplo anterior, observe que a expressão condicional (`num % 2 == 0`) é passada como um argumento embutido para o método `Where`: `Where(num => num % 2 == 0).` Essa expressão embutida é chamada de uma expressão lambda. É uma maneira conveniente de escrever um código que de outra forma precisaria ser escrito de forma mais complicada como um método anônimo, um delegado genérico ou uma árvore de expressão. No C# `=>` é o operador lambda, que é lido como "vai para". O `num` à esquerda do operador é a variável de entrada que corresponde ao `num` na expressão de consulta. O compilador pode inferir o tipo de `num` porque ele sabe que `numbers` é um tipo <xref:System.Collections.Generic.IEnumerable%601> genérico. O corpo do lambda é exatamente igual à expressão na sintaxe de consulta ou em qualquer outra expressão ou instrução C#, ele pode incluir chamadas de método e outra lógica complexa. O "valor retornado" é apenas o resultado da expressão.  
  
 Para começar a usar o LINQ, você não precisa usar as lambdas extensivamente. No entanto, determinadas consultas só podem ser expressadas em sintaxe de método e algumas delas requerem expressões lambda. Depois de se familiarizar mais com as lambdas, você descobrirá que elas são uma ferramenta poderosa e flexível em sua caixa de ferramentas do LINQ. Para obter mais informações, consulte [Expressões Lambda](../../../language-reference/operators/lambda-expressions.md).  
  
## <a name="composability-of-queries"></a>Possibilidade de Composição das Consultas  
 No exemplo de código anterior, observe que o método `OrderBy` é invocado usando o operador ponto na chamada para `Where`. `Where` produz uma sequência filtrada e, em seguida, `Orderby` opera nessa sequência classificando-a. Como as consultas retornam uma `IEnumerable`, você pode escrevê-las na sintaxe de método encadeando as chamadas de método. Isso é o que o compilador faz nos bastidores quando você escreve consultas usando a sintaxe de consulta. E como uma variável de consulta não armazena os resultados da consulta, você pode modificá-la ou usá-la como base para uma nova consulta a qualquer momento, mesmo depois que ela foi executada.  
