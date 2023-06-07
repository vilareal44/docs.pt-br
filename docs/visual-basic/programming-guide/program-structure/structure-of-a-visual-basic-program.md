---
title: Estrutura de um programa Visual Basic
ms.date: 07/20/2015
helpviewer_keywords:
- conditional compilation [Visual Basic], Visual Basic
- program structure [Visual Basic], Visual Basic
- procedures [Visual Basic], structure
- Visual Basic code, program structure
ms.assetid: ad0c6531-d762-4c77-a700-de16b07b6119
ms.openlocfilehash: dc6b38d78f02a42c8e7cc2aa964e9f3f74996f44
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408758"
---
# <a name="structure-of-a-visual-basic-program"></a>Estrutura de um programa Visual Basic
Um programa de Visual Basic é criado A partir dos blocos de construção padrão. Uma *solução* consiste em um ou mais projetos. Um *projeto* , por sua vez, pode conter um ou mais assemblies. Cada *assembly* é compilado de um ou mais arquivos de origem. Um *arquivo de origem* fornece a definição e a implementação de classes, estruturas, módulos e interfaces, que, por fim, contêm todo o seu código.  
  
 Para obter mais informações sobre esses blocos de construção de um programa de Visual Basic, consulte [soluções e projetos](/visualstudio/ide/solutions-and-projects-in-visual-studio) e [assemblies no .net](../../../standard/assembly/index.md).  
  
## <a name="file-level-programming-elements"></a>Elementos de programação em nível de arquivo  
 Quando você inicia um projeto ou arquivo e abre o editor de código, você vê algum código já em vigor e na ordem correta. Qualquer código que você escrever deve seguir a seguinte sequência:  
  
1. `Option`instruções  
  
2. `Imports`instruções  
  
3. `Namespace`instruções e elementos de nível de namespace  
  
 Se você inserir instruções em uma ordem diferente, poderão ocorrer erros de compilação.  
  
 Um programa também pode conter instruções de compilação condicional. Você pode intercalar esses arquivos no arquivo de origem entre as instruções da sequência anterior.  
  
### <a name="option-statements"></a>Instruções de opção  
 `Option`as instruções estabelecem regras básicas para o código subsequente, ajudando a evitar erros de sintaxe e lógica. A [instrução Option Explicit](../../language-reference/statements/option-explicit-statement.md) garante que todas as variáveis sejam declaradas e escritas corretamente, o que reduz o tempo de depuração. A [instrução Option Strict](../../language-reference/statements/option-strict-statement.md) ajuda a minimizar erros lógicos e perda de dados que podem ocorrer quando você trabalha entre variáveis de tipos de dados diferentes. A [instrução Option Compare](../../language-reference/statements/option-compare-statement.md) especifica a maneira como as cadeias de caracteres são comparadas entre si, com base em seus `Binary` `Text` valores ou.  
  
### <a name="imports-statements"></a>Instruções Imports  
 Você pode incluir uma [instrução Imports (namespace e tipo do .net)](../../language-reference/statements/imports-statement-net-namespace-and-type.md) para importar nomes definidos fora do seu projeto. Uma `Imports` instrução permite que seu código faça referência a classes e a outros tipos definidos no namespace importado, sem precisar qualificá-los. Você pode usar tantas `Imports` instruções quantas forem apropriadas. Para obter mais informações, consulte [References e a instrução Imports](references-and-the-imports-statement.md).  
  
### <a name="namespace-statements"></a>Instruções de namespace  
 Os namespaces ajudam você a organizar e classificar seus elementos de programação para facilitar o agrupamento e o acesso. Use a [instrução namespace](../../language-reference/statements/namespace-statement.md) para classificar as instruções a seguir em um namespace específico. Para obter mais informações, consulte [Namespaces no Visual Basic](namespaces.md).  
  
### <a name="conditional-compilation-statements"></a>Instruções de compilação condicional  
 As instruções de compilação condicional podem aparecer quase em qualquer lugar no arquivo de origem. Elas fazem com que partes do seu código sejam incluídas ou excluídas no tempo de compilação, dependendo de determinadas condições. Você também pode usá-los para depurar seu aplicativo, pois o código condicional é executado somente no modo de depuração. Para obter mais informações, consulte [compilação condicional](conditional-compilation.md).  
  
## <a name="namespace-level-programming-elements"></a>Elementos de programação de nível de namespace  
 Classes, estruturas e módulos contêm todo o código em seu arquivo de origem. Eles são elementos *de nível de namespace* , que podem aparecer em um namespace ou no nível do arquivo de origem. Eles mantêm as declarações de todos os outros elementos de programação. Interfaces, que definem as assinaturas de elemento, mas não fornecem nenhuma implementação, também aparecem no nível de módulo. Para obter mais informações sobre os elementos de nível de módulo, consulte o seguinte:  
  
- [Instrução Class](../../language-reference/statements/class-statement.md)  
  
- [Instrução Structure](../../language-reference/statements/structure-statement.md)  
  
- [Instrução Module](../../language-reference/statements/module-statement.md)  
  
- [Instrução Interface](../../language-reference/statements/interface-statement.md)  
  
 Os elementos de dados no nível de namespace são enumerações e delegados.  
  
## <a name="module-level-programming-elements"></a>Elementos de programação em nível de módulo  
 Procedimentos, operadores, propriedades e eventos são os únicos elementos de programação que podem conter código executável (instruções que executam ações em tempo de execução). Eles são os elementos de *nível de módulo* do seu programa. Para obter mais informações sobre os elementos de nível de procedimento, consulte o seguinte:  
  
- [Instrução Function](../../language-reference/statements/function-statement.md)  
  
- [Instrução Sub](../../language-reference/statements/sub-statement.md)  
  
- [Instrução Declare](../../language-reference/statements/declare-statement.md)  
  
- [Instrução Operator](../../language-reference/statements/operator-statement.md)  
  
- [Instrução Property](../../language-reference/statements/property-statement.md)  
  
- [Instrução Event](../../language-reference/statements/event-statement.md)  
  
 Os elementos de dados no nível do módulo são variáveis, constantes, enumerações e delegados.  
  
## <a name="procedure-level-programming-elements"></a>Elementos de programação de nível de procedimento  
 A maior parte do conteúdo dos elementos de *nível de procedimento* são instruções Executáveis, que constituem o código de tempo de execução do seu programa. Todo o código executável deve estar em algum procedimento (,,,,,,, `Function` `Sub` `Operator` `Get` `Set` `AddHandler` `RemoveHandler` `RaiseEvent` ). Para obter mais informações, consulte [Instruções](../language-features/statements.md).  
  
 Os elementos de dados no nível de procedimento são limitados a variáveis locais e constantes.  
  
## <a name="the-main-procedure"></a>O procedimento principal  
 O `Main` procedimento é o primeiro código a ser executado quando seu aplicativo tiver sido carregado. `Main`serve como o ponto de partida e o controle geral para seu aplicativo. Há quatro variedades de `Main` :  
  
- `Sub Main()`  
  
- `Sub Main(ByVal cmdArgs() As String)`  
  
- `Function Main() As Integer`  
  
- `Function Main(ByVal cmdArgs() As String) As Integer`  
  
 A variedade mais comum desse procedimento é `Sub Main()` . Para obter mais informações, consulte o [procedimento principal em Visual Basic](main-procedure.md).  
  
## <a name="see-also"></a>Confira também

- [Procedimento principal no Visual Basic](main-procedure.md)
- [Convenções de nomenclatura do Visual Basic](naming-conventions.md)
- [Limitações do Visual Basic](limitations.md)
