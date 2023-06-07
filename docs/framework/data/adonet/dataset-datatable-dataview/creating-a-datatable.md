---
title: Criando uma DataTable
description: Aprenda a criar uma DataTable em ADO.NET, que representa uma tabela de dados relacionais na memória, para uso de forma independente ou por outros objetos .NET Framework.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: eecf9d78-60e3-4fdc-8de0-e56c13a89414
ms.openlocfilehash: 335137eeef02e590539c6d83e46cb39901a1e03f
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286915"
---
# <a name="creating-a-datatable"></a>Criando uma DataTable
Uma <xref:System.Data.DataTable>, que representa uma tabela de dados relacionais de memória, pode ser criada e usada independentemente ou pode ser usada por outros objetos do .NET Framework, mais comumente como membro de um <xref:System.Data.DataSet>.  
  
 Você pode criar um objeto **DataTable** usando o construtor **DataTable** apropriado. Você pode adicioná-lo ao **DataSet** usando o método **Add** para adicioná-lo à coleção **Tables** do objeto **DataTable** .  
  
 Você também pode criar **objetos DataTable** dentro de um **DataSet** usando os métodos **Fill** ou **FillSchema** do objeto **DataAdapter** , ou de um esquema XML predefinido ou inferido usando os métodos **ReadXml**, **ReadXmlSchema**ou **InferXmlSchema** do **conjunto**de objetos. Observe que depois de ter adicionado uma **DataTable** como um membro da coleção **Tables** de um **conjunto**de um DataSet, você não pode adicioná-lo à coleção de tabelas de qualquer outro **conjunto**de um.  
  
 Quando você cria uma **DataTable**pela primeira vez, ela não tem um esquema (ou seja, uma estrutura). Para definir o esquema da tabela, você deve criar e adicionar <xref:System.Data.DataColumn> objetos à coleção de **colunas** da tabela. Você também pode definir uma coluna de chave primária para a tabela e criar e adicionar objetos de **restrição** à coleção de **restrições** da tabela. Depois de definir o esquema para uma **DataTable**, você pode adicionar linhas de dados à tabela adicionando objetos **DataRow** à coleção **Rows** da tabela.  
  
 Não é necessário fornecer um valor para a <xref:System.Data.DataTable.TableName%2A> propriedade quando você cria uma **DataTable**; você pode especificar a propriedade em outro momento ou deixá-la vazia. No entanto, quando você adiciona uma tabela sem um valor de **TableName** a um **conjunto**de dados, a tabela receberá um nome padrão incremental da tabela*N*, começando com "Table" para Table0.  
  
> [!NOTE]
> Recomendamos que você evite a Convenção de nomenclatura "Table*N*" ao fornecer um valor de **TableName** , pois o nome que você fornece pode entrar em conflito com um nome de tabela padrão existente no **conjunto**de valores. Se o nome fornecido já existir, será gerada uma exceção.  
  
 O exemplo a seguir cria uma instância de um objeto **DataTable** e atribui a ele o nome "Customers".  
  
```vb  
Dim workTable as DataTable = New DataTable("Customers")  
```  
  
```csharp  
DataTable workTable = new DataTable("Customers");  
```  
  
 O exemplo a seguir cria uma instância de uma **DataTable** adicionando-a à coleção **Tables** de um **DataSet**.  
  
```vb  
Dim customers As DataSet = New DataSet  
Dim customersTable As DataTable = _  
   customers.Tables.Add("CustomersTable")  
```  
  
```csharp  
DataSet customers = new DataSet();  
DataTable customersTable = customers.Tables.Add("CustomersTable");  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Data.DataTable>
- <xref:System.Data.DataTableCollection>
- [DataTables](datatables.md)
- [Populating a DataSet from a DataAdapter](../populating-a-dataset-from-a-dataadapter.md) (Preenchendo um DataSet por meio de um DataAdapter)
- [Carregar um conjunto de dados do XML](loading-a-dataset-from-xml.md)
- [Carregando informações de esquema de conjunto de dados de XML](loading-dataset-schema-information-from-xml.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
