---
title: Visão geral da Entity SQL
ms.date: 03/30/2017
ms.assetid: f0bb8120-e709-40a3-ac1e-5520dc47477d
ms.openlocfilehash: b4fe852847d8b1b4bc0b80e3ba8e1f5b4aae9ff7
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202245"
---
# <a name="entity-sql-overview"></a>Visão geral da Entity SQL
[!INCLUDE[esql](../../../../../../includes/esql-md.md)]é uma linguagem semelhante a SQL que permite consultar modelos conceituais na Entity Framework. Os modelos conceituais representam dados como entidades e relações e [!INCLUDE[esql](../../../../../../includes/esql-md.md)] permitem que você consulte essas entidades e relações em um formato familiar para aqueles que usaram o SQL.  

 O Entity Framework funciona com provedores de dados específicos de armazenamento para converter generic [!INCLUDE[esql](../../../../../../includes/esql-md.md)] em consultas específicas de armazenamento. O provedor EntityClient permite executar um comando [!INCLUDE[esql](../../../../../../includes/esql-md.md)] em um modelo de entidade e retornar tipos de dados ricos que incluem resultados escalares, conjuntos de resultados e gráficos de objeto. Ao criar objetos <xref:System.Data.EntityClient.EntityCommand>, você pode especificar um nome de procedimento armazenado ou o texto de uma consulta atribuindo uma cadeia de caracteres de consulta [!INCLUDE[esql](../../../../../../includes/esql-md.md)] a sua propriedade <xref:System.Data.EntityClient.EntityCommand.CommandText%2A?displayProperty=nameWithType>. O <xref:System.Data.EntityClient.EntityDataReader> expõe os resultados da execução de um <xref:System.Data.EntityClient.EntityCommand> em um EDM. Para executar o comando que retorna <xref:System.Data.EntityClient.EntityDataReader>, chame <xref:System.Data.EntityClient.EntityCommand.ExecuteReader%2A>.  
  
 Além do provedor EntityClient, o Entity Framework permite que você use o [!INCLUDE[esql](../../../../../../includes/esql-md.md)] para executar consultas em um modelo conceitual e retornar dados como objetos CLR fortemente tipados que são instâncias de tipos de entidade. Para obter mais informações, consulte [trabalhando com objetos](../working-with-objects.md).  
  
 Esta seção fornece informações conceituais sobre a linguagem [!INCLUDE[esql](../../../../../../includes/esql-md.md)].  
  
## <a name="in-this-section"></a>Nesta seção  
 [Como o Entity SQL difere do Transact-SQL](how-entity-sql-differs-from-transact-sql.md)  
  
 [Referência rápida de Entity SQL](entity-sql-quick-reference.md)  
  
 [Sistema de tipo](type-system-entity-sql.md)  
  
 [Definições de tipo](type-definitions-entity-sql.md)  
  
 [Construir tipos](constructing-types-entity-sql.md)  
  
 [Cache de plano de consulta](query-plan-caching-entity-sql.md)  
  
 [Namespaces](namespaces-entity-sql.md)  
  
 [Identificadores](identifiers-entity-sql.md)  
  
 [Parâmetros](parameters-entity-sql.md)  
  
 [Variáveis](variables-entity-sql.md)  
  
 [Expressões sem suporte](unsupported-expressions-entity-sql.md)  
  
 [Literais](literals-entity-sql.md)  
  
 [Literais nulos e inferência de tipos](null-literals-and-type-inference-entity-sql.md)  
  
 [Conjunto de caracteres de entrada](input-character-set-entity-sql.md)  
  
 [Expressões de consulta](query-expressions-entity-sql.md)  
  
 [Funções](functions-entity-sql.md)  
  
 [Precedência de operador](operator-precedence-entity-sql.md)  
  
 [Paginação](paging-entity-sql.md)  
  
 [Semântica de comparação](comparison-semantics-entity-sql.md)  
  
 [Composta consultas aninhadas Entity SQL](composing-nested-entity-sql-queries.md)  
  
 [Tipos estruturados que permitem valor nulo](nullable-structured-types-entity-sql.md)  
  
## <a name="see-also"></a>Veja também

- [Referência de Entity SQL](entity-sql-reference.md)
- [Entity SQL Language](entity-sql-language.md) (Linguagem SQL de entidade)
- [Especificações de CSDL, SSDL e MSL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)
