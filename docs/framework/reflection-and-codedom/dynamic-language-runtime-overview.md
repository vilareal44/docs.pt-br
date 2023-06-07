---
title: Visão geral do Dynamic Language Runtime | Microsoft Docs
description: Leia uma visão geral do DLR (tempo de execução de linguagem dinâmica) no .NET. O DLR é um ambiente de tempo de execução que adiciona um conjunto de serviços para linguagens dinâmicas para o CLR.
ms.date: 03/30/2017
helpviewer_keywords:
- dynamic language runtime
- IronPython
- DLR
- IronRuby
ms.assetid: f769a271-8aff-4bea-bfab-6160217ce23d
ms.openlocfilehash: 2272bc60af35e3cdec3e1a71bbc6516565b4ec6e
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475145"
---
# <a name="dynamic-language-runtime-overview"></a>Visão geral do Dynamic Language Runtime

O DLR (*Dynamic Language Runtime*) é um ambiente de tempo de execução que adiciona um conjunto de serviços para as linguagens dinâmicas do CLR (Common Language Runtime). O DLR torna mais fácil desenvolver linguagens dinâmicas para execução no .NET Framework e adicionar recursos dinâmicos a linguagens de tipo estático.

Linguagens dinâmicas podem identificar o tipo de um objeto no tempo de execução, enquanto em linguagens de tipo estático como C# e Visual Basic (em que se usa `Option Explicit On`) você deve especificar os tipos de objeto no tempo de design. Exemplos de linguagens dinâmicas são Lisp, Smalltalk, JavaScript, PHP, Ruby, Python, ColdFusion, Lua, Cobra e Groovy.

A maioria das linguagens dinâmicas fornece as seguintes vantagens para os desenvolvedores:

- A capacidade de usar um loop de comentários rápidos (REPL ou loop de leitura-avaliação-impressão). Isso permite que você insira várias instruções e execute-as para ver os resultados imediatamente.

- Suporte para desenvolvimento descendente ou ascendente, que é o mais tradicional. Por exemplo, quando você usa uma abordagem descendente, é possível chamar funções que ainda não foram implementadas e adicionar implementações subjacentes quando precisar deles.

- É mais fácil realizar refatoração e modificações no código, pois não é necessário alterar as declarações de tipo estático por todo o código.

Linguagens dinâmicas podem ser excelentes linguagens de script. Os clientes podem estender facilmente os aplicativos criados usando linguagens dinâmicas com novos comandos e funcionalidades. As linguagens dinâmicas também são usadas com frequência para criar sites da Web e agentes de teste, manter os farms de servidores, desenvolver vários utilitários e executar transformações de dados.

A finalidade do DLR é permitir que um sistema das linguagens dinâmicas seja executado no .NET Framework e proporcionar interoperabilidade com o .NET. O DLR adiciona objetos dinâmicos para C# e Visual Basic para dar suporte ao comportamento dinâmico nesses idiomas e habilitar sua interoperação com linguagens dinâmicas.

O DLR também ajuda você a criar bibliotecas que dão suporte a operações dinâmicas. Por exemplo, se você tiver uma biblioteca que usa objetos XML ou JSON (JavaScript Object Notation), os objetos poderão aparecer como objetos dinâmicos para as linguagens que usam o DLR. Isso permite que os usuários da biblioteca escrevam código sintaticamente mais simples e mais natural para operar com objetos e acessar membros do objeto.

Por exemplo, você pode usar o seguinte código para incrementar um contador em XML em C#.

`Scriptobj.SetProperty("Count", ((int)GetProperty("Count")) + 1);`

Usando o DLR, você poderia usar o código a seguir em vez disso para a mesma operação.

`scriptobj.Count += 1;`

