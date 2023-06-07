---
title: LINQ to ADO.NET (página do portal)
description: LINQ to ADO.NET permite consultar qualquer objeto enumerável no ADO.NET usando o modelo de programação LINQ. Saiba mais sobre as três tecnologias do LINQ ADO.NET.
ms.date: 07/20/2015
ms.assetid: 6bd269b4-3509-4688-b672-836008704182
ms.openlocfilehash: 0b09e678d29d27de5758cf5a5fcacd7391342792
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165759"
---
# <a name="linq-to-adonet-portal-page"></a>LINQ to ADO.NET (página do portal)
LINQ to ADO.NET permite consultar qualquer objeto enumerável no ADO.NET usando o modelo de programação LINQ (consulta integrada à linguagem).  
  
> [!NOTE]
> A documentação LINQ to ADO.NET está localizada na seção ADO.NET do SDK do .NET Framework: [LINQ e ADO.net](../../../../framework/data/adonet/linq-and-ado-net.md).  
  
 Há três tecnologias de LINQ (consulta integrada à linguagem) ADO.NET separadas: LINQ to DataSet, [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] e LINQ to Entities. O LINQ to DataSet fornece consultas mais sofisticadas e otimizadas do que o <xref:System.Data.DataSet>, o [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] permite que você consulte diretamente os esquemas de banco de dados do SQL Server e o LINQ to Entities permite que você consulte um Modelo de Dados de Entidade.  
  
## <a name="linq-to-dataset"></a>LINQ to DataSet  
 O <xref:System.Data.DataSet> é um dos componentes mais amplamente usados em ADO.NET e é um elemento fundamental do modelo de programação desconectada no qual o ADO.NET se baseia. No entanto, apesar dessa importância, o <xref:System.Data.DataSet> limitou os recursos de consulta.  
  
 O LINQ to DataSet permite que você crie recursos mais sofisticados de consulta no <xref:System.Data.DataSet> usando a mesma funcionalidade de consulta que está disponível para muitas outras fontes de dados.  
  
 Para obter mais informações, consulte [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md).  
  
## <a name="linq-to-sql"></a>LINQ to SQL  
 O [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] fornece uma infraestrutura em tempo de execução para gerenciar dados relacionais como objetos. No [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)], o modelo de dados de um banco de dados relacional é mapeado para um modelo de objeto expresso na linguagem de programação do desenvolvedor. Quando você executa o aplicativo, o [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] converte consultas integradas da linguagem no modelo de objeto no SQL e as envia para o banco de dados para execução. Quando o banco de dados retorna os resultados, o [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] os converte em objetos que podem ser manipulados.  
  
 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] inclui suporte para procedimentos armazenados e funções definidas pelo usuário no banco de dados e para herança no modelo de objeto.  
  
 Para obter mais informações, consulte [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md).  
  
## <a name="linq-to-entities"></a>LINQ to Entities  
 Por meio do Modelo de Dados de Entidade, os dados relacionais são expostos como objetos no ambiente .NET. Isso torna a camada de objeto um destino ideal para o suporte a LINQ, permitindo que os desenvolvedores formulem consultas no banco de dados a partir da linguagem usada para criar a lógica de negócios. Essa funcionalidade é conhecida como LINQ to Entities. Consulte [LINQ to Entities](../../../../framework/data/adonet/ef/language-reference/linq-to-entities.md) para obter mais informações.  
  
## <a name="see-also"></a>Confira também

- [LINQ e o ADO.NET](../../../../framework/data/adonet/linq-and-ado-net.md)
- [LINQ (Consulta Integrada à Linguagem) (C#)](./index.md)
