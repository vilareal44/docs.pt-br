---
title: A expressão do tipo <type> não pode ser consultada
ms.date: 07/20/2015
f1_keywords:
- bc36593
- vbc36593
helpviewer_keywords:
- BC36593
ms.assetid: 6f1f5860-bf97-4885-9ebb-bc87d028095c
ms.openlocfilehash: e61b4dac109f714b5cf25226d1029237ca77032d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409472"
---
# <a name="expression-of-type-type-is-not-queryable"></a>A expressão do tipo \<type> não pode ser consultada
A expressão do tipo \<type> não é passível de consulta. Verifique se você não tem uma referência de assembly e/ou uma importação de namespace para o provedor LINQ.  
  
 Os tipos consultáveis são definidos <xref:System.Linq> nos <xref:System.Data.Linq> <xref:System.Xml.Linq> namespaces, e. Você deve importar um ou mais desses namespaces para executar consultas LINQ.  
  
 O <xref:System.Linq> namespace permite que você consulte objetos como coleções e matrizes usando LINQ.  
  
 O <xref:System.Data.Linq> namespace permite que você consulte conjuntos de dados do ADO.net e SQL Server bancos de dados usando o LINQ.  
  
 O <xref:System.Xml.Linq> namespace permite que você consulte XML usando LINQ e use recursos XML no Visual Basic.  
  
 **ID do erro:** BC36593  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Adicione uma `Import` instrução para o <xref:System.Linq> <xref:System.Data.Linq> namespace, ou <xref:System.Xml.Linq> ao seu arquivo de código. Você também pode importar namespaces para seu projeto usando a página **referências** do designer de projeto (**meu projeto**).  
  
2. Certifique-se de que o tipo que você identificou como a origem da consulta é um tipo consultável. Ou seja, um tipo que implementa <xref:System.Collections.Generic.IEnumerable%601> ou <xref:System.Linq.IQueryable%601> .  
  
## <a name="see-also"></a>Confira também

- <xref:System.Linq>
- <xref:System.Data.Linq>
- <xref:System.Xml.Linq>
- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [LINQ](../../programming-guide/language-features/linq/index.md)
- [XML](../../programming-guide/language-features/xml/index.md)
- [Referências e a instrução Imports](../../programming-guide/program-structure/references-and-the-imports-statement.md)
- [Instrução Imports (tipo e namespace .NET)](../statements/imports-statement-net-namespace-and-type.md)
- [Página Referências, Designer de Projeto (Visual Basic)](/visualstudio/ide/reference/references-page-project-designer-visual-basic)
