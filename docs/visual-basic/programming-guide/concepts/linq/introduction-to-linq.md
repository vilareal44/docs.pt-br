---
title: Introdução ao LINQ
ms.date: 07/20/2015
ms.assetid: c6339c12-9b2d-433e-961c-0d2b7f0091c2
ms.openlocfilehash: b0fa27fd81b85eb89712f9e81f42e06505878f86
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397552"
---
# <a name="introduction-to-linq-visual-basic"></a>Introdução ao LINQ (Visual Basic)
O LINQ (consulta integrada à linguagem) é uma inovação introduzida no .NET Framework versão 3.5 que preenche a lacuna entre o mundo dos objetos e o mundo dos dados.  
  
 Tradicionalmente, consultas feitas em dados são expressas como cadeias de caracteres simples sem verificação de tipo no tempo de compilação ou suporte a IntelliSense. Além disso, você precisará aprender uma linguagem de consulta diferente para cada tipo de fonte de dados: bancos de dados SQL, documentos XML, vários serviços Web etc. O LINQ faz uma *consulta* uma construção de linguagem de primeira classe em Visual Basic. Você escreve consultas em coleções fortemente tipadas de objetos usando palavras-chave da linguagem e operadores familiares.  
  
 Você pode escrever consultas LINQ em Visual Basic para bancos de dados do SQL Server, documentos XML, conjuntos de ADO.NET e qualquer coleção de objetos que ofereça suporte <xref:System.Collections.IEnumerable> ou a <xref:System.Collections.Generic.IEnumerable%601> interface genérica. O suporte ao LINQ também é fornecido por terceiros para muitos serviços Web e outras implementações de banco de dados.  
  
 É possível usar consultas do LINQ em novos projetos ou em conjunto com consultas que não são do LINQ em projetos existentes. O único requisito é que o projeto tenha como alvo o .NET Framework 3.5 ou posterior.  
  
 A ilustração a seguir do Visual Studio mostra uma consulta do LINQ parcialmente concluída em um banco de dados do SQL Server no C# e no Visual Basic, com verificação de tipo completa e suporte a IntelliSense.  
  
 ![Diagrama que mostra uma consulta LINQ com o IntelliSense.](./media/introduction-to-linq/linq-query-intellisense.png)  
  
## <a name="next-steps"></a>Próximas etapas  
 Para saber mais detalhes sobre o LINQ, comece se familiarizando com alguns conceitos básicos na seção Introdução [introdução com o LINQ no Visual Basic](getting-started-with-linq.md)e leia a documentação da tecnologia LINQ em que você está interessado:  
  
- Bancos de dados do SQL Server: [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)  
  
- Documentos XML: [LINQ to XML (Visual Basic)](linq-to-xml.md)  
  
- Conjuntos de dados ADO.NET: [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)  
  
- Coleções, arquivos, cadeias de caracteres do .NET e assim por diante: [LINQ to Objects (Visual Basic)](linq-to-objects.md)  
  
## <a name="see-also"></a>Confira também

- [LINQ (consulta integrada à linguagem) (Visual Basic)](index.md)
