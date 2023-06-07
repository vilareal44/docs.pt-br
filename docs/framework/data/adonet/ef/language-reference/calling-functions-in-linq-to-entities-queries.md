---
title: Chamando funções em consultas no LINQ to Entities
description: Use estes artigos para ver como as classes EntityFunctions e SqlFunctions fornecem acesso às funções canônicas e de banco de dados como parte do Entity Framework.
ms.date: 03/30/2017
ms.assetid: 12a525a9-727c-4464-a0c7-71a0ef541792
ms.openlocfilehash: faa6406713592f10e5e7371cd73f29bec4b03b7b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286851"
---
# <a name="calling-functions-in-linq-to-entities-queries"></a>Chamando funções em consultas no LINQ to Entities
Os tópicos nesta seção descrevem como chamar funções em consultas LINQ to Entities.  
  
 As classes <xref:System.Data.Objects.EntityFunctions> e <xref:System.Data.Objects.SqlClient.SqlFunctions> fornecem acesso às funções canônicas e de banco de dados como parte do Entity Framework. Para obter mais informações, consulte [como: chamar funções canônicas](how-to-call-canonical-functions.md) e [como chamar funções de banco de dados](how-to-call-database-functions.md).  
  
 O processo para chamar uma função personalizada exige três etapas básicas:  
  
1. Defina uma função no modelo conceitual ou declare uma função no modelo de armazenamento.  
  
2. Adicione um método ao seu aplicativo e mapeie-o para a função no modelo com um <xref:System.Data.Objects.DataClasses.EdmFunctionAttribute>.  
  
3. Chame a função em uma consulta LINQ to Entities.  
  
 Para obter mais informações, consulte os tópicos nesta seção.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Como: Funções canônicas de chamada](how-to-call-canonical-functions.md)  
  
 [Como: Funções de base de dados de chamada](how-to-call-database-functions.md)  
  
 [Como: Funções de base de dados personalizados de chamada](how-to-call-custom-database-functions.md)  
  
 [Como: o chamar funções definidas em consultas](how-to-call-model-defined-functions-in-queries.md)  
  
 [Como: o chamar funções definidas como métodos de objeto](how-to-call-model-defined-functions-as-object-methods.md)  
  
## <a name="see-also"></a>Veja também

- [Consultas no LINQ to Entities](queries-in-linq-to-entities.md)
- [Funções canônicas](canonical-functions.md)
- [Visão geral do arquivo. edmx](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/cc982042(v=vs.100))
- [Como definir funções personalizadas no modelo conceitual](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd456812(v=vs.100))
