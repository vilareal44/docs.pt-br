---
title: Recursos que dão suporte ao LINQ
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic, LINQ features
- LINQ [Visual Basic], features supporting LINQ
ms.assetid: c821bb50-b6f6-4cf9-8aba-2717e465bd3a
ms.openlocfilehash: 15585cd8277b1a0df7e3b262db7c9b7a231b16fa
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84383424"
---
# <a name="visual-basic-features-that-support-linq"></a>Recursos do Visual Basic que suportam LINQ
A consulta integrada à linguagem de nome (LINQ) refere-se à tecnologia em Visual Basic que dá suporte à sintaxe de consulta e a outras construções de linguagem diretamente no idioma. Com o LINQ, você não precisa aprender uma nova linguagem para consultar uma fonte de dados externa. Você pode consultar dados em bancos de dados relacionais, repositórios XML ou objetos usando Visual Basic. Essa integração dos recursos de consulta no idioma permite a verificação do tempo de compilação para erros de sintaxe e segurança de tipo. Essa integração também garante que você já saiba a maior parte do que precisa saber para escrever consultas ricas e diversificadas em Visual Basic.  
  
 As seções a seguir descrevem as construções de linguagem que dão suporte ao LINQ em detalhes suficientes para permitir que você comece a ler a documentação introdutória, exemplos de código e aplicativos de exemplo. Você também pode clicar nos links para encontrar explicações mais detalhadas sobre como os recursos de linguagem se reúnem para habilitar a consulta integrada à linguagem. Um bom lugar para começar é [passo a passos: escrevendo consultas em Visual Basic](walkthrough-writing-queries.md).  
  
