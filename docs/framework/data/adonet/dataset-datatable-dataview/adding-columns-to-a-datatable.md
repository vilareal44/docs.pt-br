---
title: Adicionando colunas a um DataTable
description: Uma DataTable contém objetos DataColumn referenciados pela propriedade Columns da tabela. Use este código de exemplo para adicionar colunas a uma tabela no ADO.NET.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e85c4a0e-4f3f-458c-b58b-0ddbc06bf974
ms.openlocfilehash: 9d6d21696acd7a6b63cfd6d2ea7e906ec2acd7c9
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286941"
---
# <a name="adding-columns-to-a-datatable"></a>Adicionando colunas a um DataTable
Um <xref:System.Data.DataTable> contém uma coleção de <xref:System.Data.DataColumn> objetos referenciados pela propriedade **Columns** da tabela. Esta coleção de colunas, junto com quaisquer restrições, define o esquema, ou a estrutura, da tabela.  
  
 Você cria objetos **DataColumn** em uma tabela usando o construtor **DataColumn** ou chamando o método **Add** da propriedade **Columns** da tabela, que é um <xref:System.Data.DataColumnCollection> . O método **Add** aceita argumentos de expressão **ColumnName**, **DataType**e **expression** opcionais e cria uma nova **DataColumn** como um membro da coleção. Ele também aceita um objeto **DataColumn** existente e o adiciona à coleção e retorna uma referência para a **DataColumn** adicionada, se solicitado. Como os objetos **DataTable** não são específicos de nenhuma fonte de dados, .NET Framework tipos são usados ao especificar o tipo de dados de uma **DataColumn**.  
  
 O exemplo a seguir adiciona quatro colunas a uma **DataTable**.  
  
```vb  
Dim workTable As DataTable = New DataTable("Customers")  
  
Dim workCol As DataColumn = workTable.Columns.Add( _  
    "CustID", Type.GetType("System.Int32"))  
workCol.AllowDBNull = false  
workCol.Unique = true  
  
workTable.Columns.Add("CustLName", Type.GetType("System.String"))  
workTable.Columns.Add("CustFName", Type.GetType("System.String"))  
workTable.Columns.Add("Purchases", Type.GetType("System.Double"))  
```  
  
```csharp  
DataTable workTable = new DataTable("Customers");  
  
DataColumn workCol = workTable.Columns.Add("CustID", typeof(Int32));  
workCol.AllowDBNull = false;  
workCol.Unique = true;  
  
workTable.Columns.Add("CustLName", typeof(String));  
workTable.Columns.Add("CustFName", typeof(String));  
workTable.Columns.Add("Purchases", typeof(Double));  
```  
  
 No exemplo, observe que as propriedades da coluna **CustID** estão definidas para não permitir valores **DBNull** e para restringir valores a serem exclusivos. No entanto, se você definir a coluna **CustID** como a coluna de chave primária da tabela, a propriedade **AllowDBNull** será automaticamente definida como **false** e a propriedade **Unique** será definida automaticamente como **true**. Para obter mais informações, consulte [definindo chaves primárias](defining-primary-keys.md).  
  
> [!CAUTION]
> Se um nome de coluna não for fornecido para uma coluna, a coluna receberá um nome padrão incremental da coluna*N,* começando com "Column1", quando ela for adicionada ao **DataColumnCollection**. Recomendamos que você evite a Convenção de nomenclatura de "coluna*N*" ao fornecer um nome de coluna, pois o nome que você fornece pode entrar em conflito com um nome de coluna padrão existente no **DataColumnCollection**. Se o nome fornecido já existir, será gerada uma exceção.  
  
 Se você estiver usando <xref:System.Xml.Linq.XElement> como <xref:System.Data.DataColumn.DataType%2A> de um <xref:System.Data.DataColumn> no <xref:System.Data.DataTable>, a serialização XML não funcionará quando você ler dados. Por exemplo, se você escreve um <xref:System.Xml.XmlDocument> usando o método `DataTable.WriteXml`, na serialização para XML há um nó pai adicional no <xref:System.Xml.Linq.XElement>. Para resolver esse problema, use o tipo <xref:System.Data.SqlTypes.SqlXml> em vez de <xref:System.Xml.Linq.XElement>. `ReadXml` e `WriteXml` funcionam corretamente com <xref:System.Data.SqlTypes.SqlXml>.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Data.DataColumn>
- <xref:System.Data.DataColumnCollection>
- <xref:System.Data.DataTable>
- [Definição de esquema de DataTable](datatable-schema-definition.md)
- [DataTables](datatables.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
