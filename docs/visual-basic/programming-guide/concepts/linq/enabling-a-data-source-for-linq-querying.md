---
title: Habilitando uma fonte de dados para LINQ Querying2
ms.date: 07/20/2015
ms.assetid: c412f0cf-ff0e-4993-ab3d-1b49e23f00f8
ms.openlocfilehash: 4ab0e2a2fc3d04eb375a4646e4133e6e5cbb47db
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84375298"
---
# <a name="enabling-a-data-source-for-linq-querying"></a>Habilitando uma fonte de dados para consulta LINQ

Há várias maneiras de estender o LINQ para permitir que qualquer fonte de dados seja consultada no padrão LINQ. A fonte de dados pode ser uma estrutura de dados, um serviço Web, um sistema de arquivos ou um banco de dados, apenas para citar algumas opções. O padrão LINQ torna mais fácil para os clientes consultar uma fonte de dados para a qual a consulta LINQ está habilitada, porque a sintaxe e o padrão da consulta não são alterados. As maneiras pelas quais o LINQ pode ser estendido para essas fontes de dados incluem o seguinte:

- Implementar a <xref:System.Collections.Generic.IEnumerable%601> interface em um tipo para habilitar LINQ to Objects consulta desse tipo.

- Criando métodos de operador de consulta padrão, como <xref:System.Linq.Enumerable.Where%2A> e <xref:System.Linq.Enumerable.Select%2A> que estendem um tipo, para habilitar a consulta LINQ personalizada desse tipo.

- Criação de um provedor para sua fonte de dados que implemente a interface <xref:System.Linq.IQueryable%601>. Um provedor que implementa essa interface recebe consultas LINQ na forma de árvores de expressão, que pode ser executada de forma personalizada, por exemplo, remotamente.

- Criar um provedor para sua fonte de dados que aproveita uma tecnologia LINQ existente. Tal provedor habilitaria não apenas a consulta, mas também operações de inserção, atualização e exclusão e mapeamento para tipos definidos pelo usuário.

Este tópico aborda essas opções.

## <a name="how-to-enable-linq-querying-of-your-data-source"></a>Como habilitar consultas de LINQ da sua fonte de dados

### <a name="in-memory-data"></a>Dados na memória
 Há duas maneiras de habilitar a consulta LINQ de dados na memória. Se os dados forem de um tipo que implementa <xref:System.Collections.Generic.IEnumerable%601> , você poderá consultar os dados usando LINQ to Objects. Se não fizer sentido habilitar a enumeração do seu tipo implementando a <xref:System.Collections.Generic.IEnumerable%601> interface, você poderá definir métodos do operador de consulta padrão LINQ nesse tipo ou criar métodos do operador de consulta padrão do LINQ que estendem o tipo. As implementações personalizadas dos operadores de consulta padrão devem usar a execução adiada para retornar os resultados.

### <a name="remote-data"></a>Dados remotos
 A melhor opção para habilitar consultas LINQ de uma fonte de dados remota é implementar a <xref:System.Linq.IQueryable%601> interface. No entanto, isso é diferente de estender um provedor como o [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] para uma fonte de dados. Nenhum modelo de provedor para estender as tecnologias LINQ existentes, como [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] , para outros tipos de fonte de dados, está disponível no Visual Studio 2008.

## <a name="iqueryable-linq-providers"></a>Provedores IQueryable de LINQ
 Provedores LINQ que implementam <xref:System.Linq.IQueryable%601> o podem variar muito em sua complexidade. Esta seção discute os diferentes níveis de complexidade.

 Um provedor menos complexo de `IQueryable` poderia fazer a interface com um único método de um serviço Web. Esse tipo de provedor é muito específico porque ele espera informações específicas nas consultas que manipula. Ele possui um sistema de tipos fechado, talvez expondo um único tipo de resultado. A maior parte da execução da consulta ocorre localmente, por exemplo, usando as implementações de <xref:System.Linq.Enumerable> dos operadores de consulta padrão. Um provedor menos complexo poderia examinar somente uma expressão de chamada do método na árvore de expressões que representa a consulta e deixar que a lógica restante da consulta fosse manipulada em outro lugar.

 Um provedor de `IQueryable` de complexidade média pode destinar uma fonte de dados que tem uma linguagem de consulta parcialmente expressiva. Se o destino é um serviço Web, ele pode fazer a interface com mais de um método de serviço Web e selecionar o método a ser chamado com base na questão representada pela consulta. Um provedor de complexidade média teria um sistema de tipos maiores do que um provedor simples, mas continuaria a ser um sistema de tipo fixo. Por exemplo, o provedor pode expor os tipos que têm relação um para muitos que podem ser atravessados, mas não forneceria tecnologia de mapeamento de tipos definidos pelo usuário.

 Um `IQueryable` provedor complexo, como o [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] provedor, pode traduzir consultas completas do LINQ para uma linguagem de consulta expressiva, como SQL. Um provedor complexo é mais geral do que um provedor menos complexo porque pode manipular uma variedade mais ampla de perguntas na consulta. Ele também possui um sistema de tipos abertos e, consequentemente, deve conter uma infraestrutura extensiva para mapear tipos definidos pelo usuário. Desenvolver um provedor complexo requer uma quantidade significativa de esforço.

## <a name="see-also"></a>Confira também

- <xref:System.Linq.IQueryable%601>
- <xref:System.Collections.Generic.IEnumerable%601>
- <xref:System.Linq.Enumerable>
- [Visão geral de operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md)
- [LINQ to Objects (Visual Basic)](linq-to-objects.md)