Como o CLR, o DLR faz parte do .NET Framework e é fornecido com os pacotes de instalação do .NET Framework e do Visual Studio. A versão de software livre do DLR também está disponível para download no repositório [IronLanguages/dlr](https://github.com/IronLanguages/dlr) no GitHub.

> [!NOTE]
> A versão de software livre do DLR tem todos os recursos do DLR incluídos no Visual Studio e no .NET Framework. Ela também dá suporte adicional aos implementadores de linguagem. Para obter mais informações, consulte a documentação no repositório [IronLanguages/dlr](https://github.com/IronLanguages/dlr) no GitHub.

Exemplos de linguagens desenvolvidos usando o DLR incluem:

- IronPython. Disponível como um software livre no site [GitHub](https://github.com/IronLanguages/ironpython2).

- IronRuby. Disponível como software livre no site do [IronRuby](http://ironruby.net/) .

## <a name="primary-dlr-advantages"></a>Principais vantagens do DLR
 O DLR fornece as vantagens a seguir.

### <a name="simplifies-porting-dynamic-languages-to-the-net-framework"></a>Simplifica a portabilidade de linguagens dinâmicas para o .NET Framework
 O DLR permite que os implementadores de linguagem evitem criar analisadores tradicionais, lexicais ou semânticos, geradores de código e outras ferramentas que eles tradicionalmente precisavam criar eles mesmos. Para usar o DLR, uma linguagem precisa produzir *árvores de expressão*, que representam o código no nível de linguagem em uma estrutura em forma de árvore, rotinas auxiliares de runtime e objetos dinâmicos opcionais que implementam a interface <xref:System.Dynamic.IDynamicMetaObjectProvider>. O DLR e o .NET Framework automatizam muitas tarefas de geração de código e de análise de código. Isso permite que os implementadores de linguagem se concentrem em recursos de linguagem exclusivos.

### <a name="enables-dynamic-features-in-statically-typed-languages"></a>Habilitar recursos dinâmicos em linguagens de tipo estático
 Linguagens do .NET Framework existentes como C# e Visual Basic podem criar objetos dinâmicos e usá-los junto com objetos de tipo estático. Por exemplo, C# e Visual Basic podem usar objetos dinâmicos para reflexão de HTML, DOM (Modelo de Objeto do Documento) e .NET.

### <a name="provides-future-benefits-of-the-dlr-and-net-framework"></a>Fornece benefícios futuros do DLR e do .NET Framework
 Linguagens implementadas usando o DLR podem aproveitar melhorias futuras do DLR e do .NET Framework. Por exemplo, se o .NET Framework lançar uma nova versão que tenha um coletor de lixo aprimorado ou tempo de carregamento de assembly mais rápido, linguagens implementadas usando o DLR imediatamente obterão os mesmos benefícios. Se o DLR agregar otimizações como melhor compilação, o desempenho também melhorará para todas as linguagens implementadas usando o DLR.

### <a name="enables-sharing-of-libraries-and-objects"></a>Permite o compartilhamento de bibliotecas e objetos
 Os objetos e bibliotecas implementados em uma linguagem podem ser usadas por outras. O DLR também proporciona interoperabilidade entre linguagens dinâmicas e com tipo estático. Por exemplo, C# pode declarar um objeto dinâmico que usa uma biblioteca que é escrita em uma linguagem dinâmica. Ao mesmo tempo, linguagens dinâmicas podem usar bibliotecas do .NET Framework.

### <a name="provides-fast-dynamic-dispatch-and-invocation"></a>Fornece invocação e expedição dinâmica rápidas
 O DLR fornece a rápida execução das operações dinâmicas dando suporte a cache polimórfico avançado. O DLR cria regras para associar operações que usam objetos para as implementações de runtime necessárias e, em seguida, armazena essas regras para evitar o esgotamento de recursos de computações de associação durante execuções sucessivas do mesmo código nos mesmos tipos de objetos.

## <a name="dlr-architecture"></a>Arquitetura do DLR
 A ilustração a seguir mostra a arquitetura do dynamic language runtime.

 ![Visão geral da arquitetura de tempo de execução de linguagem dinâmica](./media/dlr-archoverview.png "DLR_ArchOverview") Arquitetura do DLR

 O DLR agrega um conjunto de serviços ao CLR para melhorar o suporte a linguagens dinâmicas. esses serviços incluem o seguinte:

- Árvores de expressão. O DLR usa árvores de expressão para representar a semântica da linguagem. Para tal, o DLR tem árvores de expressão LINQ estendidos para incluir o fluxo de controle, a atribuição e outros nós de modelagem de linguagem. Para obter mais informações, consulte [Árvores de expressão (C#)](../../csharp/programming-guide/concepts/expression-trees/index.md) ou [Árvores de expressão (Visual Basic)](../../visual-basic/programming-guide/concepts/expression-trees/index.md).

- Cache de site de chamada. Um *site de chamada dinâmico* é um local no código em que você executa uma operação como `a + b` ou `a.b()` em objetos dinâmicos. O DLR armazena em cache as características de `a` e `b` (geralmente os tipos desses objetos) e informações sobre a operação. Se uma operação tiver sido executada anteriormente, o DLR recupera todas as informações necessárias do cache para expedição rápida.

- Interoperabilidade de objeto dinâmico. O DLR fornece um conjunto de interfaces e classes que representam as operações e objetos dinâmicos e podem ser usados por implementadores de linguagem e autores de bibliotecas dinâmicas. Essas classes e interfaces incluem <xref:System.Dynamic.IDynamicMetaObjectProvider>, <xref:System.Dynamic.DynamicMetaObject>, <xref:System.Dynamic.DynamicObject> e <xref:System.Dynamic.ExpandoObject>.

O DLR usa associadores em sites de chamada para se comunicar não apenas com o .NET Framework, mas com outras infraestruturas e serviços, incluindo Silverlight e COM. Associadores encapsulam a semântica da linguagem e especificam como executar operações em um site de chamada usando árvores de expressão. Isso permite que linguagens dinâmicas e de tipo estático que utilizam o DLR compartilhem bibliotecas e obtenham acesso a todas as tecnologias às quais o DLR dá suporte.

## <a name="dlr-documentation"></a>Documentação do DLR
 Para obter mais informações sobre como usar a versão de software livre do DLR para adicionar comportamento dinâmico a uma linguagem ou sobre como habilitar o uso de uma linguagem dinâmica com o .NET Framework, consulte a documentação no repositório [IronLanguages/dlr](https://github.com/IronLanguages/dlr/tree/master/Docs) no GitHub.

## <a name="see-also"></a>Consulte também

- <xref:System.Dynamic.ExpandoObject>
- <xref:System.Dynamic.DynamicObject>
- [Common Language Runtime](../../standard/clr.md)
- [Árvores de expressão (C#)](../../csharp/programming-guide/concepts/expression-trees/index.md)
- [Árvores de expressão (Visual Basic)](../../visual-basic/programming-guide/concepts/expression-trees/index.md)
- [Walkthrough: Criando e usando objetos dinâmicos](../../csharp/programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)