## <a name="query-expressions"></a>Expressões de consulta  
 As expressões de consulta no Visual Basic podem ser expressas em uma sintaxe declarativa semelhante à do SQL ou XQuery. No momento da compilação, a sintaxe da consulta é convertida em chamadas de método para a implementação de um provedor LINQ dos métodos de extensão do operador de consulta padrão. Os aplicativos controlam quais operadores de consulta padrão estão no escopo, especificando o namespace apropriado com uma `Imports` instrução. A sintaxe para uma expressão de consulta Visual Basic tem esta aparência:  
  
 [!code-vb[VbLINQVbFeatures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#1)]  
  
 Para obter mais informações, consulte [introdução ao LINQ no Visual Basic](../../language-features/linq/introduction-to-linq.md).  
  
## <a name="implicitly-typed-variables"></a>Variáveis de tipo implícito  
 Em vez de especificar explicitamente um tipo ao declarar e inicializar uma variável, você pode habilitar o compilador para inferir e atribuir o tipo. Isso é conhecido como *inferência de tipo local*.  
  
 As variáveis cujos tipos são inferidos são fortemente tipadas, assim como as variáveis cujo tipo você especifica explicitamente. A inferência de tipo local funciona somente quando você está definindo uma variável local dentro de um corpo de método. Para obter mais informações, consulte [instrução Option Infer](../../../language-reference/statements/option-infer-statement.md) e [inferência de tipo local](../../language-features/variables/local-type-inference.md).  
  
 O exemplo a seguir ilustra a inferência de tipo local. Para usar este exemplo, você deve definir `Option Infer` como `On` .  
  
 [!code-vb[VbLINQVbFeatures#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#2)]  
  
 A inferência de tipo local também possibilita a criação de tipos anônimos, que são descritos mais adiante nesta seção e são necessários para consultas LINQ.  
  
 No exemplo de LINQ a seguir, a inferência de tipo ocorre se `Option Infer` for `On` ou `Off` . Um erro de tempo de compilação ocorre se `Option Infer` for `Off` e `Option Strict` for `On` .  
  
 [!code-vb[VbLINQVbFeatures#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#3)]  
  
## <a name="object-initializers"></a>Inicializadores de objeto  
 Os inicializadores de objeto são usados em expressões de consulta quando você precisa criar um tipo anônimo para manter os resultados de uma consulta. Eles também podem ser usados para inicializar objetos de tipos nomeados fora de consultas. Usando um inicializador de objeto, você pode inicializar um objeto em uma única linha sem chamar explicitamente um construtor. Supondo que você tenha uma classe chamada `Customer` que tenha Public `Name` e `Phone` Properties, juntamente com outras propriedades, um inicializador de objeto pode ser usado dessa maneira:  
  
 [!code-vb[VbLINQVbFeatures#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#4)]  
  
 Para obter mais informações, consulte [inicializadores de objeto: tipos nomeados e anônimos](../../language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md).  
  
## <a name="anonymous-types"></a>Tipos anônimos  
 Tipos anônimos fornecem uma maneira conveniente de agrupar temporariamente um conjunto de propriedades em um elemento que você deseja incluir em um resultado de consulta. Isso permite que você escolha qualquer combinação de campos disponíveis na consulta, em qualquer ordem, sem definir um tipo de dados nomeado para o elemento.  
  
 Um *tipo anônimo* é construído dinamicamente pelo compilador. O nome do tipo é atribuído pelo compilador e pode ser alterado com cada nova compilação. Portanto, o nome não pode ser usado diretamente. Os tipos anônimos são inicializados da seguinte maneira:  
  
 [!code-vb[VbLINQVbFeatures#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#5)]  
  
 Para obter mais informações, consulte [Tipos Anônimos](../../language-features/objects-and-classes/anonymous-types.md).  
  
## <a name="extension-methods"></a>Métodos de Extensão  
 Os métodos de extensão permitem adicionar métodos a um tipo de dados ou interface de fora da definição. Esse recurso permite que você, em vigor, adicione novos métodos a um tipo existente sem realmente modificar o tipo. Os operadores de consulta padrão são, por si, um conjunto de métodos de extensão que fornecem a funcionalidade de consulta LINQ para qualquer tipo que implementa <xref:System.Collections.Generic.IEnumerable%601> . Outras extensões para <xref:System.Collections.Generic.IEnumerable%601> incluir <xref:System.Linq.Enumerable.Count%2A> , <xref:System.Linq.Enumerable.Union%2A> e <xref:System.Linq.Enumerable.Intersect%2A> .  
  
 O método de extensão a seguir adiciona um método Print à <xref:System.String> classe.  
  
 [!code-vb[VbLINQVbFeatures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#6)]  
  
 O método é chamado como um método de instância comum de <xref:System.String> :  
  
 [!code-vb[VbLINQVbFeatures#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#7)]  
  
 Para obter mais informações, consulte [Métodos de extensão](../../language-features/procedures/extension-methods.md).  
  
## <a name="lambda-expressions"></a>Expressões lambda  
 Uma expressão lambda é uma função sem um nome que calcula e retorna um único valor. Ao contrário das funções nomeadas, uma expressão lambda pode ser definida e executada ao mesmo tempo. O exemplo a seguir exibe 4.  
  
 [!code-vb[VbLINQVbFeatures#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#8)]  
  
 Você pode atribuir a definição de expressão lambda a um nome de variável e, em seguida, usar o nome para chamar a função. O exemplo a seguir também exibe 4.  
  
 [!code-vb[VbLINQVbFeatures#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#12)]  
  
 No LINQ, as expressões lambda são a cerca de muitos dos operadores de consulta padrão. O compilador cria expressões lambda para capturar os cálculos que são definidos em métodos de consulta fundamentais, como,,, `Where` `Select` `Order By` `Take While` e outros.  
  
 Por exemplo, o código a seguir define uma consulta que retorna todos os alunos seniores de uma lista de alunos.  
  
 [!code-vb[VbLINQVbFeatures#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#9)]  
  
 A definição de consulta é compilada no código que é semelhante ao exemplo a seguir, que usa duas expressões lambda para especificar os argumentos para `Where` e `Select` .  
  
 [!code-vb[VbLINQVbFeatures#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#10)]  
  
 Qualquer versão pode ser executada usando um `For Each` loop:  
  
 [!code-vb[VbLINQVbFeatures#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#11)]  
  
 Para obter mais informações, consulte [Expressões Lambda](../../language-features/procedures/lambda-expressions.md).  
  
## <a name="see-also"></a>Confira também

- [LINQ (consulta integrada à linguagem) (Visual Basic)](index.md)
- [Introdução a LINQ no Visual Basic](getting-started-with-linq.md)
- [LINQ e cadeias de caracteres (Visual Basic)](linq-and-strings.md)
- [Instrução Option Infer](../../../language-reference/statements/option-infer-statement.md)
- [Instrução Option Strict](../../../language-reference/statements/option-strict-statement.md)
