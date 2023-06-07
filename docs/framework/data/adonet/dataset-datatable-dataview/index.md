---
title: DataSets, DataTables e DataViews
description: Aprenda várias maneiras de trabalhar com um conjunto de dados ADO.NET, uma representação residente na memória que fornece um modelo de programação relacional consistente.
ms.date: 03/30/2017
ms.assetid: 6d4c4b69-8919-4224-8a65-6cca1c61b48f
ms.openlocfilehash: 53e12f701b9be1938d62f46bbeb6e63d95c03386
ms.sourcegitcommit: e7748001b1cee80ced691d8a76ca814c0b02dd9b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86374501"
---
# <a name="datasets-datatables-and-dataviews"></a>DataSets, DataTables e DataViews

O <xref:System.Data.DataSet> do ADO.NET é uma representação de dados residentes na memória que fornecem um modelo de programação relacional consistente independentemente da origem dos dados que contém. Um <xref:System.Data.DataSet> representa um conjunto completo de dados que incluem as tabelas que contêm, pedem e restringem os dados, bem como as relações entre tabelas.  
  
Há várias formas de trabalhar com um <xref:System.Data.DataSet>, que podem ser aplicadas independentemente ou em combinação. Você pode:  
  
- Crie um <xref:System.Data.DataTable>, <xref:System.Data.DataRelation> e <xref:System.Data.Constraint> programaticamente dentro de um <xref:System.Data.DataSet> e preencha as tabelas com dados.  
  
- Preencha o <xref:System.Data.DataSet> com tabelas de dados de uma fonte de dados relacional existente usando um `DataAdapter`.  
  
- Carregue e salve o conteúdo de <xref:System.Data.DataSet> usando XML. Para obter mais informações, consulte [Using XML in a DataSet](using-xml-in-a-dataset.md) (Usando XML em um DataSet).  
  
Um <xref:System.Data.DataSet> fortemente tipado também pode ser transportado usando um serviço Web XML. O design do <xref:System.Data.DataSet> torna-o ideal para transmitir dados usando serviços Web XML. Para obter uma visão geral dos serviços Web XML, consulte [XML Web Services Overview](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/w9fdtx28(v=vs.100)) (Visão geral dos serviços Web XML). Para obter um exemplo de como consumir um <xref:System.Data.DataSet> de um serviço Web XML, consulte [Consuming a DataSet from an XML Web Service](consuming-a-dataset-from-an-xml-web-service.md) (Consumindo um conjunto de dados de um serviço Web XML).  
  
## <a name="in-this-section"></a>Nesta seção

 [Diretrizes de segurança](security-guidance.md)  
 Fornece diretrizes de segurança para o <xref:System.Data.DataSet> e o <xref:System.Data.DataTable> .

 [Criando um conjunto de uma](creating-a-dataset.md)  
 Descreve a sintaxe para criar uma instância de um <xref:System.Data.DataSet>.  
  
 [Adding a DataTable to a DataSet](adding-a-datatable-to-a-dataset.md) (Adicionando um DataTable a um DataSet)  
 Descreve como criar e adicionar tabelas e colunas em um <xref:System.Data.DataSet>.  
  
 [Adding DataRelations](adding-datarelations.md) (Adicionando DataRelations)  
 Descreve como criar relacionamentos entre tabelas em um <xref:System.Data.DataSet>.  
  
 [Navegando em DataRelations](navigating-datarelations.md)  
 Descreve como usar os relacionamentos entre tabelas em um <xref:System.Data.DataSet> para retornar as linhas filho ou pai de uma relação pai-filho.  
  
 [Merging DataSet Contents](merging-dataset-contents.md) (Mesclando o conteúdo do DataSet)  
 Descreve como mesclar o conteúdo de uma matriz <xref:System.Data.DataSet>, <xref:System.Data.DataTable> ou <xref:System.Data.DataRow> em outro <xref:System.Data.DataSet>.  
  
 [Copiando conteúdo do DataSet](copying-dataset-contents.md)  
 Descreve como criar uma cópia de um <xref:System.Data.DataSet> que pode conter o esquema bem como dados especificados.  
  
 [Manipular eventos do DataSet](handling-dataset-events.md)  
 Descreve os eventos de um <xref:System.Data.DataSet> e como usá-los.  
  
 [DataSets tipados](typed-datasets.md)  
 Descreve o que um <xref:System.Data.DataSet> tipado é e como criá-lo e usá-lo.  
  
 [DataTables](datatables.md)  
 Descreve como criar um <xref:System.Data.DataTable>, definir o esquema e manipular dados.  
  
 [DataTableReaders](datatablereaders.md)  
 Descreve como criar e usar um <xref:System.Data.DataTableReader>.  
  
 [DataViews](dataviews.md)  
 Descreve como criar e trabalhar com `DataViews` e trabalhar com eventos de <xref:System.Data.DataView>.  
  
 [Using XML in a DataSet](using-xml-in-a-dataset.md) (Usando XML em um DataSet)  
 Descreve como o <xref:System.Data.DataSet> interage com XML como uma fonte de dados, incluindo o carregamento e a persistência do conteúdo de um <xref:System.Data.DataSet> como dados XML.  
  
 [Consumir um DataSet de um serviço Web XML](consuming-a-dataset-from-an-xml-web-service.md)  
 Descreve como criar um serviço Web XML que usa um <xref:System.Data.DataSet> para transportar dados.  
  
## <a name="related-sections"></a>Seções relacionadas

 [Novidades no ADO.NET](../whats-new.md)  
 Apresenta recursos que são novos no ADO.NET.  
  
 [Visão geral do ADO.NET](../ado-net-overview.md)  
 Fornece uma introdução ao design e aos componentes do ADO.NET.  
  
 [Populating a DataSet from a DataAdapter](../populating-a-dataset-from-a-dataadapter.md) (Preenchendo um DataSet por meio de um DataAdapter)  
 Descreve como carregar um **DataSet** com os dados de uma fonte de dados.  
  
 [Atualizando fontes de dados com DataAdapters](../updating-data-sources-with-dataadapters.md)  
 Descreve como resolver as alterações nos dados em um **DataSet** de volta para a fonte de dados.  
  
 [Adding Existing Constraints to a DataSet](../adding-existing-constraints-to-a-dataset.md) (Adicionando restrições existentes a um DataSet)  
 Descreve como preencher um **DataSet** com informações de chave primária de uma fonte de dados.  
  
## <a name="see-also"></a>Confira também

- [ADO.NET](../index.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
